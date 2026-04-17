---
title: GitHub Actions使用方法
tags: [114程式設計與實習_下學期, 114_下學期_互動程式設計]

---

旨在指導他們如何利用 VS Code 的 Git 功能將期中報告上傳至 GitHub，並透過 **GitHub Actions** 實現自動化部署，或是利用 **GitHub Pages** 快速呈現網頁作品。

![image](https://hackmd.io/_uploads/HJd0sAu2-x.png)

---

🎓 課程講義：從 VS Code 到 GitHub 的自動化發佈之路
===================================

📝 學習目標
-------

1.  掌握 VS Code 內建的 Git 介面進行版本控制。
    
2.  學習將本地程式碼推送 (Push) 至 GitHub 遠端儲存庫。
    
3.  了解 GitHub Pages 與 Actions 的基礎配置，實現作品在線預覽。
    

---

第一階段：GitHub 儲存庫建立
-----------------

在開始寫程式之前，我們需要先在 GitHub 上準備一個「家」。

1.  登入 [GitHub](https://github.com/)。
    
2.  點擊右上角 **「+」** > **New repository**。
    
3.  **Repository name**: 輸入 `Midterm-Project` (或任何你喜歡的名字)。
    
4.  設為 **Public**（公開），這樣助教與老師才看得到你的網頁。
    
5.  點擊 **Create repository**。
    

---

第二階段：VS Code 本地操作與上傳
--------------------

現在回到你的 VS Code，將寫好的 `index.html`, `style.css`, `sketch.js` 上傳。

### 1\. 初始化 Git (Initialize)

-   開啟 VS Code 的 **「原始碼控制」** 圖示（快捷鍵 `Ctrl+Shift+G`）。
    
-   點擊 **Initialize Repository**。
![image](https://hackmd.io/_uploads/B19ckh_hZl.png)


    

### 2\. 暫存與提交 (Commit)

-   在檔案列表旁的 **「+」** 點一下，將檔案加入暫存區 (Stage)。
![image](https://hackmd.io/_uploads/H1ayxhdn-x.png)



-   在上方的訊息欄輸入 `Initial commit: Midterm Project`。
 ![image](https://hackmd.io/_uploads/SJPEx3d2bl.png)



-   點擊 **Commit**（提交）。
    
![image](https://hackmd.io/_uploads/BJo2qA_2Zg.png)

### 3\. 發佈至 GitHub (Publish)

![image](https://hackmd.io/_uploads/HyPli0_2Zg.png)

![image](https://hackmd.io/_uploads/Byq4iC_hZx.png)

終端機輸入以下指令
```tex!
 git remote add origin https://github.com/cfchengit/20260412_3.git
```

-   點擊 **Publish Branch**。
    
-   選擇 **Publish to GitHub public repository**。
    
-   VS Code 會自動幫你完成遠端連接與推送。
    

---

第三階段：開啟網頁預覽 (GitHub Pages)
--------------------------

GitHub Actions 最常見的用途之一就是自動部署網頁。對於靜態網頁（HTML/CSS/JS），我們可以直接開啟 GitHub Pages。

1.  到 GitHub 儲存庫頁面，點擊 **Settings**。
    
2.  左側選單選擇 **Pages**。
    
3.  在 **Build and deployment** 下方的 **Source** 選擇 `Deploy from a branch`。
    
4.  **Branch** 選擇 `main` (或 `master`)，目錄選 `/ (root)`。
    
5.  點擊 **Save**。
    
6.  等待約 1 分鐘，上方會出現網址，你的作品就變成真正的網站了！
    

---

第四階段：進階——使用 GitHub Actions (自動化)
--------------------------------

如果你想要練習自定義的自動化流程（例如：每次上傳後自動檢查程式碼語法或壓縮檔案），可以嘗試手動設定 Actions。

### 建立 Workflows 檔案

1.  在專案中建立一個資料夾 `.github`，裡面再建立一個 `workflows` 資料夾。
    
2.  建立一個檔案名為 `deploy.yml`。
    

### 範例配置代碼 (YAML)

這段代碼的功能是：每當你 `push` 程式碼時，Actions 會自動執行一個簡單的檢查。

YAML

```
name: Project-Check  # 流程名稱

on:
  push:
    branches: [ "main" ] # 當 main 分支有更新時觸發

jobs:
  build:
    runs-on: ubuntu-latest # 使用 Linux 虛擬機執行

    steps:
    - name: Checkout code
      uses: actions/checkout@v4 # 複製程式碼到虛擬機

    - name: Run a simple script
      run: echo "🎉 程式碼已成功上傳並通過 GitHub Actions 檢查！"
```

---

🚀 課後挑戰
-------

-   **挑戰 1**：試著修改 `sketch.js` 裡的顏色，再次 `Commit` 與 `Push`，觀察 GitHub Pages 網址是否自動更新。
    
-   **挑戰 2**：查看 GitHub 頁面上的 **Actions** 分頁，確認你的 `Project-Check` 流程是否顯示綠色的勾勾（成功）。
    

---

> **💡 老師的小提示：**
> 
> 記得將你的 `weekData` 裡面的 URL 路徑檢查清楚。如果你的作品放在子資料夾，URL 應該是 `./week1/index.html` 這種相對路徑，這樣上傳到 GitHub 後才能正確讀取喔！

---

## 如果出現錯誤

```tex!
選擇 Publish to GitHub public repository。
該步驟時，會顯示GitHub repository already exists
這該如何處理

```


### ❓ 常見問題：顯示 "GitHub repository already exists"
這代表你已經在 GitHub 網頁建過儲存庫了。請依照以下步驟手動連結：
1. 複製 GitHub 上的 **HTTPS 網址**。
2. 在 VS Code 按下 `Ctrl+Shift+P`，搜尋 `Git: Add Remote`。
3. 貼上網址並命名為 `origin`。
4. 點擊「同步符號 (🔄)」或「Publish Branch」即可成功上傳。

也就是先輸入
```tex
git remote add origin https://github.com/cfchengit/20260412_2.git
```
再執行一次
點擊「同步符號 (🔄)」或「Publish Branch」即可成功上傳。

---

## 修改程式碼後，同步程式碼


-   在檔案列表旁的 **「+」** 點一下，將檔案加入暫存區 (Stage)。
    ![image](https://hackmd.io/_uploads/S1Ezai_h-e.png)

-   在上方的訊息欄輸入 `Initial commit: Midterm Project`。
   ![image](https://hackmd.io/_uploads/ByMc6idhbg.png)


-   點擊 **Commit**（提交）。


![image](https://hackmd.io/_uploads/H1I-Riu3Ze.png)

