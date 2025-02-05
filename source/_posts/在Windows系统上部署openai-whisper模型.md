---
title: 在Windows系统上部署openai-whisper模型
date: 2025-02-05 13:47:09
tags:
---
本文介绍如何在Windows系统上部署openai/whisper模型。除开ffmpeg的安装外，其余步骤都可参见[openai/whisper: Robust Speech Recognition via Large-Scale Weak Supervision](https://github.com/openai/whisper)。建议使用scoop安装ffmpeg，因为Chocolatey无法更改自定义默认下载目录。

## 安装Scoop和ffmpeg音频处理库

```powershell
# 安装powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh -outfile 'install.ps1'
.\install.ps1 -ScoopDir 'E:\ProgramHome\Scoop' -ScoopGlobalDir 'E:\ProgramHome\GlobalScoopApps' -NoProxy
scoop install ffmpeg
```

## 安装whisper

```powershell
# 安装稳定版
pip install -U openai-whisper
# 安装Github仓库的版本
pip install git+https://github.com/openai/whisper.git 
```

## python用法

转录也可以在Python中进行：

```python
import whisper

model = whisper.load_model("turbo")
result = model.transcribe("audio.mp3")
print(result["text"])
```

在内部， `transcribe()`方法读取整个文件并使用一个滑动30秒的窗口来处理音频，并在每个窗口上执行自回归序列到序列预测。

以下是`whisper.detect_language()`和`whisper.decode()`的示例用法，可提供对模型的低级访问。

```python
import whisper

model = whisper.load_model("turbo")

# load audio and pad/trim it to fit 30 seconds
audio = whisper.load_audio("audio.mp3")
audio = whisper.pad_or_trim(audio)

# make log-Mel spectrogram and move to the same device as the model
mel = whisper.log_mel_spectrogram(audio, n_mels=model.dims.n_mels).to(model.device)

# detect the spoken language
_, probs = model.detect_language(mel)
print(f"Detected language: {max(probs, key=probs.get)}")

# decode the audio
options = whisper.DecodingOptions()
result = whisper.decode(model, mel, options)

# print the recognized text
print(result.text)
```

## 关于语音质量和声音预处理

通常音视频文件我们是可以直接拿来用的，例如：  

```powershell
whisper audio.mp4 --model medium
```

**但为了提高精度和处理效率，我们可以对音频进行一些预处理**

Whisper通常在16kHz采样率的单声道音频下表现最佳  
所以我们可以使用  

```powershell
ffmpeg -i input_file.wav -ar 16000 -ac 1 output_file.wav  
```

把音频文件进行预处理，这里输入文件也可以是MP4或者其他格式的音视频文件

**如果遇到音频音量过小，就也可以使用**

```powershell
ffmpeg -i input_file.wav -filter:a "volume=1.5" output_file.wav  
```

进行音频的标准化预处理。

音频处理部分，参考了: [Windows本地部署OpenAI的强大语音识别系统Whisper| 支持CPU/GPU运算 | - 米拉一频道](https://www.milaone.com/archives/85.html)

