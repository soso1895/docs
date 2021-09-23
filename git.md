[TOC]
## create a new repository on the command line

    echo "# docs" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/soso1895/docs.git
    git push -u origin master

## push an existing repository from the command line

    git remote add origin https://github.com/soso1895/docs.git
    git push -u origin master


## github添加远程仓库报错：fatal: remote origin already exists.

如果输入git remote add origin git@github.com:XXX/XXXX.git

出现出错信息：fatal: remote origin already exists.

解决方法：
1、先输入$ git remote rm origin
2、再输入$ git remote add origin git@github.com:XXX/XXXX.git


        git config --global user.name "your name"
        git config --global user.email "your email"

        git add -A / --all
        git add XX.txt
        git commit -m "XXXX"
        git push origin master

        ssh-keygen -t rsa -C "your_email@youremail.com"

## github delete history

        git checkout --orphan lastest branch
        git add -A
        git commit -am "commit message"
        git branch -D master
        git branch -m master
        git push -f origin master