# 解决cmake时extern中的文件无法下载的问题
kinect dk版本以1.4为例。

问题描述：在ubuntu中配置kinect DK的环境时，cmake时需要调用隐藏文件（主目录中ctrl + h查看）.gitmodules，在设定的目录中去下载所需要的
第三方库。但是很奇怪的是如果整个cmake .. -GNinja 进行下载时，会出现连接不上的问题。但是，如果单个从.gitmodules上的连接就可以下载。
所以需要分开下载。

## 一 下载 extern中的第三方库

例如 .gitmodules 文件为

 ```
   [submodule "extern/cjson/src"]
    path = extern/cjson/src
    url = https://github.com/DaveGamble/cJSON.git
  [submodule "extern/azure_c_shared/src"]
    path = extern/azure_c_shared/src
    url = https://github.com/Azure/azure-c-shared-utility.git
    branch = master
  [submodule "extern/spdlog/src"]
    path = extern/spdlog/src
    url = https://github.com/gabime/spdlog.git
  [submodule "extern/libyuv/src"]
    path = extern/libyuv/src
    url = https://chromium.googlesource.com/libyuv/libyuv
  [submodule "extern/libmatroska/src"]
    path = extern/libmatroska/src
    url = https://github.com/Matroska-Org/libmatroska.git
  [submodule "extern/libeml/src"]
    path = extern/libebml/src
    url = https://github.com/Matroska-Org/libebml.git
  [submodule "extern/imgui/src"]
    path = extern/imgui/src
    url = https://github.com/ocornut/imgui.git
  [submodule "extern/googletest/src"]
    path = extern/googletest/src
    url = https://github.com/google/googletest.git
  [submodule "extern/glfw/src"]
    path = extern/glfw/src
    url = https://github.com/glfw/glfw
  [submodule "extern/libsoundio/src"]
    path = extern/libsoundio/src
    url = https://github.com/andrewrk/libsoundio.git
  [submodule "extern/libjpeg-turbo/src"]
    path = extern/libjpeg-turbo/src
    url = https://github.com/libjpeg-turbo/libjpeg-turbo.git
  [submodule "extern/libusb/src"]
    path = extern/libusb/src
    url = https://github.com/libusb/libusb
  [submodule "extern/libuvc/src"]
    path = extern/libuvc/src
    url = https://github.com/microsoft/libuvc.git
    branch = Azure-Kinect-Sensor-SDK
 ```

对于cjson库，需要进入extern/cjson目录，然后 

`git clone  https://github.com/DaveGamble/cJSON.git`

成功下载后，将原先的src文件删掉，将下载的cjson改名为src

其他第三方库 同理操作

## 下载azure_c_shared中的第三方库
进入extern/azure_c_shared目录中，ctrl + h 可以发现还有一个.gitmodules文件

```
[submodule "testtools/ctest"]
	path = testtools/ctest
	url = https://github.com/Azure/azure-ctest.git
[submodule "testtools/testrunner"]
	path = testtools/testrunner
	url = https://github.com/Azure/azure-c-testrunnerswitcher.git
[submodule "deps/azure-macro-utils-c"]
	path = deps/azure-macro-utils-c
	url = https://github.com/Azure/azure-macro-utils-c.git
[submodule "deps/umock-c"]
	path = deps/umock-c
	url = https://github.com/Azure/umock-c.git
```
所以需要进行跟上一步类似的操作

例如 对于ctest库， 需要进入testtools目录，然后

`git clone https://github.com/Azure/azure-ctest.git`

成功下载后将原来的ctest文件删掉，将下载的azure-ctest改名为ctest

其他同理

## 继续cmake
在上两步下载结束后，即可回到build目录中执行`cmake .. -GNinja`与接下来的命令
