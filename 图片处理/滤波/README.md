# 双边滤波
```
void cv::bilateralFilter (InputArray src, 
                          OutputArray dst, 
                          int d, 
                          double sigmaColor, 
                          double sigmaSpace, 
                          int borderType = BORDER_DEFAULT)
```
# 普通滤波
```
void cv::blur (InputArray src, 
               OutputArray dst, 
               Size ksize, 
               Point anchor = Point(-1,-1), 
               int borderType = BORDER_DEFAULT)
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
