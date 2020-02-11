### 博客项目简介

该博客使用 hexo 搭建

#### 搭建步骤

* 1. 全局安装 hexo
```
  npm install -g hexo
```
* 2. 初始化项目
```
  hexo init
```
* 3. 部署到 GitHub
```
  // _congif.yml
  deploy:
    type: git
    repo: <你仓库的名称> # git@github.com:yuanyanmin/yuanyanmin.github.io.git
    branch: master

  // 安装 hexo-deployer-git 用于推到仓库中
  npm install hexo-deployer-git --save
```
* 4. 启动
```
  hexo s  // 本地启动 (hexo server 的缩写)
  hexo d  // 自动生成网站静态文件，病部署到设定的仓库 （hexo deploy 的缩写）
  hexo g // 生成网站静态文件到默认设置的 public (hexo generate 的缩写)

```
* 5. 常用命令
```
  hexo new page aboutme  # 新建 aboutme 文章
  hexo s -g   #生成并本地预览
  hexo d -g   #生成并上传
```
* 6. 多台电脑如何协作
> 新建分支 hexo, 用于存放开发代码，其他电脑可以拉代码，共同开发。部署生成的代码位于 master 分支。