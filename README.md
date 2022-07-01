# PersonalWebFiles
个人主页的Hexo模板、文章，使用这些文件搭建我的个人主页的步骤

## 需要安装：
- Git
- 本地电脑生成ssh，github账户添加ssh
- 安装node.js
- 安装hexo

        npm install -g hexo-cli

## 步骤：
1. 在本地电脑创建 youtian95.github.io 文件夹
1. Hexo建站

        hexo init
        
1. 安装 hexo-deployer-git

        npm install hexo-deployer-git --save
        
1. 复制这个repo的文件覆盖原来的文件夹中的文件，包括 _config.yml，source，themes
1. 部署到github

        hexo generate
        hexo clean
        hexo deploy
