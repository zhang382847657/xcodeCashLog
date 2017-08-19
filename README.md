# Xcode自带工具symbolicatecrash解析iOS Crash文件

## 1、准备工作
> 在桌面创建crash文件夹
- #### 获取.crash文件
- #### 获取.dSYM文件
- #### 获取应用程序文件
- #### 获取symbolicatecrash文件

## 2、crash log文件解析（两种方案）
> 在开始解析前，需要进行一些有关UUID的校验，只有三者的UUID相同才能解析.crash文件！
> - 查看xxx.app文件的UUID的方法，在命令行中输入
> ```
> dwarfdump --uuid xxx.app/xxx (xxx工程名字)
> ```
> - 查看xxx.app.dSYM文件的UUID的方法，在命令行中输入
> ```
> dwarfdump --uuid xxx.app.dSYM (xxx工程名字)
> ```
> - 查看.crash文件的UUID的方法：位于crash日志中的Binary Images中的第一行 如：armv7s  <13760bde0d073f1eb4d596c3df753f4b>

### 方案一：使用Symbolicatecrash 解析
- #### 使用终端cd到crash文件所在的目录
- #### 生成最终转换后的崩溃日志
  ```
  ./symbolicatecrash xxx.crash xxx.app.dSYM > test.log  （test.log代表你要生成的奔溃日志的名字）
  ```
  > 上面命令可能会出现
  > ```
  > Error: "DEVELOPER_DIR" is not defined at ./symbolicatecrash line 53
  > ```
  > 这样的错误，可以输入以下命令解决
  > ```
  > export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
  > ```
  > 然后继续执行第二步的命令



### 方案二：根据地址解析
```
xcrun atos -o xxx.app/xxx -arch armv7 0x002e5df6 0xcb000 (多个16进制地址，使用空格分开)
```
解析出的结果例如：
```
-[SecondShotViewController playVideo] (in USchoolCircle) (SecondShotViewController.m:473)
```

