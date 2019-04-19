# 限制对比对自适应均衡CLAHE(Contrast Limited Adaptive Histogram Equalization)
```
Mat m = imread("D://data//hand.jpg", IMREAD_GRAYSCALE); //input image
imshow("lena_GRAYSCALE", m);

Ptr<CLAHE> clahe = createCLAHE();
clahe->setClipLimit(4);

Mat dst;
clahe->apply(m, dst);
imshow("lena_CLAHE", dst);
// 可以对比直接使用equalizeHist的方法得到的结果
// 我个人觉得还是有差别的
Mat dts2;
equalizeHist(m, dts2);
imshow("2", dts2);
waitKey();
```
> 创建
```
Ptr<CLAHE> clahe = createCLAHE();
```
> 使用
```
virtual void cv::CLAHE::apply (InputArray src, 
                               OutputArray dst)
```
> 设置门限值
```
virtual void cv::CLAHE::setClipLimit (double clipLimit)
```
> 获取门限值
```
virtual double cv::CLAHE::getClipLimit ()const
```
> 设置局部大小
```
virtual void cv::CLAHE::setTilesGridSize (Size tileGridSize)
```
> 获取局部大小
```
virtual Size cv::CLAHE::getTilesGridSize ()const
```
