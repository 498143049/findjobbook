#工业环境下数码管的定位与识别
## opencv
### 用了那些库
 + 空间转化 cvcolor 
 + 图像滤波 
 + 连通域 查找 findContours 轮廓检测
 + 图像形态学操作 morphologyEx
 + 寻找外接矩形  boundingRect
 + svm 训练 交叉验证 网格搜索
 + 局部二值化算法 
 + mser 
 + SWT
### mat指针访问元素的方式 
 + C语言的云算子[] ptr 族 
 + 迭代器 方法 
 + 动态地址方法 at 族元素 适合随机访问 
 + LUT函数 //是用来多线程架构
 + Mat 直接的复制只是浅拷贝，而深拷贝必须调用copyto 和clone 
 + [Mat 源代码分析](http://blog.csdn.net/honpey/article/details/9028059)

### 使用的算法
#### 大津阈值法  
 最佳阈值法，根据类间方法最大的原则，进行划分元素，具体算法为尝试每一个阈值，然后根据类间方差的公式求得最佳阈值，其中类似双峰的交界处。
#### 标记分水岭算法
如何计算标记 给定标记
   
#### canny 算法的具体过程，如何做到自适应
   1. 使用sobel 算子 得到全局梯度强度
   2. 统计梯度的直方图
   3. 计算出高阈值
   4. 计算忽底阈值

   1. 高斯模糊
   2. 计算梯度幅值和方向
   3. 非最大值抑制
   4. 双阀值
   5. 滞后边界跟踪


#### Radon 是怎么估计倾斜角度的。
1. 计算其曲线的积分，简单的说就是，每一个图像选择角度的水平投影。


### [特征提取 有哪些特征](https://lidongxuan.github.io/blog/feature) 
+ Hog cell block 步长 计算梯度 让其方向
+ LBP 多少个区间 多少个维 等价模式 58 分成的区间数目 光照
+ Haar 矩特征 adboost 提取有效特征

6. [SHFI SURF FAST BRISK ORB](http://blog.jobbole.com/83919/)
   

   
7. SVM分类器 SVM的推导 
    这个的推导过程，以及KKT条件之类的。



10. Qt的基本内容，WebSocket 的通信过程。 
qt的信号与槽 
WebSocket 传递的json 浏览器通过json解析知道图片的位置，以及传递的数据。