[官方文档入口]()
# 有哪些类和结构
```
// 也算是比较惊奇的发现了，虽然更应该说意料之外，情理之中，opencv也能简单的搭建dnn模型了
// 而且根据前面的一些函数的学习，也可以看到所支持的函数有很多，非常方便
// 这里记录一下一些类函数和结构
class cv::dnn::BackendNode
class cv::dnn::BackendWrapper
class cv::dnn::Dict
struct cv::dnn::DictValue
class cv::dnn::Layer
class cv::dnn::LayerParams
class cv::dnn::Net
// 但是我想说，暂时应该用不到，最有用的应该就是，非极大值抑制和读取网络结构的函数了吧
// 不过很多高深的用法现在我还没有仔细研究，刚刚下的结论可能不够好，毕竟在不使用一些tensorflow这样的库的情况下
// 就可以搭建网络，真的是极大的方便了图片处理
```

