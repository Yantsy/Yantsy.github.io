### ffplay是如何实现音视频同步的？
#### 整体框架
要实现音视频的同步，我们首先需要一个时钟来对时间进行记录和管理。为此，ffplay创建了一个时钟类型Clock用于管理时钟,并根据不同的参考系创建了不同的时钟对象audclk,vidclk,extclk。这些时钟被交给VideoState类统一管理：

```cpp
typedef struct Clock {
    double pts;       /* clock base */
    double pts_drift; /* clock base minus time at which we updated the clock */
    double last_updated;
    double speed;
    int serial; /* clock is based on a packet with this serial */
    int paused;
    int* queue_serial; /* pointer to the current packet queue serial, used for obsolete clock
                          detection */
} Clock;
//......
typedef struct VideoState{
    //...
    Clock audclk //以音频为主时钟
    Clock vidclk//以视频为主时钟
    Clock extclk//以外部时钟为主时钟
    //...对于不同的时钟，会在后文有详细介绍，此处仅需知道这三个东西就行了喵～

}

```
具体使用过程中，先选择主时钟，然后再使用相应的同步机制。由于ffplay默认使用的是音频主时钟，故对其优先介绍。

### 以音频为主时钟

以音频为主时钟，即通过音频获取当前视频进度条a，然后计算当前最新画面对应的视频进度b，并计算二者的差值a-b，若差值为负数，则表示音频慢于视频，那么视频相关的线程就得等一等音频，反之则要让视频相关的线程加快执行，努力赶上音频进度。

为了实现音视频同步，我们至少需要回答两个问题：
```
1.如何获取音视频的最新进度？
2.音视频进度的差值在多大范围内是合理的？
```
#### 如何获取音视频的进度？

容器内的音视频流本身是不知道什么是时间的，而是借助时间戳(timestamp)和时间基(timebase)来对时间进行记录。

时间基为最基础的时间单位，时间戳的数字则代表着音视频经过了相应数量的时间基所折算的时间,以pts(presentation timestamp)为例，pts*timebase=ptstime(即该音频或视频应该被你看到或听见的时间进度)，若某段音频的pts为4272，时间基为0.0001秒,那么你就应该在视频开始播放后0.4272秒听到该段音频。

