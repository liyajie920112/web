## git init

```bash
# 初始化git仓库
git init

# 将本地仓库初始化为远程仓库
git init --bare sample.git
```

## git add

```bash
# 将test.txt文件添加到仓库
git add test.txt
```

## git commit

```bash
# 将添加的文件提交到仓库, 这个命令会提交多个文件, -m 'msg'就是这次提交的描述
git commit -m 'msg'
```

## git status

```bash
# 查看当前版本状态, filename.txt可以省略,省略就是所有文件
git status [filename.txt]
```

## git diff

```bash
# 查看版本之间的差别, 不指定文件默认查看所有文件的版本之间的差别
# 工作区和暂存区的比较
git diff [filename.txt]

#是暂存区(stage)和分支(master)的比较
git diff --cached

# 命令可以查看工作区和版本库里面最新版本的区别
git diff HEAD -- readme.txt
```

## git log 

```bash
# 查看仓库的修改日志
git log [filename.txt]

# 以简要格式输出修改日志
git log --pretty=oneline

#命令可以看到分支合并图
git log --graph
```

## git reset

```bash
# 前提是没有推送到远程仓库
# 版本退回, HEAD^ 上一个版本, HEAD^^ 上上个版本
git reset --hard HEAD^

# 退回上100个版本
git reset --hard HEAD~100

# 退回指定版本
git reset --hard commit_id
```

## git reflog

```bash
# 记录你的每一次命令
git reflog [filename.txt]
```

## git checkout

```bash
# 撤销修改
git checkout -- readme.txt
# 一种是修改后还没有被放入暂存区, 撤回就回到和版本库一样的状态
# 一种是修改后已经被提交到暂存区, 撤回后就回到了添加到暂存区后的状态.
# 总之就是回到这个文件最近一次commit 或 git add时的状态
```

## git rm

```bash
# 删除一个文件
git rm [filename.txt]
# 删除之后提交到主分支
git commit -m 'msg'
```

## 查看分支 git branch

```bash
# 创建分支
git branch <name>

# 切换分支
git checkout <name>

# 创建+切换分支
git checkout -b <name>

# 合并某分支到当前分支
git merge <name>

# 删除分支
git branch -d <name>
```

!> 当分支切换的时候, HEAD指向当前分支 , master指向最新的提交.
