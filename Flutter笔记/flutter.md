# Flutter 开发环境配置问题
## 介绍
    Flutter 是谷歌的移动UI框架，可以快速在IOS和Android上构建高质量的原生用户界面，实现一套代码在多平台运行，简化移动开发。Flutter开源且免费，它也将是构建未来Google Fuchsia应用的主要方式。
    正式版于12月5日发布。
## 需要的工具
    JAVA SDK
    Android SDK(安装Android Studio时会同时安装，若没有附带安装可以百度一哈)
    Flutter SDK
    Android Studio(非必须安装但flutter会提示安装，需要安装flutter插件)/vs code(需要安装flutter插件)

## JAVA SDK 安装
    ORACLE官网下载安装，配置环境变量，=，=不想多说。
## Flutter SDK 安装
    需要在官网(https://flutter.io/)下载安装，由于是你懂得的原因，有可能需要翻墙才能下载，也可以去github下载(https://github.com/flutter/flutter/releases)。
    下载完成后解压到你想放到的目录内，开始配置环境变量：
``` 
    FLUTTER_HOME : 你的 \flutter 目录位置
    Path : 你的 \flutter\bin 的目录位置 
```
由于flutter需要从网络获取数据，为了应对国内的网络环境，还需要添加两个环境变量来指向国内的镜像
```
    FLUTTER_STORAGE_BASE_URL : https://storage.flutter-io.cn
    PUB_HOSTED_URL ： https://pub.flutter-io.cn
```
    配置完成后就可以进入flutter的安装目录中，启动flutter_console.bat来启动flutter(不支持git等第三方终端启动)，使用flutter doctor 来检查是否缺少依赖。
    若这个时候还没有安装Android Studio，应该出现如下错误
![image](\flutter1.png)
    这时候我们来安装其所需要的依赖项。
## Android Studio 安装
    百度下载即可，改掉安装位置，安装完成后运行，应该会自动下载并配置Android SDK。
    若没有自动下载并配置Android SDK，就需要我们自己在网上下载，解压到想要放的目录，然后配置环境变量
```
    ANDROID_HOME : 你的 \AndroidSDK 目录位置
    Path : 你的 \AndroidSDK 目录位置
    Path : 你的 \AndroidSDK\platform-tools 目录位置
```
    在Android Studio中选择Configure->Setting->System Setting->Android SDK 选择你的Android SDK目录位置
    在Android Studio中选择Configure->Plugins 搜索flutter并安装此插件
    重启你的Android Studio，若在起始界面看到 Start a new Flutter project即安装成功
    需要注意，点击开始新Flutter项目后，程序会进入假死状态，是正常现象，等待1-2分钟就会进入创建页面
    创建完项目卡在Creating Flutter Project超过5分钟，请强行结束程序并重新打开，选择Open Project(不是Import！！！)找到你的项目位置即可（一般只有第一次会卡在这里）
    使用Android Studio我们需要创建一个Android虚拟设备，点击右上角的图标即可创建，选择好手机配置型号及对应的Android版本
![image](\flutter4.png)
--- 
    修改AVD和gradle位置来精简C盘空间，看以下三篇文章
>https://blog.csdn.net/qiujuer/article/details/44160127  

需要注意idea.properties文件中的路径，一定要正确  
>https://blog.csdn.net/qiujuer/article/details/44257993  

最好在更改配置文件之后还是添加一个环境变量  
>http://www.cnblogs.com/linfenghp/p/7808408.html  

只移动xxx.avd这个文件夹（不是移动整个.android文件夹），xxx.ini中的路径不能错  
最后发现一个问题，修改后运行flutter doctor 会检测你并未安装Android Studio，不影响使用但是强迫症看着很难受  
## 继续检查 Flutter 依赖
    回到flutter_console.bat重新执行flutter doctor命令，应该出现如下结果：
![image](\flutter3.png)
    执行提示的命令，再次执行flutter doctor命令，应该只有最后一个提示了  
![image](\flutter5.png)  
    在Android Studio中开启虚拟设备，再次执行flutter doctor，一切就都解决了。
## 回到Android Studio中
    检查一下 Project Structure->Modules 中的Module SDK是否为空，若为空选择你的Android SDK位置
    右上角开启虚拟设备后，<no devices>会变成当前开启的设备
    第一次运行会十分的慢，慢慢等待吧。
![image](\flutter6.png)