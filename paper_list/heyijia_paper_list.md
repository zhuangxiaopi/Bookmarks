### **基础知识材料**

- 2018年, joan sola 大神, A micro Lie theory for state estimation in robotics. 系统讲述李代数，非常棒。点击可以进他的主页，他写过非常多的笔记材料以及代码，ekf slam 工具箱啥的，各类笔记材料值得入门者反复读。科研也有非常棒的论文，TRO, IROS, ICRA 不胜枚举，我辈楷模。
- 2017年, joan sola 大神, Quaternion kinematics for the error-state Kalman ﬁlter. 四元数和 error state ekf 系统百科全书，当初学 vio 就靠他入门。
- 2000年，book, 矩阵分析，Matrix Analysis and Applied Linear Algebra. 越到 SLAM 后期，数学基础知识越需要。这本书是研一那会老师上矩阵分析课的教材，非常非常棒，跟 MIT 那个有得一拼。再次看到这个书名，是在预积分的参考文献里，SVO 那个预积分用到矩阵零空间的基等一些性质就是引用的这本书。

## **VIO 系统**

## **VIO 初始化和外参数标定**

该部分主要是 VIO系统中初始参数的确定，如相机尺度，系统初始速度，重力方向，imu bias，甚至相机和 imu 之间的外参数等等。

首先是闭式求解的方法，三篇论文一脉相承，Martinelli 作为二作和一作。

- 2011 年 ICRA ，Closed-Form Solution for Absolute Scale Velocity Determination Combining Inertial Measurements and a Single Feature Correspondence. 闭式求解相机尺度，只需要三帧和一个特征点。
- 2014 年 IJCV，closed-form solution of visual-inertial structure from motion，也是闭式求解，但是求解过程中忽略了 gyro bias 和 acc bias 的影响，因此，该方法不太实用，在实际系统中也没被采用过（17年该作者作为二作的一篇RAL说的）。
- 2017 年 RAL，Simultaneous State Initialization and Gyroscope Bias Calibration in Visual Inertial Aided Navigation，作者对上篇论文所述方法受 bias 的影响进行了分析，发现 acc bias 对系统影响不那么大，但是 gyro bias 影响较大，所以在 14 年论文的基础上提出了加入了标定 gyro bias 的方法。
  **优化迭代求解的方法**
- 2017 年 RAL，Visual-Inertial Monocular SLAM with Map Reuse. ORBSLAM VIO 论文，主要是利用 IMU 预积分和单目 ORBSLAM 估计的姿态之间构建约束，从而迭代求解 IMU 初始状态所有参数（甚至包括acc bias），不包括外参数的标定。
- 2017 年 TASE，Monocular Visual–Inertial State Estimation With Online Initialization and Camera–IMU Extrinsic Calibration. 沈老师组论文，sfm 求解相机姿态和 gyro 积分构建rotation约束，从而求解相机imu之间的旋转外参数，然后固定rotation，求解其他参数，如重力方向，速度，外参数平移，特征深度等。论文中直接构建一个最小二乘对上述参数进行优化求解。
- 2017 年 IROS，Robust Initialization of Monocular Visual-Inertial Estimation on Aerial Robots. 这篇大家比较熟悉，相对于上篇直接丢到误差函数进行优化求解的方法，沈老师他们借鉴了 ORBSLAM VIO 的做法，分步求解gyro bias, 尺度，速度，重力方向等参数。这篇论文和 ORBSLAM VIO 不同之处在于，它在初始化过程中不考虑 acc bias 的影响，因为通常 acc bias 相对于重力加速度不太可观，作者在论文中用实验表明，初始化过程中，只有当系统有足够的旋转时（超过30度），acc bias 才能收敛的比较好。反过来，如果初始化过程中忽略 acc bias, 通常 acc bias 对单目相机尺度的估计影响是在一个可接受范围内。
- 2018 年 ICRA，Online Initialization and Automatic Camera-IMU Extrinsic Calibration for Monocular Visual-Inertial SLAM. 求解VIO初始化过程中所有的参数，该论文在 ORBSLAM VIO 的框架下加入了外参数的标定。外参数旋转和平移的计算则参考沈老师的他们的做法。
- 2017 年 IROS, Inertial-Based Scale Estimation for Structure from Motion on Mobile Devices. 上面几篇都是基于 IMU 预积分的, 把短时间内的加速度什么的积分起来, 然后和视觉算的姿态构建误差, 优化出那些变量. 这篇论文不一样, 它是将相机姿态转换成角速度和加速度,和 imu 测量值去构建误差. 并且提出了频域对齐的方法, 值得一读.

## **时间戳标定**

