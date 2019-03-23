#include <opencv2/opencv.hpp> 
#include <iostream> 
using namespace cv;

Mat src, dst;
char OUTPUT_WIN[] = "output image";
int element_size = 0;
int max_size = 4;
char a[5][17] = {{ "CV_MOP_OPEN" }, { "CV_MOP_CLOSE" }, { "CV_MOP_GRADIENT" }, { "CV_MOP_TOPHAT" }, {"CV_MOP_BLACKHAT"}};
void CallBack_Demo(int, void*);
int main(int argc, char** argv) {
	
	src = imread("LS.jpg");
	if (!src.data) {
		printf("could not load image...\n");
		return -1;
	}
	resize(src, src, Size(500, 500));
	
	namedWindow("input image", CV_WINDOW_AUTOSIZE);
	imshow("input image", src);

	namedWindow(OUTPUT_WIN, CV_WINDOW_AUTOSIZE);
	createTrackbar("Element Size :", OUTPUT_WIN, &element_size, max_size, CallBack_Demo);
	CallBack_Demo(0, 0);

	waitKey(0);
	return 0;
}

void CallBack_Demo(int, void*) {
	//int s = element_size * 2 + 1;
	char a1[17]; 
	char *p;
	p = a1;
	p = a[element_size];
	printf("%s\n", p);
	Mat structureElement = getStructuringElement(MORPH_RECT, Size(5, 5), Point(-1, -1));
	switch(element_size)
	{
		case 0:morphologyEx(src, dst, CV_MOP_OPEN, structureElement);break;
		case 1:morphologyEx(src, dst, CV_MOP_CLOSE, structureElement);break;
		case 2:morphologyEx(src, dst, CV_MOP_GRADIENT, structureElement);break;
		case 3:morphologyEx(src, dst, CV_MOP_TOPHAT, structureElement);break;
		case 4:morphologyEx(src, dst, CV_MOP_BLACKHAT, structureElement);break;
	}
	
	//erode(src, dst, structureElement);
	imshow(OUTPUT_WIN, dst);
	return;
}
