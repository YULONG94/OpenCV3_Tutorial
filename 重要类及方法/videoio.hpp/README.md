[官方文档入口](https://docs.opencv.org/3.4.2/dd/de7/group__videoio.html)
> class cv::VideoCapture 

> class cv::VideoWriter
```
// 官方的这个例子很好，直接贴出来
#include <opencv2/core.hpp>
#include <opencv2/videoio.hpp>
#include <opencv2/highgui.hpp>
#include <iostream>
#include <stdio.h>

using namespace cv;
using namespace std;

int main(int, char**)
{
    Mat src;
    // use default camera as video source
    VideoCapture cap(0);
    // check if we succeeded
    if (!cap.isOpened()) {
        cerr << "ERROR! Unable to open camera\n";
        return -1;
    }
    // get one frame from camera to know frame size and type
    cap >> src;
    // check if we succeeded
    if (src.empty()) {
        cerr << "ERROR! blank frame grabbed\n";
        return -1;
    }
    bool isColor = (src.type() == CV_8UC3);

    //--- INITIALIZE VIDEOWRITER
    VideoWriter writer;
    int codec = VideoWriter::fourcc('M', 'J', 'P', 'G');  // select desired codec (must be available at runtime)
    double fps = 25.0;                          // framerate of the created video stream
    string filename = "./live.avi";             // name of the output video file
    writer.open(filename, codec, fps, src.size(), isColor);
    // check if we succeeded
    if (!writer.isOpened()) {
        cerr << "Could not open the output video file for write\n";
        return -1;
    }

    //--- GRAB AND WRITE LOOP
    cout << "Writing videofile: " << filename << endl
         << "Press any key to terminate" << endl;
    for (;;)
    {
        // check if we succeeded
        if (!cap.read(src)) {
            cerr << "ERROR! blank frame grabbed\n";
            break;
        }
        // encode the frame into the videofile stream
        writer.write(src);
        // show live and wait for a key with timeout long enough to show images
        imshow("Live", src);
        if (waitKey(5) >= 0)
            break;
    }
    // the videofile will be closed and released automatically in VideoWriter destructor
    return 0;
}
```
# 先开个头说明一下读取帧的四种方法
```
// 方法一 
capture.read(frame); 
// 方法二 
capture.grab(); 
// 方法三
capture.retrieve(frame); 
// 方法四
capture >> frame;
```
# 一些类函数
> virtual double cv::VideoCapture::get (int propId)const

> virtual double cv::VideoWriter::get (int propId)const
```
// 用来获得两种类的相关属性
// videocapture类的基本属性参见
https://docs.opencv.org/3.4.2/d4/d15/group__videoio__flags__base.html#gaeb8dd9c89c10a5c63c139bf7c4f5704d
// videocapture类的附加属性
https://docs.opencv.org/3.4.2/dc/dfc/group__videoio__flags__others.html
// videowrite类的基本属性
https://docs.opencv.org/3.4.2/d4/d15/group__videoio__flags__base.html#ga41c5cfa7859ae542b71b1d33bbd4d2b4
// videowrite类的附加属性
https://docs.opencv.org/3.4.2/dc/dfc/group__videoio__flags__others.html
```
> virtual bool cv::VideoCapture::grab ()
```
// 读取下一帧，如果读取成功就返回true
```
> virtual bool cv::VideoWriter::isOpened ()const
```
//判断video capturing是否初始化成功
```
> virtual bool cv::VideoCapture::open (const String & filename)
```
// 其实这只是四种方法中的一种，与初始化capture一样，四种重载函数只有参数是不一样的
```
> virtual bool cv::VideoCapture::read (OutputArray image)
```
// 读取视频帧
```
> virtual void cv::VideoCapture::release ()
```
// 关闭视频流
```
> virtual bool cv::VideoCapture::retrieve (OutputArray image, int flag = 0)
```
// 读取和解码视频帧
```
> virtual bool cv::VideoCapture::set (int propId, double value)
```
// 设置videocapture的属性
```
> static Ptr<IVideoWriter> cv::VideoWriter::create (const String & filename, 
    int fourcc, double fps, Size frameSize, bool isColor = true )

