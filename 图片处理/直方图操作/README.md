# 反向投影
```
void cv::calcBackProject (const Mat * images, 
                          int nimages, 
                          const int * channels, 
                          InputArray hist, 
                          OutputArray backProject, 
                          const float ** ranges, 
                          double scale = 1, 
                          bool uniform = true)

void cv::calcBackProject (const Mat * images, 
                          int nimages, 
                          const int * channels, 
                          const SparseMat & hist, 
                          OutputArray backProject, 
                          const float ** ranges, 
                          double scale = 1, 
                          bool uniform = true)

void cv::calcBackProject (InputArrayOfArrays images, 
                          const std::vector< int > & channels, 
                          InputArray hist, 
                          OutputArray dst, 
                          const std::vector< float > & ranges, 
                          double scale)

```
# calcHist
```
void cv::calcHist (const Mat * images, 
                   int nimages, 
                   const int * channels, 
                   InputArray mask, 
                   OutputArray hist, 
                   int dims, 
                   const int * histSize, 
                   const float ** ranges, 
                   bool uniform = true, 
                   bool accumulate = false)
                   
void cv::calcHist (const Mat * images, 
                   int nimages, 
                   const int * channels, 
                   InputArray mask, 
                   SparseMat & hist, 
                   int dims, 
                   const int * histSize, 
                   const float ** ranges, 
                   bool uniform = true, 
                   bool accumulate = false)

void cv::calcHist (InputArrayOfArrays images, 
                   const std::vector< int > & channels, 
                   InputArray mask, 
                   OutputArray hist, 
                   const std::vector< int > & histSize, 
                   const std::vector< float > & ranges, 
                   bool accumulate = false)

```
# compareHist
```
double cv::compareHist (InputArray H1, 
                        InputArray H2, 
                        int method)

double cv::compareHist (const SparseMat & H1, 
                        const SparseMat & H2, 
                        int method)

```
# EMD
```
float cv::EMD (InputArray signature1, 
               InputArray signature2, 
               int distType, 
               InputArray cost = noArray(), 
               float * lowerBound = 0,
               OutputArray flow = noArray())

```
# equalizeHist
```
void cv::equalizeHist (InputArray src, 
                       OutputArray dst)
```
# wrapperEMD
```
float cv::wrapperEMD (InputArray signature1, 
                      InputArray signature2, 
                      int distType, 
                      InputArray cost = noArray(), 
                      Ptr< float > lowerBound = Ptr< float >(), 
                      OutputArray flow = noArray())
```
