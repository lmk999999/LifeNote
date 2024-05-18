# 簡介
以下是本人透過 `factoriotools/factorio` ，在 `Windows10` 作業系統上開啟 `Factorio` 的 `Docker` 版伺服器的筆記內容。


# 快速導覽

- [簡介](#簡介)
- [快速導覽](#快速導覽)
- [系統環境](#系統環境)
- [安裝](#安裝)
- [設定](#設定)
  - [設定伺服器設定](#設定伺服器設定)
  - [設定模組](#設定模組)
- [運行](#運行)
- [結論](#結論)
  - [優點](#優點)
  - [缺點](#缺點)
  - [相關連結](#相關連結)
  - [筆者](#筆者)
    - [主筆](#主筆)
    - [協助檢稿](#協助檢稿)


|
[回到快速導覽](#快速導覽)
|
[回到遊戲類別](../Game.md)
|
[回到筆記說明](.../README.md)
|


# 系統環境

+ 作業系統：`Windows 10`
+ 安裝程式：`Docker Desktop`


# 安裝
請依照以下步驟安裝好 Docker，並執行基本容器。

1. 下載並安裝 Docker Desktop。
   1. 前往 Docker 的[官方網站](https://www.docker.com/products/docker-desktop/)。
   2. 點擊 `Download For Windows` 下載 `Docker Desktop Installer.exe` 檔案。
   3. 執行下載的 `Docker Desktop Installer.exe` 檔案。
   4. 按照 ``Docker Desktop Installer.exe` 檔案的步驟安裝 Docker Desktop。
   5. 結束安裝程式。
2. 建立要放置 Factorio 伺服器相關資料的資料夾。
   +  路徑位置可以隨意挑選，但是以防萬一建議路徑全英文。
   +  路徑的容量建議至少有 10 GB 的可用空間。
3. 在該資料夾內進行後續操作。
4. 下載 `factoriotools/factorio` 的 image。
   1. 在該資料夾內開啟 `cmd`（`cmd` 的執行路徑記得要在該資料夾）。
   2. 在 `cmd` 輸入指令：`docker pull factoriotools/factorio:stable`。
   3. 等待 `cmd` 的指令完成即下載完成。
5. 使用 image 加入參數執行 Docker。
   1. 在該資料夾內開啟 `cmd`（`cmd` 的執行路徑記得要在該資料夾）。
   2. 在 `cmd` 輸入指令：`docker run -d -p 34197:34197/udp -p 27015:27015/tcp -v .\indocker\factorio:/factorio --name `*`這裡輸入Docker容器的名稱`*` --restart=unless-stopped factoriotools/factorio:stable`。
   3. `cmd` 的指令完成後代表 Docker 的容器已經在執行，但是伺服器不一定完成了。
6. 這時候開啟 Docker Desktop 的應用程式。
7. Docker Desktop 左邊的選單選擇 `Containers`。
8. Docker Desktop 右邊的列表選擇你剛剛在指令內輸入的 *`Docker容器的名稱`*。
9. 查看 `Logs` 分頁的內容，確認容器的狀態。
10. 可透過右上角的 `⏹️` 圖示來停止 Docker 容器。


# 設定

## 設定伺服器設定
Factorio 的伺服器要有基本設定才能繼續順利地執行，因此請設定基本參數。

1. 前往路徑 `$你放置 Factorio 伺服器相關資料的資料夾$indocker\factorio\config` 的資料夾。
2. 打開 `server-settings.json` 設定伺服器的設定。


## 設定模組
當然，這個 Factorio 伺服器也可以安裝模組。
> [!TIP]
> 模組可以從個人版的遊戲中下載後轉移過去。

1. 前往路徑 `$你放置 Factorio 伺服器相關資料的資料夾$indocker\factorio\mods` 的資料夾。
2. 放入你的模組。


# 運行

1. 開啟 Docker Desktop 的應用程式。
2. Docker Desktop 左邊的選單選擇 `Containers`。
3. Docker Desktop 右邊的列表選擇你剛剛在指令內輸入的 *`Docker容器的名稱`*。
4. 透過右上角的 `▶️` 圖示來開始運行 Docker 容器。


# 結論

## 優點

+ Docker 運行的伺服器不容易因為 Windows 本身的小問題（遠端連線中斷、螢幕保護程式）斷線。
+ Docker 運行的伺服器不需要設定 Windows 環境。
+ Docker 運行的伺服器理論上可以跨平台，但是筆者還沒試過。


## 缺點

- 目前筆者這方案所使用的 image 由第三方維護，所以隨時都可能失效。


## 相關連結

+ [factoriotools](https://hub.docker.com/r/factoriotools/factorio/) on DockerHub
+ [factoriotools](https://github.com/factoriotools/factorio-docker) on GitHub


## 筆者
### 主筆
Lmk999999

### 協助檢稿
無


---

|
[回到快速導覽](#快速導覽)
|
[回到遊戲類別](../Game.md)
|
[回到筆記說明](.../README.md)
|