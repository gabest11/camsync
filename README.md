# camsync
PHP script to download recordings from reolink and yihack cameras to a local directory.

### Example

I have something similar in my crontab on a router running openwrt and an ssd connected to its usb port. The script should also work on Windows.

```
timeout 29m ~/camsync.php --yihack="http://yicam:password@192.168.0.2:8080/" -d /mnt/usb/camera/yicam -l /tmp/camsync_yicam.flock -h 72 2>&1 >> /mnt/usb/camera/yicam.log
timeout 29m ~/camsync.php --reolink="http://admin:password@192.168.0.3/" -d /mnt/usb/camera/reolink -l /tmp/camsync_reolink.flock -h 72 --throttle=500 2>&1 >> /mnt/usb/camera/reolink.log
```

Recommended to use `timeout` to kill stuck downloads.

```
-d target directory
-h hours to download and auto-delete older files from the local mirror
-l lock file to prevent multiple runs (optional)
--throttle KB/s, this only works with reolink, as HD files can kill wifi, this isn't really a problem with yi-hack
--parallel number of curl session to spawn, also only for reolink, they throttle http, but be careful, >4 can crash the camera
--reolink http server has to be enabled on the camera
--yihack needs ffmpeg to remux bogus mp4 files
```

# camtimelapse

Once you have the video files downloaded from the camera, you can create a timelapse with this script. It can also process the directory structure of the automatic FTP uploads.

### Example

```
php camtimelapse.php -i camsynclocaldir -o . -f 5 -c nvidia -h 24
```

```
-i input directory where the video files are
-o output directory
-h hours from now, may be in HH:MM format
-m minutes
-l duration in seconds
-c codec to use (amd => hevc_amf, nvidia => h264_nvenc), default is libx264 
-f number of frames to skip
-v verify video files before adding them to the list
```
