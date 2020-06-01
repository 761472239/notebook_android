**安卓中使用VolumeShaper控制振幅**

1. 您可以VolumeShaper在音频应用中使用淡入，淡出，淡入淡出，淡入淡出以及其他短暂的自动音量转换。VolumeShaper类是在8.0的Android（API级26）和后可用。

2.  您可以通过对 `AudioTrack` 或 `MediaPlayer` 的实例调用 `createVolumeShaper()` 来创建 `VolumeShaper`。`VolumeShaper` 仅对创建它的 AudioTrack 或 MediaPlayer 生成的音频起作用。 

# VolumeShaper.Configuration

 `VolumeShaper` 的行为由它的 `VolumeShaper.Configuration` 定义。该配置指定了*音量曲线、插值器类型和持续时间。* 

## 音量曲线

音量曲线表示振幅随时间的变化。它由一对浮点数的数组 x[] 和 y[] 定义，该数组定义了一系列控制点。每个 (x, y) 对分别表示时间和音量。这些数组的长度必须相等，并且包含 2 到 16 个值。（最大曲线长度在 `getMaximumCurvePoints()` 中定义。）

时间坐标在 [0.0, 1.0] 的区间内指定。第一个时间点必须为 0.0，最后一个时间点必须为 1.0，并且时间必须单调递增。

音量坐标在 [0.0, 1.0] 区间内按线性调整的方式指定。

```java
setCurve(new float[]{0f, 0.3f, 0.6f, 1f},
         new float[]{0f, 1f, 0f, 1f})
```

**注意：时间区间是[0 , 1] 音量区间可以渐变**



## 插值器类型

音量曲线始终会通过指定的控制点。控制点之间的值由样条曲线根据配置的插值器类型派生而来。可用的 `VolumeShaper` 插值器类型有四个常量：

- VolumeShaper.Configuration.INTERPOLATOR_TYPE_STEP
  使用分段曲线的方式控制音量变化

- VolumeShaper.Configuration.INTERPOLATOR_TYPE_LINEAR
  使用线性的插值方式控制音量变化

- VolumeShaper.Configuration.INTERPOLATOR_TYPE_CUBIC

  使用三次曲线的插值方式控制音量变化

- VolumeShaper.Configuration.INTERPOLATOR_TYPE_CUBIC_MONOTONIC

  使用保持局部单调的三次曲线的插值方式控制音量变化

```java
setInterpolatorType(VolumeShaper.Configuration.INTERPOLATOR_TYPE_LINEAR);
```



## 持续时间



区间 [0.0, 1.0] 内的指定时间坐标会根据您指定的持续时间（以毫秒为单位）调整。这决定了 shaper 在运行曲线并将其应用于音频输出时，音量曲线在时间维度上的实际长度。 

```java
setDuration(1000 * 60 * 2);
```

# 使用 VolumeShaper

## 创建配置

 在构建 `VolumeShaper` 之前，您必须创建 `VolumeShaper.Configuration` 的实例。您可以使用 `VolumeShaper.Configuration.Builder()` 执行此操作： 

```java
VolumeShaper.Configuration config =
    new VolumeShaper.Configuration.Builder()
    .setDuration(3000)
    .setCurve(new float[] {0.f, 1.f}, new float[] {0.f, 1.f})
    .setInterpolatorType(VolumeShaper.Configuration.INTERPOLATOR_TYPE_LINEAR)
    .build();
```

在没有参数的情况下，`VolumeShaper.Configuration.Builder` 构造函数会返回使用以下默认设置创建配置的构建器：INTERPOLATOR_TYPE_CUBIC、1 秒的持续时间、没有曲线。您必须先向构建器添加曲线，然后再调用 `build()`。

框架为采用预构建曲线（每个曲线的持续时间为 1 秒）的配置提供常数：

- `VolumeShaper.Configuration.LINEAR_RAMP`
- `VolumeShaper.Configuration.CUBIC_RAMP`
- `VolumeShaper.Configuration.SINE_RAMP`
- `VolumeShaper.Configuration.SCURVE_RAMP`



## 创建 VolumeShaper

```java
    volumeShaper = myMediaPlayer.createVolumeShaper(config);
    volumeShaper = myAudioTrack.createVolumeShaper(config);
```

单个曲目或媒体播放器可以有多个 shaper 与之关联，您可以单独控制每个 shaper。一个曲目或播放器上的所有 shaper 的输出会叠加在一起。不能在 `AudioTracks` 或 `MediaPlayers` 之间共用 `VolumeShaper`，但您可以在对 `createVolumeShaper` 的调用中使用相同的配置，以便在多个 `AudioTracks` 或 `MediaPlayers` 上构建相同的 shaper。

创建 shaper 时，其第一个控制点（t = 0 时）将应用于音频流。如果初始音量不是 1.0，并且在创建 shaper 时，您的应用正在播放素材，则音频的音量可能会突然改变。最佳做法是从静音开始播放音频，并使用 `VolumeShaper` 在开始播放时实现渐强。创建一个音量从 0 开始并逐渐增强的 `VolumeShaper`。例如：

```java
setCurve(new float[] {0.f, 1.f}, new float[] {0.f, 1.f})    
```

 同时启动播放和 shaper。这样可以确保从没有声音开始播放，然后声音逐渐增强至最大音量。 



