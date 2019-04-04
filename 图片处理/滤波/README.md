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

双边滤波器是一种可以保边去噪的滤波器。可以滤除图像数据中的噪声，且还会
保留住图像的边缘、纹理等（因噪声是高频信号，边缘、纹理也是高频信息，高
斯滤波会在滤除噪声的同时使得边缘模糊）
原理：在平坦区域，像素差值较小，对应值域权重接近于1，此时空域权重起主
要作用，相当于直接对此区域进行高斯模糊，在边缘区域，像素差值较大，值域
系数下降，导致此处核函数下降（因w=rd），当前像素受到的影响就越小，从而
保持了边缘的细节信息。

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

本质就是均值滤波
关于ddepth的设置，根据src的depth，接受的可以是下面情况
src.depth() = CV_8U, ddepth = -1/CV_16S/CV_32F/CV_64F
src.depth() = CV_16U/CV_16S, ddepth = -1/CV_32F/CV_64F
src.depth() = CV_32F, ddepth = -1/CV_32F/CV_64F
src.depth() = CV_64F, ddepth = -1/CV_64F
```
# 高斯金字塔
```
void cv::buildPyramid (InputArray src, 
                       OutputArrayOfArrays dst, 
                       int maxlevel, 
                       int borderType = BORDER_DEFAULT)
		       
src：输入图像
dst：输出所有层的图像
maxlevel：表示建立金字塔的层数
borderTyp：表示对边界的处理方式

//举个例子
Mat img = imread("../data/1.jpg");
vector<Mat> gpyramid;
buildPyramid(img, gpyramid, 4);
vector<Mat>::iterator it = gpyramid.begin();
vector<Mat>::iterator itend = gpyramid.end();
int i = 0;
for (; it < itend; it++)
{
	stringstream title;
	title << "第" << i << "层";
	namedWindow(title.str());
	imshow(title.str(), *it);
	i++;
}
waitKey();
return 0;
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
		 
src：输入图，可以多通道，深度可以CV_8U、CV_16U、CV_16S、CV_32F或CV_64F
dst：输出图，和输入图相同
kernel：结构元素，如果kernel=Mat()则为预设的3×3矩形，越大膨胀效果越明显
        可以通过getStructuringElement()来设计自己的结构。
anchor：原点位置，预设为结构元素的中点
iterations：执行次数，默认一次，执行越多效果越明显
//例子
Mat img = imread("../data/1.jpg");
Mat result;
dilate(img, result, Mat());
// k = getStructuringElement(MORPH_RECT,Size(5,5))
// dilate(img, result, k)
imshow("result", result);
waitKey();
return 0;
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
		
与膨胀的参数设置一样，例子也可以直接替换
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
		   
