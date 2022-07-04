### 一、下载离线包

### 二、重点

#### 1、解压.exe文件，解压出一个压缩包

#### 2、解压第1步生成的压缩包

#### 3、把解压出来目录下面的chrome.exe放到版本号命名的文件夹里面

#### 4、在版本号命名的文件夹里面，新建一个文件夹，命名为user-data

#### 5、为chrome.exe创建一个快捷方式，选中快捷方式，右键编辑“属性”，修改“目标”字段的值

在原来的基础上加上`--XX`，其中XX为新建的文件夹的完整路径，例如：

```
D:\software\chrome78\78.0.3904.108_chrome_installer\Chrome-bin\chrome.exe --user-data-dir="D:\software\chrome78\78.0.3904.108_chrome_installer\Chrome-bin\78.0.3904.108\user-data"
```



**参考：**

[一台电脑安装多个Chrome](https://blog.csdn.net/oro99/article/details/116751952)