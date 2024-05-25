# 簡介
直接使用 Raspberry Pi 指令，進行 Mosquitto 安裝，並且對設備環境做出一定的設定。


# 快速導覽
- [簡介](#簡介)
- [快速導覽](#快速導覽)
- [前置工作](#前置工作)
  - [Raspberry Pi  本體的選擇](#raspberry-pi--本體的選擇)
  - [基本配備](#基本配備)
  - [SD 卡的格式化](#sd-卡的格式化)
    - [Windows 儲存用 SD 卡的格式化](#windows-儲存用-sd-卡的格式化)
    - [Windows 磁碟分割過的 SD 卡的格式化](#windows-磁碟分割過的-sd-卡的格式化)
    - [曾經安裝過 Pi 系統的 SD 卡的格式化](#曾經安裝過-pi-系統的-sd-卡的格式化)
- [安裝 Raspberry Pi 作業系統](#安裝-raspberry-pi-作業系統)
  - [安裝 Raspberry Pi Imager](#安裝-raspberry-pi-imager)
  - [安裝 Raspberry Pi 上的作業系統](#安裝-raspberry-pi-上的作業系統)
- [後續優化工作](#後續優化工作)
  - [DHCP IP 定址設定](#dhcp-ip-定址設定)
  - [PuTTY 遠端控制設定](#putty-遠端控制設定)
    - [關於 PuTTY](#關於-putty)
    - [安裝 PuTTY](#安裝-putty)
    - [Raspberry Pi 允許 PuTTY 連線的設定（開啟 SSH 介面）](#raspberry-pi-允許-putty-連線的設定開啟-ssh-介面)
    - [PuTTY 連線](#putty-連線)
- [結論](#結論)
  - [優點](#優點)
  - [缺點](#缺點)
  - [相關連結](#相關連結)
    - [技術參考資料](#技術參考資料)
    - [基礎參考資料](#基礎參考資料)
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


## Raspberry Pi  本體的選擇
請依照需求來選擇 Raspberry Pi 。

如果是「初次體驗」可以選擇下列型號作為練習用設備，優點是價格便宜又有基礎的完整功能（基本功能+GPIO 40 針腳）。

推薦作業系統為「Raspberry Pi OS (32-bit)」。

+ Raspberry Pi 1A+
+ Raspberry Pi 1B+
+ Raspberry Pi 2B
+ Raspberry Pi 3B
+ Raspberry Pi 3B+
+ Raspberry Pi 3A+

如果是作為「小型伺服器」使用，可以選擇下列型號，有負擔較大量訊息傳輸的能力。

推薦作業系統為「Raspberry Pi OS (32-bit)」。

+ Raspberry Pi 2B
+ Raspberry Pi 3B
+ Raspberry Pi 3B+
+ Raspberry Pi 3A+
+ Raspberry Pi 4B 2GB
+ Raspberry Pi 4B 4GB
+ Raspberry Pi 4B 8GB

如果是以「小型輕量化」為優先，則是可以選擇超輕量化的 Zero 系列。

推薦作業系統為「Raspberry Pi OS Lite (32-bit)」。

+ Raspberry Pi 1A
+ Raspberry Pi 1A+
+ Raspberry Pi 1B
+ Raspberry Pi Zero
+ Raspberry Pi Zero W (wireless LAN and Bluetooth)

如果是作為「文書用電腦」使用，可以選擇下列型號，有較強勁的性能也穩定。

推薦作業系統為「Raspberry Pi OS Full(32-bit)」。

+ Raspberry Pi 4B 2GB
+ Raspberry Pi 4B 4GB
+ Raspberry Pi 4B 8GB
+ Raspberry Pt 5

## 基本配備
這裡的建議配備有必要性跟非必要性，列表如下：

+ 必要性配備
  + [ ] 作為 Raspberry Pi 儲存媒介的 SD 卡（需格式化）
  + [ ] 可使用之鍵盤或無線鍵盤
  + [ ] 基本螢幕以及可搭配之傳輸線以及轉換線
  + [ ] USB 充電器與充電線或傳輸線
+ 非必要性配備
  + [ ] 滑鼠
  + [ ] 完整的區域網路（給予 PuTTY 遠端控制及 IOT 用）
  + [ ] 網路線或是完整的無線網路環境（更新作業系統使用）

## SD 卡的格式化
此章皆會提供三種不同情境下的 SD 卡格式化方式，同時後者對前者環境是有效的，如果不想嘗試可以直接使用「[曾經安裝過 Pi 系統的 SD 卡的格式化](#曾經安裝過-pi-系統的-sd-卡的格式化)」此方案。

SD 卡大小不限，但是依照需求還是有不同的容量建議，請自行斟酌以及查詢。

官方提供的三個作業系統，本人建議容量如下：

+ Raspberry Pi OS (32-bit)，SD Card 32GB↑
+ Raspberry Pi OS Lite (32-bit)，SD Card 8GB↑
+ Raspberry Pi OS Full (32-bit)，SD Card 64GB↑

> [!TIP]
> 如果格式化失敗，請重新把 SD 卡拔出讀卡機在插入，可能是有雜質影響讀取。

> [!TIP]
> Raspberry Pi Imager 本身有提供簡易格式化服務，但是對複雜環境、分割磁碟無效。


### Windows 儲存用 SD 卡的格式化

1. 開啟「檔案總管」到達「本機」。請按對 SD 卡右鍵選擇「格式化」。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-01-01.png)

2. 開啟格式化視窗後，選擇「開始」。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-02-01.png)

3. 跳出資料將會全部清除的警告，請按「確定」

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-03-01.png)

4. 跳出以下視窗代表完成。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-04-01.png)

> [!TIP] 
> 如果格式化失敗，請重新把 SD 卡拔出讀卡機在插入，可能是有雜質影響讀取。
 
### Windows 磁碟分割過的 SD 卡的格式化

1. 請按下 Win + R 按鍵，進入執行程序。輸入「`diskmgmt.msc`」並執行，將會開啟磁碟管理工具。
2. 在下半部的視窗，找到 USB 的 SD 卡磁碟。
3. 把所有磁碟區刪除。
     > [!TIP] 
     > 如果刪除失敗，請重新把 SD 卡拔出讀卡機在插入，可能是有雜質影響讀取。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-05-01.png)

4. 會有詢問請點擊「是」。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-06-01.png)

5. 刪除完所有磁區後選擇新增簡單磁碟區。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-07-01.png)

6. 進入設置畫面，點選下一步。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-08-01.png)

7. 磁區隨便，下一步。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-09-01.png)

8. 代號隨便，下一步。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-10-01.png)

9.  設定隨便，下一步。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-11-01.png)

10. 最終確認完成直接選擇「完成」。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-12-01.png)

11. 出現完整單一磁碟區代表成功。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-13-01.png)



### 曾經安裝過 Pi 系統的 SD 卡的格式化

1. 先到 SD 卡協會官方網站下載「[SD Memory Card Formatter for Windows Download](https://www.sdcard.org/downloads/formatter/sd-memory-card-formatter-for-windows-download/)」。
直接拉到網頁最下方，選擇「Accept」許可用戶使用條款。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-14-01.png)

2. 下載並儲存「SDCardFormatterv5_WinEN.zip」
3. 解壓縮並執行「SD Card Formatter 5.0.1 Setup.exe」
4. 同意條款
5. 安裝資料夾依照個人喜好
6. 確認完畢後直接開始安裝
7. 關閉安裝程式
8. 啟動程式

9.  開啟「SD Card Formatter」，由上而下選擇設定選項。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-20-01.png)

10. 選擇插入的 SD 卡。
11. 選擇「Overwrite format」完整格式化。
12. 點擊「Format」開始格式化。
     > [!TIP]
     > 如果格式化失敗，請重新把 SD 卡拔出讀卡機在插入，可能是有雜質影響讀取。
13. 跳出資料將會全部清除的警告，請按「是」

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-21-01.png)

14. 運作中請等待

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-22-01.png)

15. 跳出以下視窗代表完成。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-23-01.png)



# 安裝 Raspberry Pi 作業系統


## 安裝 Raspberry Pi Imager

1. 前往 [Raspberry Pi 官方網站](https://www.raspberrypi.com/software/)，下載官方的 Raspberry Pi Imager 安裝程式
     > [!NOTE]
     > Raspberry Pi Imager 是官方推出的 Raspberry Pi 作業系統安裝套件，有格式化、安裝作業系統的功能。

2. 開啟下載的 `Imager_1.8.5.exe`。
     > [!NOTE]
     > 2024/05/25 最新版本號為 1.8.5。

3. 點擊「Install」執行安裝。
4. 安裝完成時勾選啟動 Raspberry Pi Imager。


## 安裝 Raspberry Pi 上的作業系統
這裡建議採用官方提供的 Raspberry Pi OS（原名 Raspbin）以及其衍伸系列，軟硬體契合度高也方便使用。

以下教學是以已經具備可用之 Raspberry Pi、SD 卡、以及可閱覽並控制 Raspberry Pi 的操作介面（螢幕、鍵盤、滑鼠）。

1. 再來將 Raspberry Pi 用的 SD 卡放入讀卡機，並連接至個人電腦平台。

     ![Raspberry Pi Imager 啟動後畫面](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-26-01.png)

     > [!TIP]
     > 如果不小心關閉安裝程式，可透過電腦系統的搜尋尋找 Raspberry Pi Imager 來開啟。

2. 先選擇作業系統，在 Raspberry Pi Imager 上點擊「CHOOSE OS」。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-27-01.png)

3. 然後選擇下列三種作業系統的其中一項。
   + Raspberry Pi OS
     > 最推薦使用，此版本涵蓋最基本架構的 Raspberry Pi OS 功能。
   + Raspberry Pi OS Lite
     > 官方作業系統 Raspberry Pi OS 的簡化版本，不具有視窗畫面，全部以指令控制。如果 Raspberry Pi 本身只打算做為 Broker 使用，完全沒有其他功能則可以考慮。
     
     > [!TIP]
     > 該選項位於 Raspberry Pi OS (other) 分類之中。
   + Raspberry Pi OS Full
     > 此版本涵蓋最完整的 Raspberry Pi OS 功能，但是可能同時安裝了很多用不到的程式。
     
     > [!TIP]
     > 該選項位於 Raspberry Pi OS (other) 分類之中。

   > [!WARNING]
   > 本教學是以上面三種作業系統為基礎撰寫，使用其他平台也可以安裝，但是可能步驟會有差異。

4. 在 Raspberry Pi Imager 上點擊「CHOOSE STORAGE」。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-29-01.png)

5. 然後選擇讀卡機的磁碟。
6. 點擊「WRITE」執行安裝。
7. Raspberry Pi Imager 會自動格式化 SD 卡再安裝。
8. 他會跳出警告說這會清除 SD 內既有資料，這時候選擇「YES」就會開始。
9. 安裝時請等待，程式需要格式化、下載作業系統、安裝作業系統，這至少會需要 20 分鐘以上。

     > [!TIP]
     > 如果安裝失敗，請重新把 SD 卡拔出讀卡機在插入，可能是有雜質影響讀取。

10. 顯示以下畫面代表安裝完畢。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-33-01.png)

11. 安裝完畢後，從個人電腦上退出 SD 卡。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-34-01.png)

     > [!NOTE]
     > 非必要操作，尤其是在 Windows11。
     >
     > 但是為了確保程式正常運行還是建議施行本步驟。


12. 並把 SD 移動到 Raspberry Pi ，同時 Raspberry Pi 接上螢幕、鍵盤等周邊配備。
13. 開啟 Raspberry Pi 。
14. 等待 Raspberry Pi 跑完初始化之後，就可以進行 Raspberry Pi 的基本設定，這部分依照系統指示即可。
15. 設定完成後等待 Raspberry Pi 更新。
16. 更新完成後作業系統就算安裝完畢。

# 後續優化工作
以下步驟是針對以後控制、維護、管理方便的設定。

## DHCP IP 定址設定
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

## PuTTY 遠端控制設定

### 關於 PuTTY 
PuTTY 是一款整合虛擬終端、系統控制台和網路檔案傳輸為一體的自由及開放原始碼的程式。
這裡可當作就是一種可以以指令碼方式，遠端控制其他設備的開源程式。

### 安裝 PuTTY

1. 前往[官方網站](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)下載 PuTTY 安裝程式「`putty-64bit-0.81-installer.msi`（64 位元用）」。
     > [!IMPORTANT]
     > 如果是 32 位元 Windows 作業系統請下載「`putty-0.81-installer.msi`」。

     > [!TIP]
     > 以上選項皆在下圖紅框處。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/PiOs-windows11-20240525-02-mark-01.png)


2. 開啟您的 PuTTY 安裝程式，理論上會看到類似於下圖的視窗，請點選 Next 進下一步。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-37-01.png)

3. 接著是設定安裝資料夾的頁面，可以不用管它，請點選 Next 進下一步。
4. 為了方便開啟 PuTTY ，請點選 這裡 更改設定，讓程式自動建立桌面捷徑。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-39-01.png)

5. 設定完畢即可點選 Install 開始安裝。
6. 接著是安裝的頁面，這時就請等待它安裝完成，通常不會太久。
7. 看到這畫面代表順利安裝完成，同時可以點選 Finish 結束安裝程式。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-41-01.png)


### Raspberry Pi 允許 PuTTY 連線的設定（開啟 SSH 介面）
PuTTY是透過 SSH 協議來傳輸指令的，所以 Raspberry Pi 需要把預設關閉的 SSH 介面開啟。

1. 開啟 Raspberry Pi 的 console （命令列模式）。
2. 先進入 Raspberry Pi 設定，輸入以下指令：`sudo raspi-config`
3. 選擇「Interfacing Options」選項。
4. 導向並選擇「SSH」選項。
5. 選擇「YES」。
6. 選擇「OK」。
7. 選擇「Finish」。


### PuTTY 連線
> [!IMPORTANT] 
> 先確認你的 Raspberry Pi 和 PC 端都已經透過有線或是無線的方式連線到同一個區域網路。
> 
> 先確認你的 Raspberry Pi 已經開啟 SSH 介面。

1. 請點擊 PC 端的 PuTTY 應用程式。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-42-01.png)

2. 開啟 PuTTY 後，可以設定以下資訊。

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-43-01.png)

