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
// interpolation:差值方式
![Image text](https://github.com/YULONG94/OpenCV3_Tutorial/blob/master/images/InterpolationFlags.PNG)
// borderMode:边界
// borderValue:边界的数值，默认为0
```
![Image text](https://github.com/YULONG94/OpenCV3_Tutorial/blob/master/images/InterpolationFlags.PNG)
# 仿射变换
# 透视变换
# 极坐标变换
