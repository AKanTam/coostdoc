# coostdoc 文档网站构建指南

## 项目概述
这是一个使用Hugo静态网站生成器构建的文档网站，用于展示coost v3.0.2 项目API的文档。项目使用hugo-book主题，支持中英文双语内容。
删除新版本hugo不再支持的google_analytics_async.html依赖

## 快速开始

### 1. 安装Hugo
请确保已安装Hugo扩展版：
```bash
# 检查Hugo版本
hugo version
```
### 2. 本地开发
```bash
# 启动本地开发服务器(默认端口1313)
hugo server
```
### 3. 构建静态网站
```bash
# 生成静态网站到public目录
hugo
```
## 项目结构
```bash
coostdoc/
├── content/      # 文档内容(Markdown格式)
│   ├── cn/       # 中文文档
│   └── en/       # 英文文档
├── public        # 构建生成的静态文件(构建后自动创建)
├── themes/       # Hugo主题(使用hugo-book)
├── config.toml   # 主配置文件
└── public/       # 构建生成的静态文件(构建后自动创建)
```
