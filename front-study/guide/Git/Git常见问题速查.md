#### 1、`git add` 时提示 “Your branch is up-to-date with ‘origin/master‘”

**（1）确认是否是空文件夹；**

如果是，则需要在文件夹下面添加文件之后，再进行`git add`操作：

#### 2、`git status` 时 中文显示成一串数字

![Git常见问题速查-1](../../images/Git/Git常见问题速查-1.png)

**解决方法：**

```
git config --global core.quotepath false
```

![Git常见问题速查-2](../../images/Git/Git常见问题速查-2.png)

#### 3、`git push`之后未修改的文件显示`modify`,`git commit`时提示“nothing to commit, working tree clean”

![Git常见问题速查-3](../../images/Git/Git常见问题速查-3.png)

**解决方法：**

```
git reset --hard
```

#### 4、`git clone`

**错误提示：** "unable to access https://github.com/vuejs/vue-next.git/": OpenSSL SSL_read: Connection was reset, errno 10054


**解决方法：**

```
git config --global http.sslVerify "false"
```

**注：** 还有可能和网速有关，如果设置之后无效，可以尝试切换网络。

#### 5、`git push -u origin master`

**错误提示：** "error: src refspec main does not match any"

**解决方法：**

`github`上创建仓库时默认分支是`main`，但`git`本地创建仓库时默认分支是`master`。

```
// 修改默认分支的名字
git branch -m master main

// 重新推送
git push -u origin main
```