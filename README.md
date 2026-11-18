# 哼唱转乐谱 · humming-to-sheet

把一段哼唱或清唱的录音变成五线谱。上传音频（或直接录音），在浏览器里自动识别音高，生成乐谱，还能播放出来听。

**在线使用：<https://yangtianxu920-arch.github.io/humming-to-sheet/>**

所有处理都在你自己的浏览器里完成，录音不会上传到任何服务器。

## 功能

- **输入**：拖入音频文件（M4A、WAV、MP3、AAC、FLAC、OGG），或者直接用麦克风录音。
- **识别音高**：用神经网络模型从人声里找出每个音的音高和起止时间。
- **生成五线谱**：可以设置拍号、速度、调号和音域；自带节拍器。
- **点击打拍**：跟着自己的哼唱点几下，算出实际速度，再按这个速度重新转谱。
- **播放和导出**：把转出来的谱子播放出来核对，也可以导出为文本。

## 是怎么做的

整个工具是一个 `index.html`，没有后端。

- 音频先在浏览器里重采样到 22050 Hz。
- 音高识别用的是 Spotify 开源的 [Basic Pitch](https://github.com/spotify/basic-pitch) 模型，通过 [TensorFlow.js](https://www.tensorflow.org/js) 在浏览器里运行（模型文件在 `model/` 目录）。
- 模型给出的是一帧一帧的音高，之后的步骤是自己写的：把相邻的帧合并成音符、过滤掉太短的抖动、按设定的速度和拍号对齐到节拍上，最后画成五线谱。

## 在自己电脑上运行

模型文件需要通过网页服务器加载，直接双击 `index.html` 打不开模型。下载仓库后在文件夹里起一个本地服务器即可，例如：

```
python -m http.server 8000
```

然后在浏览器里打开 `http://localhost:8000/`。

## 许可

- 本仓库的代码以 [MIT 许可证](LICENSE) 开源。
- `model/` 目录里的模型，以及 `index.html` 里内嵌的 Basic Pitch 推理代码，来自 Spotify 的 [Basic Pitch](https://github.com/spotify/basic-pitch) 项目，按其 Apache License 2.0 许可使用。