- 2018 年 ECCV, Modeling Varying Camera-IMU Time Offset in Optimization-Based Visual-Inertial Odometry 沈老师组博士 yongen lin 的论文. 基于VINS-Mono的论文. 该论文认为imu 和 cam之间不是时间同步的, 时间延迟 td 也不是常数, 而是一个时变的变量. 可以认为作者把这个时间延迟当做了一个 imu 的 bias, 每次迭代优化出时间 td, 都会将 camera 的姿态进行补偿, 从而把时间因此耦合到误差项进行优化. 这是在腾讯 AI Lab 的工作, 代码没有开源.
- 2018 年 IROS, Online Temporal Calibration for Monocular Visual-Inertial Systems. 沈老师他们最新 VINS-Mono 代码里已经集成了这个时间戳标定的代码, 算法假设 imu 和 cam 之间的延迟是常数的, 和以往将相机姿态利用速度和角速度乘以时间差进行补偿不同, 沈老师他们设计的非常巧妙 (比上一篇感觉更优雅). 将 imu 和相机之间的时间延迟, 变成图像平面特征检测的位置的延迟, 这样就简化了整个误差函数.

## **自监督在线标定**

- 2013 年 IVS，Self-supervised Calibration for Robotic Systems. 标定工具 Kalibr 对应的论文。这篇论文非常值得一读，将传感器内外参数和机器人状态一起在线估计，并详细推导和描述了什么时候用测量数据自动更新估计的传感器参数。论文从最小二乘出发，描述了高斯牛顿估计所有参数的过程，并指出了数值计算不稳定会影响信息矩阵可观性的解决方法（即对特征值小于一定阈值的维度不进行更新）。最后提出了使用信息熵来判断新的测量数据是否用来更新内外参数的算法流程（直观理解：判断在新数据条件下待估计参数的协方差的秩相比之前数据条件下待估计参数的协方差的秩是否下降了一定阈值）。这篇论文中提到的可观性，信息矩阵，以及他的在线更新方法都很值得一读。同时，作者在ETH的硕士论文以及truncated_svd代码都在github上开放了。
- 2017 年 ICRA, Visual-inertial Self-calibration on Informative Motion Segments. 李名扬二作，思路和上篇差不错，也是判断信息熵协方差之类的决定参数是否更新。

## **可观性，一致性，不变性**

- 2012 年 ICRA, Consistency Analysis for Sliding-Window Visual Odometry. 其实sliding window VO早期的论文应该是2006年G.Sibley，但是个人感觉没Dong-Si 的论文写得好。Dong-Si 12 年的这篇论文把滑动窗口BA中雅克比不一致导致的可观性消失讲的清晰易懂，很容易就理解了FEJ的来头。八卦一下，Dong-Si 的个人主页，工作简历可以看出12年毕业后就没做 SLAM 了，搞数据库SQL啥的去了，联想到2012年在TRO上首次提出预积分的作者也做玩具去了，估计那会SLAM不好找工作吧。
- 2006 年，Observability and fisher information matrix in nonlinear regression. 说了什么是可观不可观, 以及和信息矩阵的联系。不是从滤波那个状态方程去推导的。
- 2012 年， msckf 那个实验室的技术报告，Observability-constrained Vision-aided Inertial Navigation. 之前的操作是 FEJ, 带来的问题是线性化点的误差比较大，这篇论文直接对可观性矩阵进行强制零空间约束，也就是强制使得传递矩阵的零空间维数不会减小。论文中对这个约束对雅克比矩阵的影响进行了推导，得到了一个闭式解，宾大的开源代码 mskcf_mono 对这个进行了实现。
- 2016 年 arxv，Barrau，An EKF-SLAM algorithm with consistency properties. 不变性理论：当给定数据和系统，作者认为系统每个时刻的协方差（不确定度）应该就确定了，和我们估计的各时刻状态量没关系。上面那些认为线性化点不同而造成协方差估计出现问题的论文并不本质，因此作者提出了不变性 EKF. 这篇论文从 2d ekf 这个简单的例子进行阐述，对理解该算法很有帮助，该算法确实找到了问题本质。作者的其他论文都较理论，下面那篇是他的 15 年的博士论文，感兴趣的可以继续深入。
- 2015 年 phd thesis, Non-linear state error based extended Kalman filters with applications to navigation. 作者Axel Barrau 和他导师一直沿着 invariant ekf 的路在走，发了很多关于不变性的文章。
- 2018 年 IROS，Invariant Smoothing on Lie Groups. Barrau 的小师弟把不变性做从 ekf 扩展到了优化的框架。
- 2018 年 Phd Thesis，Improving Visual-Inertial Navigation Using Stationary Environmental Magnetic Disturbances. 该作者也去过Cremers组，发了一篇鱼眼 LSDSLAM。不过博士论文跟那个没关系，他的第七章对一致性的几种方法（EFJ,OC-EKF, condition, robocent, invariant）进行了大致算法分类和梳理，并在自己系统中采用了 invariant ekf 。

