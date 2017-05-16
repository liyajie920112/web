## git init

```bash
# 初始化git仓库
git init
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
git diff [filename.txt]
```

## git log 

```bash
# 查看仓库的修改日志
git log [filename.txt]

# 以简要格式输出修改日志
git log --pretty=oneline
```

## git reset

```bash
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