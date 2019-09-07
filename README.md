# Laser-line-extraction
The project is camera and Laser Emitter to measure for person.The laser wavelength is 850nm
简介：这个项目是线状激光发射器加摄像头用来测量人的距离，包括前后距离和左右距离，有个前提是能够识别到人的位置并且给出坐标，然后我在人的身上识别提取出
激光线的位置。以此来判断人的具体坐标。
现在完成的部分是在太阳光不太强的情况下可以提取出在人身上的激光线。
使用的器材是，1：只能通过850nm波段或者只能通过850nm波段和可见光波段的摄像头。
            2:500mw的850nm的线状激光发射器。


思路：
            基于论文ROI区域的查找方法，首先我们的激光是线状的，水平发射出去，如果在太阳光不太强的情况下，那么我们的激光在每一列应该是最亮的点，也就是
            灰度值最大，这样我们就可以遍历每一列找出灰度值最大的点。
            但是由于激光打出去照射在人的身上会形成光斑，所以我们可以把每一列相同的最大的点只取最后一个，这样就可以解决光斑的问题，不需要再用灰度重心法，或者极值法。
            在实际应用中可能光斑比较分散，导致光斑的上边沿和下边沿都有分布每列最大灰度值的点，所以还有设置一个高度是20像素点的区域，要用这个区域来遍历每行，最后统计哪一行里面包含的最大灰度值的点最多，就说明激光线在这个区域的可能性最大，最后取平均值确定激光线的位置。
            虽然方法比较简单，但是却比较精准实用，原来尝试过三个方法：
            一：阈值加sobel卷积和霍夫变换
	此方法步骤是：
	1、灰度处理。
	2、sobel算子卷积。
	3、阈值处理
	4、霍夫变换找直线。
            方法用自定义的sobel算子效果还算好一点。
            但是sobel算子和激光线的光斑终究会把激光线变得弯曲，弯曲之后会和霍夫变换相矛盾，还是无法检测的准确。
            二：基于hessian矩阵的steger算法
            此算法利用的是hessian矩阵的各向异性，找出在X方向和Y方向的特征向量值，最后比较哪个大就用哪个特征向量作为法线的方向，最后用steger求出中心点，也可以达到亚像素级别。
            这种方法在室内用，检测光斑用着还可以，但是对于漫反射或者室外效果不好。
            三：使用大阈值加形状检测来识别光斑，激光线，这种方法适用的场景更加单一，必须用阈值把除激光线以外的东西全通过大阈值（最少200），然后二值化处理掉，只留下激光，然后用findcounts检测出轮廓，最后图像拟合，画出形状，求出周长，算中心点。
            
            
            总结：这三种方法都不好用，当然我的最前面的方法在强光下也无法使用，而且还必须检测到人的坐标的前提下
            还有待完善，还请大家指点。
