# 部署指南　·　把能量帳放上 GitHub Pages

完成後，你會有一個公開的網址（類似 `https://你的帳號.github.io/energy-ledger/`），
從手機瀏覽器打開後可以「加入主畫面」變成像 App 一樣的圖示，且離線可用。

整個流程約 10–15 分鐘，**不需要寫程式**，只需要會用瀏覽器、複製貼上。


---

## 一、檔案清單

部署前確認你有這些檔案，整個資料夾結構應該是：

```
energy-ledger/
├── energy-ledger.html      ← 主程式
├── index.html              ← 自動跳轉到 energy-ledger.html
├── manifest.json           ← PWA 設定
├── service-worker.js       ← 離線快取
├── icons/
│   ├── icon.svg
│   ├── icon-maskable.svg
│   ├── icon-192.png
│   ├── icon-512.png
│   ├── icon-maskable-512.png
│   └── apple-touch-icon.png
├── README.txt
└── DEPLOY.md               ← 這份文件
```


---

## 二、GitHub Pages 部署步驟

### Step 1　·　註冊 GitHub 帳號（已有可跳過）

到 https://github.com/signup 註冊免費帳號。


### Step 2　·　建立新 Repository

1. 登入 GitHub，右上角點「+」→「New repository」
2. Repository name 填：`energy-ledger`（也可以用其他名字）
3. 選「Public」（GitHub Pages 免費版需要公開倉庫）
4. **不要勾選**「Add a README file」（我們已經有了）
5. 點「Create repository」


### Step 3　·　上傳檔案

最簡單的方法：

1. 在剛建立的 repository 頁面，點「uploading an existing file」
2. 把整個 `energy-ledger/` 資料夾裡的所有檔案**和 icons 子資料夾**拖進去
3. 下方訊息框填寫（例如）「Initial commit」
4. 點「Commit changes」

**注意**：拖檔案時要確保 `icons/` 是一個資料夾，不是把裡面的 PNG 跟其他檔案混在一起。
網頁上傳會保留資料夾結構，沒問題。


### Step 4　·　開啟 GitHub Pages

1. 在 repository 頁面，點上方「Settings」
2. 左側選單往下找「Pages」
3. 在「Source」區塊：
   - Branch 選 `main`
   - Folder 選 `/ (root)`
   - 點「Save」
4. 大約等 1–2 分鐘
5. 重新整理頁面，最上方會出現：
   ```
   Your site is live at https://你的帳號.github.io/energy-ledger/
   ```


### Step 5　·　用手機開啟並安裝

#### iPhone (Safari)

1. 用 Safari 打開上面那個網址
2. 點底部「分享」按鈕（中間那個方框 + 上箭頭）
3. 往下滑找到「加入主畫面 / Add to Home Screen」
4. 點「新增」
5. 桌面會出現「能量帳」圖示，點開就像 App 一樣

#### Android (Chrome)

1. 用 Chrome 打開網址
2. 通常會自動跳出「加入主畫面」橫幅，點「安裝」
3. 沒跳的話：右上角三個點 →「加到主畫面 / Install app」


---

## 三、之後想修改怎麼辦？

### 改一行字、調整顏色等小修改

1. 在 GitHub 上點開那個檔案 → 點鉛筆圖示「Edit」→ 直接編輯
2. 下方點「Commit changes」
3. **重點**：service worker 會快取舊版，所以如果你想讓所有人都看到新版，
   要同時修改 `service-worker.js` 第 10 行：
   ```js
   const CACHE_VERSION = 'energy-ledger-v1';
   ```
   改成 `v2`、`v3`⋯⋯每次修改主程式都遞增一次

### 已經安裝在手機上的，會自動更新嗎？

會。Service worker 偵測到 `CACHE_VERSION` 變了，會在背景下載新版本，
下次開啟 App 時就是新的。

但有時 iOS 比較頑固，可以手動：刪除桌面圖示 → 重新從 Safari 加入主畫面。


---

## 四、自訂網域（選用）

如果你有自己的網域（例如 `energy.example.com`）：

1. 在域名供應商設定 CNAME 記錄指向 `你的帳號.github.io`
2. GitHub Settings → Pages → Custom domain 填入你的網域
3. 勾選「Enforce HTTPS」


---

## 五、常見問題

### Q1：在手機上開啟，但「加入主畫面」按鈕沒有特殊效果？

確認三件事：
- 網址是 `https://`，不是 `http://`
- 開啟 DevTools (桌面瀏覽器) → Application → Manifest 看有沒有錯誤
- 至少 192 與 512 兩個 PNG 圖示都存在

### Q2：離線打開後資料不見了？

`localStorage` 是綁定到網域的。只要是同一個網址，資料會一直保留。
但如果你「清除瀏覽器資料」或「移除網站數據」，紀錄就會消失。

### Q3：可以同步到多台裝置嗎？

目前不行——資料只存在當下使用的瀏覽器中。
這是刻意的：沒有伺服器、沒有帳號、完全私密。
如果未來想要同步，可以加入匯出 JSON 功能，手動在裝置間搬移。

### Q4：可以給朋友用嗎？

可以。把網址傳給他，他在自己的瀏覽器打開，
他的紀錄會存在他的瀏覽器，跟你的完全分開，不會互相看到。


---

## 六、之後我有可能要做的事

把這幾件事記下來，將來想動工時不會找不到頭緒：

- [ ] 加入「匯出 JSON / CSV」按鈕，可以備份資料
- [ ] 加入「匯入 JSON」按鈕，可以換裝置時把資料搬過去
- [ ] 月底實際試用「回顧」功能，看哪些提示句要調整
- [ ] 觀察一個月後，看哪個「能量屬性」最常被選，哪個從沒用過，
      考慮要不要簡化


---

願這個小工具，
真的能成為你看見自己的一面鏡子。
