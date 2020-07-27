## Git的使用

#### 初始化

```
 $ git config --global user.name "Your name" 

 $ git config --global user.email "Your email"
```

#### 创建版本库

``` 
$ mkdir (目录名字) //创建空目录
$ pwd             //显示当前目录
$ cd (目录名字)    //选定某个目录


$ git init        //讲该目录变成可以管理的仓库
$ ls -ah          //查看隐藏的.git目录
```

#### 把文件添加到版本库

```
$ git add (文件名) (文件名) ....
$ git commit -m "对此次提交的说明"
$ ls   $ dir       //查看当前目录下的所有文件
```



#### 时光机穿梭

``` 
$ git status       //查看当前仓库的状态(修改、删除 什么的)
$ git diff         //查看修改的文件的信息
$ git log          //查看修改日志


$ git reset --hard head^   //回退到上一个版本  一个^代表回退一个版本
                           //此时已查看不到最新版本，可以使用该版本特有的commit ID来回溯
$ git reset --hard (commit id)         
$ git reflog       //查看命令历史
$ git diff -- (文件名)     //查看该文件在工作区和版本库里面最新版本的区别
$ cat (文件名)     //查看某个文件在git中的内容


$ git checkout -- (文件名) //撤销修改
						//一种是readme.txt自修改后还没有被放到暂存区，现在，撤销						  //修改就回到和版本库一模一样的状态；
						//一种是readme.txt已经添加到暂存区后，又作了修改，现在
						//撤销修改就回到添加到暂存区后的状态。
						//总之，就是让这个文件回到最近一次git commit或git add时						 //的状态。
						
$ git reset head (文件名)  //将暂存区的修改撤销掉	

$ git rm -- 文件名         //将该文件从版本库中删掉
						// 在工作区误删可用checkout 还原
```



#### 连接远程库

```
$ git remote add origin git@github.com:个人ID/仓库名字.git //连接远程仓库
$ git push -u origin master                        //将本地库所有内容推送远程仓库
$ git push origin master                           //更新远程库的内容
```

#### 分支管理

```
$ git checkout -b dev      //创建dev分支，并且切换到dev分支
$ git switch -c dev

$ git branch               //查看当前分支

$ git checkout master      //切换到master分支
$ git switch master

$ git merge dev            //合并dev分支到当前分支

$ git branch -d dev        //删除分支
```

#### 解决合并冲突

```
$ git status  			  //查看冲突文件
$ cat 文件名			   //查看冲突内容
$ git log --graph --pretty=oneline --abbrev-commit //查看分支合并情况
											  //修改后再添加后提交
```

#### 分支管理协作

```
$ git merge --no-ff -m "merge with no-ff" dev    //合并分支时使用,表示不使用Fast 											  					 //forward模式合并

$ git log --graph --pretty=oneline --abbrev-commit //查看分支历史
```

#### BUG分支

```
$ git stash                //将工作现场藏起来，切换到其他分支修复bug
$ git stash list           //查看藏起来的工作现场

//修复
$ git stash apply          //恢复后，stash内容并不删除,除非用git stash drop来删除
$ git stash pop            //恢复后，把stash内容也删了
$ git stash apply stash@{0}//恢复制定的工作分支

//在其他分支修复的bug复制到之前的分支
$ git cherry-pick (修复bug所commit的ID)  //能复制一个特定的提交到当前分支
```

#### Feature分支

```
$ git branch -D (分支名字)   //删除一个没有被合并的分支
```

#### 多人协作

```
$ git remote -v                 //查看远程库的信息
$ git push origin (本地分支)    //将该分支上所有本地提交推送到远程库

//默认情况下，你的小伙伴只能看到本地的master分支，没有其他分支
//若要进行开发，则该小伙伴要在他的本地自己建一个其他分支

//如果有两个人同时提交dev并且有冲突
//将最新提交的dev拉取下来，然后合并，解决冲突，再推送
$ git pull origin/dev

//如果git pull 失败，则要先要将本地dev分支与原程origin/dev分支的链接
$ git branch --set-upstream-to=origin/dev dev

```

#### rebase

```
$ git rebase     //将分叉的提交历史整理成一条直线，看上去更直观
```