## **其他**

- 2018 年 IROS, zhichao zhang, A Tutorial on Quantitative Trajectory Evaluation for Visual(-Inertial) Odometry. 轨迹评测的论文，Scaramuzza 组做的工作都非常扎实，细致。
- 2018 年 IROS, Arno Solin, VIO 数据集，用 iPhone 等设备采集的，对标 ARkit, ARcore, ADVIO: An Authentic Dataset for Visual-Inertial Odometry. 除了数据集贡献，作者也写过 VIO 论文， 18 年的 wacv 论文，PIVO: Probabilistic Inertial-Visual Odometry for Occlusion-Robust Navigation，17 年的 Inertial Odometry on Handheld Smartphones.
- 2010 年，Zero-Velocity Detection—An Algorithm Evaluation

## **VO 前端**

## **光照处理**

- 2017 年 ICRA, Illumination Change Robustness in Direct Visual SLAM ETH的论文，针对直接法对光照不鲁棒的问题，调研了十来种处理方法，有些参考意义。

## **特征匹配**

- 2018 年 IROS, Geometric-based Line Segment Tracking for HDR Stereo Sequences. PL-SLAM 作者做的一个关于双目序列图像的直线匹配工作. 传统的直线匹配都是提取描述子之类的, 但是对于序列图像, 图像之间的运动变化比较小, 可以利用直线在图像里的几何性质来完成匹配(如直线在图像里的角度). 作者提出的方法不依赖图像特征, 匹配速度也比 LBD 有所提高. 直线长度差不多, 直线角度差不错, 对于直线中心点也加了一个极线条件. 将这些条件组合在一起构建了一个误差函数, 进行L1优化, 每个直线只找到一个对应的匹配项,使得匹配误差最小.

## **VO 后端**

- 2009 年，SBA: A Software Package for Generic Sparse Bundle Adjustment. g2o 里的 LM 算法流程参考的就是这篇论文的实现方法，老的经典论文还是值得反复读。
- PCG代码实现参考资料：1. 2018 年 master thesis，Efficient Optimization For Robust Bundle Adjustment. TUM 的硕士论文，里面有 PCG 等算法的伪代码流程。2. The Preconditioned Conjugate Gradient Method 这是一个课程的note, 伪代码流程非常的清晰，如果想自己实现代码，可以照着这个复现。对应的在网上找了别人写的 PCG 函数，点这里。
- 2009 年，A Brief Introduction to The Conjugate Gradient Method. 如果你对 CG 方法原理不是很熟，可以看看这个 8 页的pdf, 就能该方法的思想和出发点了解个大概。
- 1994 年， An Introduction to the Conjugate Gradient Method Without the Agonizing Pain. 当然如果你想进一步知道 CG 和 PCG，这篇白话就非常值得一读。
- 2004 年 book, Iterative methods for sparse linear systems. 第九章讲了PCG，这本书引用上万，估计也是圣经。
- 2014 年 ECCV, Christopher Zach, Robust Bundle Adjustment Revisited. 这篇论文主要讲了 robust cost function 在代码里实现的四种方式。这篇论文是参加 ECCV 2018 的 workshop 时，谷歌的大佬们推荐的，论文中提到的 Triggs Correction 方法正是 ceres 采用的。同时，我也在网上找到了这篇论文对应的代码(github链接).

## **激光 SLAM**

## **基础知识**

- 推荐两个课程： 2013年，弗莱堡大学 Cyrill Stachniss 教授的 SLAM Course， youtube 有课程视频，网上pdf 也有。Coursera 上 Robotics: Estimation and Learning， 这个课就比较简短一些，但是非常适合新手快速入门。
- Occupancy Grid Map. 地图是激光 slam 系统的核心，通常激光 slam 都采用 logodds 算法对栅格地图进行概率更新。知乎上有个人对 Coursera 上课笔记进行了总结，写得非常好，对公式的推导很简洁，完整详细的推导得看 Stachniss 教授的课视频，课件。

## **2D SLAM**

