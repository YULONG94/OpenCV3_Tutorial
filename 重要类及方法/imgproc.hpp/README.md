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
