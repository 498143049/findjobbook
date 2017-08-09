#工业环境下数码管的定位与识别
## opencv
1. 用了那些库
 + 空间转化 cvcolor 
 + 图像滤波 
 + 连通域 查找 findContours 轮廓检测
 + 图像形态学操作 morphologyEx
 + 寻找外接矩形  boundingRect
 + svm 训练 交叉验证 网格搜索
 + 局部二值化算法 
 + mser 
 + SWT
2. mat指针访问元素的方式 
//Mat 源代码分析
http://blog.csdn.net/honpey/article/details/9028059
 Mat 直接的复制只是浅拷贝，而深拷贝必须调用copyto 和clone 
 + C语言的云算子[] ptr 族 
 2. 迭代器 方法 
 3. 动态地址方法 at 族元素 适合随机访问 
 4. LUT函数 //是用来多线程架构

3. 大津阈值法 
   最佳阈值法，根据类间方法最大的原则，进行划分元素。
4. 标记分水岭算法
   如何计算标记 给定标记
   
5. 特征提取 有哪些特征 
   Hog cell block 步长
   LBP 多少个区间 多少个维 等价模式 58
   Haar 矩特征 adboost 提取有效特征

6. SHFI SURF FAST BRISK ORB
   //http://blog.jobbole.com/83919/
   
   
6. SVM分类器 SVM的推导
7. canny 算法的具体过程，如何做到自适应
8. Radon 是怎么估计倾斜角度的。
9. Qt的基本内容，WebSocket 的通信过程。 