- 2015 年 master thesis. Precise indoor localization for mobile laser scanner. 这篇硕士论文整理了比较了 hector, gmapping, karto 三种激光 slam 方法，论文实验比较的非常详细。各自优缺点以及存在的问题都大致总结了。
- 2009 年 ICRA, Edwin olson. Real-Time Correlative Scan Matching. 这篇 Scan Matching 的论文是 catographer, karto 等基于图优化的 SLAM 算相对 pose 的基础。另外，AprilTag 也是作者Prof. Olson 2011 年的论文。
- 2011 年 SSRR , Stefan Kohlbrecher. A Flexible and Scalable SLAM System with Full 3D Motion Estimation. 这篇论文是 hector slam 的 scan match 方法，hector 代码里有对应的代码实现，很清晰简洁，如果你熟悉高斯牛顿的话，能很快上手。cartographer 里的real time scan match 就是采用的这篇论文的计算方法。
- 2012 年 IROS，大佬 Wolfram Burgard 加持，On the Position Accuracy of Mobile Robot Localization based on Particle Filters Combined with Scan Matching. 采用 amcl + pl-icp。
- 2016 年， High-accuracy vehicle localization for autonomous warehousing. amcl + pl icp + DFT。
- 2008 年 IROS，Andrea Censi，csm, 以及 icp 的协方差估计。

## **码盘内参数以及激光外参数标定**

- 2013 年 TRO，Andrea Censi，Simultaneous calibration of odometry and sensor parameters for mobile robots. 这篇论文将码盘的内外参数一起标定了，作者提供了代码。论文方法简单，读论文就可以，这里想隆重介绍下作者，Censi，做啥都牛，喜欢各种开源自己的代码，激光 icp 方面出了csm。 优化方面也和 gtsam 那些大佬有诸多合作，也和木吒（Scaramuzza）合作挺多，总之，论文和工程都贡献不少, 感兴趣的看一下他的简历，能发现不少好工作。

## **激光相机外参数标定**

- 2004 年 Qilong Zhang, Extrinsic Calibration of a Camera and Laser Range Finder (improves camera calibration). 这是改领域较早且非常有名的一篇论文，开放了相应的 matlab 代码。标定方法使用一个标定板，相机和激光都能看到这个标定板，相机能实时计算出到标定板的姿态，标定板的平面方程能事先假设，然后利用激光落到里这个标定板平面（点在平面里），构建约束激光到相机的外参数约束，从而求解外参数。这个方法每一次观测实际上只能提供两个自由度的约束（因为平面里的激光点共线了，相当于只利用了两个激光端点构建约束），因此需要不同位置拍摄多组观测，最后一起求解。代码1, 代码2.
- 2017 年 ICRA, Wenbo Dong, A Novel Method for the Extrinsic Calibration of a 2D Laser Rangefinder and a Camera. 这篇论文对历年的经典外标定算法进行了简单的点评，并提出了自己的单帧观测就能计算外参的算法。该算法利用两个相交平面（不需要是直角相交）来标定，虽然不要求直角，作者要求两个平面是两个三角形构建的（当然制作标定板的时候，打印边框成三角形的形式，然后折纸飞机一样折一下就行了），这样标定板和相机之间就有多个平面了（标定板本身的两个平面，激光线和标定板三角边也会相交这里又提供了两个平面）一共六个约束，所以单帧可解外参数。作者实验也验证了他的方法比04年的那个精度要高。当都采用20组观测进行标定时，他的平移误差4mm, 04年的那个12mm左右。虽然这个方法单帧可以出结果，但是他每次都依赖激光和标定板那几条直线的交点，一旦交点不准，比如激光传感器的噪声比较大，如1cm时，单帧标定误差好几cm。作者也建议使用多帧一起标定。感觉和04年那篇差不多。
- 2015 年 ICRA, Ruben Gomez-Ojeda, Extrinsic Calibration of a 2D Laser-Rangefinder and a Camera based on Scene Corners. 这篇文章放在这主要是因为作者是 PL-SLAM 作者，他也做过激光视觉标定。他的方法不需要标定板，直接利用三个垂直面的三条线的相交点确定三条直线（如果直接用 lsd 检测，可能三条直线不会相交于一点，误差较大），根据激光和直线的相交点构建点在平面上的方程。这个方法感觉比较复杂，容错率比较低。论文开放了matlab源码，在二作的github页面上。
- 2018 年 IROS，Lipu Zhou, isam 作者 Kaess 的学生，Automatic Extrinsic Calibration of a Camera and a 3D LiDAR using Line and Plane Correspondences. 这篇是 3D 激光雷达和相机之间外参数。这篇也是利用多个线和平面约束，直接单次观测可以出外参数，不需要把传感器动来动去采集多组数据。
- 2017 年 ICCV，Zoltan Pusztai, Accurate Calibration of LiDAR-Camera Systems using Ordinary Boxes. 这篇还没读，先不评论。