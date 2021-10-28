---
layout: post
title: Django打包成可执行文件
date: 2021-10-28
Author: qiangwushuang 
tags: [Django]
comments: true
toc: false
---

1. 环境  
    windows7x64 + python3.8 + Django + Pyinstaller  
    ```需要确保当前环境可以正确执行Django```
2. 制作spec文件(在项目路径下)
   ```bash
   pyi-makespec -D manage.py
   ```
3. 打包(在项目路径下)
   ```bash
   pyinstaller manage.spec
   ```
4. 执行  
   生成路径".\dist\manage"，运行可执行文件manage.exe  
   ```bash
   manage.exe runserver 0.0.0.0:8000 --noreload 
   ```