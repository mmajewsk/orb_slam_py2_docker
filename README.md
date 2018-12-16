# ORB_SLAM_2_DOCKER	

This is a repositroy that contains dockerfile and instructions for installing the working fork of [ORB SLAM 2](https://github.com/mmajewsk/ORB_SLAM2), 
specifically for the purpose of using it with [Tonic](https://github.com/mmajewsk/Tonic).

## Installing pre-requisite docker

In order to run this docker you need another one. I did not merge those together because the first one contains OpenCV, and can take A LOT of time to build.
So more sensible way is to first build the first one, and build second one.

```

wget https://github.com/Valian/docker-python-opencv-ffmpeg/archive/master.zip -O docker-python-opencv-ffmpeg.zip
unzip -q docker-python-opencv-ffmpeg.zip
rm docker-python-opencv-ffmpeg.zip
cd docker-python-opencv-ffmpeg-master
```

Then build the image:

```
sudo docker image build -t cv3_cuda_contrib_py2 -f Dockerfile-py2-cuda .
```

So building this will take about 2-3 hours, go grab coffee, solve world hunger or smth.
Go watch [Geohot building a slam](https://youtu.be/7Hlb8YX2-W8).

And then delete unused files:

```
cd ..
rm -rf docker-python-opencv-ffmpeg-master
```

## Preparing workspace

This repository contains only dockerfile, but most of the code is stored in my fork of [ORB_SLAM2](https://github.com/mmajewsk/ORB_SLAM2). So you need two repositories, you can clone only one. For dev purposes its better tyo clone orb slam, and wget the current one:

```
wget https://github.com/mmajewsk/orb_slam_py2_docker/archive/master.zip -O orb_slam_py2_docker.zip
unzip -q orb_slam_py2_docker.zip
mv orb_slam_py2_docker-master orb_slam_py2_docker
rm orb_slam_py2_docker.zip
cd orb_slam_py2_docker
```

## Installing some dependencies, for dev purposes

For easier development, some dependencies are staying outside the docker.
Inside `orb_slam_py2_docker` folder run these lines.

```
git clone https://github.com/mmajewsk/ORB_SLAM2
```

```
wget https://github.com/Algomorph/pyboostcvconverter/archive/master.zip -O pyboostcvconverter.zip
unzip -q pyboostcvconverter.zip
rm pyboostcvconverter.zip
mv pyboostcvconverter-master/include/pyboostcvconverter/pyboostcvconverter.hpp ORB_SLAM2/include/pyboostcvconverter.hpp
mv pyboostcvconverter-master/src/pyboost_cv3_converter.cpp ORB_SLAM2/src/pyboost_cv3_converter.cc
rm -rf pyboostcvconverter-master
```

## Building image

Everything is contained in the dockerfile, so just build it.

```
sudo docker image build -t orb_slam_py2 -f Dockerfile-orb-slam-py2 .
```

This should not take long.

## Running

In order to be able to see gui, you must start the x session (for example mobaxterm), and then you can just run:


```
docker run --rm -p 2207:2207 -e DISPLAY=192.168.1.195:0.0 -it orb_slam_py2 
```

You can also run bash.

```
docker run --rm -p 2207:2207 -e DISPLAY=192.168.1.195:0.0 -it orb_slam_py2 bash
```

The port 2207 is needed for the python service.

## Improvements

So the way this is currently installed and build is a bit overcomplicated, I would love to simplify things.
Also, the service runs python2 not python3, but I wasted too much time on trying to change the version. 

## Dependencies and other useful links

https://github.com/Valian/docker-python-opencv-ffmpeg

https://github.com/Algomorph/pyboostcvconverter

https://github.com/mmajewsk/ORB_SLAM2