## 运行 VolumeShaper

 虽然第一个控制点的音量会在创建 shaper 后立即应用于音频路径，但在使用 `VolumeShaper.Operation.PLAY` 调用 `apply()` 方法之前，shaper 不会沿曲线前进。**创建 shaper 后，在第一次调用 `apply()` 时必须指定 `PLAY` 操作才能启动 shaper**。这会从其第一个控制点到最后一个控制点运行曲线： 

```java
shaper.apply(VolumeShaper.Operation.PLAY);
```



当 shaper 运行时，您可以发出指定 REVERSE（颠倒） 和 PLAY 操作的交替 `apply()` 调用。这样每次都会改变控制点的读出方向。

shaper 会不断调整音量并通过所有控制点，直到过期为止。当 shaper 到达曲线中的最后一个（对于 PLAY 操作）或第一个（对于 REVERSE 操作）控制点时，便会过期。

**shaper 过期时，音量会保持在最后一个设置，这可能是第一个或最后一个控制点。**您可以随时调用 **`VolumeShaper.getVolume()` 获取当前的音量。**

shaper 过期后，您可以发出另一个 `apply()` 调用以相反的方向运行曲线。例如，如果 shaper 在运行 `PLAY` 时过期，则下一个 `apply()` 必须为 `REVERSE`。在 `PLAY` 过期后调用 `PLAY` 或是在 `REVERSE` 过期后调用 `REVERSE` 没有影响。

您必须交替执行 `PLAY` 操作和 `REVERSE` 操作。无法将曲线从其第一个控制点播放到最后一个控制点，然后又从第一个控制点重新开始播放。您可以使用下一部分中介绍的 `replace()` 方法将曲线替换为其自身的副本。此操作会重置 shaper，需要执行 `PLAY` 操作来重新启动它。





## 更改曲线

 使用 `replace()` 方法可以更改 `VolumeShaper` 的曲线。此方法可以接受配置、操作和联接参数。您可以在 shaper 运行时或过期后随时调用 `replace()` 方法： 

```java
VolumeShaper.Configuration newConfig =
    new VolumeShaper.Configuration.Builder()
    .setDuration(1000)
    .setCurve(new float[] {0.f, 0.5f}, new float[] {0.f, 1.f})
    .setInterpolatorType(VolumeShaper.Configuration.INTERPOLATOR_TYPE_LINEAR)
    .build();
boolean join = true;
shaper.replace(newConfig, VolumeShaper.Operation.PLAY, join);
```

当在 shaper 运行期间调用 `replace()` 时，它会停止更改音量并保持在其当前值。然后，shaper 会尝试从第一个控制点开始运行新曲线。这意味着，操作参数可以控制 shaper 在调用之后是否运行。指定 `PLAY` 会立即开始运行新曲线，指定 `REVERSE` 会使 shaper 暂停在新曲线中第一个控制点的音量上。您可以稍后使用 `apply(VolumeShaper.Operation.PLAY)` 启动 shaper。

当您使用 `join = false` 调用 `replace()` 时，shaper 会在其曲线的第一个控制点指定的音量上开始运行曲线。这可能会导致音量不连贯。您可以通过使用 `join = true` 调用 `replace()` 来避免这种情况。这会将新曲线的第一个控制点设置为 shaper 的当前音量，并调整第一个和最后一个控制点之间的所有控制点的音量以维持新曲线的相对形状（最后一个控制点保持不变）。调整操作会永久更改 shaper 新曲线中的控制点。



## 移除 VolumeShaper

 当 `VolumeShaper` 的 `AudioTrack` 或 `MediaPlayer` 被释放或不再使用时，系统会将 VolumeShaper 关闭并进行垃圾回收。您可以对 shaper 调用 `close()` 方法来立即将其销毁。系统会在大约 20ms 内从音频流水线中移除 shaper。在音频播放期间关闭 `VolumeShaper` 时要小心。如果 shaper 的音量在您调用 `close()` 时小于 1.0，则 shaper 的音量值会变为 1.0。这可能会导致音量突然增大。 





# 使用Video播放视频

1. 界面布局定义一个VideoView组件，或者在程序中创建VideoView组件。

2. 调用VideoView的两个方法来加载指定视频：

   ```java
   setVideoPath();
   setVideoURI();
   ```

3. 调用VideoView的start()、stop() 、pause()方法来控制视频。

4. 与VideoView一起结合使用的还有一个MediaContraller类，他的作用是提供一个友好的控制界面，通过该控制界面来控制视频的播放。





```java
package com.zzu.rensiyu.myvideo1047;

import android.content.Context;
import android.util.AttributeSet;
import android.widget.VideoView;


public class MyVideoView extends VideoView {
    public MyVideoView(Context context) {
        super(context);
    }

    public MyVideoView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public MyVideoView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
//        getDefaultSize(int size, int measureSpec)
        int h = getDefaultSize(0, heightMeasureSpec);
        int w = getDefaultSize(0, widthMeasureSpec);
//        setMeasuredDimension(int measuredWidth, int measuredHeight)
        setMeasuredDimension(w, h);
    }
}

```

```java
package com.zzu.rensiyu.myvideo1047;

import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.view.Window;
import android.widget.MediaController;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {


    private MyVideoView videoView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        this.requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_main);
        videoView = findViewById(R.id.video);
        videoView.setMediaController(new MediaController(this));
        videoView.setVideoURI(Uri.parse("android.resource://" + getPackageName() + "/" + R.raw.video));
        videoView.start();
        videoView.findFocus();
        videoView.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
            @Override
            public void onCompletion(MediaPlayer mp) {
                videoView.start();
            }
        });


    }
}

```

