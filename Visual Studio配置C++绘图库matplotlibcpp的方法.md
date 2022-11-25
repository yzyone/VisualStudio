# Visual Studio配置C++绘图库matplotlibcpp的方法

  本文介绍在**Visual Studio**软件中配置、编译**C++** 环境下matplotlibcpp库的详细方法。

  matplotlibcpp库是一个**C++** 环境下的绘图工具，其通过调用**Python**接口，实现在**C++** 代码中通过matplotlib库的命令绘制各类图像。由于其需要调用**Python**接口，因此在配置matplotlibcpp库时有些较为麻烦的操作。本文就将matplotlibcpp库的具体配置方法进行详细介绍。

# 1 Git配置

  **Git**是一个分布式开源版本控制系统，在后期我们需要基于其完成vcpkg包管理器的下载与安装，因此需要首先完成**Git**的配置；具体方法大家可以参考[Git的下载、安装方法与使用Git复制GitHub代码的流程](https://www.toutiao.com/i7161264733503472164/?group_id=7161264733503472164)这篇文章。

# 2 vcpkg配置

  vcpkg是一个开源的**C++** 包管理器，在后期我们需要基于其完成matplotlibcpp库的下载与安装，因此需要首先完成vcpkg的配置。

  首先，选定一个路径作为vcpkg的保存路径；随后，在这一文件夹下，按下Shift按钮并同时右击鼠标，选择“**在此处打开Powershell窗口**”。

![img](./matplot/43a211b8a71b492dad1a2c1f7eda76d3.image)



  随后，将弹出如下所示的窗口。

![img](./matplot/1a5ef5031af1424998a09f92b8d09b67.image)



  接下来，在其中输入如下的代码，并运行。

```
git clone https://github.com/microsoft/vcpkg
```

  具体如下图所示。

![img](./matplot/6a44e63de2fa4224abb077fed2531d3a.image)



  稍等片刻，出现如下所示的界面，说明vcpkg安装完毕。

![img](./matplot/a32cad06c4494d7789c1640ca1bfd3fc.image)



  随后，输入如下代码，进入vcpkg保存路径。

```
cd vcpkg
```

  再输入如下代码，激活vcpkg环境。

```
.\bootstrap-vcpkg.bat
```

  具体如下图所示。

![img](./matplot/20bcc143f1214c379011333569f82288.image)



  运行完毕后，将得到如下所示的结果。

![img](./matplot/8a69c691af714e3c91c32c68a853dcad.image)



  接下来，再输入如下所示的代码，将vcpkg与我们的**Visual Studio**软件相连接。

```
.\vcpkg integrate install
```

  具体如下图所示。

![img](./matplot/5a3e65f80f0640878f6bd408aef1ef56.image)



  代码运行完毕后，如下图所示。

![img](./matplot/e764fa96ca0c49fa9414458a97c76486.image)



# 3 matplotlibcpp配置

  接下来，我们即可开始进行matplotlibcpp库的配置。

# 3.1 matplotlibcpp安装

  首先，依然在刚刚的界面中，输入如下代码，安装matplotlibcpp库。

```
.\vcpkg install matplotlib-cpp
```

  代码运行结束后，得到如下所示的结果。

![img](./matplot/b9547207c7a14d1eae9a09e62cbd5b5d.image)



  随后，输入如下所示的代码，安装64位的matplotlibcpp库。

```
 .\vcpkg install matplotlib-cpp:x64-windows
```

  运行代码后，得到如下所示的结果。

![img](./matplot/ed6da61a23354ab195632c182392a890.image)



# 3.2 matplotlibcpp配置

  首先，在刚刚配置的vcpkg的保存路径中，通过以下路径，找到matplotlibcpp.h文件，并将其打开。

![img](./matplot/91dd2a6463f8444389d9cac06b0a139e.image)



  随后，在其#include部分的最下方，添加如下代码。

```
#include <string>
```

  具体如下图所示。

![img](./matplot/f828136831b4419280e95f74c7ad5825.image)



  同时，在该文件340行左右，将template开头的两行注释掉，如下图所示。

![img](./matplot/256124d166204c0ba94119ef3f54b828.image)



# 4 Python配置

  由于matplotlibcpp库是通过调用**Python**接口，实现在**C++** 代码中通过matplotlib库的命令绘制各类图像，因此配置matplotlibcpp库时还需要保证电脑中拥有**Python**环境。而这里的**Python**环境也有一个具体的要求——需要具有Debug版本的**Python**。

  因此，可以分为3种情况：第一种情况，是大家电脑中**之前没有安装过任何Python环境**；第二种情况，是大家**之前有通过Anaconda下载Python环境**；第三种情况，则是大家之前**有通过Python官方下载Python环境**。针对这三种情况该具体如何配置，我们也会在接下来的文章中具体提及。

  首先，对于第二种情况，也就是**之前有通过Anaconda下载Python环境**的情况，大家从这里开始看就好。首先，需要看一下**Anaconda**中**Python**的版本；如下图所示，我这里就是在**Anaconda**中有3.9.12版本的**Python**。

![img](./matplot/3db2fe46c5624cea9f03700cdf6527de.image)



  其次，对于第一种情况，也就是**之前没有安装过任何Python环境**的情况，大家从这里开始看就好。我们在**Python**的官方下载地址（
https://www.python.org/downloads/）中，下载最新的**Python**版本即可（如果是之前有通过**Anaconda**下载**Python**环境的情况，大家这里下载和自己**Anaconda**中**Python**版本不一样的版本即可。

![img](./matplot/bcffc69e5dad437794ff0171e8084848.image)



  随后，双击打开刚刚下载好的安装包。对于第三种情况，即大家之前**有通过Python官方下载Python环境**的情况，那么直接找到当初的安装包，然后进行如下的操作即可。

  首先，选择“**Customize installation**”选项。

![img](./matplot/6c077b3f83b64812a59d7acb7fdc296c.image)



  接下来的页面，选择默认的配置即可。

![img](./matplot/e1cb6131010c46a8a34971ea2698c75c.image)



  随后的页面，选中第一个方框中所包含的勾选项，并在其下方配置自定义安装路径；这个路径建议大家自己修改一下，同时记下来这个路径，之后会经常用到。

![img](./matplot/9fe8a7fcbfad460fb1168c09c6bdad14.image)



  随后，依据文章[Win10用户变量、系统变量等两种环境变量的新建、编辑修改与删除](https://www.toutiao.com/i7058491241071215118/?group_id=7058491241071215118)提到的方法，首先将以下两个路径添加到**环境变量**中的**用户变量**的Path中。具体这两个路径的前缀，和大家前面所选的**Python**安装路径有关。

![img](./matplot/0b13b6e2308c4fba8cd91e9b1590c1ae.image)



  接下来，将这两个路径同样在**环境变量**的**系统变量**的Path中添加一下；此外，还要注意，如果大家的**环境变量**中，有原本的**Python**路径，大家最好将原本的路径放在我们新建的变量的下方，如下图所示。

![img](./matplot/088fc7c78a5842299e5e28766a74e5c1.image)



  此外，还需要在**系统变量**中，添加如下所示的两个内容；其中，“**变量**”一栏依次填写PYTHONHOME与PYTHONPATH，“**值**”一栏就是刚刚我们的**Python**安装路径。

![img](./matplot/9a61c5da2d394c07b9ac6d049871ea86.image)



  随后，我们在计算机中进入**Python**环境，就默认进入我们刚刚配置的、新的**Python**环境；之后如果我们需要正常使用**Python**了，可以用我们这次配置的新的**Python**；也可以将刚刚配置的PYTHONHOME与PYTHONPATH两个系统变量删除，并将原有**Python**所对应的**环境变量**提前到刚刚配置好的**Python**的**环境变量**之前，从而使用我们原先版本的**Python**。

  接下来，我们需要对新创建的**Python**进行matplotlib库与numpy库的安装。这里就使用**Python**最传统的pip安装方法即可，首先输入如下的代码。

```
pip install -U matplotlib
```

  出现如下所示的界面即说明matplotlib库已经安装完毕。

![img](./matplot/4f54c2ee153344528f0faded798fdac9.image)



  随后，输入如下所示的代码。

```
pip install numpy scipy matplotlib
```

  即可完成numpy库的安装。

# 5 解决方案配置

  接下来，我们创建或打开需要调用matplotlibcpp库的解决方案。

  首先，将前述**Python**安装路径下的以下两个.dll文件复制（具体文件名称与**Python**版本有关）。

![img](./matplot/552ad319f82b458db7030ccff452d0b5.image)



  并将其复制到解决方案的文件夹下。

![img](./matplot/a2b8e1180ab94c849895bfe9b78b3669.image)



  随后，依据文章[Visual Studio调用配置好的C++库的方法](https://www.toutiao.com/i7164724850538758692/?group_id=7164724850538758692)提到的方法，分别进行以下配置。

  首先，在“**附加包含目录**”中，将**Python**和numpy库的include文件夹放入其中。

![img](./matplot/0252ee7c4289407c8e520e2e94ed903f.image)



  其次，在“**附加库目录**”中，将**Python**安装路径下libs文件夹的路径放入其中。

![img](./matplot/03228ce665804a8183c9bc2ca0f8f18a.image)



  再次，在“**附加依赖项**”中，将**Python**安装路径下libs文件夹中如下所示的4个.lib文件放入其中。

![img](./matplot/13bce94f467547318909029d6b70b4c6.image)



  随后，对于需要调用matplotlibcpp库的程序，需要添加以下代码。

```
#include "matplotlibcpp.h"
namespace plt = matplotlibcpp;
```

  具体如下图所示。

![img](./matplot/b3a9c9ad5d8045409e9f62308a21c38a.image)



  随后，即可开始运行代码。这里提供一个最简单的matplotlibcpp库调用代码。

```
#include "matplotlibcpp.h"

namespace plt = matplotlibcpp;

int main() {
    plt::plot({ 1, 2, 3, 4 });
    plt::show();
    return 0;
}
```

  运行代码，出现如下所示的窗口。

![img](./matplot/4e52a41f09fb471ab58ec7318249984b.image)



  以上，即完成了matplotlibcpp库的配置。

欢迎关注：疯狂学习GIS