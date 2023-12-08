# gblog

使用 Github Discussion 作数据源 + Hugo + Github Actions 全自动搭建更新博客

## 实现原理和思路
程序和数据分离防止火葬场

主要有2个部分组成

1. 数据源仓库：`xxnuo/notes`

    作用：

    1. 提供博客的内容部分，只用到它的 `Discussion`
    
    2. 创建一个 `Github Action`，在 `Discussion` 有内容更新的时候向`程序部署仓库`发起 `push` 等能触发`程序部署仓库`的 `Github Action` 的操作

        > 或者创建一个 `Github Action`，在 `Discussion` 有内容更新的时候访问指定 `webhook` 接口，需要经过一个独立的中间服务器，暂不考虑

2. 程序部署仓库：`xxnuo/pages`

    作用：

    1. 设置启用 `Github Pages` ，部署构建后页面到仓库的 `gh-pages` 分支并服务

    2. 创建一个 `Github Action`，触发条件依据`数据源仓库`的设置，该 `Github Action` 实现 获取`数据源仓库`的 `Discussion` 更新的内容并 拼接、转换 为`博客引擎`的博文格式，如特定的 `.md` Markdown 格式后通过`博客引擎`生成静态文件

    3. 推送生成后的静态文件到本仓库的 `gh-pages` 分支后等待 `Github Pages` 服务自动部署即可完成
