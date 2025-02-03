---
title: 配置butterfly主题
date: 2025-02-01 21:16:27
tags:
---
本文章介绍如何使用Hexo的butterfly主题。

## 安装

```bash
git submodule add -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

## 应用主题

修改 Hexo 根目錄下的 `_config.yml`，把主題改為 `butterfly`

```yaml
theme: butterfly
```

## 升级建议

為了減少升級主題後帶來的不便，請使用以下方法（建議，可以不做）：

在 hexo 的根目錄創建一個文件 `_config.butterfly.yml`，並把主題目錄的 `_config.yml` 內容複製到 `_config.butterfly.yml` 去。

## 安装插件

将Github Action修改为如下内容：

```bash
# .github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
          # Ref: https://github.com/actions/setup-node#supported-version-syntax
          node-version: "latest"
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

More info: [hexo-theme-butterfly](https://github.com/jerryc127/hexo-theme-butterfly)