3. 設定完畢後點擊「Open」開始連線。
     > [!TIP] 
     > 如果跳出錯誤訊息，請檢察設定與網路狀態。
     > 
     > 如果是第一次連線，此時會跳出安全警告，請點擊「是」。
     > 
     > ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-45-01.png)

4. 如果 Raspberry Pi 有設定密碼，會出現 `login as:` 訊息要求帳號密碼，沒有的話請跳過此步驟。
   1. 在這後面輸入 Raspberry Pi 的使用者帳號，並按下「Enter」。
     ```cmd
     login as: **這裡是使用者帳號**
     ```
   2. 在這後面輸入 Raspberry Pi 的使用者密碼（不會顯示），並按下「Enter」。
     ```cmd
     **這裡是使用者帳號**@192.168.10.xxx’s password: 
     ```
     > [!TIP]
     > 「`**這裡是使用者帳號**`」會是你輸入的使用者帳號，「192.168.10.xxx」是代表你連線到的Raspberry Pi 設備的 IP。

5. 如果登入成功會顯示下列訊息，此時你可以直接輸入指令開始控制 Raspberry Pi。
     ```cmd
     **這裡是使用者帳號**@**Raspberry Pi設備名稱**:~ $ ▋ 
     ```
     範例 A：
     ```cmd
     lmk999999@DHT22-Pi:
     ```
     範例 B：

     ![](./image/Raspberry%20Pi%20上的作業系統安裝指南/oldimage-windows10-20240525b-46-01.png)
	
# 結論

## 優點

+ 目前懶得寫，抱歉。


## 缺點

- 目前懶得寫，抱歉。


## 相關連結


### 技術參考資料
以下網站包含本文章所教學的內容，本教學是從其中萃取出並測試，然後重新敘述。

+ [PuTTY 安裝教學](https://docs.google.com/presentation/d/1I7oCONC9mEQWUkqMunSk_8OyVYmh15MbvSMthvGe8fI/)


### 基礎參考資料
閱讀本文張前請具備以下資料中的基礎能力與設備。

+ 暫無


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