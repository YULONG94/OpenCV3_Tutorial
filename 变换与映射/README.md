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
// interpolation:差值方式, 下图中除了INTER_AREA
// borderMode:边界
// borderValue:边界的数值，默认为0
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
2. 使用getRotationMatrix2D来获得旋转矩阵或者getAffineTransform来获得仿射变化矩阵
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

# 极坐标变换
