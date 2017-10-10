# MATLAB语言基础知识
已有功底的飘过~~~~~
### 编程知识之一：matlab程序结构
MATLAB语言广泛应用于各种仿真研究中，尤其是自动控制领域。与Python，R等具有计算功能的语言相比，MATLAB具有更为专业的工具包。
MATLAB程序通常包括四部分，下面我们来详细说明。
#### 第一部分：清除主界面和内存空间
MATLAB几十年来一直沿用主界面风格，可以在命令窗口（Command window）上调试某一条语句是否正确，还可以执行输入命令。当程序出现错误时，命令窗口也会出现红色报错提示，想要输出某个变量的结果时，命令窗口也会非常方便地显示运行结果。可见主界面的内容是相当丰富的，但主界面保存的变量并不会自己清空，其历史命令（Command history）及工作空间 (Workspace)的变量自动保存着。
这会使用户弄不清楚哪个是当前程序的执行结果，当程序报错时，也给错误排查带来了困难。因此在程序执行的开始，最好清除主界面现有的内容，这样在本次程序执行之后，方便分析本次程序执行的结果。

在MATLAB中，清除主界面的语句是

    clc; 
    
清空内存变量是  

    clear; 
    
关闭所有图形窗口是

    close all; 
    
建议使用者在程序的开始就使用以上命令，这个习惯对管理程序数据空间很有帮助。
MATLAB的“管辖”的数据空间范围包括内存空间、路径，输入命令，类似这样的超简单的语句：

    a=b+1
MATLAB首先寻找变量b的值，如果内存空间已经有该变量的值，该语句在执行的时候就不会出错。但如果变量b在之前的程序中已被赋值，那么，在内存空间中已经存在b值，当前执行的程序当中并没有对b进行赋值，但由于MATLAB在内存空间中有b值就导致a被赋值。

然而，此时程序是错的，因为没有在使用变量之前进行赋值。人们会大呼“程序通过了！”然后，很开心地关机，当下一次打开MATLAB时，MATLAB会再次运行原程序，会发现系统出现错误提示“b没有赋值”，这对一个初学者来讲是相当费解的一件事：程序昨天还好好的，为什么又出现了错误？

现在明白了吧：由于没有使用clear语句清除内存空间，导致最初运行程序时，b变量没有初始化的错误“漏”报。

这样的错误不是每次都会有，但是在算法研究的过程中错误率的确非常高。初学者由于缺乏专业的编程经验，在变量命名时比较随意，常常使用c、A、PP、n等简单的字母变量，在内存空间中留下痕迹。因此，养成一个好习惯至关重要，即在程序的开始使用清除主界面的clc语句，在“视觉”上有一个良好的开始，迎接“干干净净”的主界面；使用clear清除内存空间，使内存空间“干干净净”，程序每次都重新开始。

#### 第二部分
设置问题中交代的因变量，一般是一些已知的数据；

#### 第三部分
需要解决计算问题的结果变量，需要依据题意进行计算；

#### 第四部分
结果输出，有的只需要输出数据，而大多数情况下都需要用图形来表达。这几部分很多MATLAB书籍进行了详细介绍，本书不再赘述。下面使用一段具体的实例进行说明。
    
#### 例
假设温度传感器A具有均匀分布测量噪声，噪声的均值为0，方差为2，请模拟当室内温度是20的时候，传感器的输出数据是多少？给出1000个数。

    % 清除主界面和内存空间
    clc;
    clear;
    % 设置变量：
    m=20;
    v=2;
在MATALB中，通过使用randn函数产生随机数。这里，测量值是真实值与测量噪声的叠加。根据测量方差和均值可以模拟出噪声，也就是将randn产生的结果乘以标准差，然后加上期望均值并将其与真实值叠加：

    % 室内温度
    s1=m+sqrt(v)*randn(1,1000); ％这里期望均值为0，所以语句中没有显示均值。
最后，将模拟出的测量噪声如图所示，测量噪声是一个离散序列，所以形式非常简单，使用plot函数就可以实现：

    plot(s1)
    xlabel(‘采样时刻’)
    ylabel(‘测量数据’)
![模拟传感器A的测量数据输出](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/2SensorA.png)


模拟传感器A的测量数据输出

MATLAB画图函数非常丰富，还可以对图设定适当的标注来提高效果图的表达方式和内容，增强可视化效果，比如横坐标、纵坐标的设置，用不同线型来区别不同变量的曲线，或在一张图上画出多个图进行对比分析。详情可查阅MATLAB相关书籍。

下面再看几个类似的例子：
#### 例
若传感器B的噪声更大些，其噪声的均值为0，方差为8，请模拟当室内温度是20的时候，传感器的输出数据是多少？给出1000个数。
同理，MATLAB程序应该有四大部分，可以给出下面这段程序：

    %C2_1
    clc;
    clear;
    m=20; 
    v=8; 
    s2=m+sqrt(v)*randn(1,1000); 
    plot(s2) 
    xlabel(‘采样时刻’) 
    ylabel(‘测量数据’) 
 
