# ShapeTransformation

- 本项目实现形状转换的功能，从两张图片中捕获形状并基于隐函数在两张图片的形状之间进行线性插值，得到中间形状（参考论文：Turk G, O'brien J F. Shape transformation using variational implicit functions[M]//ACM SIGGRAPH 2005 Courses. 2005: 13-es.）
- ![meeting_01](./README.assets/meeting_01.gif)
- 项目结构
  - main.cpp：程序入口
  - algorithm/：算法模块
    - ImageProcess.hpp：图像处理文件，包括3个图像处理函数，作用为从图片文件路径得到图片轮廓边界，并得到求解隐函数未知数需要的边界约束和法向约束
    - ImplicitFuntion.hpp：隐函数文件，包括隐函数未知数求解、隐函数值求解和隐函数零值点求解的功能
    - PointProcess.hpp：点处理文件，包括二维的凸包和凹包算法，以及将图片中点转换为OpenGL三维空间中立体点的转换算法
    - LinearSystem.hpp：线性方程组求解算法
  - settings/：设置模块
    - Shader.h：着色器设置文件
    - Camera.h：OpenGL的相机设置文件
    - Setting.hpp：GLFW回调函数设置
  - resources/：资源模块
    - *.vert：顶点着色器文件
    - *.frag：片段着色器文件
  - lib/：额外依赖模块
    - imgui/
    - glad.c
- 宏定义说明
  - main.cpp
    - WRITE_MODE：程序分为读模式和写模式，需要进行读和写两个过程。第一步，宏定义了WRITE_MODE时，程序会进行图像处理，并将图像设定像素处的隐函数值写入文本文档；第二步，宏未定义WRITE_MODE时（如将其定义为WRITE_MODEx），程序读取文本文档并在两个隐函数之间插值，将结果在OpenGL的窗口中显示
    - EDGE_MODE：程序分为点模式和点线模式。宏定义EDGE_MODE时，程序会在OpenGL显示前运行α-shape算法，把结果中的点云筛选之后将点和线一起显示；宏未定义EDGE_MODE时，程序直接显示结果中的点云
    - DATA_DEBUG：开启时输出数据调试信息
    - IMAGE_DEBUG：开启时输出图片调试信息
    - DIMENSION：显示在OpenGL中的数据维度
    - MAX_MATRIX_DIMENSION：约束求解时系数矩阵的最大维数，系数矩阵维数与DIMENSION以及约束数量有关
    - MAX_ALLOC_SIZE：程序中一段动态分配的内存的最大空间，单位为字节
    - OPENGL_SCALE：图像像素坐标值和OpenGL世界坐标之间的缩放倍数
  - algorithm/PointProcess.hpp
    - POINT_SIZE：OpenGL三维点大小
    - ALPHA：α-shape算法所用圆半径大小
  - algorithm/ImplicitFunction.hpp
    - STEP：隐函数值插值计算时的像素跨度
    - TOLERANCE：隐函数值的容差
  - algorithm/ImageProcess.hpp
    - AREA_LIMIT：processImage1()和processImage2()提取轮廓时的轮廓大小限制
    - EPSILON, LOW_THRESHOLD, HIGH_THRESHOLD, APERTURE_SIZE：cv::Canny()的参数
    - SAMPLE_NUM：processImage3()的采样点数目
    - OFFSET：processImage2()和processImage3()中法向约束点对边界约束点的偏移量