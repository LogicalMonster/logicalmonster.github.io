---
title: 配置Hexo+Obsidian
date: 2025-02-03 16:30:43
tags:
---
详细步骤见下述链接，需要注意的是需要忽略`.obsidian/workspace.json`。

若使用`hexo-backlink`，除了需要修改Github Action文件外，还需在根目录的`_config.yml`中添加

```yml
backlink:
  enable: true
```

More info: 
[Obsidian+Git完美维护Hexo博客 - 知乎](https://zhuanlan.zhihu.com/p/554333805)
[Cyrusky/hexo-backlink: This plugin is for transfer Obsidian-type backlink to standard hexo in-site post link.](https://github.com/Cyrusky/hexo-backlink)
