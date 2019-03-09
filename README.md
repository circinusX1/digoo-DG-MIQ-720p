# digoo-DG-MIQ-720p
### DIGOO DG-MIQ Notes

The serial (I have no picture) pins are by the side of the micros SD card. The pins are Rx Gnd Tx, and they are 3 little soldering pads with holes.

Telnet was enabled by default.
```
telnet 192.168.1.148
Trying 192.168.1.148...
Connected to 192.168.1.148.
Escape character is '^]'.

localhost login: root
Password: cxlinux
BusyBox v1.20.2 (2017-11-07 11:47:24 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.
# 
```
First: vi /home/wps_supplicant.conf
home folder is permanent and can be changed.
```
mount
rootfs on / type rootfs (rw)
/dev/root on / type squashfs (ro,relatime)
proc on /proc type proc (rw,relatime)
tmpfs on /dev type tmpfs (rw,relatime)
tmpfs on /tmp type tmpfs (rw,relatime)
sysfs on /sys type sysfs (rw,relatime)
tmpfs on /tmp type tmpfs (rw,relatime)
devpts on /dev/pts type devpts (rw,relatime,mode=600,ptmxmode=000)
/dev/mtdblock3 on /home type jffs2 (rw,relatime)
# 

```

vi /home/wpa_supplicant.conf 
```
ctrl_interface=/var/run/wpa_supplicant
update_config=1
network={
ssid="YOUR_SSID"
key_mgmt=WPA-EAP WPA-PSK IEEE8021X NONE
pairwise=TKIP CCMP
scan_ssid=1
group=CCMP TKIP WEP104 WEP40
psk="clear_text_password"
}
# 
```
Second: configure camera with the mobile app.
Disable the cloud access: commenting all as shown
./cloud.ini & ./cloud_oversea.ini
```
[SERVERINFO]
#server_name=arcsoft.com
#xmpp_server_ip=xmpp.icloseli.com.
#relay_server_ip=relayus-w.arcsoftcloud.com
#auto_update_server_ip=update.icloseli.com.
#lecam_purchase_server_ip=esd.icloseli.com.
#upns_pnserver=upns.icloseli.com.
#upns_xmpp_name=arcsoft.com
#upns_xmpp_ip=xmpp.icloseli.com.
##argus_api_server_ip=argus.icloseli.com.
#argus_server_ip=argus.icloseli.com.
#relay_server_domain_name=relay.icloseli.com.
#stun_server_ip=
#cloud_auth_server_name=api.icloseli.com.
#bell_server_ip=
# 
```

In config.cfg screw up the ID's 

```
# cat ./config.cfg
devicename=Camera
username=youremail_form phone_registration@gmail.com
unifiedid=youremail_form phone_registration@gmail.com1552099689206
locale=1en_US
motionsensitivity=80
timezone=
cameraoffstauts=1
camerawifi=your_SSID
sounddetection=1
soundsensitivity=90
turnontime=0
unmutetime=0
hdvideo=1
cloudrecordtime=0
cameraratote=0
facedetection=0
tamperdetection=1
cloudrecordstatus=1
upnsdid=1555410
#cloudtoken=magic:****
cloudtoken=magic:screw_this-----
clouddid=SCREW_THIS
#clouddid=******
configmotiondetection=1
devled=1
devinfrared=1
devnightmode=2
checkupdateinterval=0
updateprestatus=0
volumemute=1
checkdisabledupdate=0
antiflicker=50
getrebootlog=0
updateid=
updatetype=0
updateversion=
secret=
notificationinterval=10
personcountsch=0
personstauts=0
personcounttime=0
```
Save.

Edit start.sh, Look for: <<<<
```
echo V > /dev/watchdog  ## if you don't want watchdog <<<<<<<<<<<
#/bin/wpa_supplicant -B B -Dwext -iwlan0 -c/home/wpa_supplicant.conf &
# exit # <<<<<  you can exit and run the /tmp/p2pcam   manually

cd /tmp
(
export CLOSELICAMERA_LOGMAXLINE=1000
./p2pcam 
echo -e '[data]\ntime = '`date +%s` > /home/reboot.time
if [ -f /tmp/firmware.bin ]; then
    /bin/sdc_tool -d $BOARD_ID /tmp/firmware.bin
    sync
#   reboot  ## if you don't want reboot <<<<<<<<<<<
else
    echo "Crashed ? dump log..."
    #killall -10 tees ## if you don't want killall <<<<<<<<<<<
fi
)&

```

If you did not run p2pcam you can configure your wifi form shell

```
/bin/wpa_supplicant -B -iwlan0 -Dwext -c /home/wpa_supplicant.conf &
ifconfig wlan0 192.168.1.233 netmask 255.255.255.0
route add default gw 192.168.1.1 wlan0

```

The rtsp password is in: 
```
# cat /home/hwcfg.ini 
[config]
model = CloudCam
sensor_position = 0
ir_detect_type = 2
adc_setting_max = -500
adc_setting_min = 600
support_allid = 1
support_onvif = 1
passwd = dg20160404  # <<<<   here
support_mp4record = 1# 

```
The rtsp access is:

    -   ffplay rtsp://admin:dg20160404@192.168.1.148/
    -or
    -   ffplay rtsp://admin:dg20160404@192.168.1.148/onvif1
    -worksd as well as
    -   ffplay rtsp://admin:dg20160404@192.168.1.148/onvif0




The camera wont access the cloud anymore.

You can get serrial 115200,8,N,1 on these pins

![alt text](https://github.com/comarius/digoo-DG-MIQ-720p/blob/master/digoo.png "serial")

