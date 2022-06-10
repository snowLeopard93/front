### 一、常用命令

#### 1、下载文件 `wget`

```
wget http://192.168.100.220:9020/images/resource/test.png
```

#### 2、删除文件 `rm`

```
rm -rf blog-master
```

#### 3、解压zip `unzip`

```
unzip blog-master.zip
```

#### 4、后台启动服务 `nohup...&`

```
nohup http-server -p 9020 &
```

#### 5、查找服务 `ps -ef`

```
ps -ef | grep http
```

#### 6、打补丁 `yum update`

```
// XXX为包名
yum update XXX
```

#### 7、查看文件列表 `ls`

```
ls
```

#### 8、显示当前分区 `fdisk -l`

```
fdisk -l
```

#### 9、挂载 `mount`

```
mount /dev/sdf1 a
```

#### 10、卸载 `umount`

```
umount /a
```

#### 11、进入文件夹（返回上一层） `cd`

```
cd
cd ..
```

#### 12、查看当前路径 `pwd`

```
pwd
```

#### 13、编辑文件 `vi`


```
// 打开文件
vi

// 进入编辑模式
i

// 退出编辑模式
esc键

// 保存文件并退出
:wq

// 不保存，强制离开vi
:q!
```

#### 14、自动补全文件名 `Tab`

```
Tab键
```

#### 15、重命名

```
mv a b
```

### 二、进程相关

#### 1、杀进程 `kill`

```
// XXXX为对应进程id
kill -9 XXXX
```

#### 1、杀进程 `taskkill`

```
// XXXX为对应进程pid
taskkill /f /pid XXXX
```
