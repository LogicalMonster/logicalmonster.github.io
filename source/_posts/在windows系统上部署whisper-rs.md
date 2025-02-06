---
title: 在windows系统上部署whisper-rs
date: 2025-02-06 23:27:04
tags:
---
本文介绍如何在Windows系统上部署whisper-rs项目进行语言流式转录。

## 前置条件

构建该项目需要在VS中安装Clang组件，具体教程见：[How to Install Clang on Windows - wikiHow](https://www.wikihow.com/Install-Clang-on-Windows)。安装组件后，需先打开Developer Command Prompt for VS 2022，在其中输入

```Developer Command Prompt for VS 2022
where.exe clang
```

将显示的路径所对应的形如`C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\Llvm\x64\bin`的路径作为环境变量LIBCLANG_PATH，并将路径添加到PATH中。注意不是`Llvm\bin`，而是`Llvm\x64\bin`。

## 构建并使用

```powershell
git clone --recursive https://github.com/tazz4843/whisper-rs.git
cd whisper-rs
```

下载[ggerganov/whisper.cpp at main](https://huggingface.co/ggerganov/whisper.cpp/tree/main)中的模型到项目根目录(如ggml-base.en.bin)。还可下载[whisper.cpp/samples at master · ggerganov/whisper.cpp](https://github.com/ggerganov/whisper.cpp/tree/master/samples)中的测试音频文件到项目根目录(如samples_jfk.wav)。

```
cargo run --example basic_use .\ggml-base.en.bin .\samples_jfk.wav
cargo run --example audio_transcription
```

注意，`example/audio_transcription.rs`需手动设置模型和测试音频文件路径。对于ggml-tiny模型，`audio_transcription`似乎有bug会触发panic。

更多内容，请见项目：[tazz4843/whisper-rs: Rust bindings to https://github.com/ggerganov/whisper.cpp](https://github.com/tazz4843/whisper-rs)。
