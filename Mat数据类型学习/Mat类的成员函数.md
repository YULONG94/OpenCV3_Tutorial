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
> Mat cv::Mat::colRange()和rowRange()
```
Mat mat_ori = Mat::eye(5, 5, CV_8UC1);
Mat m1 = mat_ori.rowRange(1, 3);
Mat m2 = mat_ori.rowRange(Range(1, 3));
Mat m3 = mat_ori.colRange(1, 2);
```
> Mat cv::Mat::col(int x)和row(int x)
```
//返回第x列或行
Mat mat_ori = Mat::eye(5, 5, CV_8UC1);
Mat m1 = mat_ori.row(2);
Mat m3 = mat_ori.col(4);
```
> Mat数据类型的成员变量
```
int cv::Mat::cols
int cv::Mat::rows
int cv::Mat::data
int cv::Mat::dims
int cv::Mat::size
```
> int cv::Mat::depth()
```
返回Mat的深度
CV_8U - 8-bit unsigned integers ( 0..255 )
CV_8S - 8-bit signed integers ( -128..127 )
CV_16U - 16-bit unsigned integers ( 0..65535 )
CV_16S - 16-bit signed integers ( -32768..32767 )
CV_32S - 32-bit signed integers ( -2147483648..2147483647 )
CV_32F - 32-bit floating-point numbers ( -FLT_MAX..FLT_MAX, INF, NAN )
CV_64F - 64-bit floating-point numbers ( -DBL_MAX..DBL_MAX, INF, NAN ) 
```
> Mat cv::Mat::diag(int d = 0) const
```
Mat m = (Mat_<int>(3,3) <<
            1,2,3,
            4,5,6,
            7,8,9);
Mat d0 = m.diag(0);
Mat d1 = m.diag(1);
Mat d_1 = m.diag(-1);
```
> dot()
```
//矩阵点乘，如果是想实现矩阵乘法可以直接使用*
//矩阵点乘是对应元素相乘再进行加和
Mat A;
Mat B;
double AB = A.dot(B)
Mat AB = A*B
```
> mul()
```
//点对点相乘，得到的结果A，B和AB的尺寸一致，对应点数值满足a*b=ab
//同时也能通过这种方式进行除法运算，
Mat A;
Mat B;
Mat AB = A.mul(B)
Mat ADB = A.mul(1/B)
```
> cross()
```
//两个三维向量做叉乘
//数据的格式一定要是<float>，验证过如果是<int>会计算出错，很奇怪
Mat a = (Mat_<float>(3,1) << 1, 0, 0);
Mat b = (Mat_<float>(3,1) << 0, 1, 0);
Mat c = a.cross(b);
```
> mean()和sum()
```
//计算每一层的平均值和加和
Mat a = Mat(2, 3, CV_8UC3);
Scalar s = sum(a);
Scalar m = mean(a);
```
> void cv::Mat::pop_back(size_t nelems = 1)
```
//删除最后nelems个数据
Mat A = Mat::eye(3, 3, CV_8UC1);
A.pop_back(2);
```
> ptr()
```
// 与at(),类似，就不介绍非常复杂的情况了，能掌握下面几种访问Mat数据的方式应该就够了
// 对于三通道的图像
for(int h = 0 ; h < image.rows ; ++ h)
{
	for(int w = 0 ; w < image.cols / 2 ; ++ w)
	{
		uchar *ptr = image.ptr<uchar>(h , w) ;
		ptr[0] = 255 ;
		ptr[1] = 0 ;
		ptr[2] = 0 ;
	}
}
for(int h = 0 ; h < image.rows ; ++ h)
{
	for(int w = 0 ; w < image.cols / 2 ; ++ w)
	{
		Vec3b *ptr = image.ptr<Vec3b>(h , w) ;
		ptr->val[0] = 0 ;
		ptr->val[1] = 255 ;
		ptr->val[2] = 0 ;
	}
}
// 单通道图像
for(int h = 0 ; h < image.rows ; ++ h)
{
	uchar *ptr = image.ptr(h) ;
	for(int w = 0 ; w < image.cols / 2 ; ++ w)
	{
		ptr[w] = 128 ;
	}
}
for(int h = 0 ; h < image.rows ; ++ h)
{
	for(int w = 0 ; w < image.cols / 2 ; ++ w)
	{
		uchar *ptr = image.ptr<uchar>(h , w) ;
		*ptr = 255 ;
	}
}
```
> Mat Mat::reshape(int cn, int rows=0) const
```
// cn: 表示通道数(channels), 如果设为0，则表示保持通道数不变，否则则变为设置的通道数。
// rows: 表示矩阵行数。 如果设为0，则表示保持原有的行数不变，否则则变为设置的行数。
Mat data = Mat(20, 30, CV_32F);  //设置一个20行30列1通道的一个矩阵
Mat dst = data.reshape(0, 1);  //通道数不变，将矩阵序列化1行N列的行向量
Mat dst = data.reshape(0, data.rows*data.cols);  //通道数不变，将矩阵序列化N行1列的列向量
Mat dst = data.reshape(2, 0);  //通道数由1变为2，行数不变
Mat dst = data.reshape(2, data.rows/5);  //通道数由1变为2，行数变为原来的五分之一
```
> void resize(InputArray src, OutputArray dst, Size dsize, double fx=0, double fy=0, int interpolation=INTER_LINEAR)
```
// dsize和fx+fy是两种对输出图像的尺寸控制的方法
// 如果dsize有一维是零，则通过fx和fy来控制，也可以直接设置为None
// 如果dsize有效，则忽略后面fx和fy的取值
Mat A = imread("../data/1.jpg");
Mat B;
resize(A, B, Size(200, 1000));
imshow("A", A);
imshow("B", B);
waitKey();
```
> void pyrDown(InputArray src, OutputArray dst, const Size& dstsize=Size(), int border-Type=BORDER_DEFAULT)
```
// 下采样，图像变为原来的1/2，dstsize可以不设置
// 如果设置dstsize，一定需要满足|dstsize.width * 2 - src.cols| ≤ 2;|dstsize.height * 2 - src.rows| ≤ 2; 否则会报错
Mat A = imread("../data/1.jpg");
Mat C;
pyrDown(A, C);
imshow("A", A);
imshow("C", C);
waitKey();
```
> void pyrUp(InputArray src, OutputArray dst, const Size& dstsize=Size(), int border-Type=BORDER_DEFAULT)
```
// 与下采样相对
Mat A = imread("../data/1.jpg");
Mat B;
pyrUp(A, B);
imshow("A", A);
imshow("B", B);
waitKey();
```
> Mat& setTo(InputArray value, InputArray mask=noArray()); 
```
// 功能：把矩阵mask中元素不为0的点全部变为value值；
// 当默认不添加mask的时候，表明mask是一个与原图尺寸大小一致的且元素值全为非0的矩阵，
// 因此不加mask的时候，会将原矩阵的像素值全部赋值为value；
Mat src(3, 3, CV_8UC1);
Mat mask1(3, 3, CV_8UC1, Scalar(0));
Mat mask2(3, 3, CV_8UC1, Scalar(10));
src.setTo(100, mask1);
cout << src << endl;
src.setTo(100, mask1);
cout << src << endl;
```
