Git 的基本操作教學
=========

# 快速導覽

- [Git 的基本操作教學](#git-的基本操作教學)
- [快速導覽](#快速導覽)
- [了解 Git 概念](#了解-git-概念)
  - [基本概念](#基本概念)
  - [Git、GitHub](#gitgithub)
- [對於 Git 的個人見解](#對於-git-的個人見解)
  - [分支](#分支)
    - [個人分支習慣](#個人分支習慣)
      - [Master 分支](#master-分支)
      - [hotfix 分支](#hotfix-分支)
      - [develop 分支](#develop-分支)
      - [其他分支](#其他分支)
  - [提交](#提交)
  - [合併](#合併)
- [Git 指令操作說明](#git-指令操作說明)
  - [常用指令](#常用指令)
    - [git init](#git-init)
  - [Git 帳號與權限設定](#git-帳號與權限設定)
  - [複製遠端的專案](#複製遠端的專案)
  - [專案第一次 PUSH 到 Gitea 該怎麼操作](#專案第一次-push-到-gitea-該怎麼操作)
  - [專案新增提交](#專案新增提交)
  - [分支合併](#分支合併)
- [結論](#結論)
  - [筆者](#筆者)
    - [主筆](#主筆)
    - [協助檢稿](#協助檢稿)

|
[回到快速導覽](#快速導覽)
|
[回到主題簡介](./Git.md)
|
[回到程式類別](../Program.md)
|



# 了解 Git 概念

## 基本概念

Git 是一種分佈式版本控制系統，用於跟蹤和管理源代碼變更。由 Linus Torvalds 創建，主要用於軟件開發。Git 的核心概念包括：

+ 倉庫 (Repository)：存儲所有文件及其變更歷史的地方，可以是本地或遠程。
+ 提交 (Commit)：一次變更的記錄，每個提交都有唯一的哈希值。
+ 分支 (Branch)：從主線分離出的獨立開發線路，允許並行工作。
+ 合併 (Merge)：將分支上的變更合併回主分支或其他分支。
+ 克隆 (Clone)：從遠程倉庫複製一個本地副本。
+ 暫存區 (Staging Area)：暫存已修改但還未提交的變更。

Git 支持離線工作，每個副本都是完整的倉庫。常用指令包括 git init (初始化倉庫)、git add (添加變更到暫存區)、git commit (提交變更)、git push (推送到遠程倉庫)、git pull (拉取並合併遠程變更)。Git 提供強大的分支和合併功能，使團隊協作和並行開發更加高效。

[回到快速導覽](#快速導覽)


## Git、GitHub
Git 是一種版本管理工具、一種方案，當你在電腦安裝了 Git 就能夠使用，並且可以用它管理各種檔案，並且確保檔案的備份。

而 GitHub 則是一個支援 Git 儲存的網站、平台，使用者可以把自己使用 Git 所管理的內容上傳到 GitHub 進行公開、分享、或備份。除此之外，還有 Gitea 這種可以自架伺服器備份與管理的方案。

:::mermaid
graph LR

   subgraph local["本地端"]
      subgraph os["作業系統"]
         subgraph git["Git 工具"]
            bl1["專案A"]
         end
      end
   end

   subgraph cloud["雲端"]
      subgraph web["GitHub"]
         bw1["專案A"]
      end
   end

   bl1 ---> |git push| bw1
   bw1 ---> |git pull| bl1
:::

[回到快速導覽](#快速導覽)


# 對於 Git 的個人見解

Git 是一種常見的版本控制方案，雖然有很多複雜的功能，但是個人認為主要搞懂分支的概念就可以開始上手，跟我一樣的初學者先把詳細的功能先拋諸腦後，先了解大致的構成流程優先最好。

讓我從外往內解釋...

[回到快速導覽](#快速導覽)


## 分支

Git 的分支用於區分「`做甚麼`」。就像工廠的流水線一樣，在同一條線上就是製作同一個產品。

> [!TIP]
> 你不會在流水線的頭放入蛋糕的材料，結果流水線最後出來的是鋼鐵吧？

而 Git 的分支（`branch`）就是那個流水線，只是在完成這條線的目標前，你不知道中間會有幾道工序而已。

:::mermaid
gitGraph

   commit id:"工廠"

   branch "蛋糕流水線"
   commit id:"打蛋功能"
   commit id:"調味功能"
   commit id:"蛋糕造型功能"
   commit id:"蒸烤功能"

   checkout main
   merge "蛋糕流水線"
   commit id:"蛋糕工廠"
:::

當然也不一定只有一個分支，畢竟工廠也不一定只有一個流水線。
除此之外，如果不同的分支用到同一段功能程式碼，也可以先做好該功能之後再分支。

:::mermaid
gitGraph

   commit id:"工廠"

   branch cake
   commit id:"打蛋功能"
   commit id:"調味功能"

   branch bread
   commit id:"麵包造型功能"
   commit id:"發酵功能"
   commit id:"麵包蒸烤功能"

   checkout main
   merge bread
   commit id:"麵包工廠"

   checkout cake
   commit id:"蛋糕造型功能"
   commit id:"蛋糕蒸烤功能"

   checkout main
   merge cake
   commit id:"糕點工廠"
:::

[回到快速導覽](#快速導覽)


### 個人分支習慣

#### Master 分支
或是 `main` 分支，是 `正式發布版` 的主要分支，該分支下的所有檔案都是正式發布的內容。

> [!WARNING]
> 請注意該分支不要隨意合併與覆蓋，會影響到現在運行中的所有設備。

:::mermaid
gitGraph

   commit id:"正式版 v1.0"
   commit id:"正式版 v1.1"
   commit id:"正式版 v1.2"
   commit id:"正式版 v1.3"

:::

[回到快速導覽](#快速導覽)


#### hotfix 分支
`維護版` 的分支，該分支在本專案的管理規劃上來源一定是 `master` 分支，並且是唯一用於該用途的分支。並且，該分支用於各種 BUG 維護用，不增加功能，目標快速修整修完畢後合併到 `Master`。用於應對小型 BUG 以及需要立即處理的 BUG。

:::mermaid
gitGraph

   commit id:"正式版 v1.0"
   branch hotfix
   commit id:"修正 #1"
   commit id:"修正 #2"
   checkout main
   merge hotfix
   commit id:"正式版 v1.3"
:::

[回到快速導覽](#快速導覽)


#### develop 分支
`開發版` 的分支，該分支在本專案的管理規劃上來源一定是 `master` 分支，並且是唯一用於該用途的分支。並且，該分支的更改涉及功能的增減、樣式的改善、以及需求的配合追加這類的更新，不求立即快修所用的分支。

:::mermaid
gitGraph

   commit id:"正式版 v1.0"
   branch develop
   commit id:"功能追加 A"
   commit id:"功能追加 B"
   checkout main
   merge develop
   commit id:"正式版 v1.3"
:::

[回到快速導覽](#快速導覽)


#### 其他分支
該類別的分支，來源不一定是 `master` 分支。該類別分支涉及重大版本跟新、翻修、或是版本飛躍時就會採用，本專案規劃上會特別獨立一個分支來進行處理。除此之外，多次的重大更新也會產生相應的分支數目，方便針對每次的重大更新進行追訴探討。

:::mermaid
gitGraph

   commit id:"正式版 v1.0"
   branch Update2
   commit id:"改善優化"
   commit id:"功能追加"
   commit id:"錯誤修正"
   commit id:"顯示翻新"
   checkout main
   merge Update2
   commit id:"正式版 v2.0"
:::

[回到快速導覽](#快速導覽)


## 提交
從 Git 圖形來看，其中每一個圓點都是一次提交（`commit`），一次次的提交就是專案依次次更新的版本，所以版本管理的目的就是管理這些提交。Git 允許透過指令回溯到同一個分支上的任意一次提交，並且也可以回溯後建立新的分支進行管理。

這就是 Git 的管理核心，在各種提交間反覆跳躍。

[回到快速導覽](#快速導覽)


## 合併
Git 合併（`merge`）是一個將兩個或多個分支的變更合併到一個分支的過程。
這通常用於在分支完成一項功能或修復後，將其合併回主分支（如 `main` 或 `master`），以使這些變更成為主分支的一部分。

:::mermaid
gitGraph

   commit id:"原本程式的穩定發布版（也是提交）"

   branch "使用者用不到，開發者在開發的版本"
   commit id:"這裡是第一次提交"
   commit id:"這裡是第二次提交"
   commit id:"這裡是第三次提交"
   commit id:"這裡是第四次提交"

   checkout main
   merge "使用者用不到，開發者在開發的版本" id:"這裡就是合併"
   commit id:"更新發布版（也是提交）"

:::

[回到快速導覽](#快速導覽)


# Git 指令操作說明

## 常用指令

|指令|分類|說明|參考|
|:---|:---:|:---|:---:|
| `git init`|初始化|初始化一個新的 Git 倉庫|[說明章節](#git-init)
| `git clone [url]`|遠端操作|克隆遠程倉庫到本地|[參考章節](#複製遠端的專案)
| `git status`|查詢|顯示工作區和暫存區的狀態|[參考章節](#專案新增提交)
| `git add [file]`|管理|添加指定文件到暫存區|[參考章節](#專案新增提交)
| `git add .`|管理|添加當前目錄下的所有變更到暫存區|[參考章節](#專案新增提交)
| `git commit -m "message"` |管理|提交暫存區的變更並附上提交信息|[參考章節](#專案新增提交)
| `git commit -a -m "message"` |管理| 跳過 `git add` 步驟，直接提交所有已跟蹤的文件變更|[參考章節](#專案新增提交)
| `git push`|遠端操作| 推送本地提交到遠程倉庫|[參考章節](#專案第一次-push-到-gitea-該怎麼操作)
| `git pull`|遠端操作| 拉取並合併遠程倉庫的變更到本地|參考章節](#專案第一次-push-到-gitea-該怎麼操作)
| `git fetch`|遠端操作| 從遠程倉庫下載所有分支的最新變更，但不合併|參考章節](#專案第一次-push-到-gitea-該怎麼操作)
| `git merge [branch]`|管理| 將指定分支合併到當前分支|
| `git branch`|管理| 列出所有本地分支|
| `git branch [branch-name]`|管理| 創建新分支|
| `git checkout [branch]`|管理| 切換到指定分支|
| `git checkout -b [branch]`|管理| 創建並切換到新分支|
| `git log`|查詢| 顯示提交歷史|
| `git log --oneline`|查詢| 以簡潔模式顯示提交歷史|
| `git diff`|查詢| 比較工作區和暫存區的差異|
| `git diff --staged`|查詢| 比較暫存區和最新提交的差異|
| `git reset [file]`|管理| 從暫存區移除指定文件，但保留工作區的變更|
| `git reset --hard [commit]`|管理| 重置倉庫到指定提交狀態，包括工作區和暫存區|
| `git stash`|管理| 暫存當前工作區的變更，並清理工作區|
| `git stash pop`|管理| 應用最新的暫存變更並移除該暫存|
| `git remote -v`|遠端操作| 顯示所有遠程倉庫的詳細信息|參考章節](#專案第一次-push-到-gitea-該怎麼操作)
| `git remote add [name] [url]` |遠端操作| 添加遠程倉庫|參考章節](#專案第一次-push-到-gitea-該怎麼操作)
| `git tag [tag-name]`|管理| 為當前提交創建標籤|
| `git show [commit]`|查詢| 顯示指定提交的詳細信息|
| `git config user.name [name]`|權限|設定 Git 使用者名稱|
| `git config user.email [email]`|權限|設定 Git 使用者信箱|

[回到快速導覽](#快速導覽)


### git init
Git 的初始化指令，將資料夾內的所有內容（包括子資料夾的內容）都納管，並且生成 Git 管理用的 `.git` 資料夾。

> [!CAUTION]
> 請注意！`.git` 資料夾絕對不要刪除了。

[回到快速導覽](#快速導覽)


## Git 帳號與權限設定
> [!TIP]
> 請準備好遠端平台的權限。

1. 前往 Windows 設定
2. 搜尋 `認證管理員`
3. 點選 `Windows 認證`
4. 下拉點選 `一般認證`
5. 尋找 `git:******平台網址******`：
    +  如果有 `git:******平台網址******`：
         1. 點擊 `git:******平台網址******`：
         2. 點擊 `編輯`
         3. 輸入您的 `使用者名稱` 以及 `密碼`
         4. 輸入完成後，點擊 `儲存`
    +  如果沒有 `git:******平台網址******`：
         1. 點擊 `新增一般憑證`
         2. `網際網路或網路位址:` 欄位輸入 `git:******平台網址******`：
         3. 輸入您的 `使用者名稱` 以及 `密碼`
         4. 輸入完成後，點擊 `確定`
6. 打開 `cmd`
7. 使用指令 `git config --global user.name ******使用者名稱******` 設定這份專案的使用者名稱。
8. 使用指令 `git config --global user.email ******使用者信箱******` 設定這份專案的使用者信箱。
   > [!NOTE]
   > `--global ` 參數設定取代作業系統裡的預設使用者名稱和使用者信箱，如果是個別專案有個別使用者，則不用增加這個指令。

[回到快速導覽](#快速導覽)


## 複製遠端的專案
`git clone` 是一種遠端操作指令，從遠端（網址、伺服器、網站）複製一個專案，只要專案是開放的，這個指令無須權限即可執行。
在使用 GitHub 的開元專案時，這項指令常常是第二個指令（第一個是[初始化](#git-init)）。

> [!TIP]
> 複製下來的專案通常與原本專案相同，但是沒有遠端連線。

複製一個遠端專案的流程如下：
> [!TIP]
> 請依照《[Git 帳號與權限設定](#git-帳號與權限設定)》章節做好基本設定

1. 打開 `cmd`
2. 使用指令 `git init` 初始化這個資料夾。
3.  使用指令 `git clone ******專案網址******` 進行專案的複製。

:::mermaid
graph LR

   subgraph local["本地端"]
      subgraph os["作業系統"]
         subgraph git["Git 工具"]
            bl1["專案A"]
         end
      end
   end

   subgraph cloud["雲端"]
      subgraph web["GitHub"]
         bw1["專案A"]
      end
   end

   bw1 ---> |git clone| bl1
:::

[回到快速導覽](#快速導覽)


## 專案第一次 PUSH 到 Gitea 該怎麼操作

1. 到您的專案資料夾
2. 打開 `cmd`
3. 確認自己的專案狀態，使用指令 `git remote`
    +  如果列表有遠端的專案：請直接前往下一個步驟。
    +  如果列表沒有遠端的專案：
         1. 使用指令 `git remote add ***本地專案名稱*** ***遠端專案網址***`
            > [!NOTE]
            > 本步驟為本地端的 Git 增加一個遠端的數據庫，本地端的辨識名稱是 `***本地專案名稱***`（此名稱可更改，建議全英文），而連結的遠端數據庫是 `***遠端專案網址***`。

            > [!NOTE]
            > 如果遠端的數據庫在本地端的辨識名稱設定錯誤，可以透過 `git remote rename 錯誤名稱 修正名稱` 的指令來更換。或是使用 `git remote remove 錯誤名稱` 來直接刪除該辨識名稱的連結。
         2. 使用指令 `git pull`
         3. 進入下一個步驟。
4.  使用指令 `git push -u ***遠端專案網址*** ***要推送的分支名稱***`

      > [!WARNING]
      > 如果是維護版（hotfix）、開發版（develop）或是其他非 `master` 的 **分支**，請先確認您的本地端是否在相應的 **分支**。

      > [!NOTE]
      > 透過本地端的辨識名稱，指定了一個遠端的數據庫，並將該庫中的 `***要推送的分支名稱***` 分支，更新推送（push）成本地端 `***要推送的分支名稱***` 的內容。
      > 
      > 如果不加上 `***要推送的分支名稱***` 則預設推送 `master` 分支。

      > [!NOTE]
      > 在 `git push -u` 指令中，`-u` 參數的意義是將當前分支設置為其對應的上游分支（upstream branch）。
      >
      > :::mermaid
      > graph LR
      >   subgraph Local Repository
      >      A[main] --> B[feature]
      >   end
      > 
      >   subgraph Remote Repository
      >      C[origin/main]
      >      D[origin/feature]
      >   end
      > 
      >   B -- set upstream --> D
      >   C -- tracks --> A
      >   D -- tracks --> B
      > :::
      > 
      > 1. **本地倉庫 (Local Repository)**：包含兩個分支：main 和 feature。 feature 是您正在開發的新功能分支。
      > 2. **遠程倉庫 (Remote Repository)**：包含遠程分支：origin/main 和 origin/feature。
      > 3. **設置上游分支 (set upstream)**：當您執行 git push -u origin feature 時，Git 將 origin/feature 設置為本地分支 feature 的上游分支。這表示本地 feature 分支將追蹤（track）遠程 origin/feature 分支的變更。
      > 4. **後續操作簡化**：設置上游分支後，您可以直接執行 git push 和 git pull，而不需要每次都指定遠程倉庫和分支名稱。Git 知道本地 feature 分支的變更應該推送到 origin/feature，並從那裡拉取變更。

      > [!NOTE]
      > 如果要完整推送整個專案的所有分支，則使用以下指令擇一：
      > + `git push -u ***遠端專案網址*** '*:*'`
      > + `git push -u ***遠端專案網址*** --all`
      > + `git push -u --all origin`
      >
      > 參考資料：[Git 一次性 pull push 所有的分支](https://blog.csdn.net/bingyu9875/article/details/84841521)

[回到快速導覽](#快速導覽)


## 專案新增提交

1. 專案編寫完成後，回到您的專案根資料夾（有 `.git` 資料夾的位置）
2. 打開 `cmd`
3. 首先必須將此版本的專案在本地端 `commit` 給 Git 軟體。
   1. 使用指令 `git add .`
   2. 使用指令 `git status` ，確認是否所有更改的檔案已經顯示綠色。
   3. 使用指令 `git commit -m "***版本敘述***"`。
   
      > [!NOTE]
      > 版本編號可以另外用標籤（`git tag`）設定。

      > [!NOTE]
      > 如果這次的修改要覆蓋前一個提交，可以在指令中追加 `--Amend`。
      > 
      > 因此提交指令會是：`git commit -m --Amend`。

   4. 使用指令 `git log HEAD...HEAD~~2`，可以確認該 `commit` 是否已經在本地端 Git 完成。

4. 如果是重大版本，或是關鍵版本，請追加標籤。
   1. 追加標籤請使用指令 `git tag -a ***標籤版本號*** -m "***標籤敘述***"`
   2. 使用指令 `git tag` ，可以確認該 `tag` 是否已經標記。
5. 確認以上對於該版本專案的操作都完成後，就可以進入後續步驟開始 Push 專案至遠端。
   1. 確認自己的專案狀態，使用指令 `git remote`
    +  如果列表有遠端的專案：請直接前往下一個步驟。
    +  如果列表沒有遠端的專案：
         1. 使用指令 `git remote add ***本地專案名稱*** ***遠端專案網址***`
            > [!NOTE]
            > 本步驟為本地端的 Git 增加一個遠端的數據庫，本地端的辨識名稱是 `***本地專案名稱***`（此名稱可更改，建議全英文），而連結的遠端數據庫是 `***遠端專案網址***`。

            > [!NOTE]
            > 如果遠端的數據庫在本地端的辨識名稱設定錯誤，可以透過 `git remote rename 錯誤名稱 修正名稱` 的指令來更換。或是使用 `git remote remove 錯誤名稱` 來直接刪除該辨識名稱的連結。
         2. 使用指令 `git pull`
         3. 進入下一個步驟。
   2.  使用指令 `git push -u ***遠端專案網址*** ***要推送的分支名稱***`

         > [!WARNING]
         > 如果是維護版（hotfix）、開發版（develop）或是其他非 `master` 的 **分支**，請先確認您的本地端是否在相應的 **分支**。

## 分支合併
Git 的管理中，專案的分支與合併，都可以在本地端處理完畢後，再傳到遠端。不需要在遠端進行分支與合併的操作。

1. 專案編寫完成後，回到您的專案根資料夾（有 `.git` 資料夾的位置）
2. 打開 `cmd`
3. 首先必須將此版本的專案在本地端 `commit` 給 Git 軟體。
4. 如果是重大版本，或是關鍵版本，請追加標籤。
   1. 追加標籤請使用指令 `git tag -a ***標籤版本號*** -m "***標籤敘述***"`
   2. 使用指令 `git tag` ，可以確認該 `tag` 是否已經標記。
5. 確認以上對於該版本專案的操作都完成後，就可以進入合併操作。
   1. 先使用指令進入要合併過去的分支：`git checkout ***要合併過去的分支名稱***`
   2. 使用指令進行合併：`git merge ***要併過來的分支名稱***`
   3. 操作後，` ***要合併過去的分支名稱***` 的分支就會有 `***要併過來的分支名稱***` 的分支的內容。
6. 確認以上對於該版本專案的操作都完成後，就可以進入後續步驟開始 Push 專案至遠端。

[回到快速導覽](#快速導覽)


# 結論

## 筆者

### 主筆
Lmk999999


### 協助檢稿
無

|
[回到快速導覽](#快速導覽)
|
[回到主題簡介](./Git.md)
|
[回到程式類別](../Program.md)
|
