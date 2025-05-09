---
title: uv的使用
date: 2025-02-20 16:18:41
tags:
---
```bash
uv venv --python python3.12
# 从 requirements.txt 安装
uv pip install -r requirements.txt
# 同时安装和同步到 requirements.txt
uv pip sync requirements.txt
# 导出当前环境的依赖到 requirements.txt
uv pip freeze > requirements.txt
# 只包含直接安装的包（不包含依赖的依赖）
uv pip freeze --exclude-editable > requirements.txt
# 以兼容 pip 的格式导出
uv pip freeze --format pip > requirements.txt
```