// 注意边界类型，默认的是反射层，也就是说填充边界以外的方式是通过反射
Mat img = imread("../data/1.jpg");
Mat result;
Mat kern = (Mat_<char>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
filter2D(img, result, -1, kern);
imshow("result", result);
waitKey();
return 0;
```
# 高斯滤波
```
void cv::GaussianBlur (InputArray src, 
                       OutputArray dst, 
                       Size ksize, 
                       double sigmaX, 
                       double sigmaY = 0, 
                       int borderType = BORDER_DEFAULT)
		       
ksize:高斯核大小，ksize.width和ksize.height可以不同，但是都必须为正的奇数
     （或者为0，此时它们的值会自动由sigma进行计算）
sigmaX:高斯核在x方向的标准差
sigmaY:高斯核在y方向的标准差（sigmaY=0时，其值自动由sigmaX确定（sigmaY=sigmaX）；
       sigmaY=sigmaX=0时，它们的值将由ksize.width和ksize.height自动确定）
// 例子
Mat img = imread("../data/1.jpg");
Mat result;
GaussianBlur(img, result, Size(5, 5), 0, 0);
imshow("result", result);
waitKey();
return 0;
```
# 获得过滤核
```
void cv::getDerivKernels (OutputArray kx, 
                          OutputArray ky, 
                          int dx, 
                          int dy, 
                          int ksize, 
                          bool normalize = false, 
                          int ktype = CV_32F)

通过预先设定来获得想要的过滤核，
前两个为接受输出的两个方向的核，
dx和dy为所设置的两个方向的阶数，一阶就表示像素的梯度（一阶导），二阶就是梯度的梯度（二阶导）
ksize为需要的核的大小
通过normalize的设置，可以获得归一化之后的核
这个和下面的几个函数都是为了方便设计过滤核

//例子：
Mat kx, ky;
getDerivKernels(kx, ky, 1, 1, 5, true);
cout << kx << endl;
return 0;
```
# 获得Gabor核
```
Mat cv::getGaborKernel (Size ksize, 
                        double sigma, 
                        double theta, 
                        double lambd, 
                        double gamma, 
                        double psi = CV_PI *0.5, 
                        int ktype = CV_64F)

这个核可以结合Filter2D滤波器进行使用，参数：
λ表示滤波的波长； 
θ表示Gabor核函数图像的倾斜角度； 
ψ表示相位偏移量，取值范围是-180~180； 
σ表示高斯函数的标准差； 
γ表示长宽比，决定这Gabor核函数图像的椭圆率。
```
想要具体了解每个参数的效果可以查看一个博客[Gabor滤波简介与Opencv中的实现及参数变化实验](https://blog.csdn.net/lhanchao/article/details/55006663)
# 获得高斯核
```
Mat cv::getGaussianKernel (int ksize, 
                           double sigma, 
                           int ktype = CV_64F)
			   
返回n行1列归一化的一维高斯系数，将两个一维高斯核相乘就获得二维的高斯核
结合Filter2D函数可以达到和GaussianBlur一样的效果
// 例子
Mat src = Mat::eye(10, 10, CV_8UC1);
Mat kernelx = getGaussianKernel(3, 0.8);
Mat kernely = getGaussianKernel(3, 0.8);
Mat kernel2d = kernelx * kernely.t();
Mat result1, result2;
filter2D(src, result1, -1, kernel2d);
GaussianBlur(src, result2, Size(3, 3), 0.8);
imshow("result2", result2);
imshow("result1", result1);
cout << result1 << endl;
cout << result2 << endl;
waitKey();
return 0;
```
# 其他的一些预设定核
```
Mat cv::getStructuringElement (int shape, 
                               Size ksize, 
                               Point anchor = Point(-1,-1))
			       
这个函数的第一个参数表示内核的形状，有三种形状可以选择。
矩形：MORPH_RECT;
交叉形：MORPH_CORSS;
椭圆形：MORPH_ELLIPSE;

第二和第三个参数分别是内核的尺寸以及锚点的位置。一般在调用erode以及dilate函数之前，
先定义一个Mat类型的变量来获得getStructuringElement函数的返回值。对于锚点的位置，
有默认值Point（-1,-1），表示锚点位于中心点。element形状唯一依赖锚点位置，其他情况下，
锚点只是影响了形态学运算结果的偏移。
// 例如：
Mat kernelx = getStructuringElement(MORPH_ELLIPSE, Size(5, 5));
cout << kernelx << endl;
```
# 拉普拉斯滤波
```
void cv::Laplacian (InputArray src, 
                    OutputArray dst, 
                    int ddepth, 
                    int ksize = 1, 
                    double scale = 1, 
                    double delta = 0, 
                    int borderType = BORDER_DEFAULT)
		    
Laplace函数实现的方法是先用Sobel 算子计算二阶x和y导数，再求和。
scale是缩放导数的比例常数，默认情况下没有伸缩系数；
delta是一个可选的增量，将会加到最终的dst中，同样，默认情况下没有额外的值加到dst中；

//例如：
Mat src = imread("../data/1.jpg");
Mat result;
Laplacian(src, result, -1, 3, 1.0, 0.0, 4);
imshow("result", result);
waitKey();
```
# 中值滤波
```
void cv::medianBlur (InputArray src, 
                     OutputArray dst, 
                     int ksize)
		     
//这个函数使用起来可选的参数比较少，不多做介绍，功能也可以顾名思义
//性能的话，可以说在处理噪点什么的有奇效，不过速度相当于线性的算子较慢

//例如：
Mat src = imread("../data/1.jpg");
Mat result;
medianBlur(src, result, 3);
imshow("result", result);
waitKey();
```
# 形态学变换
```
void cv::morphologyEx (InputArray src, 
                       OutputArray dst, 
                       int op, 
                       InputArray kernel, 
                       Point anchor = Point(-1,-1), 
                       int iterations = 1, 
                       int borderType = BORDER_CONSTANT, 
                       const Scalar & borderValue = morphologyDefaultBorderValue())
		       
参数方面没什么需要多做介绍的，除了int op这一项，接受以下情况：
0：MORPH_ERODE，就是erode
1：MORPH_DILATE，就是dilate
2：MORPH_OPEN，dst=open(src,element)=dilate(erode(src,element))称为开运算
3：MORPH_CLOSE，dst=close(src,element)=erode(dilate(src,element))称为闭运算
4：MORPH_GRADIENT，dst=morph_grad(src,element)=dilate(src,element)−erode(src,element)
		   称为形态学梯度
5：MORPH_TOPHAT，dst=tophat(src,element)=src−open(src,element)称为顶帽运算
6：MORPH_BLACKHAT，dst=blackhat(src,element)=close(src,element)−src称为黑帽运算
7：MORPH_HITMISS，击中击不中变换（只对CV_8UC1类型可用，可以用在二值图可以提取一些特定的形状）
//例如：
Mat src = imread("../data/1.jpg", 0);
Mat result;
Mat k = getStructuringElement(MORPH_RECT, Size(3, 3));
morphologyEx(src, result, MORPH_OPEN, k);
imshow("result", result);
waitKey();
```
# 下采样
```
void cv::pyrDown (InputArray src, 
                  OutputArray dst, 
                  const Size & dstsize = Size(), 
                  int borderType = BORDER_DEFAULT)
		  
这个其实在另一篇笔记中记录过了，重新写一下
// 下采样，图像变为原来的1/2，dstsize可以不设置
// 如果设置dstsize，一定需要满足|dstsize.width * 2 - src.cols| ≤ 2;|dstsize.height * 2 - src.rows| ≤ 2; 否则会报错

Mat A = imread("../data/1.jpg");
Mat C;
pyrDown(A, C);
imshow("A", A);
imshow("C", C);
waitKey();
```
# pyrMeanShiftFiltering
```
void cv::pyrMeanShiftFiltering (InputArray src, 
                                OutputArray dst, 
                                double sp, 
                                double sr, 
                                int maxLevel = 1, 
                                TermCriteria termcrit = TermCriteria(TermCriteria::MAX_ITER+TermCriteria::EPS, 5, 1))

均值漂移算法：一种通用的聚类算法，它的基本原理是：对于给定的一定数量样本，任选其中一个样本，以该样本为中心点划定一个圆形区域，求
取该圆形区域内样本的质心，即密度最大处的点，再以该点为中心继续执行上述迭代过程，直至最终收敛。可以利用均值偏移算法的这个特性，实
现彩色图像分割。这个函数严格来说并不是图像的分割，而是图像在色彩层面的平滑滤波，它可以中和色彩分布相近的颜色，平滑色彩细节，侵蚀
掉面积较小的颜色区域。

第一个参数src，输入图像，8位，三通道的彩色图像，并不要求必须是RGB格式，HSV、YUV等Opencv中的彩色图像格式均可；
第二个参数dst，输出图像，跟输入src有同样的大小和数据格式；
第三个参数sp，定义的漂移物理空间半径大小；
第四个参数sr，定义的漂移色彩空间半径大小；
//这两个参数越大，计算需要的时间也会更加久，效果也更明显
第五个参数maxLevel，定义金字塔的最大层数；
第六个参数termcrit，定义的漂移迭代终止条件，可以设置为迭代次数满足终止，迭代目标与中心点偏差满足终止，或者两者的结合；

// 例如：
Mat src = imread("../data/1.jpg");
Mat result;
pyrMeanShiftFiltering(src, result, 10, 10, 1);
imshow("result", result);
waitKey();

meanShift均值偏移算法主要对彩色图像进行平滑操作，为了达到分割的目的，需要借助另外一个漫水填充函数（floodFill）
这里先补充一这个函数的形式，具体的会在《各种各样的图像转换》笔记中进行介绍。

int cv::floodFill (InputOutputArray image, 
		   InputOutputArray mask, 
		   Point seedPoint, 
		   Scalar newVal, 
		   Rect * rect = 0, 
		   Scalar loDiff = Scalar(), 
		   Scalar upDiff = Scalar(), 
		   int flags = 4)
或者不使用mask：
int cv::floodFill (InputOutputArray image, 
		   Point seedPoint, 
		   Scalar newVal, 
		   Rect * rect = 0, 
		   Scalar loDiff = Scalar(), 
		   Scalar upDiff = Scalar(), 
		   int flags = 4)

```
# 上采样
```
void cv::pyrUp (InputArray src, 
                OutputArray dst, 
                const Size & dstsize = Size(), 
                int borderType = BORDER_DEFAULT)

// 与下采样相对
Mat A = imread("../data/1.jpg");
Mat B;
pyrUp(A, B);
imshow("A", A);
imshow("B", B);
waitKey();
```
# Scharr滤波器
```
void cv::Scharr (InputArray src, 
                 OutputArray dst, 
                 int ddepth, 
                 int dx, 
                 int dy, 
                 double scale = 1,
                 double delta = 0, 
                 int borderType = BORDER_DEFAULT)

对于小一点的核（3x3）而言，使用Sobel算子近似计算导数的缺点是精度比较低
Scbarr滤波器同sobel滤波器一样快，但是准确率更高，故当你利用3x3滤波器实现图像度量的时候应该使用Scharr滤波器

dx：x方向导数的阶数
dy：y方向导数的阶数
scale：放缩的尺度，默认为零
delta：漂移数值，默认为零

注意：经验证，dx和dy有且只有一个能为1，另一个必须为零，从而实现求这个方向上的一阶导数

//例如：
Mat src = imread("../data/1.jpg");
Mat result;
Scharr(src, result, -1, 1, 0, 1, 0);
imshow("result", result);
waitKey();
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

用分解的核函数对图像做卷积：
首先，图像的每一行与一维的核kernelX做卷积；
然后，运算结果的每一列与一维的核kernelY做卷积

ddepth支持如下情况：
src.depth() = CV_8U, ddepth = -1/CV_16S/CV_32F/CV_64F
src.depth() = CV_16U/CV_16S, ddepth = -1/CV_32F/CV_64F
src.depth() = CV_32F, ddepth = -1/CV_32F/CV_64F
src.depth() = CV_64F, ddepth = -1/CV_64F

//例如：
Mat src = imread("../data/1.jpg");
Mat result;
Mat kx = (Mat_<float>(1, 3) << 0, -1, 0);
Mat ky = (Mat_<float>(1, 3) << -1, 0, -1);
sepFilter2D(src, result, src.depth(), kx, ky, Point(-1, -1), 0, BORDER_DEFAULT);
imshow("result", result);
waitKey();


```
# Sobel滤波器
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
