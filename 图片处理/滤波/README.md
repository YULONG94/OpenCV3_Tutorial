# 双边滤波
```
void cv::bilateralFilter (InputArray src, 
                          OutputArray dst, 
                          int d, 
                          double sigmaColor, 
                          double sigmaSpace, 
                          int borderType = BORDER_DEFAULT)
d:过滤期间使用的各像素邻域的直径
sigmaColor:色彩空间的sigma参数，该参数较大时，各像素邻域内相距较远的颜色会被混合到一起，
           从而造成更大范围的半相等颜色
sigmaSpace:坐标空间的sigma参数，该参数较大时，只要颜色相近，越远的像素会相互影响
```
双边滤波器是一种可以保边去噪的滤波器。可以滤除图像数据中的噪声，且还会
保留住图像的边缘、纹理等（因噪声是高频信号，边缘、纹理也是高频信息，高
斯滤波会在滤除噪声的同时使得边缘模糊）
原理：在平坦区域，像素差值较小，对应值域权重接近于1，此时空域权重起主
要作用，相当于直接对此区域进行高斯模糊，在边缘区域，像素差值较大，值域
系数下降，导致此处核函数下降（因w=rd），当前像素受到的影响就越小，从而
保持了边缘的细节信息。
```
//举个例子
Mat src = imread("../data/1.jpg");
imshow("origin", src);
Mat result;
bilateralFilter(src, result, 25, 25 * 2, 25 / 2);
imshow("result", result);
waitKey();
```
# 普通滤波（均值滤波）
```
void cv::blur (InputArray src, 
               OutputArray dst, 
               Size ksize, 
               Point anchor = Point(-1,-1), 
               int borderType = BORDER_DEFAULT)
InputArray src: 输入图像 
OutputArray dst: 输出图像
Size ksize: 滤波模板kernel的尺寸，一般使用Size(w, h)来指定，如Size(3,3) 
Point anchor=Point(-1, -1): 锚点，处理的像素位于kernel的什么位置
                            默认值为(-1, -1)即位于kernel中心点
//例如
Mat g_srcImage= imread("lena.jpg");
Mat g_dstImage;
blur(g_srcImage, g_dstImage, Size(3, 3));
```
# 箱式过滤器
```
void cv::boxFilter (InputArray src, 
                    OutputArray dst, 
                    int ddepth, 
                    Size ksize, 
                    Point anchor = Point(-1,-1), 
                    bool normalize = true, 
                    int borderType = BORDER_DEFAULT)
```
# 高斯金字塔
```
void cv::buildPyramid (InputArray src, 
                       OutputArrayOfArrays dst, 
                       int maxlevel, 
                       int borderType = BORDER_DEFAULT)
```
# 膨胀
```
void cv::dilate (InputArray src, 
                 OutputArray dst, 
                 InputArray kernel, 
                 Point anchor = Point(-1,-1), 
                 int iterations = 1, 
                 int borderType = BORDER_CONSTANT, 
                 const Scalar & borderValue = morphologyDefaultBorderValue())
```
# 腐蚀
```
void cv::erode (InputArray src, 
                OutputArray dst, 
                InputArray kernel, 
                Point anchor = Point(-1,-1), 
                int iterations = 1, 
                int borderType = BORDER_CONSTANT, 
                const Scalar & borderValue = morphologyDefaultBorderValue())
```
# 卷积滤波
```
void cv::filter2D (InputArray src,
                   OutputArray dst, 
                   int ddepth, 
                   InputArray kernel, 
                   Point anchor = Point(-1,-1), 
                   double delta = 0, 
                   int borderType = BORDER_DEFAULT)
```
# 高斯滤波
```
void cv::GaussianBlur (InputArray src, 
                       OutputArray dst, 
                       Size ksize, 
                       double sigmaX, 
                       double sigmaY = 0, 
                       int borderType = BORDER_DEFAULT)
```
# 核
```
void cv::getDerivKernels (OutputArray kx, 
                          OutputArray ky, 
                          int dx, 
                          int dy, 
                          int ksize, 
                          bool normalize = false, 
                          int ktype = CV_32F)
```
# 核
```
Mat cv::getGaborKernel (Size ksize, 
                        double sigma, 
                        double theta, 
                        double lambd, 
                        double gamma, 
                        double psi = CV_PI *0.5, 
                        int ktype = CV_64F)
```
# 获得高斯核
```
Mat cv::getGaussianKernel (int ksize, 
                           double sigma, 
                           int ktype = CV_64F)
```
# getStructureeement
```
Mat cv::getStructuringElement (int shape, 
                               Size ksize, 
                               Point anchor = Point(-1,-1))
```
# 拉普拉斯
```
void cv::Laplacian (InputArray src, 
                    OutputArray dst, 
                    int ddepth, 
                    int ksize = 1, 
                    double scale = 1, 
                    double delta = 0, 
                    int borderType = BORDER_DEFAULT)
```
# 中值滤波
```
void cv::medianBlur (InputArray src, 
                     OutputArray dst, 
                     int ksize)
```
# 型态滤波
```
void cv::morphologyEx (InputArray src, 
                       OutputArray dst, 
                       int op, 
                       InputArray kernel, 
                       Point anchor = Point(-1,-1), 
                       int iterations = 1, 
                       int borderType = BORDER_CONSTANT, 
                       const Scalar & borderValue = morphologyDefaultBorderValue())
```
# 下采样
```
void cv::pyrDown (InputArray src, 
                  OutputArray dst, 
                  const Size & dstsize = Size(), 
                  int borderType = BORDER_DEFAULT)
```
# pyrMeanShiftFiltering
```
void cv::pyrMeanShiftFiltering (InputArray src, 
                                OutputArray dst, 
                                double sp, 
                                double sr, 
                                int maxLevel = 1, 
                                TermCriteria termcrit = TermCriteria(TermCriteria::MAX_ITER+TermCriteria::EPS, 5, 1))
```
# 上采样
```
void cv::pyrUp (InputArray src, 
                OutputArray dst, 
                const Size & dstsize = Size(), 
                int borderType = BORDER_DEFAULT)
```
# Scharr
```
void cv::Scharr (InputArray src, 
                 OutputArray dst, 
                 int ddepth, 
                 int dx, 
                 int dy, 
                 double scale = 1,
                 double delta = 0, 
                 int borderType = BORDER_DEFAULT)
```
# sepFilter2D
```
void cv::sepFilter2D (InputArray src, 
                      OutputArray dst, 
                      int ddepth, 
                      InputArray kernelX, 
                      InputArray kernelY, 
                      Point anchor = Point(-1,-1), 
                      double delta = 0, 
                      int borderType = BORDER_DEFAULT)
```
# Sobel
```
void cv::Sobel (InputArray src, 
                OutputArray dst, 
                int ddepth, 
                int dx, 
                int dy, 
                int ksize = 3, 
                double scale = 1, 
                double delta = 0, 
                int borderType = BORDER_DEFAULT)
```
# spatialGradient
```
void cv::spatialGradient (InputArray src, 
                          OutputArray dx, 
                          OutputArray dy, 
                          int ksize = 3, 
                          int borderType = BORDER_DEFAULT)
```
# sqrBoxFilter
```
void cv::sqrBoxFilter (InputArray _src, 
                       OutputArray _dst, 
                       int ddepth, 
                       Size ksize, 
                       Point anchor = Point(-1, -1), 
                       bool normalize = true, 
                       int borderType = BORDER_DEFAULT)
```
# 
