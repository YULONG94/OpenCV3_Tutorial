## Mat数据类型介绍

## Mat数据类型的构造
### 方法零（create构造法）
**验证发现这种方法不能对初值进行设置，都是默认的205**
> void cv::Mat::create(int rows, int cols, int type)
                     
ex:
```
Mat M;
M.create(2, 3, CV_8UC3);
```
> void cv::Mat::create(Size size(int cols, int rows), int type)

ex:
```
Mat M;
M.create(Size(4, 5), CV_8UC1);
```

> void cv::Mat::create(int ndims, const int* sizes, type)

ex:
```
Mat M;
int SZ[3] = { 2, 3 };
// sizes的形式按照{rows, cols}
// ndims必须小于等于2才行
M.create(2, SZ, CV_8UC1);
```

> void cv::Mat::create(const std::vector<int>& sizes, int type)

ex:
```
Mat M;
// sizes的规则也是按照{rows, cols}，维度也最多是2维
vector<int> sv{ 2,3 };
M.create(sv, CV_8UC1);
```
