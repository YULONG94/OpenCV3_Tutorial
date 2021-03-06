# 重映射
```
void cv::remap (InputArray src, 
                OutputArray dst, 
                InputArray map1, 
                InputArray map2, 
                int interpolation, 
                int borderMode = BORDER_CONSTANT, 
                const Scalar & borderValue = Scalar())
// src:输入源像
// dst:输出图像
// map1:x坐标的转化表（CV_16SC2 , CV_32FC1, or CV_32FC2）
// map2:y坐标的转化表
// 如果map1的形式是(x,y)，那么map2可以是None
// interpolation:差值方式, 下图中除了INTER_AREA
// borderMode:边界
// borderValue:边界的数值，默认为0
运算公式为：dst(x,y)=src(mapx(x,y),mapy(x,y))
```
![Image text](https://github.com/YULONG94/OpenCV3_Tutorial/blob/master/images/InterpolationFlags.PNG)
![Image text](https://github.com/YULONG94/OpenCV3_Tutorial/blob/master/images/BorderTypes.PNG)
```
//举个例子
Mat srcImage = imread("2.jpg");
Mat dstImage, map_x, map_y;
dstImage.create(srcImage.size(), srcImage.type());//创建和原图一样的效果图
map_x.create(srcImage.size(), CV_32FC1);
map_y.create(srcImage.size(), CV_32FC1);
//遍历每一个像素点，改变map_x & map_y的值,实现翻转180度
for (int j = 0; j < srcImage.rows; j++)
{
    for (int i = 0; i < srcImage.cols; i++)
    {
        map_x.at<float>(j, i) = static_cast<float>(i);
        map_y.at<float>(j, i) = static_cast<float>(srcImage.rows - j);
    }
}
//进行重映射操作
remap(srcImage, dstImage, map_x, map_y, INTER_LINEAR, BORDER_CONSTANT, Scalar(0, 0, 0));
imshow("重映射效果图", dstImage);  
waitKey();
```
# 仿射变换
OpenCV通过两个函数的组合使用来实现仿射变换：
1. 使用warpAffine来实现简单重映射
```
void cv::warpAffine (InputArray src, 
                     OutputArray dst, 
                     InputArray M, 
                     Size dsize, 
                     int flags = INTER_LINEAR, 
                     int borderMode = BORDER_CONSTANT, 
                     const Scalar & borderValue = Scalar())
```
2. 使用getRotationMatrix2D来获得旋转矩阵或者getAffineTransform来获得仿射变化矩阵
```
Mat M1=getAffineTransform(const Point2f* src, const Point2f* dst)
Mat M2=getRotationMatrix2D (CvPoint2D32f  center,double angle,double scale)
```
注意：
仿射变换指的是一个向量空间进行一次线性变换并接上一个平移，变换为另一个向量空间的过程。
图像进行仿射变换后，有以下几个特点：
二维图形之间的相对位置关系保持不变，平行线依旧是平行线，且直线上的点的位置顺序保持不变。
一个任意的仿射变换都可以表示为乘以一个矩阵（线性变换）接着再加上一个向量（平移）的形式。
```
// 举个例子：
Mat src = imread("lol9.jpg");
Mat dst_warp, dst_warpRotateScale;
Point2f srcPoints[3];//原图中的三点  
Point2f dstPoints[3];//目标图中的三点  

//第一种仿射变换的调用方式：三点法
//三个点对的值,上面也说了，只要知道你想要变换后图的三个点的坐标，就可以实现仿射变换  
srcPoints[0] = Point2f(0, 0);
srcPoints[1] = Point2f(0, src.rows - 1);
srcPoints[2] = Point2f(src.cols - 1, 0);
//映射后的三个坐标值
dstPoints[0] = Point2f(0, src.rows*0.3);
dstPoints[1] = Point2f(src.cols*0.25, src.rows*0.75);
dstPoints[2] = Point2f(src.cols*0.75, src.rows*0.25);

Mat M1 = getAffineTransform(srcPoints, dstPoints);//由三个点对计算变换矩阵  
warpAffine(src, dst_warp, M1, src.size());//仿射变换  


//第二种仿射变换的调用方式：直接指定角度和比例                                          
//旋转加缩放  
Point2f center(src.cols / 2, src.rows / 2);//旋转中心  
double angle = 45;//逆时针旋转45度  
double scale = 0.5;//缩放比例  

Mat M2 = getRotationMatrix2D(center, angle, scale);//计算旋转加缩放的变换矩阵  
warpAffine(dst_warp, dst_warpRotateScale, M2, src.size());//仿射变换  

imshow("原始图", src);
imshow("仿射变换1", dst_warp);
imshow("仿射变换2", dst_warpRotateScale);
waitKey(0);
```
# 透视变换
```
void cv::warpPerspective (InputArray src, 
                          OutputArray dst, 
                          InputArray M, 
                          Size dsize, 
                          int flags = INTER_LINEAR, 
                          int borderMode = BORDER_CONSTANT, 
                          const Scalar & borderValue = Scalar())
```
透视变换的API和仿射变换的API很像，原理也相同，不同的只是由之前的三点变为四点法求透视变换矩阵，变换矩阵也由2x3变为3x3。
获得转化变换矩阵是通过getPerspectiveTransform()
> Mat cv::getPerspectiveTransform (const Point2f src[], const Point2f dst[])
```
//举个例子
Mat src, dst;
src = imread("C:/Users/59235/Desktop/image/girl5.jpg");
if (!src.data) {
	printf("could not load image...\n");
	return -1;
}
namedWindow("original image", CV_WINDOW_AUTOSIZE);
imshow("original image", src);
Mat dst_warp, dst_warpRotateScale, dst_warpTransformation, dst_warpFlip;
Point2f srcPoints[4];//原图中的四点 ,一个包含三维点（x，y）的数组，其中x、y是浮点型数
Point2f dstPoints[4];//目标图中的四点  
srcPoints[0] = Point2f(0, 0);
srcPoints[1] = Point2f(0, src.rows);
srcPoints[2] = Point2f(src.cols, 0);
srcPoints[3] = Point2f(src.cols, src.rows);
//映射后的四个坐标值
dstPoints[0] = Point2f(src.cols*0.1, src.rows*0.1);
dstPoints[1] = Point2f(0, src.rows);
dstPoints[2] = Point2f(src.cols, 0);
dstPoints[3] = Point2f(src.cols*0.7, src.rows*0.8);
Mat M1 = getPerspectiveTransform(srcPoints, dstPoints);//由四个点对计算透视变换矩阵  
warpPerspective(src, dst_warp, M1, src.size());//仿射变换  
imshow("perspective transformation1（四点法）", dst_warp);
waitKey(0);
```
# 极坐标变换（考虑到基本不用，所以就不研究了）
```
void cv::warpPolar (InputArray src, 
                    OutputArray dst, 
                    Size dsize, 
                    Point2f center, 
                    double maxRadius, 
                    int flags)
void cv::cvLinePolar(const CvArr * src, 
                    CvArr * dst, 
                    CvPoint2D32f center, 
                    double maxRadius, 
                    int flags)
void cv::cvLogPolar (const CvArr * src, 
                    CvArr * dst, 
                    CvPoint2D32f center, 
                    double maxRadius, 
                    int flags)
```
