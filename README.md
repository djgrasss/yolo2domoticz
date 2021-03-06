# yolo2domoticz

### What it does:

Let's take our example house with 10 IP cameras, Jeff our example person want's his home automation system to be able to recognise objects in the images and perform home automation actions within his home when certain objects are detected (a cat for example). main.sh starts a Yolo Darknet process on Jeff's Nvidia GPU and keeps it running ready to process images as they are fed from the 10 IP cameras in jpeg format.

Please note you will need a Nvidia Cuda GPU with at least 8gb of memory to be able to run Yolov3 at the default 608 resolution. If you use a GPU with less memory you may need to decrease the resolution (and accept lower object recognition accuracy). If you need a good single slot GPU I suggest looking at the Nvidia Quadro RTX 4000.
<BR>
<BR>
  
#### Instructions:

1) Install Yolo with Cuda support into /opt/darknet follow: https://pjreddie.com/darknet/yolo/
2) apt-get install jpeginfo screen wget jq ffmpeg uuid sudo
3) Check you can see the Nvidia card and Cuda version: nvidia-smi
4) Copy main.sh into /opt/darknet
<BR>

#### How to use main.sh:

main.sh is the script that must be running on the host pc that contains the Nvidia graphics card, Cuda and Darknet software. When started the script starts an instance of Darknet on the Nvidia GPU and keeps it running ready to handle images and display the results.

#### Start the processor

```
/opt/darknet/main.sh "start"
```

#### Stop the processor
  
```
/opt/darknet/main.sh "stop"
```
<BR>
  
#### Process an image and display results in terminal
<BR>
  
The script supports a wide range of image inputs including from some popular surveillance software, see below for examples:
<BR>
<BR>
Source: Synology Surveillance Station (number at the end is the camera number)
  
```
/opt/darknet/main.sh "process" "synology" "https://192.168.0.2:5001" "username" "password" "4"
```
Source: http or https (example URL is for a Foscam R2)
  
```
/opt/darknet/main.sh "process" "http" "https://192.168.0.63/cgi-bin/CGIProxy.fcgi?cmd=snapPicture2&usr=username&pwd=password"
```
Source: rtsp
  
```
/opt/darknet/main.sh "process" "rtsp" "rtsp://username:password@192.168.0.126/onvif1"
```
Source: Blue Iris
  
```
/opt/darknet/main.sh "process" "blueiris" "https://192.168.0.120:443" "USERNAME" "PASSWORD" "CAMERASHORTNAME"
```
Source: Local file
  
```
/opt/darknet/main.sh "local" "/pathtofile.jpeg"
```
<BR>

#### Sample terminal output:
<BR>
  
```
root@bigcat:/opt/darknet# /opt/darknet/main.sh "process" "synology" "https://192.168.0.2:5001" "username" "password" "4"
Predicted in 0.188455 seconds.
oven: 77%
microwave: 56%
```
<BR>

#### Set main.sh to run automatically at startup:
Paste as a single command in terminal:

```
cd /etc/init.d; wget https://raw.githubusercontent.com/ben423423n32j14e/yolo2domoticz/master/bin/mainsh; chmod 755 mainsh; update-rc.d mainsh defaults
```
<BR>

#### Using main.sh remotely over SSH:
If you want to get at the data on the host running main.sh from another machine that's running for example Domoticz, apt-get install sshpass, login via ssh at least once manually to accept the certificate then you should be able to use a command similar to below:

```
sshpass -p PASSWORD ssh USERNAME@192.168.0.43 /opt/darknet/main.sh "process" "synology" "https://192.168.0.2:5001" "username" "password" "4"
```
