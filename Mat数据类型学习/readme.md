# Mat数据类型介绍

## Mat数据类型的构造
> 方法零（create构造法）

void cv::Mat::create(int rows,\n
                     int cols,\n
                     int type)\n
例如\n
Mat M\n
M.create(2, 3, CV_8UC3)
