# AM8280_QF12
Gaining acces to the ACER Aopen QF12 cheapo led beamer, also known as am_8280_sz_qf12 .
Actual manufacturer seems to be Actions-Microelectronics.Co.,Ltd. 

# tldr
current status: Serial shell found. root pw found: Actions2020!
```
VERSION_PROCESSOR = AM8280
VERSION_VENDOR = Actions-Microelectronics.Co.,Ltd.
VERSION_PRODUCT = EZ.PROJECTOR
```

#boot into hdmi
log in as root
remount root rw
insert 'disp_source.app -c hdmi1'  into  /etc/init.d/rcS
```
mount -o rw,remount / 
echo "disp_source.app -c hdmi1" >> /etc/init.d/rcS
reboot
```

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


# usefull links found:
 got me started  https://github.com/c3c/miracast
 https://github.com/c3c/miracast/issues/4 didnt quite get me to mountable image, but good enugh, confirmed dl adress
https://github.com/ramikg/actionsfirmware-parser > wiki
> they tried on simmilar device https://alertzero.tumblr.com/post/611852056863571968/how-easy-it-is-to-hack-an-iot-device 