![模拟传感器B的测量数据输出](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/2SensorB.jpg)


模拟传感器B的测量数据输出

#### 例
如果室温在上升，实际的上升曲线是2*t+20，传感器A的输出数据是多少？给出1000个数。

与上面两例不同，本例的实际室温随着时间的变化而变化，为了描述这个关系，需要给出时间t的变化，然后再考虑测量噪声与真值的关系。在实际的仿真中，需要指定时间t的仿真范围，也就是开始时间和结束时间，一般情况下开始时间可以设为从0开始，而结束的时间则可以根据实际情况进行设置。但是题中要求我们给出1000个数据，也就是t要包含1000个数据点。

我们可以用语句t=0:0.01:9.99来表示从0时刻到9.99秒的采样时间，每0.01秒采集的一个数据共获得1000个测量数据。或者使用另外一个语句t=linspace(0,9.99,1000)，也能输出1000个结果值。若使用传感器A进行测量，在真值之上需要叠加测量噪声获得测量数据，程序如下
    
    %C2_2 
    clc; 
    clear; 
    t=0:0.01:9.99; 
    m=2*t+20; 
    v=2; 
    s3=m+sqrt(v)*randn(1,1000); 
    plot(t,s3) 
    xlabel('t') 
    ylabel('measurement data')

![室内温度变化时模拟传感器A测量数据的输出](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/IndoorTemperatureChange.jpg)


室内温度变化时模拟传感器A测量数据的输出

### 编程知识之二：MATLAB函数文件 
本小节对MATLAB函数的知识进行简单介绍。

MATLAB语言除了主程序（如上述编写的这些小程序）之外，还可以编写函数文件。

以下面的小例子来说明函数文件的格式： 
1）在MATLAB的新建function文件中，编写如下程序并进行保存： 

    function s = sumfunc (a,b) s = a+b； 
    
2）然后在MATLAB的工作空间中调用sumfunc函数，便可完成MATLAB函数的编写目的。 

在上述程序中，文件需要有关键字，即文件中第一行。函数文件必须由function开头，然后标明函数的输入输出变量。在函数体中需要表明函数输入输出变量之间的关系，如在上述小例子中，函数的输入输出是简单的求和关系。

在调用函数时可以使用

    y=sumfunc (3,4)
语句，会得出y=7的计算结果。

总结一下，本小节介绍了仿真语言的基础知识，以编写仿真程序的四部分步骤为例，简单介绍了MATLAB语言的基本使用方法，以及函数的编写。给出了几个小实例，来说明仿真过程产生的数据方法。如果想要更深入地了解MATLAB语言，可以参阅其它相关书籍。
# 第一次课的内容
开始啦！！ 
## 什么是信息融合
从广义上讲，多源信息融合无时不刻地存在于我们人类的生活、工作和学习当中，在战争中更是如此。战场上，指挥官都要融合各种各样的信息才能获得一场战争的胜利，从古代的战争到现在的战争都是如此。

信息融合的定义从广义上来讲是研究利用多个数据源更好地完成任务，这些任务包括在时间上和空间上能够更全面的利用数据信息，能够更高层次的归纳数据信息，获得我们想要知识，以达到我们的目的。多源信息融合系统有各种各样的数据，不同的数据可能会完成不同的任务。

从方法上来讲，我们通常把融合方式分为两类方法，一类是集中式的，一类是分布式的。因为传感器有各种各样不同的特点，融合各个传感器的优势来获得最终的一个更好的结果就显得非常必要了。

中医上讲的望、闻、问、切就是一个典型的信息融合过程，一个医生把病人的各种状况都采集下来，然后，有经验的老中医会把这些信息能够很好的融合在一起，给出更好的诊断结果。
## 信息融合技术简介
最早提出来信息融合技术的是美国军方提出来的。该定义是这样的：信息融合是对从单个和多个信息源获取的数据和信息进行关联相关和综合，以获得精确的位置和身份估计，以及对态势和威胁及其重要程度进行全面及时评估的信息。

信息融合技术从1973年年初提出来，以后经历了20世纪的80年代、90年代初和90年代末的三次研究热潮。从1973年美国国防部资助开发的声纳信号理解系统首次提出了数据融合的概念。1988年，美国国防部把数据融合列为90年代重点研究开发的20项关键技术之一。又过了三年之后，1991年美国就已经在这方面有了很多的研究成果，已经有54个融合系统引入到了军用电子系统中去。1995年，我国首次数据融合技术会议，有很多研究工作者，包括军方研究所和大学都有了很多这方面的研究。

