### 一、adb简介
ADB（Android Debug Bridge）是Android软件开发工具包（SDK）中的一个通用命令行工具。
它允许开发者与Android设备进行通信，执行各种调试操作、管理设备或模拟器状态等。
通过ADB，你可以安装和调试应用程序、运行shell命令、传输文件、获取日志输出等

### 二、adb自身指令
1. 查看adb版本
```
adb version
```
2. 启动adb
```
adb start-server
```
3. 关闭adb
```
adb kill-server
```
4. 修改adb的端口
```
adb -P <port> start-server
默认端口:5037
```
### 三、adb操作设备指令
1. 查看已连接的设备列表(deviceID为设备唯一标识)
```
显示详细信息:adb devices -l
简要信息：adb devices
```
2. 指定具体某台设备
```
adb -s deviceID ...
```
3. 拉取文件
```
adb pull /sdcard/example.png  [本地目录]
/sdcard/example.png:手机文件路径
```
4. 录屏
```
adb shell screenrecord /sdcard/example.mp4
ctrl+c：结束录制；默认录制时长（最大录制时长）为180s，/sdcard/example.mp4：手机内存储文件路径
```
5. 截屏
```
adb shell screencap /sdcard/example.png
```
6. 安装apk
```
adb install -[r/t/d/g/p/...] apk地址
-r:重新安装，保留数据
-t:允许安装测试包
-d:允许降级安装，保留数据
-g:安装后授予所有运行时权限
-p:保持数据和缓存目录不变，仅更新应用（适用于签名相同的更新包）
``` 
7. adb断开连接
```
adb disconnect [ip:5555端口]
```
8. 删除app
```
adb uninstall [-k] [app包名]
```
9. adb shell操作：查看第三方应用包名
```
adb shell pm list packages -3
```
10. adb shell操作：启动app
```
adb shell am start [app包名]
```
11. adb shell操作：关闭app
```
adb shell am force-stop [app包名]
```
12. adb shell操作：查看手机系统版本
```
adb shell getprop ro.build.version.release
```
13. adb shell操作：查看当前打开app的包名和appactivity
```
adb shell dumpsys activity | findstr "mResume"
```
14. adb shell操作：查看CPU信息
```
adb shell cat /proc/cpuinfo
```
15. adb shell操作：获取设备的IP地址
```
adb shell ifconfig
```
### 四、adb操作日志指令
1. 清空logcat日志
```
adb logcat -c
```
2. 打印logcat日志在控制台
```
adb logcat -d
```
3. 打印logcat日志到文件
```
adb logcat > C:\Users\cidana\Desktopa\example.txt(电脑上的地址)
```
4. log级别
    1. V: Verbose(最低优先级)
    2. D: Debug(次低优先级)
    3. I: 显示Info及以上级别
    4. W: 显示Warning及以上级别
    5. E: 显示Error及以上级别
    6. F: 显示Fatal及以上级别
    7. S: 显示silent及以上级别

### 五、设备连接教程
#### 1.有线连接：
    1. 设备打开开发者模式，打开USB调试；
    2. USB数据线连接设备和电脑，输入adb devices，查看连接情况；
#### 2.WiFi连接（电脑和设备连接同一网络）：
    1. 设备打开开发者模式，打开USB调试；
    2. USB数据线连接设备和电脑，输入adb devices，查看链接情况；
    3. 让设备在5555端口监听TCPIP连接：adb tcpip 5555
    4. 获取设备的IP地址：adb shell ifconfig
    5. adb连接：adb connect [ip:5555端口]
    6. 查看连接情况：adb devices
#### 3.无线连接（电脑和设备连接同一网络）：
    1. 打开手机设置，搜索无线调试（一般都在开发者选项里面）
    2. 开启无线调试，点击使用配对码配对设备
    3. 将电脑和手机连接至同一网络
    4. 电脑adb输入：adb pair ip:port 配对码（这三个值在无线调试里面都有）
    5. 输入adb connect ip：port（手机无线调试也有显示）




