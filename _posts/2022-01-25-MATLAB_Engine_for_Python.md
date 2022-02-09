---
layout: post
title: python调用matlab API
categories: 教程
subtitle: "python调用matlab API的配置过程及调用示例"
author: "WUFUJIAN"
header-img: "img/Matlab_Logo.png"
header-mask: 0.2
tags:
  - python
  - matlab
---


# 安装用于 Python 的 MATLAB 引擎 API
以下的一大段话基本上都是抄matlab官网的描述，然后作一些修改，要看原文请点击[这里](https://ww2.mathworks.cn/help/matlab/matlab_external/install-the-matlab-engine-for-python.html)
## 验证您的配置
在安装之前，确认您的 Python 和 MATLAB 配置。

* 检查您的系统是否具有受支持的 Python 版本和 MATLAB R2014b 或更新版本。有关详细信息，请参阅 [MATLAB 产品（按版本）兼容的 Python 版本](./python-compatibility.pdf)。

* 要检查您的系统上是否已安装 Python，请在操作系统提示符下运行 Python。

* 将包含 Python 解释器的文件夹添加到您的路径（如果尚未在该路径中）。

* 找到 MATLAB 文件夹的路径。启动 MATLAB，并在命令行窗口中键入 matlabroot。复制 matlabroot 所返回的路径。

## 安装引擎 API
要安装引擎 API，请选择以下选项之一。您必须在指定的文件夹中调用此 python 安装命令。

* 在 Windows 操作系统提示符下（您可能需要管理员特权才能执行这些命令）-
```
cd "matlabroot\extern\engines\python"
python setup.py install
```
* 在 macOS 或 Linux 操作系统提示符下（您可能需要管理员特权才能执行这些命令）-
```
cd "matlabroot/extern/engines/python"
python setup.py install
```
在 MATLAB 命令提示符下 -
```
cd (fullfile(matlabroot,'extern','engines','python'))
system('python setup.py install')
```
## 为多个 MATLAB 版本安装 Python 引擎
通过将 MATLAB Python 包安装到特定于版本的位置，可以从 Python 脚本指定要运行的 MATLAB 版本。例如，假设您要从 Python 版本 3.6 脚本中调用 MATLAB R2019a 或 R2019b。

* 从 Windows 系统提示符下，将 R2019a 包安装在名为 matlab19aPy36 的子文件夹中：
```
cd "c:\Program Files\MATLAB\R2019a\extern\engines\python" 
python setup.py install --prefix="c:\work\matlab19aPy36"
```
* 将 R2019b 包安装在 matlab19bPy36 子文件夹中：
```
cd "c:\Program Files\MATLAB\R2019b\extern\engines\python" 
python setup.py install --prefix="c:\work\matlab19bPy36"
```
* 从 Linux 系统提示符下：
```
cd "/usr/local/MATLAB/R2019a/bin/matlab/extern/engines/python"
python setup.py install --prefix="/local/work/matlab19aPy36"
cd "/usr/local/MATLAB/R2019b/bin/matlab/extern/engines/python"
python setup.py install --prefix="/local/work/matlab19bPy36"
```
* 从 Mac 终端：
```
cd "/Applications/MATLAB_R2019a.app/extern/engines/python"
python setup.py install --prefix="/local/work/matlab19aPy36"
cd "/Applications/MATLAB_R2019b.app/extern/engines/python"
python setup.py install --prefix="/local/work/matlab19bPy36"
```
其中**c:\work\matlab19bPy36**是python的**安装所在的路径（如有多个python）**

# 从 Python 调用 MATLAB
## 函数
### Python 函数


<table>
 
  <tr>
    <td>
    <a href="https://ww2.mathworks.cn/help/matlab/apiref/matlab.engine.start_matlab.html">matlab.engine.start_matlab</a></td>
    <td>Start MATLAB Engine for Python</td>
  </tr>
<tr>
    <td>
    <a href="https://ww2.mathworks.cn/help/matlab/apiref/matlab.engine.find_matlab.html">matlab.engine.find_matlab</a></td>
    <td>Find shared MATLAB sessions to connect to MATLAB Engine for Python</td>
  </tr>
  <tr>
    <td>
    <a href="https://ww2.mathworks.cn/help/matlab/apiref/matlab.engine.connect_matlab.html">matlab.engine.connect_matlab</a></td>
    <td>Connect shared MATLAB session to MATLAB  Engine for Python</td>
  </tr>
</table>

### matlab 函数

<table>
 
  <tr>
    <td>
    <a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.engine.shareengine.html">matlab.engine.shareEngine</a></td>
    <td>将正在运行的 MATLAB 会话转换为共享会话</td>
  </tr>
<tr>
    <td>
    <a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.engine.enginename.html">matlab.engine.engineName</a></td>
    <td>返回共享 MATLAB 会话的名称</td>
  </tr>
  <tr>
    <td>
    <a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.engine.isengineshared.html">matlab.engine.isEngineShared</a></td>
    <td>确定 MATLAB 会话是否共享</td>
  </tr>
</table>


## 示例
* 首先在python中先写入
```
import matlab
import matlab.engine
```
matlab是用来做数据转换等工作，matlab.engine是用来调用函数的。
* 然后新建一个matlab脚本，count.m
```
function a = count(c,d)
a = c+d;
```
* 调用自己写的matlab函数
```
eng = matlab.engine.start_matlab()#可以为所欲为的调用matlab内置函数
a = eng.count(1.0,2.0) #引用自写的脚本
print(a) 
b = eng.sqrt(4.) #引用matlab内置函数
print(b)
'''
ouput:
3.0
2.0
'''
```