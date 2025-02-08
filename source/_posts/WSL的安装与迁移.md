---
title: WSL的安装与迁移
date: 2025-02-08 10:01:54
tags:
---
管理员身份运行PowerShell，执行：

```powershell
wsl -l -v
wsl --install Ubuntu-22.04
wsl --shutdown Ubuntu-22.04
wsl --export Ubuntu-22.04 E:\Ubuntu22.tar
wsl --unregister Ubuntu-22.04
wsl --import Ubuntu-22.04 E:\WSL\Ubuntu22 E:\Ubuntu22.tar --version 2
ubuntu2204 config --default-user <username>
```

注意：需要手动创建E:\WSL\Ubuntu22目录

具体可见：
[WSL 的基本命令 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands)
[WSL安装linux系统以及从C盘迁移至其他盘_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1EF4m1V7pp/?spm_id_from=333.337.search-card.all.click&vd_source=0bba60c332ccedf1cb8cc66720f1610f)
