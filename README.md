# AM8280_QF12
Gaining acces to the ACER Aopen QF12 cheapo led beamer, internally called am_8280_sz_qf12.
Also known as [EZCastBeam H3](https://www.ezcast.com/product/ezcast/beam/beamh3).

Actual manufacturer is Actions-Microelectronics.Co.,Ltd. also known as iEZcast or just EZcast. 


current status:

Serial shell found. 
root pw found: Actions2020!

Can boot straight into hdmi (or any other source)

Youtube Tv and dlna running. All services autostarting. 

Device supports Miracast, Google Home, AirPlay, DLNA, and YouTube TV.


# easy way in
   /mnt/usb/developHelpScript.sh is executed on usb hotplug. 
   
# serial shell
![location of uart qf12](aopen%20qf12%20uart%20small.jpg)

Connect as shown. 
115200 baud 

user: root pw: Actions2020!

# boot into hdmi
use [disp_source.app](disp_source.app.md) to switch to desired source in /etc/init.d/rcS

howto:

remount root rw

insert 'disp_source.app -c hdmi1'  into  /etc/init.d/rcS
```
mount -o rw,remount / 
echo "disp_source.app -c hdmi1 &" >> /etc/init.d/rcS
sync
mount -o ro,remount / 
```
for slightly faster boot insert the line below 'swf_mfr.app -f main_source.swf -c start &'
to save even more time you can disable the menu entirly.


# adblock needed
the device sends usage statistics to google analytics (and maybe others)
> https://ssl.google-analytics.com/collect    UA-70708060-1

can be blocked by dns. ( mount rw, unlink, edit /etc/resolv.conf, writeprotect)

# dlna and youtube
The device supports more services then normally started, call "wifi_display.app -c start &" to start them all. You can also add that line to /etc/init.d/rcS (or the developHelpScript.sh script) to launch them at start.

```
wifi_display <options>
<options>
   -q : quit wifi display
   -l : list status 
   -c <sub_options>
      start: start all wifi display services, includes DLNA Miracast AirplayMirror and EZView
      stop: stop all service
      background: start all wifi display servier in background but no action for user casting data.
      remove: removed wifi_display.app in background mode
```
it starts: <APP_Chromecast> <COM_AVAHI> <APP_Airmirror> <APP_Airplay> <APP_DLNA> <APP_EZDisplay> 

Youtube tv also works decently. Sadly its a bit crash prone, needs governor.

This hangs if started before network is up. Dont combine with booot to hdmi.




# wip log
Analyze pcb:
> Uart found!
  >with serical console!
    >.. Password protected

Portscan:
Found httpd, contains dongleinfo.json with real device name > am_8280_sz_qf12
> find update url
> fail to download firmware
> find proper parameters at https://github.com/c3c/miracast/issues/4#issuecomment-1029875282
```
curl -X POST -H "Content-type: application/json; charset=utf-8" -i https://www.iezvu.com/upgrade/ota_rx.php -d'{
        "version":      1,
        "vendor":       "am_8280_sz_qf12",
        "mac_address":  "",
        "softap_ssid":  "",
        "firmware_version":     ""
}'
```

Reply:
```
{
  "ota_conf_file": "http://cdn.iezcast.com/upgrade/am_8280_sz_qf12/official/am_8280_sz_qf12_official-1.24819.20220424.conf",
  "ota_fw_file": "http://cdn.iezcast.com/upgrade/am_8280_sz_qf12/official/am_8280_sz_qf12_official-1.24819.20220424-0xFE5D0757.gz",
  "ota_enforce": false,
  "safety_ota_conf_file": "https://cdn.iezcast.com/upgrade/am_8280_sz_qf12/official/am_8280_sz_qf12_official-1.24819.20220424.conf",
  "safety_ota_fw_file": "https://cdn.iezcast.com/upgrade/am_8280_sz_qf12/official/am_8280_sz_qf12_official-1.24819.20220424-0xFE5D0757.gz"
}
```

dl& demangle firmware using binwalk, gzip, strings.

find pw hash, yay, root:$1$4KIaBBOz$KSUnRF3PF.cNaiBiGTjBc/

crack using john 

SUCCESS, yay:  
> Actions2020!

```
VERSION_PROCESSOR = AM8280
VERSION_VENDOR = Actions-Microelectronics.Co.,Ltd.
VERSION_PRODUCT = EZ.PROJECTOR
```

# usefull links found:

 got me started  https://github.com/c3c/miracast
 
 https://github.com/c3c/miracast/issues/4 didnt quite get me to mountable image, but good enugh, confirmed dl adress
 
https://github.com/ramikg/actionsfirmware-parser  wiki got device list, didnt try the parser itself but is said to work. no need for it atm got img via tty and dd

https://github.com/rampageX/firmware-mod-kit

they tried on simmilar device, interesting how widespread those are https://alertzero.tumblr.com/post/611852056863571968/how-easy-it-is-to-hack-an-iot-device 
 
https://github.com/gipi/teardown/tree/master/MiraScreen another actions micro device some reversing of the update process
