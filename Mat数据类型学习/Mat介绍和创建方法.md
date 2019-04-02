## Mat数据类型介绍
Mat类型是opencv2.0后的类型，使用此类型无需进行内存管理，即无需手动分配内存，在不需要时自动释放内存，但因目前的许多嵌入式系统仅支持c语言，故而除了在某些嵌入式系统中使用cvMat或IplImage, 基本使用Mat类型。OpenCV使用的是引用计数系统：每个Mat对象都有着自己的header，但matrix可在两个实例中通过指向同一个matrix首地址的指针来共享数据。拷贝构造和赋值均只拷贝其header和指向matrix的指针，而非像素数据的本身。当拷贝一个Mat对象的header时，关于matrix的计数器数值增加，一旦header被清理了，counter减少，当counter减少到0的时候，matrix的内存空间会被释放。
## Mat数据类型的构造
### 方法零（create构造法）
#### 验证发现这种方法不能对初值进行设置，都是默认的205
> void cv::Mat::create(int rows, int cols, int type)
```
Mat M;
M.create(2, 3, CV_8UC3);
```
> void cv::Mat::create(Size size(int cols, int rows), int type)
```
Mat M;
M.create(Size(4, 5), CV_8UC1);
```
> void cv::Mat::create(int ndims, const int* sizes, type)
```
Mat M;
int SZ[2] = { 2, 3 };
// sizes的形式按照{rows, cols}
// ndims可以大于2，但是对于三维以上的Mat不能在使用cout进行打印输出了
M.create(2, SZ, CV_8UC1);
```
> void cv::Mat::create(const std::vector<int>& sizes, int type)
```
Mat M;
// sizes的规则也是按照{rows, cols}，维度也是可以超过2维的，不过不能通过cout打印检查
vector<int> sv{ 2,3 };
M.create(sv, CV_8UC1);
```
### 方法一（Mat构造法）
> cv::Mat::Mat(int rows, int cols, int type)
```
Mat M = Mat(2, 3, CV_8UC1);
```
> cv::Mat::Mat(Size size(int cols, int rows), int type)
```
Mat M = Mat(Size(2, 3), CV_8UC1);
```
> cv::Mat::Mat(int rows, int cols, int type, const Scalar& s)
```
Mat M = Mat(2, 3, CV_8UC1, Scalar::all(0));
```
> cv::Mat::Mat(Size size(int cols, int rows), int type, const Scalar& s)
```
Mat M = Mat(Size(2, 3), CV_8UC1, Scalar::all(0));
```
> cv::Mat::Mat(int ndims, const int* sizes, int type)
```
int SZ[2] = { 2, 3 };
Mat M = Mat(2, SZ, CV_8UC1);
```
> cv::Mat::Mat(const std::vector<int> & sizes, int type)
```
vector<int> sv{ 2,3 };
Mat M = Mat(sv, CV_8UC1);
```
> cv::Mat::Mat(int ndims, const int* sizes, int type, const Scalar& s)
```
int SZ[2] = { 2, 3 };
Mat M = Mat(2, SZ, CV_8UC1, Scalar::all(0));
```
> cv::Mat::Mat(const std::vector< int > & sizes, int type, const Scalar& s)
```
vector<int> V{ 2, 3 };
Mat M = Mat(V, CV_8UC1, Scalar::all(0));
```
> cv::Mat::Mat(const Mat & m)
```
//不同于M.clone(),下面的几种根据已有的举证通过Mat法创建的例子也都只是复制指针头
Mat M0 = Mat(2, 3, CV_8UC1)
Mat M = Mat(M0);
```
> 以下四种暂时没学（用到的时候再说）
```
cv::Mat::Mat(int rows, int cols, int type, void* data, size_t step = AUTO_STEP)
cv::Mat::Mat(Size size, int type, void* data, size_t step = AUTO_STEP)
cv::Mat::Mat(int ndims, const int* sizes, int type, coid* data, const size_t* steps = 0)
cv::Mat::Mat(const std::vector<int> & sizes, int type, void* data, const size_t* steps = 0)
```
> cv::Mat::Mat(const Mat& m, const Range & rowRange, const Range & colRange = Range::all())
```
Mat M0 = Mat(10, 10, CV_8UC1);
Range r1;  //	或者Range r1(0, 5);
r1.start = 0;
r1.end = 5;
Mat M = Mat(M0, r1, Range::all());
```
> cv::Mat::Mat(const Mat & m, const Rect & roi)
```
Mat M0 = Mat(10, 10, CV_8UC1);
Rect r;  //或者Rect r(2, 2, 4, 4)
r.x = 2;
r.y = 2;
r.height = 4;
r.width = 4;
Mat M = Mat(M0, r);
```
> 以下几种也估计很少用到，先放着，有前面一些应该够用一阵子了
```
cv::Mat::Mat(const Mat & m, const Range * ranges)
cv::Mat::Mat(const Mat & m, const std::Vector<Range> & ranges)
cv::Mat::Mat(const std::vector<_Tp> & vec, bool copyDta = false)
cv::Mat::Mat(const Vec<_Tp, n> & vec, bool copyData = true)
cv::Mat::Mat(const Matx<_Tp, m, n> & mtx, bool copyData = True)
cv::Mat::Mat(const Point3_<_Tp> pt, bool copyData = true)
cv::Mat::Mat(const MatCommaInitializer_<_Tp> & commaInitializer)
cv::Mat::Mat(const cuda::GpuMat & m)
```
### 方法二（复制拷贝法）
> clone()和copyTo()
```
实现对图像的拷贝，而且结果与拷贝源独立，这就是与直接利用Mat法创建的区别
```
