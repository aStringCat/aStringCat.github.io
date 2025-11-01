---
title: Hello World
date: 2025-10-31 23:10:11
---
## 💻 我的 Hexo 搭建历程

每个人心中可能都有一块想认真记录和分享的自留地。就在今天，这个“心血来潮”的念头突然冒了出来：“不如，自己搭个博客吧！”

我想，既然每天都在浏览别人写的精彩内容，为什么不尝试把自己的想法也整理出来呢？我希望有一个属于自己的空间，可以随心记录生活中的点滴，也能分享一些学习和探索的心得。

一番探索和比较后，我选择了久负盛名的Hexo和Github Page

既然已经成功搭建，那么第一篇博客，就顺理成章地献给Hexo 的极速搭建教程吧！

### 🚀 第一步：初始化你的博客项目
安装好 Node.js 和 git 后，我们就可以开始搭建 Hexo 了。

全局安装 Hexo：

``` bash
npm install -g hexo-cli
```
创建博客文件夹并初始化： 找一个你喜欢的位置，创建一个文件夹（比如叫 my-blog），进入该文件夹，然后执行初始化命令。

``` bash
hexo init pages
```

这一步，Hexo 会自动为你生成基本的博客文件结构，包括主题、配置文件等。

之后，我们只需要照着命令行提示，一步步搭建即可


### 🎨 第二步：个性化和配置
一个好的主题是博客的门面。Hexo 社区有大量优秀的主题，比如 Next、Matery、Sakura 等。

安装新主题： 以最流行的 Next 主题为例。

```bash
git clone https://github.com/next-theme/hexo-theme-next themes/next
```
激活主题： 打开博客根目录下的主配置文件 _config.yml (注意是博客根目录的，不是主题文件夹里的！)，找到 theme: 这一行，将其修改为你新安装的主题名称。

```yml
# _config.yml (根目录下)
# ... 其他配置
theme: next  # 将默认的 landscape 改为 next
# ...
```

其他基本配置： 在同一个 _config.yml 文件中，修改你的网站名称、作者、语言等基础信息。

### ✍️ 第三步：创作你的第一篇文章
写博客，当然是用 Markdown！

Hexo 会在 source/_posts 目录下为你生成一个名为 hello-world.md 的文件，编辑这个文件开启hexo之旅吧。

```Markdown
---
title: hello-world
---
# 在这里开始你的创作！
```

### ☁️ 第四步：部署上线

写完文章，最后一步就是发布啦。最常见的方式是使用 GitHub Pages。

安装部署插件：

```bas
npm install hexo-deployer-git --save
```

配置部署信息： 再次打开博客根目录下的主配置文件 _config.yml，在文件末尾添加你的 GitHub 仓库信息：

```yml

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: <你的GitHub Pages仓库地址，如：https://github.com/username/username.github.io>
  branch: main # 或 master，取决于你的仓库分支设置
```

生成并部署：

```bash
hexo clean
hexo generate    # 生成静态文件
hexo deploy      # 一键部署到 GitHub
```

当看到 INFO Deploy done: git 时，大功告成！等待几分钟，访问你的 GitHub Pages 域名，你的博客就正式上线啦！