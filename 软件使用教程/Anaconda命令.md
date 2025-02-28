### conda和pip关系
###### 1.conda可以管理非python包，pip只能管理python包
###### 2.创建虚拟环境时，默认不会安装任何包和库，可以先给验证（conda list）
###### 2.conda安装的包，pip可以卸载，但不能卸载依赖包；pip安装的包，只能pip卸载
###### 3.conda install xxx：安装在anaconda/pkgs目录下，且多个虚拟环境会同用一个库/包
###### 4.pip install xxx:
1. 该虚拟环境中有python和pip包：anaconda/envs/current_env/lib/python3.x/site-packages
2. 虚拟环境中无python和pip包：会使用系统（base）的python安装，且安装到
~/.local/lib/python3.x/site-packages文件夹中
##### 最重要的是：
1. 安装包/库：使用conda install xxx
2. conda安装失败：使用pip安装，
    * 先确认当前虚拟环境中：是否存在python和pip（conda list或conda list pkgname）
    * 再使用python -m pip install xxx
    * 再使用conda list或pip show xxx确认一下
3. 将包/库安装到指定位置：pip install --target xxx位置

#### 处理conda自身
##### 1.查看conda版本 
```
conda --version
```
##### 2.查看某个命令用法
```
conda create --help
```
#### 处理虚拟环境
##### 1.创建虚拟环境  
```
conda create -n env_name python = 版本号
```
##### 2.查看虚拟环境
```
conda env list
```
##### 3.激活虚拟环境
```
conda actiavte env_name
```
##### 4.删除虚拟环境
```
conda env remove -n env_name 
```
##### 5.导出环境
```
conda env export --name env_name > env_name.yml
```
##### 6.还原环境
```
conda env create -f env_name.yml
```
##### 7.退出虚拟环境
```
conda activate
conda deactivate
```
##### 8.重置和恢复base环境
```
conda list --revisions (查看修改版本历史)
conda install --rev 0 (恢复初始版本)
```
#### 包管理
##### 1.查询所有包
```
conda list
pip list(查看使用pip安装的库)
```
##### 2.搜索某个包是否存在
```
conda list pkgname
```
##### 3.安装和更新包
```
conda install pkgname=version_number (可以将包改成某个版本)
conda update pkgname
```
##### 4.卸载包：将依赖于这个包的所有其它包也同时卸载
```
conda unintall pkgname
```
##### 5.清理anaconda缓存
```
conda clean -p    #删除没用的包
conda clean -t    #删除tar打包
conda clean -y -all        #删除所有包及cache
```
##### 6.查询包安装位置
```
pip show pkgname
```
##### 7.在当前虚拟环境中安装包  
```
python -m pip install pkgname
```
#### 管理python
##### 1.改变python为指定版本
```
conda install python=3.5
```
##### 2.更新python最新版本
```
conda update python
```
#### 卸载pip安装的包
##### 1.删除pip安装的包
```
pip uninstall pkgname
```
##### 2.删除所有已安装的包
```
pip freeze > requirements.txt
pip uninstall -r requirements.txt -y
```
##### 3.删除pip
```
python -m pip uninstall pip
```
##### 4.查看pip默认安装位置
```
python -m site
```
##### 5.查看当前项目所依赖的包列表
```
1.安装pipreqs
2.找到项目根目录
3.运行cmd：pipreqs .> requirement.txt
或者
pip freeze > requirements.txt
```
#### conda配置清华源
##### 1.删除默认镜像渠道
```
conda config --remove channels defaults
```
##### 2.删除其他镜像源，恢复默认镜像源
```
conda config --remove-key channels
```
##### 3. 显示镜像通道
```
conda config --show channels
```
##### 4.添加镜像源
```
conda config --add channels 镜像源
```