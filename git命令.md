# git命令

**版本库---repository**：又名仓库，英⽂文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任 何时刻都可以追踪历史，或者在将来某个时刻可以“还原”，工作区有⼀一个隐藏⽬录“**.git**”，这个不算⼯工作区，⽽而是Git的版本库。 Git的版本库⾥里存了很多东西，其中重要的就是称为**stage**（或者叫index）的**暂存区**，还有Git为我们⾃自动创建的第一个分支master，以及指向master的⼀一个指针叫HEAD。

![image-20200207123524019](git命令/image-20200207123524019.png)

```powershell
git init  # 初始化一个git仓库，。 
```

```powershell
git add xxx   # 添加文件到暂存区
```

```powershell
git commit -m "first commit"   # 把暂存区的所有内容提交到当前分支   -m 输入提交说明
```

为什么Git添加⽂文件需要add，commit⼀一共两步呢？因为commit可以⼀一次提交很多⽂文件， 所以你可以多次add不同的⽂文件，比如：

```powershell
$ git add file1.txt 
$ git add file2.txt 
$ git add file3.txt 
$ git commit -m "add 3 files."
```

```powershell
git status # 查看当前仓库的状态
```

```powershell
git diff 文件名  #查看修改修改内容
```

```powershell
git log # 查看修改日志
git log --pretty=oneline # 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 --pretty=oneline参数： 
```

```powershell
git reset --hard HEAD^ # 回退到上一个版本   HEAD^^上上一个版本   HEAD~100网上100个版本

git reset --hard xxxx(commit Id) # 回退到以前的版本后，只要命令窗口未关闭，就可以回到现在的版本

git reflog # 查看每一次命令
```

```powershell
git diff HEAD -- xxx  #查看暂存区和版本库里面最新版本的区别
```

```powershell
git checkout -- file # ⽂件在⼯作区的修改全部撤销
```

```powershell
git rm test.txt  # 删除暂存区的文件
```

# 远程仓库操作

```powershell
git remote add origin https://github.com/zqqpluto/Test.git #与远程仓库关联

git push -u origin master  # 把本地库的所有内容推送到远程库上：  第一次推送要加 -u

# 正确步骤如下：
git init //初始化仓库

git add .(文件name) //添加文件到本地仓库

git commit -m “first commit” //添加文件描述信息

git remote add origin + 远程仓库地址 //链接远程仓库，创建主分支

git pull origin master // 把本地仓库的变化连接到远程仓库主分支

git push -u origin master //把本地仓库的文件推送到远程仓库
```

```powershell
git clone  xxx # 克隆远程仓库
```

# 分支

```powershell
#查看分⽀支：git branch 
#创建分⽀支：git branch name 
#切换分⽀支：git checkout name
#创建+切换分⽀支：git checkout -b name 
#合并某分⽀支到当前分⽀支：git merge name 
#删除分⽀支：git branch -d name 


当Git⽆无法⾃自动合并分⽀支时，就必须⾸首先解决冲突。解决冲突后，再提交，合并完成。
⽤用git log --graph命令可以看到分⽀支合并图。 


git merge --no-ff -m "merge with no-ff" dev # 注意--no-ff参数，表⽰示禁⽤用“Fast forward”： 
#通常，合并分⽀支时，如果可能，Git会⽤用“Fast forward”模式，但这种模式下，删除分⽀后，会丢掉分支信息。
#如果要强制禁⽤用“Fast forward”模式，Git就会在merge时⽣生成⼀一个新的commit，这样，从分支历史上就可以看出分支信息。 
```



# Bug分支

```powershell
#修复bug时，我们会通过创建新的bug分⽀支进⾏行修复，然后合并，后删除； 当⼿手头⼯工作没有完成时，先把⼯工作现场git stash⼀一下，然后去修复bug，修复后，再git stash pop，回到⼯工作现场。
```



# Feature功能

```powershell
# 开发⼀一个新feature，好新建⼀一个分⽀支； 如果要丢弃⼀一个没有被合并过的分⽀支，可以通过git branch -D name强⾏行删除。
```

# 多人协作

```powershell
#多⼈人协作的⼯工作模式通常是这样： 
#1. ⾸首先，可以试图⽤用git push origin branch-name推送⾃自⼰己的修改； 
#2. 如果推送失败，则因为远程分⽀支⽐比你的本地更新，需要先⽤用git pull试图合并； 
#3. 如果合并有冲突，则解决冲突，并在本地提交； 
#4. 没有冲突或者解决掉冲突后，再⽤用git push origin branch-name推送就能成功！

#如果git pull提⽰示“no tracking information”，则说明本地分⽀支和远程分⽀支的链接关系没 有创建，⽤用命令git branch --set-upstream branch-name origin/branch-name。 这就是多⼈人协作的⼯工作模式，⼀一旦熟悉了，就⾮非常简单。

```

总结;

* 查看远程库信息，使⽤用`git remote -v`
* 本地新建的分⽀支如果不推送到远程，对其他⼈人就是不可⻅见的； 
* 从本地推送分⽀支，使⽤用`git push origin branch-name`，如果推送失败，先⽤用git pull抓 取远程的新提交； 
* 在本地创建和远程分⽀支对应的分⽀支，使⽤用`git checkout -b branch-name origin/branchname`，本地和远程分⽀支的名称好⼀一致；
* 建⽴立本地分⽀支和远程分⽀支的关联，使⽤用`git branch --set-upstream branch-name origin/branch-name`；
* 从远程抓取分⽀支，使⽤用`git pull`，如果有冲突，要先处理冲突。

# 标签管理

* 命令git tag name⽤用于新建⼀一个标签，默认为HEAD，也可以指定⼀一个commit id； 
* -a tagname -m "blablabla..."可以指定标签信息； 
* -s tagname -m "blablabla..."可以⽤用PGP签名标签； 
* 命令git tag可以查看所有标签；



* 命令git push origin tagname可以推送⼀一个本地标签； 
* 命令git push origin --tags可以推送全部未推送过的本地标签； 
* 命令git tag -d tagname可以删除⼀一个本地标签； 
* 命令git push origin :refs/tags/tagname可以删除⼀一个远程标签。

# 使用Github

* 在GitHub上，可以任意Fork开源仓库； 
* ⾃自⼰己拥有Fork后的仓库的读写权限； 
* 可以推送pull request给官⽅方仓库来贡献代码。

