

| **Attribute**            | **Default** | **Description**                                                                         |
|:-------------------------|:------------|:----------------------------------------------------------------------------------------|
| `background-video`       |             | 单个视频源，或用逗号分隔的视频源列表。                      |
| `background-video-loop`  | false       | 视频是否应重复播放。                                              |
| `background-video-muted` | false       | 音频是否应该静音。                                                     |
| `background-size`        | cover       | 使用 `cover` 进行全屏和部分裁剪，或使用 `contain` 进行信包处理。            |
| `background-opacity`     | 1           | 背景视频的不透明度（0-1）。0 表示透明，1 表示完全不透明。 |

例如：

``` markdown
## Slide Title {background-video="video.mp4" background-video-loop="true" background-video-muted="true"}

本幻灯片的背景视频将以静音方式循环播放.
```
---
