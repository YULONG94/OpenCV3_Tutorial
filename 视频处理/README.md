# 视频背景分割
```
Ptr<BackgroundSubtractorKNN> cv::createBackgroundSubtractorKNN (int history = 500, 
                                                                double dist2Threshold = 400.0, 
                                                                bool detectShadows = true)

Ptr<BackgroundSubtractorMOG2> cv::createBackgroundSubtractorMOG2 (int history = 500, 
                                                                  double varThreshold = 16, 
                                                                  bool detectShadows = true)

// 举个例子，原理方面的没具体看，主要应该就是通过分析前后的若干帧视频来判断当前帧中哪些像素点是背景，哪些不是
VideoCapture cap;
cap.open("../data/vtest.avi");
// cap.open("D://data//1_1.avi");
Ptr<BackgroundSubtractorMOG2> bgsubtractor = createBackgroundSubtractorMOG2(500, 16, false);
Ptr<BackgroundSubtractorKNN> bgsubtractor_knn = createBackgroundSubtractorKNN(500, 400, false);
Mat frame, bgmask_mog2, bgmask_knn;
namedWindow("src");
namedWindow("segm_mog2");
namedWindow("segm_knn");
while (1)
{
	cap >> frame;
	if (frame.empty())
		break;
	bgsubtractor_knn->apply(frame, bgmask_mog2);
	bgsubtractor_knn->apply(frame, bgmask_knn);
	imshow("src", frame);
	imshow("segm_mog2", bgmask_mog2);
	imshow("segm_knn", bgmask_knn);
	if (waitKey(10)>=0)
		break;
}
cap.release();
```
# 物体跟踪
```

```
