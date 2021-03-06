# flutter 图文安装

_made by caowujun,2022.1.19_

---

## 1. android studio 安装

### 1.1 下载 4.2.2

注意不要选最新的，最新的会使用 java 的 jdk11，而我们用的 1.8。可以下载 android-studio-ide-202.7486908-windows.exe

### 1.2 更改 sdk 路径

sdk 很大，所以我们不要放在 C 盘。
![安卓sdk](/images/flutter/1.png)

在 sdk tools 里注意的是，我们上面选的是 sdk platform30,所以在这里也选择 30.
![安卓sdk](/images/flutter/2.png)
![安卓sdk](/images/flutter/3.png)

### 1.3 安装后需设置环境变量。

_假定我们已经安装了 java1.8，并且设置了环境变量。_
![jsva环境变量](/images/flutter/4.png)
![jsva环境变量](/images/flutter/5.png)

然后我们设置安卓的环境变量：

![安卓环境变量](/images/flutter/6-1.png)
![安卓环境变量](/images/flutter/7.png)

测试结果如下：
![环境变量测试](/images/flutter/8-1.png)
![环境变量测试](/images/flutter/9-1.png)

## 2. 安装

### 2.1 下载 flutter2.8.1

在任意文件夹下解压缩

### 2.2 设置用户变量

在用户变量 path 编辑并增加 flutter\bin 目录的完整路径
![环境变量测试](/images/flutter/11.png)

### 2.3 命令窗口运行 flutter 检查命令

执行

```bash
flutter doctor
```

返回如下
![环境变量测试](/images/flutter/12-1.png)

如果检查不通过会有提示以实际情况：
1）

```bash
 X cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
```

这是由于一开始的时候 sdk tools 未安装 command-line tools
2）

```bash
 X Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/docs/get-started/install/windows#android-setup for more details.
```

这是由于未执行下面的命令（一路 y 就可以）

```bash
flutter doctor --android-licenses
```

---

再次运行 ，flutter doctor 命令，最终结果如下
![环境变量测试](/images/flutter/13-1.png)

### 2.4 改国内镜像（不然会卡住）

添加环境变量 PUB_HOSTED_URL 和 FLUTTER_STORAGE_BASE_URL，内容分别是：

```
https://pub.flutter-io.cn
https://storage.flutter-io.cn
```

修改 E:\android\flutter\flutter_windows_2.8.1-stable\flutter\packages\flutter_tools\gradle\flutter.gradle

1)注释掉 google() 和 jcenter() 更改为

```xml
maven { url 'https://maven.aliyun.com/repository/google' }
maven { url 'https://maven.aliyun.com/repository/jcenter' }
maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
```

2)找到 https://storage.googleapis.com 修改为 https://storage.flutter-io.cn

### 2.5 IDE 配置

![环境变量测试](/images/flutter/21.png)
![环境变量测试](/images/flutter/22.png)

### 2.6 模拟器换路径

模拟器路径很大，默认在 C:\Users\user\.android\avd 下面
可以直接把模拟器挪走到 E 盘
![环境变量测试](/images/flutter/23.png)
然后改下配置文件的路径

```
path=E:\android\avd\Pixel_4_XL_API_30.avd
```