## 大信息融合的概念与信息融合技术的关系
![两者的关系](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/ApplicationofFusion.png)
感觉到这两者不一样了吧！！希望课程能让你既可以应用信息融合的概念处理生活、学习中的事情，让自己更有智慧，又可以学会信息融合技术做一个IT精英！
## 本门课程的主要内容
以数据融合中的位置估计为主要内容，讲授基于RFID的室内跟踪仿真系统。

我们一起搭建一个基于MATLAB的仿真系统，界面就像这样的。。
![RFID仿真系统界面](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/RFIDTrackingSystem.png)
##  数据源的误差
接下来我们来谈一下传感器获得的数据源的问题。我们知道我们必须要通过一定的方式来采集多源信息融合。我们这门课程所研究的信息主要是来自于传感器，因此这一节我们先首先来讨论一下关于传感器的测量问题。

首先要说明的是传感器测量会有很大的不确定性，也就是说我们通过传感器获得的数据和真实的数据往往是有很大的偏差的。

那么这些传感器所获得的数据具有哪些不准确性呢？这些不准确性主要包括常值误差、漂移误差和测量噪声。常值误差指的是在测量的过程中，由于读数、或传感器本身的性能上存在的一个偏差，它会是一个常量，测量值始终是和真实值相差一个常量。 漂移误差是测量结果和真实值之间会产生一定的逐渐加大的偏差，有时是由于传感器的温度逐渐升高造成的。我们会发现这个偏差越来越大。

还有一种就是由于传感器本身的性能，以及周围的一些干扰，导致测量数据会含有噪声。传感器的这三种不同测量误差都需要我们消除或者了解。
![真实值和各种不确定性](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/MeasurementNoise.png)

真实值和各种不确定性，包括常值误差、漂移误差和测量噪声
![测量值](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/Measurement.png)

获得的测量值，它和真实值是多么的不一样呀！所以，有时传感器很不准，有木有？？

通常情况下，我们的处理是将常值误差去掉获得更准确的值，这个过程我们一般把它叫做标定。漂移误差也要通过一定的标定的方式，比如说将传感器运行一段时间之后，然后获得这些漂移，再把它们从测量的数值中去掉。

对于测量噪声来讲，因为这是一个统计随机过程的统计量，所以说我们不能够把它去掉，但是我们需要知道这个噪声的统计特性，比如说它是否是高斯白噪声以及它的方差等等，利用这些信息在融合过程当中来考虑这些测量数据的不准确性，以获得更准确的融合结果。

### 练习
利用MATLAB实现以下问题。
![课堂练习——获得模拟的测量数据](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/ClassWork1forKalmanFilter.png)

# 第二次课的主要内容
## Kalman 滤波器
本节讲Kaman滤波器的原理及应用，它是基于多传感器跟踪方法的基础。

它只有五个公式，但有人觉着它很难，因为这五个公式的关系挺复杂的。

我们这里不讲Kalman滤波器是怎么来的，它为什么在参数已知的条件下是最有的等，我们只讲怎么用，具体来讲，我们准备讲明白以下几点：

1. 这五个公式啥关系？

2. 给你Matlab程序，来加深一下印象。

3. Kalman滤波器在使用时的一些小技巧。

### 1. 这五个公式啥关系？
![Kalman滤波器](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/KalmanFiler.png)

Kalman滤波器5个公式的关系
### 2. 给你Matlab程序，来加深一下印象。
    
     function [xe,pk,p1]=kalmanfun(A,C,Q,R,xe,z,p)
     %This function is to calculate the estimation state by Kalman filter.
     xe=A*xe;                     % 计算向前一步预测估计
     P1=A*p*A'+Q;                 % 计算向前一步估计方差
     K=p1*C'*inv(C*p1*C'+R);      % 计算估计增益
     xe=xe+K*(z-C*xe);            % 计算估计结果
     pk=(eye(size(p1))-l*C)*p1;   % 计算估计方差

### 3. Kalman滤波器在使用时的一些小技巧。
#### 稳态Kalman滤波器是啥？有什么用？
运行一下本文件夹中的C4_1.m这个程序，感受一下Kalman滤波器是怎样快速收敛到稳态的。
#### 估计的方差说明什么？方差越大说明什么？

### 练习
利用MATLAB实现以下问题。
![kalman滤波器](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/CouseWork2forKalman.png)
如果你已经完成了问题11那么就从第2个问题开始吧。


# 第三次课
我们讲述四种不同的状态融合估计方法。

### 作业
需要在前两个问题的基础上，完成以下作业的第3题。
![第一次仿真作业](https://github.com/Xue-boJin/data-fusion-for-indoor-tracking-by-RFID/blob/resource/HomeWork1forFusion.png)

## 参考文献

[1] 金学波，![Kalman滤波器理论与应用——基于MATLAB实现](http://www.ecsponline.com/goods.php?id=177510)，科学出版社，2016，第2章、第4章
