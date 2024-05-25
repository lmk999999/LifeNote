# 簡介

直接使用 Raspberry Pi 指令，進行 Mosquitto 安裝，並且對設備環境做出一定的設定。


# 快速導覽
- [簡介](#簡介)
- [快速導覽](#快速導覽)
- [前置工作](#前置工作)
  - [Raspberry Pi 的準備](#raspberry-pi-的準備)
    - [挑選 Raspberry Pi](#挑選-raspberry-pi)
    - [安裝作業系統](#安裝作業系統)
- [安裝 Mosquitto](#安裝-mosquitto)
  - [更新 APT](#更新-apt)
  - [安裝 Mosquitto](#安裝-mosquitto-1)
  - [關閉安全模式並開啟外部連線權限](#關閉安全模式並開啟外部連線權限)
    - [其他可用相關指令](#其他可用相關指令)
    - [Mosquitto-clients 單平台測試](#mosquitto-clients-單平台測試)
  - [後續優化工作](#後續優化工作)
    - [DHCP IP 定址設定](#dhcp-ip-定址設定)
- [結論](#結論)
  - [優點](#優點)
  - [缺點](#缺點)
  - [相關連結](#相關連結)
    - [技術參考資料](#技術參考資料)
    - [基礎參考資料](#基礎參考資料)
      - [MQTT教學（by cubie）](#mqtt教學by-cubie)
  - [筆者](#筆者)
    - [主筆](#主筆)
    - [協助檢稿](#協助檢稿)

|
[回到快速導覽](#快速導覽)
|
[回到主題簡介](./Raspberry%20Pi.md)
|
[回到程式類別](../Program.md)
|
[回到筆記說明](.../README.md)
|

# 前置工作
開始 Mosquitto 的安裝前需要準備的工作。

如果已經有完整可用的 Raspberry Pi 可以跳過此章節。


## Raspberry Pi 的準備
這裡將從 Raspberry Pi 的安裝、設定與構建逐步開始教學。


### 挑選 Raspberry Pi 
Mosquitto 是 MQTT 當中扮演 Broker 角色的伺服器套件，依照加入 Broker 的設備越多，處理相應也會提升。

因此挑選擔當此責任的 Raspberry Pi ，以處理器性能為優先，然後整體性能越高越好。
非常不建議使用 Pi Zero 系列或是 Pi Pico 系列做為 Broker 使用，這兩個系列的處理能力能負擔的設備與主題數量極低。

個人建議，作為 Mosquitto 載體的樹梅派推薦以下版本，會有較好的處理能力。經簡單測試，面對 Qos=0 有大約有瞬間 5000 件左右的處理量，Qos=1 則是有 50 件左右的處理量。
+ Raspberry Pi 3B（1GB）
+ Raspberry Pi 3B+（1GB）
+ Raspberry Pi 4B（2/4/8 GB）
至於最新的 Raspberry Pi 5 尚未獲得測試過，因此無法提供數據。


### 安裝作業系統
這裡建議採用官方提供的 Raspberry Pi OS（原名 Raspbin）以及其衍伸系列，軟硬體契合度高也方便使用。

以下教學是以已經具備可用之 Raspberry Pi、SD 卡、以及可閱覽並控制 Raspberry Pi 的操作介面（螢幕、鍵盤、滑鼠）。

請參考「[Raspberry Pi 上的作業系統安裝指南](./Raspberry%20Pi%20上的作業系統安裝指南.md)」，並且強烈建議安裝時請選擇下列三種作業系統的其中一項：

+ Raspberry Pi OS
  > 最推薦使用，此版本涵蓋最基本架構的 Raspberry Pi OS 功能。
+ Raspberry Pi OS Lite
  > 官方作業系統 Raspberry Pi OS 的簡化版本，不具有視窗畫面，全部以指令控制。如果 Raspberry Pi 本身只打算做為 Broker 使用，完全沒有其他功能則可以考慮。
+ Raspberry Pi OS Full
  > 此版本涵蓋最完整的 Raspberry Pi OS 功能，但是可能同時安裝了很多用不到的程式。

> [!IMPORTANT]
> 本教學是以上面三種作業系統為基礎撰寫，使用其他平台也可以安裝但是可能步驟會有差異。


# 安裝 Mosquitto


## 更新 APT
以下步驟為更新 APT 套件，確定整個 Raspberry Pi 處於最新版本的環境中。

1. 開啟 Raspberry Pi 的 console （命令列模式）。
2. 更新 APT 套件本身，輸入以下指令：`sudo apt-get update`
3. 等待更新完成。
4. 更新 APT 當中的既有套件，輸入以下指令：`sudo apt-get upgrade`
5. 等待更新完成。
6. APT 全部更新完成。


## 安裝 Mosquitto
此步驟是將 Mosquitto 安裝到 Raspberry Pi 上的詳細步驟。

1. 開啟 Raspberry Pi 的 console （命令列模式）。
2. 安裝 Mosquitto 本體，輸入以下指令：`sudo apt-get install mosquitto`

     > [!TIP] 
     > 如果有需要，也可以順便安裝官方所提供的客戶端套件「mosquitto-clients」，但是此套件只支援透過 Console 指令收發。指令改成如下：
     > 
     > `sudo apt-get install mosquitto mosquitto-clients`
     > 
     > 該指令系列可以用於簡易測試。

3. 等待安裝完成。
4. 檢查 Mosquitto 是否安裝並執行，輸入以下指令：`service mosquitto status`
5. 如果出現綠色的「active (running)」代表 Mosquitto 安裝成功並且正確運行。
6. 狀態正常則安裝全部完成。

> [!NOTE]
> 不正常請至步驟 2 重新開始。
> 
> 仍不行可能是 APT 套件錯誤，請重新執行 APT 更新步驟。
> 
> 還是不行請檢察網路環境與設備狀況。


## 關閉安全模式並開啟外部連線權限
從 Mosquitto 2.0.0 版本開始，安裝完成的 Mosquitto 會優先進入安全模式，只接受 Localhost 的連線。

因此需要針對這狀況增加連線權限，或是關閉安全模式。

1. 開啟 Raspberry Pi 的 console （命令列模式）。
2. 開啟 Mosquitto 設定檔：`sudo nano /etc/mosquitto/mosquitto.conf`
3. 開啟預設 1883 的對外聆聽，同時允許匿名
     ```conf
     listener 1883 
     allow_anonymous true
     ```
     或者允許其他特定埠、特定 IP 的全部訊息（未測試）
     ```conf
     listener <你要的連接埠>
     bind_address <訂閱者或發布者的IP位址>
     ```
     或者只允許區域網路的全部訊息（未測試）
     > 透過指定網路介面卡實現，如果是透過 wi-fi 要改成 wlan0。（未測試）
     ```conf
     listener 1883
     bind_interface eth0
     ```
4. 完成的 mosquitto.conf 文件類似下表（黃色是追加的部分）
     ```conf
     # Place your local configuration in /etc/mosquitto/conf.d/
     #
     # A full description of the configuration file is at
     # /usr/share/doc/mosquitto/examples/mosquitto.conf.example

     pid_file /run/mosquitto/mosquitto.pid

     persistence true
     persistence_location /var/lib/mosquitto/

     log_dest file /var/log/mosquitto/mosquitto.log

     listener 1883
     allow_anonymous true

     include_dir /etc/mosquitto/conf.d
     ```
5. 之後重啟服務，如果重啟失敗代表設定有錯誤：

     `sudo service mosquitto stop`

     `sudo service mosquitto start`


### 其他可用相關指令

+ 顯示 Mosquitto 的版本資訊，並且會註解是否開啟安全模式：`mosquitto -v`
+ 開啟 Mosquitto 設定檔：`sudo nano /etc/mosquitto/mosquitto.conf`
+ 找出正在監聽的 Port：`netstat -tulpn | grep LISTEN`
+ 找出 Port 的使用狀況：`netstat -tulpn | grep :80`
+ 網路設定文件：`sudo nano /etc/dhcpcd.conf`

### Mosquitto-clients 單平台測試
此步驟是給予有安裝 Mosquitto-clients 的使用者進行測試的步驟，以確認在同一設備內部 Mosquitto 是否也正常運作。

1. 開啟第一個 Raspberry Pi 的 console （命令列模式）。
2. 並使其進入訂閱者監聽狀態，輸入以下指令：`mosquitto_sub -t "#"`
     > [!NOTE] 
     > **指令解釋**
     >
     > 「-t」代表指定主題，「#」代表訂閱全部主題。

3. 開啟第二個 Raspberry Pi 的 console （命令列模式）。
4. 發送單一訊息，輸入以下指令：`mosquitto_pub -h <linux_ip> -p 1883 -t "test/MQTT" -m "hello! this is a test."`
     > [!NOTE] 
     > **指令解釋**
     >
     > 「-h」代表指定 IP，請填寫 Broker 主機的 IP，「<linux_ip>」代表IP位址，如果為同一設備可以跳過不填寫。
     > 
     > 「-t」代表指定連接埠，「1883」是預設的連接埠。
     > 
     > 「-t」代表指定主題，「joy/test」是隨便填寫的。
     > 
     > 「-m」代表傳送內容，「“ hello! this is a test.”」就是傳送的內容。

5. 如果第一個 Raspberry Pi 的 console （命令列模式）有收到訊息，代表成功。


## 後續優化工作
以下步驟是針對以後控制、維護、管理方便的設定。

### DHCP IP 定址設定
將 Broker 定址，協助我們日後設備連線的方便。

1. 開啟 Raspberry Pi 的 console （命令列模式）。
2. 透過 NANO 編輯器進入 DHCP 設定檔，輸入以下指令：`sudo nano /etc/dhcpcd.conf`

     > [!WARNING] 
     > 請確認是否有既有設定，有的話建議將其全部註解（文句前端追加 #）以防萬一。

3. 確認 Raspberry Pi 以後主要使用的網路介面、環境、IP 位址後輸入以下資料：
   1. 輸入網路介面。
      1. 如果是走「有線網路」則輸入`interface eth0`
      2. 如果是走「無線網路」則輸入`interface wlan0`
   2. 請之照您的區域網路來選擇，並輸入固定 IP 位址：`static ip_address = 192.168.1.150/24`
      > 以「192.168.1.150」為範例，「/24」代表遮罩不可遺漏。
   3. 請之照您的區域網路來選擇，並輸入路由器 IP 位址：`static routers=192.168.1.1`
   4. 請之照您的區域網路來選擇，並輸入DNS IP 位址：`static domain_name_servers=192.168.1.1 8.8.8.8`
      > 通常 DNS 直接指向路由器即可，以防萬一可以追加 8.8.8.8 的 Google DNS 服務。

   5. 完成後大致上會像是以下結果：
      ```conf
      interface wlan0
      static ip_address=192.168.1.150/24
      static routers=192.168.1.1
      static domain_name_servers=192.168.1.1 8.8.8.8
      ```

4. 儲存檔案並取代舊有檔案，之後重啟 Raspberry Pi 即可。


# 結論

## 優點

+ 目前懶得寫，抱歉。


## 缺點

- 目前懶得寫，抱歉。


## 相關連結

### 技術參考資料

以下網站包含本文章所教學的內容，本教學是從其中萃取出並測試，然後重新敘述。

+ [Eclipse Mosquitto](https://mosquitto.org/)
+ [樹莓派安裝 Mosquitto 輕量級 MQTT Broker 教學，連接各種物聯網設備](https://blog.gtwang.org/iot/raspberry-pi/raspberry-pi-mosquitto-mqtt-broker-iot-integration/)
+ [樹莓派使用 MQTT 與 Windows, Ubuntu 進行通訊](https://medium.com/ching-i/樹莓派使用-mqtt-與-windows-ubuntu-進行通訊-84f4fa10dd4b)


### 基礎參考資料
閱讀本文張前請具備以下資料中的基礎能力與設備。

#### MQTT教學（by cubie）
+ [（一）：認識MQTT](https://swf.com.tw/?p=1002)
+ [~~（二）：安裝MQTT伺服器Mosquitto，Windows系統篇~~](https://swf.com.tw/?p=1005)
+ [~~（三）：安裝MQTT伺服器Mosquitto，macOS系統篇~~](https://swf.com.tw/?p=1007)
+ [（四）：使用MQTTLens訂閱與發布MQTT訊息](https://swf.com.tw/?p=1009)
+ [（五）：「保留」發布訊息以及QoS品質設定](https://swf.com.tw/?p=1015)
+ [~~（六）：使用PubSubClient程式庫開發Arduino MQTT應用~~](https://swf.com.tw/?p=1021)
+ [~~（七）：使用Node.js訂閱MQTT訊息~~](https://swf.com.tw/?p=1023)
+ [~~（八）：使用MQTTlens上傳資料到ThingSpeak的MQTT伺服器~~](https://swf.com.tw/?p=1087)
+ [~~（九）：使用ESP8266上傳資料到ThingSpeak MQTT伺服器~~](https://swf.com.tw/?p=1089)
+ [（十）：設定Mosquitto 2.x版的mosquitto.conf設置檔](https://swf.com.tw/?p=1473)


## 筆者

### 主筆
Lmk999999


### 協助檢稿
無


---

|
[回到快速導覽](#快速導覽)
|
[回到主題簡介](./Raspberry%20Pi.md)
|
[回到程式類別](../Program.md)
|
[回到筆記說明](.../README.md)
|


