# 安装Hugo

[浅谈我为什么从 HEXO 迁移到 HUGO - 少数派](https://sspai.com/post/59904)

[官网下载](https://github.com/gohugoio/hugo/releases)
**安装extended版本，功能更多**

放到`e:\hugo\bin`下
在此文件夹中打开`git bash`
`hugo version`：出现版本号安装成功
[官方文档：Quick start](https://gohugo.io/getting-started/quick-start/)
`hugo new site 'document'`：我的项目在`e:\hugo\document`下
![[picture/hugo_document.png]]
`cd document` 
`git init`
`https://themes.gohugo.io/`：安装主题

国内用https下载很慢，记得开加速
或者使用浏览器的gitzip插件，下载zip文件，解压缩到`themes/PaperMod`
`git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod`：我用的PaperMod主题

`hugo.toml`：网站配置文件
`echo 'theme = "PaperMod"' >> hugo.toml`：将主题配置写入Hugo的网站配置文件，注意不是`config.toml`
`hugo new posts/my-first-post.md`：在`content\posts`文件下新建一个
`hugo server -D`：启动Hugo的本地开发服务器，实时预览你的网站，`-D`表示显示`draft`
``



# 部署到GitHub 作为代码托管
[官方文档：Host on GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/)

新建一个`repositories`，以`用户名.github.io`命名，这是静态网站托管仓库
**注意仓库大小写，必须！全部改成小写！** 😭
修改`hugo.toml`中的，`baseURL = 'https://用户名.github.io/'`
在项目根目录下`e:\hugo\document`
```bash
mkdir -p .github/workflows
touch .github/workflows/hugo.yaml    # GitHub Actions 工作流定义文件，脚本自动部署网站
```
复制[官方文档](https://gohugo.io/host-and-deploy/host-on-github-pages/)中的标准工作流配置
**注意修改**
1. 修改时区：  
    将第25行的 `TZ: Europe/Oslo` 改为 `TZ: Asia/Shanghai`，这样生成的文章时间戳会更符合本地时间。
2. 确认主题子模块：  
    这个工作流在第29行设置了 `submodules: recursive`，这能确保你的PaperMod主题被正确拉取。必须是使用 `git submodule add` 方式安装的。如果是`zip`下载，直接注释掉这一行（避免弹出报错）。

在`hugo.toml`中，把图片缓存在`c:\user\.cache`中
```toml
[caches]
  [caches.images]
    dir = ':cacheDir/images'
```

托管仓库基本不手动操作。所有更改通过Actions自动推送。
**单仓管理就行，笨蛋AI推荐双仓管理，结果配置出错了**

首先检查是否有`.gitignore`，在git提交时忽略的项目。不忽略会导致项目很大
```bash
# 由Hugo生成的网站文件夹
/public/
/resources/_gen/
# Hugo的模块缓存
/_vendor/
# 操作系统产生的临时文件
.DS_Store
Thumbs.db
# 编辑器/IDE的配置文件
.vscode/
.idea/
*.swp
*.swo
# 环境变量或本地配置文件
.env
.env.local
```

如果已经commit过，建议及时清理
```bash
# 从Git暂存区移除public/目录（不会删除本地文件）
git rm --cached -r public/
git add . #重新添加
git status # 检查一下
```

绑定用户（如果没有）
```bash
git config --global user.name "你的用户名"
git config --global user.email "你的GitHub主邮箱地址"
git config --global user.name user.email
```

设置分支，和GitHub平台同步，Git默认是master，GitHub默认是main，这在推送时会导致错误。
```bash
git config --global init.defaultBranch #如果为空就
git config --global init.defaultBranch main
git config --global init.defaultBranch # 检查一下
```

修改Actions
- 在左侧边栏找到并点击 **“Pages”**。
- 在右侧的 **“Build and deployment”**（构建和部署）部分，将 **“Source”** 下拉菜单从 `None` 或 `Deploy from a branch` 更改为 **“GitHub Actions”**

```bash
# 确保在项目根目录 
cd /e/hugo/document 
# 初始化Git（如果还没做） 
git init 
# 添加所有文件（包含主题和刚创建的 .github/workflows/hugo.yaml） 
git add . 
# 提交 
git commit -m “初始提交：包含Hugo源码和Actions工作流” 
# 关联远程仓库（替换下面的URL） 
git remote add origin url 
# 推送 
git branch -M main 
git push -u origin main
```



# 提交更改

```bash
git add .     # 将所有更改提交到暂存区
git add <文件名>  # 或者这种方法
git status   # 暂存区有哪些文件
git diff --cached # 查看暂存区的修改
```