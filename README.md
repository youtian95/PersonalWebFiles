# PersonalWebFiles
个人主页的Hexo模板、文章，使用这些文件搭建我的个人主页的步骤

## 需要安装
- Git
- 本地电脑生成ssh，github账户添加ssh
- 安装node.js
- 安装hexo  
`npm install -g hexo-cli`

## (1) 复制本仓库的内容
1. 创建新文件夹，并初始化  
        `git init`
1. 从远程同步到本地仓库  
        `git remote add --fetch --tags origin git@github.com:youtian95/PersonalWebFiles.git`
1. 根据本地仓库创建本地分支  
        `git checkout -b master origin/master`

## (2) 搭建网站
1. 在本地电脑创建 youtian95.github.io 文件夹
1. Hexo建站  
        `hexo init`
1. 安装 hexo-deployer-git  
        `npm install hexo-deployer-git --save`
        
1. 复制这个repo的文件覆盖原来的文件夹中的文件，包括 _config.yml，source，themes
1. 部署到github  
        `hexo generate`  
        `hexo clean`  
        `hexo deploy`  

## (3) 发布文章
1. 发布文章  
        `hexo new post "test new post"`
1. 用编辑器修改.md文章 source\_posts\test-new-post.md   
1. 修改主页的相应文本内容： themes\Theme2\layout\index.ejs，同时修改其他网页source\_posts\others-index.md  
1. `hexo generate`
1. `hexo clean`
1. `hexo deploy`

## (4) 删除文章
1. 删除文件  
        `source\_posts\test-new-post.md` 
1. `hexo clean`  
1. `hexo deploy`  

## （4）修改PersonalWebFiles仓库
1. 将新文件覆盖本地PersonalWebFiles仓库中的文件  
        `cd ..\PersonalWebFiles\`
1. 同步远程PersonalWebFiles仓库  
        `git add .`  
        `git commit -m <commit message>`  
        `git push origin master`  
