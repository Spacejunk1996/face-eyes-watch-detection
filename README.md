# face-eyes-watch-detection
This project could detect to human face, eyes and watch. It is my first haar cascade project and it is my first time to use cascade trainning to get watch haar cascade classification.
## Features
### Detecting face and eyes:
![Detecting face and eyes](https://github.com/Spacejunk1996/face-eyes-watch-detection/blob/master/detecting%20face%20and%20eyes.png)

### Detecting watch:
![Detecting watch](https://github.com/Spacejunk1996/face-eyes-watch-detection/blob/master/detecting%20watch.png)

## Install
First, you need to buy a server cause we will train our self cascade on linux environment.
Once you have your server ready to go, you will want to get the actual OpenCV library.
Change directory to server's root, or whervere you want to place your workplace.
```
cd ~
sudo apt-get update
sudo apt-get upgrade

mkdir opencv_workspace
cd opencv_workspace
```
Now let's grab OpenCV:
```
sudo apt-get install git
```
git clone https://github.com/Itseez/opencv.git
Now we have cloned the latest version of OpenCV here. Now let's get some essentials:
```
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```
Finally, let's grab the OpenCV development library:
```
sudo apt-get install libopencv-dev
```

So when you want to build a haar cascade, you need "positive" images, and "negayive" images. The positive images are images that contain the object you want to find. This can either be images that just mainly have the project, or itcan be images that contain the object, and you specify the ROI (region of interest) where the object is .With these positives, we build a vector file that is basically all of these positive put together. One nice thing about the positives is that you can actually just have one image of the object you wish to detect, and then have a few thousand negative images. Yes, a few thousand. The negative images can be anything, except they cannot contain your object.

Anyway, I have put watch5050.jpg as a positive image in my file, and what we need to do next step is to download plenty of negitive images. (about 2000) By "download-image-by-link.py" we could download gray scale image from "image.net". It probably need some time. After you have downloaded negitive images, you need to filter them, which means that some of images which cannot download successfully we have to elimate them. And the next step is that generate bg.txt file. Finally, we should upload all of these sort of files to server. (In windows you could use Winscp, in macos you could trainning in your host machine or upload file by ssh)

Ok, now we already have all of the file that we need and we could begin to train.

```
mkdir data
mkdir info
opencv_createsamples -img watch5050.jpg -bg bg.txt -info info/info.lst -pngoutput info -maxxangle 0.5 -maxyangle 0.5 -maxzangle 0.5 -num 1950
opencv_createsamples -info info/info.lst -num 1950 -w 20 -h 20 -vec positives.vec
opencv_traincascade -data data -vec positives.vec -bg bg.txt -numPos 1800 -numNeg 900 -numStages 10 -w 20 -h 20
```

After trainning finish, you could download .xml file from server to your host and test it.

## Reference:
https://www.youtube.com/watch?v=t0HOVLK30xQ&t=0s&index=20&list=PLQVvvaa0QuDdttJXlLtAJxJetJcqmqlQq
