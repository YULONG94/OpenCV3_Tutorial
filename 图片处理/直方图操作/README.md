# 反向投影
```
void cv::calcBackProject (const Mat * images, 
                          int nimages, 
                          const int * channels, 
                          InputArray hist, 
                          OutputArray backProject, 
                          const float ** ranges, 
                          double scale = 1, 
                          bool uniform = true)

void cv::calcBackProject (const Mat * images, 
                          int nimages, 
                          const int * channels, 
                          const SparseMat & hist, 
                          OutputArray backProject, 
                          const float ** ranges, 
                          double scale = 1, 
                          bool uniform = true)

void cv::calcBackProject (InputArrayOfArrays images, 
                          const std::vector< int > & channels, 
                          InputArray hist, 
                          OutputArray dst, 
                          const std::vector< float > & ranges, 
                          double scale)
// 反向投影图计算函数，可以通过统计物体直方图特征信息来分析图像
// 是一种比较原始的利用特征寻找图片中物体的方法
// 虽然这种方法能够在物体大小和旋转改变的情况下适用，但是却不适合图片中有其他类似的物体

//举个例子
int nc[] = { 0, 0 };
Mat src = imread("D:/data/test01.png");
imshow("源图像", src);
Mat src_test = imread("D:/data/test0.png");
imshow("测试图像", src_test);

// 将源图像转化到HSV空间中 
Mat src_hsv;
cvtColor(src, src_hsv, CV_BGR2HSV);
// 截获H通道
Mat hue = Mat(src_hsv.size(), src_hsv.depth());
mixChannels(&src_hsv, 1, &hue, 1, nc, 1);
int bins = 100;

float range[] = {0, 180};
const float *histranges = { range };
int hist_w = 400;
int hist_h = 400;
int bin_w = hist_w / bins;
Mat h_hist;
Mat histImage(hist_w, hist_h, CV_8UC3, Scalar::all(0));
calcHist(&hue, 1, 0, Mat(), h_hist, 1, &bins, &histranges, true, false);
normalize(h_hist, h_hist, 0, 255, NORM_MINMAX, -1, Mat());

Mat backProjectionImg;
calcBackProject(&hue, 1, 0, h_hist, backProjectionImg, &histranges, 1, true);
imshow("源图反向投影", backProjectionImg);

Mat backProjectionImg2;
Mat src_test_hsv;
cvtColor(src_test, src_test_hsv, CV_BGR2HSV);
Mat hue_test = Mat(src_test_hsv.size(), src_test_hsv.depth());
mixChannels(&src_test_hsv, 1, &hue_test, 1, nc, 1);

calcBackProject(&hue_test, 1, 0, h_hist, backProjectionImg2, &histranges, 1, true);
imshow("测试图反向投影", backProjectionImg2);

waitKey();
```
上面的例子是我自己测试时用的，也可以查看一个博客[calcBackProject 直方图反射函数](https://blog.csdn.net/kakiebu/article/details/79522547)
# 直方图计算
```
void cv::calcHist (const Mat * images, 
                   int nimages, 
                   const int * channels, 
                   InputArray mask, 
                   OutputArray hist, 
                   int dims, 
                   const int * histSize, 
                   const float ** ranges, 
                   bool uniform = true, 
                   bool accumulate = false)
                   
void cv::calcHist (const Mat * images, 
                   int nimages, 
                   const int * channels, 
                   InputArray mask, 
                   SparseMat & hist, 
                   int dims, 
                   const int * histSize, 
                   const float ** ranges, 
                   bool uniform = true, 
                   bool accumulate = false)

void cv::calcHist (InputArrayOfArrays images, 
                   const std::vector< int > & channels, 
                   InputArray mask, 
                   OutputArray hist, 
                   const std::vector< int > & histSize, 
                   const std::vector< float > & ranges, 
                   bool accumulate = false)
// 计算图片直方图，上面的例子也用到了这个函数
```
# 比较直方图
```
double cv::compareHist (InputArray H1, 
                        InputArray H2, 
                        int method)

double cv::compareHist (const SparseMat & H1, 
                        const SparseMat & H2, 
                        int method)
// 用于比较两个直方图之间的差异，有其中比较方法可选
```
# EMD计算
```
float cv::EMD (InputArray signature1, 
               InputArray signature2, 
               int distType, 
               InputArray cost = noArray(), 
               float * lowerBound = 0,
               OutputArray flow = noArray())
// 俗称搬土距离，类似于计算直方图差距的函数，都是为了获得两幅图像的差异程度
```
# 直方图均衡化
```
void cv::equalizeHist (InputArray src, 
                       OutputArray dst)

// 为了提高图像的对比度而进行如下操作
// 计算src源像的直方图，注意需要源像是灰度图
// 直方图归一化，使得所有pin值加起来是255
// 计算直方图的积分图作为查找表
// 根据查找表进行图像转化
```
# wrapperEMD
```
float cv::wrapperEMD (InputArray signature1, 
                      InputArray signature2, 
                      int distType, 
                      InputArray cost = noArray(), 
                      Ptr< float > lowerBound = Ptr< float >(), 
                      OutputArray flow = noArray())

// 个人愚见，作用和EMD（废话）
```
