# KalmanFilter
Which is the module of apollo used the ExtendedKalmanFilter?
Apollo的那个模块使用了扩展卡曼滤波？

卡尔曼滤波是一种高效率的递归滤波器(自回归滤波器), 它能够从一系列的不完全包含噪声的测量(英文:measurement)中，估计动态系统的状态，然而简单的卡尔曼滤波必须应用在符合高斯分布的系统中。也就是说卡尔曼滤波第一是递归滤波，其次KF用于线性系统。
但经过研究和改进，出现了很多卡尔曼，如EKF（extended kalman filter）扩展卡尔曼，UKF（Unscented Kalman Filter）无迹卡尔曼等等。
而我们就来研究EKF，而EKF的中心思想就是将非线性系统线形化后再做KF处理。

为什么要用EKF？
KF的假设之一就是高斯分布的x在预测后仍服从高斯分布，高斯分布的x变换到测量空间后仍服从高斯分布。
可是，假如F、H是非线性变换，那么上述条件则不成立。

既然非线性系统不行，那么很自然的解决思路就是将非线性系统线性化。

对于一维系统，采用泰勒一阶展开即可得到：

f(x)≈f(μ)+∂f(μ)∂x(x−μ)

对于多维系统，仍旧采用泰勒一阶展开即可得到：

T(x)≈f(a)+(x−a)TDf(a)
其中，Df(a)是Jacobian矩阵。

而在Apollo中，perception模块中在障碍检测中的融合部分用到了卡尔曼滤波，但这里对卡尔曼滤波的公式作了变形。
在更新噪声协方差时标准卡尔曼为：
P=（I-K * H） * P_
而在Apollo中：
P=(I-K * H) * P_ * (I- K * H).transpose + K * R * K.transpose

Apollo中使用卡曼滤波的具体模块为perception/obstacle/camera/filter和perception/obstacle/lidar/tracker/hm_tracker
