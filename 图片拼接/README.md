
贴一张跑过的拼接程序，很明显一起拼接的图片越多，消耗的时间几乎可以用指数上升来描述，所以尽可能的不要尝试一次拼接四五张以上的图片
```
#include <iostream>
#include <fstream>
#include <opencv2/core/core.hpp>
#include "opencv2/highgui/highgui.hpp"
#include<opencv2/stitching.hpp>
#include<Windows.h>
#include<time.h>

using namespace cv;

bool try_use_gpu = false;
std::vector<Mat> imgs, imgs1;
std::string result_name = "result.jpg";

int pictures_plus()
{
	clock_t start, finish;
	double totaltime;
	start = clock();
	Mat img1 = imread("../data/1.jpg");
	Mat img2 = imread("../data/2.jpg");
	Mat img3 = imread("../data/3.jpg");
	Mat img4 = imread("../data/4.jpg");


	imgs.push_back(img1);
	imgs.push_back(img2);
	//imgs.push_back(img3);
	//imgs.push_back(img4);

	Mat pano;
	Stitcher stitcher = Stitcher::createDefault(try_use_gpu);
	Stitcher::Status status = stitcher.stitch(imgs, pano);

	imgs1.push_back(pano);
	imgs1.push_back(img3);
	imgs1.push_back(img4);
	status = stitcher.stitch(imgs1, pano);
	

	if (status != Stitcher::OK)
	{
		std::cout << "Can't stitch images, error code = " << status << std::endl;
	}
	finish = clock();
	totaltime = (double)(finish - start) / CLOCKS_PER_SEC;
	std::cout << "\n此程序的运行时间为" << totaltime << "秒！" << std::endl;
	namedWindow(result_name);
	imshow(result_name, pano);
	imwrite(result_name, pano);
	waitKey();
	return 0;
}
```
