## Mat型数据的方法
> void cv::Mat::addref()
```
其实这个不是很懂
该方法递增与矩阵数据关联的引用计数。如果矩阵头指向外部的数据集（见 Mat::Mat()），
则引用计数为 NULL，并且该方法在这种情况下不起作用。通常情况下，为避免内存泄漏，
不应显式调用该方法。它是由该矩阵赋值运算符隐式调用。在支持的它平台上，引用计数
器递增是一个原子操作。因此，对相同的矩阵，在不同的线程异步操作是安全的。
```
> Mat& cv::Mat::adjustROI(int dtop, int dbottom, int dleft, int dright)
```
//四个参数分别记录在原来的ROI基础上四边是怎么调整的，可以为负值
Mat M = imread("../data/testtest.png");
Mat M_roi = M(Rect(M.cols / 4, M.rows/4, M.cols / 4, M.rows / 4));
imshow("roi1", M_roi);
M_roi.adjustROI(0, -M.rows/10, 0,0);
imshow("roi2", M_roi);
waitKey();
```
> void cv::Mat::assignTo(Mat & m, int type = -1) const
```
类似于convertTo()
```
> at()
```
Mat H(100, 100, CV_64F);
for(int i = 0; i < H.rows; i++)
    for(int j = 0; j < H.cols; j++)
        H.at<double>(i,j)=1./(i+j+1);
```
> begin()和end()
```
Mat a = imread("../data/1.jpg", 0);  //读取灰度单通道图
double sum = 0;
MatIterator_<uchar> it, end;
for (it = a.begin<uchar>(), end = a.end<uchar>(); it != end; ++it)
{
	sum += *it;
}
cout << sum << endl;
//注意：在学习过程中尝试将*it打印出来发现数值很奇怪，是因为此时是uchar数据类型，要想指导对应的int值可以加上强制转化符号。
Mat a = imread("../data/1.jpg", 1);  //读取三通道
MatIterator_<Vec3b> it, end;
for (it = a.begin<Vec3b>(), end = a.end<Vec3b>(); it != end; ++it)
{
	(*it)[0] = 255 - (*it)[0];
	cout << int((*it)[0])<<endl;
}
```
> channels()
```
//获取Mat的通道数
Mat a = imread("../data/1.jpg", 0);
cout << a.channels() << endl;
```

