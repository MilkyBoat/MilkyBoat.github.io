---
title: windows+vs+opencv环境配置
date: 2021-03-08 19:24:44
tags: opencv
---

# windows + VisualStudio + opencv 环境配置

> 此blog编辑于2021年3月8日，具体操作可能随版本变化而变化，注意教程时效性

## step1：下载opencv集成包

点击[链接](https://opencv.org/releases/)下载最新版本的opencv，下载好后运行，这是一个自解压文件，解压之后的`opencv`文件夹找个地方放好，比如`D:\lib\opencv`。

## step2：配置环境变量

依次点击：`此电脑`->顶上的`系统属性`->右边或往下滑`高级系统设置`->`环境变量`

然后上面的`用户变量`或下面的`系统变量`任选一个，点击新建->变量名填写`OPENCV`，变量值为`D:\lib\opencv\bulid`如果你之前选择了其它目录，这里修改"opencv\bulid"之前的内容与之一致。

然后找到`Path`环境变量，如果之前选择在用户变量中添加，这里可能没有，需要新建该环境变量。然后点击`编辑`->`新建`，输入值`%OPENCV%\x64\vc15\bin`

## step3：VisualStudio C++全局设置

进入`C:\Users\Luczydoge\AppData\Local\Microsoft\MSBuild\v4.0`文件夹，其中Luczydoge是你的电脑用户名。这里面的AppData是隐藏文件夹，找不到去`查看`里面开启`隐藏的项目`选项。

修改`Microsoft.Cpp.x64.user.props`文件为以下内容

```xml
<?xml version="1.0" encoding="utf-8"?> 
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ImportGroup Label="PropertySheets">
  </ImportGroup>
  <PropertyGroup Label="UserMacros" />
  <PropertyGroup>
    <ExecutablePath>$(ExecutablePath)</ExecutablePath>
    <IncludePath>$(OpenCV)include;$(IncludePath)</IncludePath>
    <ReferencePath>$(ReferencePath)</ReferencePath>
    <LibraryPath>$(OpenCV)x64\vc15\lib;$(LibraryPath)</LibraryPath>
    <SourcePath>$(SourcePath)</SourcePath>
    <ExcludePath>$(ExcludePath)</ExcludePath>
  </PropertyGroup>
  <ItemDefinitionGroup>
    <Link>
      <AdditionalDependencies>kernel32.lib;user32.lib;gdi32.lib;winspool.lib;comdlg32.lib;advapi32.lib;shell32.lib;ole32.lib;oleaut32.lib;uuid.lib;odbc32.lib;odbccp32.lib;opencv_world451d.lib;%(AdditionalDependencies)</AdditionalDependencies>
    </Link>
  </ItemDefinitionGroup>
  <ItemGroup />
</Project>
```

## step4：去VS试一下

打开VS，新建空项目，将顶上的解决方案平台由`x86`改成`x64`，新建一个cpp文件，粘贴以下代码：

```c++
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main() {
	string fileName = "test.jpg";
	Mat image = imread(fileName);
	imshow("显示图像：", image);
	waitKey();
	return 0;
}
```

然后随便找一个jpg图片放在cpp文件同一目录下，调试运行，如果能弹窗显示你刚刚放的图片说明opencv已经配置好了
