#include "cv.h"
#include "highgui.h"
#include "math.h"
 
int main()
{
	 IplImage* obraz_zrodlowy;
	 IplImage* obraz_szarosc;
	 IplImage* obraz_progowanie;
	 IplImage* obraz_wygladzenie;
	 IplImage* obraz_wyostrzenie;
 
	 obraz_zrodlowy = cvLoadImage("linux.jpg", 1);
	 obraz_szarosc = cvCreateImage (cvSize(obraz_zrodlowy->width, obraz_zrodlowy->height), IPL_DEPTH_8U, 1);
	 cvCvtColor (obraz_zrodlowy, obraz_szarosc, CV_BGR2GRAY);
 
	 obraz_progowanie = cvCloneImage (obraz_szarosc);
	 obraz_wygladzenie = cvCloneImage (obraz_zrodlowy);
	 obraz_wyostrzenie = cvCloneImage (obraz_zrodlowy);
 
	 cvNamedWindow ("Zrodlowy", 1);
	 cvNamedWindow ("Szarosc", 1);
	 cvNamedWindow ("Progowanie", 1);
	 cvNamedWindow ("Wygladzenie", 1);
	 cvNamedWindow ("Wyostrzenie", 1);
 
	 cvThreshold(obraz_szarosc, obraz_progowanie, 150, 255, CV_THRESH_BINARY);
 
	 CvMat *dolnoprzepustowa = cvCreateMat(3,3,CV_32FC1);
	 cvZero(dolnoprzepustowa);
	 dolnoprzepustowa->data.fl[0] = 1.0;
	 dolnoprzepustowa->data.fl[1] = 1.0;
	 dolnoprzepustowa->data.fl[2] = 1.0;
	 dolnoprzepustowa->data.fl[3] = 1.0;
	 dolnoprzepustowa->data.fl[4] = 1.0;
	 dolnoprzepustowa->data.fl[5] = 1.0;
	 dolnoprzepustowa->data.fl[6] = 1.0;
	 dolnoprzepustowa->data.fl[7] = 1.0;
	 dolnoprzepustowa->data.fl[8] = 1.0;
	 cvFilter2D(obraz_zrodlowy,obraz_wygladzenie,dolnoprzepustowa,cvPoint(1,1));
 
	 CvMat *gornoprzepustowa = cvCreateMat(3,3,CV_32FC1);
	 cvZero(gornoprzepustowa);
	 gornoprzepustowa->data.fl[0] = -1.0;
	 gornoprzepustowa->data.fl[1] = -1.0;
	 gornoprzepustowa->data.fl[2] = -1.0;
	 gornoprzepustowa->data.fl[3] = -1.0;
	 gornoprzepustowa->data.fl[4] = 9.0;
	 gornoprzepustowa->data.fl[5] = -1.0;
	 gornoprzepustowa->data.fl[6] = -1.0;
	 gornoprzepustowa->data.fl[7] = -1.0;
	 gornoprzepustowa->data.fl[8] = -1.0;
	 cvFilter2D(obraz_zrodlowy,obraz_wyostrzenie,gornoprzepustowa,cvPoint(1,1));
 
	 cvShowImage ("Zrodlowy", obraz_zrodlowy);
	 cvShowImage ("Szarosc", obraz_szarosc);
	 cvShowImage ("Progowanie", obraz_progowanie);
	 cvShowImage ("Wygladzenie", obraz_wygladzenie);
	 cvShowImage ("Wyostrzenie", obraz_wyostrzenie);
 
	 cvWaitKey(0);
 
	 cvDestroyWindow ("Zrodlowy");
	 cvDestroyWindow ("Szarosc");
	 cvDestroyWindow ("Progowanie");
	 cvDestroyWindow ("Wygladzenie");
	 cvDestroyWindow ("Wyostrzenie");
 
	 cvReleaseImage (&obraz_zrodlowy);
	 cvReleaseImage (&obraz_szarosc);
	 cvReleaseImage (&obraz_progowanie);
	 cvReleaseImage (&obraz_wygladzenie);
	 cvReleaseImage (&obraz_wyostrzenie);
 
	 cvReleaseMat(&dolnoprzepustowa);
	 cvReleaseMat(&gornoprzepustowa);
 
	 return 0;
}
