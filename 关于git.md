# 关于git

## 团队开发

* 项目团队开发同步代码的过程是：**add->commit->pull->push**
* add，把代码放到缓冲区。
* commit，把缓冲区里面的代码存放到本地库里面。
* pull，把远程库中的改动同步到本地，可查看别人更改的代码与自己的代码有什么冲突。
* push，把自己的代码同步到远程仓库，这时远程库中的代码一致。

## git相关命令

* git init，初始化文件夹
* git add 文件名，把文件放到缓存区
* git commit -m "文件说明"，将文件提交到本地仓库
* git status，查看工作区是否还有没add过的新修改
* git  log：查看文件的修改日志
* git reflog：查看分支引入记录
* git diff：查看文件最新改动的地方
* git  reset：版本回退
  * 回退到上一个版本：`git reset ––hard HEAD^`
  * 回退到上上一个版本：`git reset ––hard HEAD^^`
  * 回退到上 N 个版本：`git reset ––hard HEAD~N`（N 是一个整数）
  * 回退到任意一个版本：`git reset ––hard 版本号`

* git clone：下载远程仓库到本地
  * 下载远程仓库到当前路径：`git clone 仓库的URL`
  * 下载远程仓库到特定路径：`git clone 仓库的URL 存放仓库的路径`

* git pull：下载远程仓库的最新信息到本地仓库
* git push：将本地仓库信息推送到远程仓库