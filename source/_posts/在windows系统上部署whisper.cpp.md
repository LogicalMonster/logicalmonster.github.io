---
title: 在windows系统上部署whisper.cpp
date: 2025-02-06 15:34:46
tags:
---
本文介绍如何在Windows系统上部署whisper.cpp项目进行语言流式转录。

## 克隆仓库并下载base.en模型

```powershell
git clone https://github.com/ggerganov/whisper.cpp.git
cd whisper.cpp
.\models\download-ggml-model.cmd base.en
```

base.en模型是English-only model，若使用Multilingual model请去掉.en。所有可用模型，请见：[whisper.cpp/models/README.md at master · ggerganov/whisper.cpp](https://github.com/ggerganov/whisper.cpp/blob/master/models/README.md)。

## 下载 vcpkg 并配置环境变量

请参见: [在 Visual Studio Code 中使用 CMake 安装和管理包 | Microsoft Learn](https://learn.microsoft.com/zh-cn/vcpkg/get_started/get-started-vscode?pivots=shell-powershell)。需要注意的是不要忘记配置环境变量`VCPKG_ROOT`，配置该环境变量，可以免于创建`CMakeUserPresets.json`文件。`CMakePresets.json`文件中的configurePresets项中的name，需要与`CMakeUserPresets.json`文件中的configurePresets项中的inherits相同。

## 安装SDL2库

```powershell
vcpkg new --application
vcpkg add port sdl2
```

`CMakePresets.json`文件模板：

```json
{
	"version": 8,
	"configurePresets": [
		{
			"name": "amd64",
			"displayName": "Visual Studio Community 2022 Release - amd64",
			"description": "将编译器用于 Visual Studio 17 2022 (x64 体系结构)",
			"generator": "Visual Studio 17 2022",
			"toolset": "host=x64",
			"architecture": "x64",
			"binaryDir": "${sourceDir}/build",
			"cacheVariables": {
				"CMAKE_INSTALL_PREFIX": "${sourceDir}/install",
				"CMAKE_C_COMPILER": "cl.exe",
				"CMAKE_CXX_COMPILER": "cl.exe",
				"CMAKE_TOOLCHAIN_FILE": "$env{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake",
				"GGML_CUDA": "1",
				"WHISPER_SDL2": "ON"
			}
		}
	],
	"buildPresets": [
        {
            "name": "amd64-debug",
            "displayName": "Visual Studio Community 2022 Release - amd64 - Debug",
            "configurePreset": "amd64",
            "configuration": "Debug"
        },
        {
            "name": "amd64-release",
            "displayName": "Visual Studio Community 2022 Release - amd64 - Release",
            "configurePreset": "amd64",
            "configuration": "Release"
        }
    ]
}
```

若使用的非英伟达显卡，去掉`"GGML_CUDA": "1",`这一行。

将`examples\CMakeLists.txt`的第九行

```CMakeLists
find_package(SDL2 REQUIRED)
```

改为

```CMakeLists
find_package(SDL2 CONFIG REQUIRED)
```

## 编译

```powershell
cmake --preset amd64
cmake --build --preset amd64-release
```

编译中若遇到问题就删除build或out目录，重新执行上述命令。

## 运行

```powershell
.\build\bin\Release\whisper-stream.exe -l auto -m .\models\ggml-base.en.bin -t 8 --step 500 --length 5000
```

## whisper-stream的选项

| 短命令      | 长命令             | 默认                      | 说明                         |
| -------- | --------------- | ----------------------- | -------------------------- |
| -h       | --help          | default                 | 显示此帮助消息并退出                 |
| -t N     | --thread N      | 4                       | 计算期间使用的线程数                 |
|          | --step N        | 3000                    | 音频步长（以毫秒为单位）               |
|          | --length N      | 10000                   | 音频长度（以毫秒为单位）               |
|          | --keep N        | 200                     | 保留上一步的音频（以毫秒为单位）           |
| -c ID    | --capture ID    | -1                      | 捕获设备 ID                    |
| -mt N    | --max-tokens N  | 32                      | 每个音频块的最大令牌数                |
| -ac N    | --audio-ctx N   | 0                       | 音频上下文大小（0 - 全部）            |
| -vth N   | --vad-thold N   | 0.60                    | 语音活动检测阈值                   |
| -fth N   | --freq-thold N  | 100.00                  | 高通频率截止                     |
| -tr      | --translate     | false                   | 从源语言翻译成英语                  |
| -nf      | --no-fallback   | false                   | 解码时不使用温度回退                 |
| -ps      | --print-special | false                   | 打印特殊标记                     |
| -kc      | --keep-context  | false                   | 保留音频块之间的上下文                |
| -l LANG  | --language LANG | en                      | 口语                         |
| -m FNAME | --model FNAME   | models/ggml-base.en.bin | 模型路径                       |
| -f FNAME | --file FNAME    |                         | 文本输出文件名                    |
| -tdrz    | --tinydiarize   | false                   | 启用 tinydiarize（需要 tdrz 模型） |
| -sa      | --save-audio    | false                   | 将录制的音频保存到文件                |
| -ng      | --no-gpu        | false                   | 禁用 GPU 推理                  |
| -fa      | --flash-attn    | false                   | 推理期间闪烁注意                   |

