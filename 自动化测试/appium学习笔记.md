# appium自动化测试笔记
## 一、环境搭建
### 1.软件安装
1. 安装java环境，安装Node.js环境
2. 安装android SDK(可以姐姐安装Android Studio，里面有SDK工具包)
3. 安装appium server
4. 安装appium Inspector
5. 安装pycharm
### 2.python库安装
推荐使用anaconda安装，简要步骤如下：
1. 下载anaconda安装包，并安装
2. 学习anaconda命名行操作：[Anaconda命令](./Anaconda命令.md)
3. anaconda中创建虚拟环境，下载所需库，如下：
```
Appium-Python-Client==4.1.0
selenium==4.23.1
requests==2.28.2
urllib3==1.26.2
```
### 3.参考文章
[【Appium】最新版本环境搭建](https://blog.csdn.net/weixin_42297382/article/details/123886326)，真机测试无需安装虚拟器