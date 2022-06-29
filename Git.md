# Git 操作

## 提交本地项目代码到git仓库

```shell
# 1. 初始化项目文件夹
git init   

# 2. 将所有文件添加到暂存区
git add .  // 

# 3. 提交到本地仓库，双引号内是提交的备注信息
git commit -m "first commit"   // 

# 4. （XXX就是你github或者码云等远程仓库的地址，git branch这个命令可以看到你所在的分支，删除某个仓库地址使用git remote rm origin）
git remote add origin https://gitee.com/xxxxxxxx     //  

# 5. 拉取远程主分支信息，首次拉取合并信息
git pull    // 

# 6. 提交到远程仓库，这个命令中的 -f 是强制推送，因为远程仓库只有初始化的文件，所以强制推送上去就行了，不加-f 会报当前分支没有远程分支，强制推送可以覆盖master，这样就完成了第一次提交的步骤)
git push -u -f origin master  // 

```

