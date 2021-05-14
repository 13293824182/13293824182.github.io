---
title: hexo-换电脑更新博客
abbrlink: 397eb78d
date: 2021-05-12 11:18:03
tags:
---

开始
<!--more-->


## 步骤：在原电脑上操作，给 username.github.io 博客仓库创建hexo分支，并设为默认分支。（具体可参考这篇文章的操作，有图示）如果未给你的 github 账号添加过当前电脑生成的 ssh key，需要创建 ssh key 并添加到 github 账号上。（如何创建和添加 github help 就有）随便一个目录下，命令行执行 git clone git@github.com:username/username.github.io.git 把仓库 clone 到本地。显示所有隐藏文件和文件夹，进入刚才 clone 到本地的仓库，删掉除了 .git 文件夹以外的所有内容。命令行 cd 到 clone 的仓库，git add -A ，git commit -m "--"，git push origin hexo，把刚才删除操作引起的本地仓库变化更新到远程，此时刷新下 github 端博客hexo分支，应该已经被清空了。将上述 .git 文件夹复制到本机本地博客根目录下（即含有 themes、source 等文件夹的那个目录），现在可以把上述 clone 的本地仓库删掉了，因为它已经没有用了，本机博客目录已经变成可以和 hexo 分支相连的仓库了。将博客目录下 themes 文件夹下每个主题文件夹里面的 .git .gitignore 删掉。 cd 到博客目录，git add -A ，git commit -m "--"，git push origin hexo，将博客目录下所有文件更新到 hexo 分支。如果上一步没有删掉 .git .gitignore，主题文件夹下内容将传不上去。至此原电脑上的操作结束。在新电脑上操作，先把新电脑上环境安装好，node.js、git、hexo，ssh key 也创建和添加好。选好博客安装的目录， git clone git@github.com:username/username.github.io.git 。cd 到博客目录，npm install、hexo g && hexo s，安装依赖，生成和启动博客服务。正常的话，浏览器打开 localhost:4000 可以看到博客了。至此新电脑操作完毕。以后无论在哪台电脑上，更新以及提交博客，依次执行，git pull，git add -A ，git commit -m "--"，git push origin hexo，hexo clean && hexo g && hexo d 即可。


链接：https://www.zhihu.com/question/21193762/answer/489124966

更换电脑操作一样的，跟之前的环境搭建一样，安装gitsudo apt-get install git
设置git全局邮箱和用户名git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
设置ssh keyssh-keygen -t rsa -C "youremail"
#生成后填到github和coding上（有coding平台的话）
#验证是否成功
ssh -T git@github.com
ssh -T git@git.coding.net #(有coding平台的话)
安装nodejssudo apt-get install nodejs
sudo apt-get install npm
安装hexo  sudo npm install hexo-cli -g
但是已经不需要初始化了，直接在任意文件夹下，git clone git@………………
然后进入克隆到的文件夹：cd xxx.github.io
npm install
npm install hexo-deployer-git --save
生成，部署：hexo g
hexo d
然后就可以开始写你的新博客了hexo new newpage
Tips:不要忘了，每次写完最好都把源文件上传一下git add .
git commit –m "xxxx"
git push 
如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了git pull