# 创建方式
```
Ptr<BackgroundSubtractorKNN> cv::createBackgroundSubtractorKNN (int history = 500, 
                                                                double dist2Threshold = 400.0, 
                                                                bool detectShadows = true)

Ptr<BackgroundSubtractorMOG2> cv::createBackgroundSubtractorMOG2 (int history = 500, 
                                                                  double varThreshold = 16, 
                                                                  bool detectShadows = true)

// 这两个函数的第一个参数都是关于利用多少帧历史来做背景提前的，
// 第二个参数是模型内部算法的阈值参数
// 第三个是关于阴影检测的，如果是true，那么就会单独对阴影进行检测和标定，所以会稍微消耗一点额外的时间
// 如果第三个参数是false，那么阴影也会与其他运动的物体的标定方式一样
```
# class cv::BackgroundSubtractor
> virtual void cv::BackgroundSubtractor::apply (InputArray image, OutputArray fgmask, double learningRate = -1)
```
// 开始提取运动的前景
```
> virtual void cv::BackgroundSubtractor::getBackgroundImage (OutputArray backgroundImage)const
```
// 提取视频中的背景
```
> example
```
// 这里重新贴上之前写过的一个例子
// 其中包含了前面两个函数的使用
	VideoCapture cap;
	cap.open("../data/vtest.avi");
	// cap.open("D://data//1_1.avi");
	Ptr<BackgroundSubtractorMOG2> bgsubtractor = createBackgroundSubtractorMOG2(500, 16, false);
	Ptr<BackgroundSubtractorKNN> bgsubtractor_knn = createBackgroundSubtractorKNN(500, 400, false);
	Mat frame, bgmask_mog2, bgmask_knn;
	Mat backg;
	namedWindow("src");
	namedWindow("segm_mog2");
	namedWindow("segm_knn");
	namedWindow("background");
	while (1)
	{
		cap >> frame;
		if (frame.empty())
			break;
		bgsubtractor_knn->apply(frame, bgmask_mog2);
		bgsubtractor_knn->apply(frame, bgmask_knn);
		bgsubtractor_knn->getBackgroundImage(backg);
		imshow("src", frame);
		imshow("segm_mog2", bgmask_mog2);
		imshow("segm_knn", bgmask_knn);
		medianBlur(backg, backg, 3);
		imshow("background", backg);
		if (waitKey(10) >= 0)
			break;
	}
	cap.release();
	return 0;
```
# class cv::BackgroundSubtractorKNN
> virtual bool cv::BackgroundSubtractorKNN::getDetectShadows ()const
```
// 返回当前分割器对于检测阴影的参数
Ptr<BackgroundSubtractorMOG2> bgsubtractor = createBackgroundSubtractorMOG2(500, 16, true);
cout << bgsubtractor->getDetectShadows() << endl;
```
> virtual double cv::BackgroundSubtractorKNN::getDist2Threshold ()const
```
// 返回当前分割器的阈值参数
```
> virtual int cv::BackgroundSubtractorKNN::getHistory ()const
```
// 返回第一个参数的数值
```
> virtual int cv::BackgroundSubtractorKNN::getkNNSamples ()const
```
// 剩下的这个类的一些函数都是关于其他一些内部算法参数的设置和读取，可以发现这些函数在定义的时候都是不能直接设置的。
// 相同的，对于另一种分割器也是如此，不做过多的举例，需要使用的话，需要先学习一下内部这些参数是怎么在算法中起作用的。
```
> virtual int cv::BackgroundSubtractorKNN::getNSamples ()const
```
```
> virtual int cv::BackgroundSubtractorKNN::getShadowValue ()const
```
```
> virtual void cv::BackgroundSubtractorKNN::setDetectShadows (bool detectShadows)
```
```
> virtual void cv::BackgroundSubtractorKNN::setDist2Threshold (double _dist2Threshold)
```
```
> virtual void cv::BackgroundSubtractorKNN::setHistory (int history)
```
```
> virtual void cv::BackgroundSubtractorKNN::setkNNSamples (int _nkNN)
```
```
> virtual void cv::BackgroundSubtractorKNN::setNSamples (int _nN)
```
```
> virtual void cv::BackgroundSubtractorKNN::setShadowThreshold (double threshold)
```
```
> virtual void cv::BackgroundSubtractorKNN::setShadowValue (int value)
```
```
# class cv::BackgroundSubtractorMOG2
> virtual void cv::BackgroundSubtractorMOG2::apply (InputArray image, OutputArray fgmask, double learningRate = -1)
```
```
> virtual double cv::BackgroundSubtractorMOG2::getBackgroundRatio ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getComplexityReductionThreshold ()const
```

```
> virtual bool cv::BackgroundSubtractorMOG2::getDetectShadows ()const
```

```
> virtual int cv::BackgroundSubtractorMOG2::getHistory ()const
```

```
> virtual int cv::BackgroundSubtractorMOG2::getNMixtures ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getShadowThreshold ()const
```

```
> virtual int cv::BackgroundSubtractorMOG2::getShadowValue ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getVarInit ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getVarMax ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getVarMin ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getVarThreshold ()const
```

```
> virtual double cv::BackgroundSubtractorMOG2::getVarThresholdGen ()const
```

```
> virtual void cv::BackgroundSubtractorMOG2::setBackgroundRatio (double ratio)
```

```
> virtual void cv::BackgroundSubtractorMOG2::setComplexityReductionThreshold (double ct)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setDetectShadows (bool detectShadows)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setHistory (int history)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setNMixtures (int nmixtures)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setShadowThreshold (double threshold)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setShadowValue (int value)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setVarInit (double varInit)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setVarMax (double varMax)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setVarMin (double varMin)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setVarThreshold (double varThreshold)
```
```
> virtual void cv::BackgroundSubtractorMOG2::setVarThresholdGen (double varThresholdGen)
```
```
