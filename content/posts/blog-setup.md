---
title: "如何建立自己的个人博客"
date: 2025-11-28
draft: false
tags: ["blog","tags","git"]
categories: ["今日所学","技术教程"]
summary: "使用静态网站生成器 Hugo 搭建一个快速、简洁、高度可定制的个人学习博客，并通过 GitHub Pages 实现自动化部署和托管。博客可用于记录学习笔记、技术文章及开发日志"
---

## 1.如何设置合理tags&categories
```markdown
**分类（categories）：**
├── 计算机基础
├── 数据结构与算法
├── 客户端开发
├── 后端开发
├── 项目实践
├── 今日所学
├── 今日所思


**标签（tags）：**
├── 技术教程
├── 读书笔记
├── c++
├── MySQL
├── 排序算法
├── 图论
├── 调试技巧
├── Leetcode
├── Git
```
## 2.使用技术栈
 
| 技术      | 用途说明                          |
|-----------|----------------------------------
| **Hugo**  | 静态网站生成器，用于构建博客结构    
| **GitHub**| 代码托管与版本控制                 
| **GitHub Pages** | 静态网站托管服务            
| **Markdown** | 文章撰写格式                    
| **Git**   | 项目管理与部署流程                  

## 3.实现步骤

### 3.1 项目目录结构说明
```markdown
Myblog/
├── hugo.yaml              ← 博客配置文件（主题、菜单、样式等）
├── content/               ← 存放你写的博客文章（.md 文件）
├── assets/                ← 自定义样式，如 custom.css
├── static/                ← 静态资源（如图片、封面图）
├── public/                ← Hugo 构建后的静态页面（不要手动修改）
├── themes/                ← 博客主题目录（比如 PaperMod）
├── .github/workflows/     ← GitHub 自动部署配置目录
│   └── deploy.yml         ← GitHub Actions 脚本，用来自动部署博客
```

### 3.2 deploy.yml 中关键内容说明
```yaml
on:
  push:
    branches: ["main"]

```
→ 当你向 main 分支推送内容时触发部署
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Install Hugo CLI
```
→ 在 GitHub 的云服务器（Ubuntu）上安装 Hugo
```yaml
- name: Build with Hugo
  run: hugo --minify
```
使用 hugo 命令构建网站，把 Markdown 转成 HTML
```yaml
- name: Deploy
  uses: peaceiris/actions-gh-pages
```
→ 将生成的静态网站（public/）部署到 GitHub Pages

### 3.3 github仓库操作逻辑

```bash
git init               # 初始化本地仓库
git add .              # 添加所有文件到暂存区
git commit -m "初始化" # 提交更改

git remote add origin https://github.com/你的用户名/Myblog.git #先在github建一个仓库（Myblog），这一步是将本地文件夹链接了github云仓库

git push -u origin main #推送代码到仓库
```

### 3.4 更新博客步骤

```bash
git add .
git commit -m "新增文章：xxx" #添加必要说明方便版本管理
git push -u origin main

```

### 3.5 最常用git操作速查表

| 功能         | 命令                                       | 说明                           |
|--------------|--------------------------------------------|--------------------------------|
| 初始化项目   | `git init`                                 | 开始一个新的 Git 仓库             |
| 添加文件     | `git add .`                                | 添加当前目录下所有更改             |
| 提交改动     | `git commit -m "说明"`                      | 将更改记录为一个历史快照           |
| 推送更新     | `git push`                                 | 上传提交到 GitHub               |
| 拉取更新     | `git pull`                                 | 下载远程仓库的更新                 |
| 设置远程仓库 | `git remote add origin 仓库地址`            | 关联本地项目和 GitHub             |
| 克隆仓库     | `git clone 仓库地址`                        | 从远程仓库复制项目                 |


### 3.6 难点解答

#### 区分“提交改动”（git commit）和“推送更新”（git push）
前者是把保存修改到本地git历史中，后者是上传历史到github
#### 如何理解分支
| 分支名         | 用法场景                       |
| ----------- | -------------------------- |
| `main`      | 主线 / 稳定版，线上网站展示            |
| `dev`       | 开发版，测试新功能、实验配置             |
| `feature/x` | 某个功能，比如 `feature/comments` |
| `bugfix/x`  | 修复 bug 的分支                 |
  
  在git中创造独立的工作线，可以再不影响主线的情况下自由修改，再决定是否合并
#### 本地git本质是什么
本地 Git 是一个“看不见的项目档案室”，真实存在于你项目目录的 .git/ 文件夹里。它只占用磁盘，不吃内存，能让你拥有任意版本的“悔棋”机会
| 目录 / 文件    | 作用说明                       |
| ---------- | -------------------------- |
| `objects/` | 存放你每一次提交（commit）的快照对象、文件变动 |
| `refs/`    | 保存分支、HEAD 指针等引用            |
| `HEAD`     | 当前分支指针（比如你现在在 `main`）      |
| `config`   | 当前仓库的 Git 配置信息             |
| `logs/`    | 操作日志（如切换分支历史）              |



