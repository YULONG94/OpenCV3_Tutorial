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
> 收集垃圾？(滑稽)
```
virtual void cv::CLAHE::collectGarbage ()
```
# 广义hough变换GeneralizedHough
从霍夫变换到广义霍夫变换的理论介绍参见[霍夫变换到广义霍夫变换](https://blog.csdn.net/tiandijun/article/details/47251913)
```
// 存在下面三种Hough模型
// 不过我试了一下，貌似结果不怎么样啊，难道是我的用法有问题吗
class cv::GeneralizedHough
class cv::GeneralizedHoughBallard
class cv::GeneralizedHoughGuil

// 贴出我的检测代码，欢迎检查出来之后对我进行批评

#include <vector>
#include <iostream>
#include <string>

#include "opencv2/core.hpp"
#include "opencv2/core/utility.hpp"
#include "opencv2/imgproc.hpp"
// #include "opencv2/cudaimgproc.hpp"
#include "opencv2/highgui.hpp"

using namespace std;
using namespace cv;

static Mat loadImage(const string& name)
{
	Mat image = imread(name, IMREAD_GRAYSCALE);
	if (image.empty())
	{
		cerr << "Can't load image - " << name << endl;
		exit(-1);
	}
	return image;
}

int ght_main(/*int argc, const char* argv[]*/)
{
	const string templName = "D://data//pic3.png";
	const string imageName = "D://data//pic1.png";
	const bool full = false;

	const double minDist = 100;
	const int levels = 360;
	const int votesThreshold = 30;
	const int angleThresh = 10000;
	const int scaleThresh = 1000;
	const int posThresh = 100;
	const double dp = 2;
	const double minScale = 0.5;
	const double maxScale = 2;
	const double scaleStep = 0.05;
	const double minAngle = 0;
	const double maxAngle = 360;
	const double angleStep = 1;
	const int maxBufSize = 1000;


	Mat templ = loadImage(templName);
	Mat image = loadImage(imageName);

	Ptr<GeneralizedHough> alg;

	if (!full)
	{
		Ptr<GeneralizedHoughBallard> ballard = createGeneralizedHoughBallard();

		ballard->setMinDist(minDist);
		ballard->setLevels(levels);
		ballard->setDp(dp);
		ballard->setMaxBufferSize(maxBufSize);
		ballard->setVotesThreshold(votesThreshold);

		alg = ballard;
	}
	else
	{
		Ptr<GeneralizedHoughGuil> guil = createGeneralizedHoughGuil();

		guil->setMinDist(minDist);
		guil->setLevels(levels);
		guil->setDp(dp);
		guil->setMaxBufferSize(maxBufSize);

		guil->setMinAngle(minAngle);
		guil->setMaxAngle(maxAngle);
		guil->setAngleStep(angleStep);
		guil->setAngleThresh(angleThresh);

		guil->setMinScale(minScale);
		guil->setMaxScale(maxScale);
		guil->setScaleStep(scaleStep);
		guil->setScaleThresh(scaleThresh);

		guil->setPosThresh(posThresh);
		alg = guil;
	}

	vector<Vec4f> position;
	TickMeter tm;


	alg->setTemplate(templ);
	tm.start();

	alg->detect(image, position);


	tm.stop();

	cout << "Found : " << position.size() << " objects" << endl;
	cout << "Detection time : " << tm.getTimeMilli() << " ms" << endl;

	Mat out;
	cv::cvtColor(image, out, COLOR_GRAY2BGR);
	for (size_t i = 0; i < position.size(); ++i)
	{
		Point2f pos(position[i][0], position[i][1]);
		float scale = position[i][2];
		float angle = position[i][3];
		RotatedRect rect;
		rect.center = pos;
		rect.size = Size2f(templ.cols * scale, templ.rows * scale);
		rect.angle = angle;
		Point2f pts[4];
		rect.points(pts);
		line(out, pts[0], pts[1], Scalar(0, 0, 255), 3);
		line(out, pts[1], pts[2], Scalar(0, 0, 255), 3);
		line(out, pts[2], pts[3], Scalar(0, 0, 255), 3);
		line(out, pts[3], pts[0], Scalar(0, 0, 255), 3);
	}
	imshow("out", out);
	waitKey();
	return 0;
}

```
