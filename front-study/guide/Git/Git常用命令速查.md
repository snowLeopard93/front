## Git常用命令速查

> **git init**

创建一个本地git仓库

**示例：**

```
git init blog
```

> **git add**

将新的文件或目录添加到本地仓库

**示例：**

```
git add "1.md"

git add "directory1"
```

> **git commit**

将修改后的文件提交到本地仓库

> **git push**

将本地修改的文件推送到远程仓库

> **git push -f origin master/git push -f origin main**

将本地仓库的代码强制推送到远程仓库，其中`master`和`main`是远程仓库的分支名。

> **git push -u origin master/git push -u origin main**

将本地修改的文件推送到远程仓库，其中`master`和`main`是远程仓库的分支名。

> **git pull**

将远程仓库的代码拉取到本地

> **git log**

查看提交记录，版本回退的时候需要用到

> **git reset --hard**

将本地代码还原到某一版本

**示例：**

```
git reset --hard 44bd896bb726be3d3815f1f25d738a9cd402a477
```

> **git status**

查看本地修改的文件或未放入到本地仓库的文件、目录

> **git rm -r --cached**

将git上已经有版本控制的文件移除

```
git rm -r --cached "study/.idea/workspace.xml"

git commit -m "delete workspace.xml"
```

> **git remote add origin**

添加远程仓库地址

**示例：**

```
git remote add origin git@github.com:snowLeopard93/blog.git
```

> **git restore**

还原本地仓库暂存的文件

```
git restore xxx
```

> **git branch -m**

修改分支名字

```
git branch -m master main
```

**推荐：**

[一张脑图带你掌握Git命令](https://juejin.im/post/6869519303864123399)
