# Tesla Record Viewer

一個純前端、100% 本地處理的 Tesla 行車記錄器 (Dashcam / Sentry Mode) 網頁檢視器。**所有資料絕不離開你的電腦，沒有任何上傳。**

## 線上 Demo

**https://aaronfei.github.io/tesla-record-viewer/**

（首次部署後可能需要 1~3 分鐘建置時間）

## 特色

- **左右雙欄版面**：左邊載入資料夾 + 事件清單（可摺疊、內部捲動），右邊同步播放器，盡量單頁不捲動
- **多鏡頭同步播放**：最新 6 鏡頭（front / back / left_repeater / right_repeater / left_pillar / right_pillar），排列為「前/後 → 左右後視鏡 → 左右 B 柱」
- **時間戳記浮水印**：依每段影片實際錄影起始時間 + 播放進度即時計算，疊在畫面上並隨播放走（也可選自訂文字，或兩者並存）
- **遙測疊加（SEI）**：按需解析 Tesla 內嵌的 SEI 遙測，顯示**時速、方向燈、檔位、煞車、Autopilot 狀態**，與影片同步
- **匯出帶浮水印影片**：在瀏覽器內即時合成多鏡頭畫面 + 浮水印 + 遙測（Canvas + MediaRecorder）
- 事件自動分組（Recent / Saved / Sentry）+ 時間排序
- 純靜態，適合 GitHub Pages，中文介面

## 使用方式（本地測試）

1. 用 **Chrome / Edge** 開啟 `index.html`（或用任何靜態 server）
2. 左欄點「選擇 TeslaCam 資料夾」→ 選你 USB 裡的 `TeslaCam`（或直接選 SentryClips / SavedClips）
3. 等掃描完成 → 點事件即可**立即播放**
4. 想看遙測時，按播放控制列的 **「解析遙測」**（不按就完全不解析，播放零負擔）
5. 浮水印 / 匯出設定在右下角可摺疊面板中

## ⚠️ Tesla 2026.20 之後：行車記錄「預設加密」

從 **2026.20** 起，Tesla 會把影片存成 `EncryptedClips`（加密資料），一般播放器與本檢視器都**無法直接播放** —— 這不是 codec / H.265 問題。

- **解密已錄好的影片**：把加密 `.mp4` 複製到電腦 → 開 [dashcam.tesla.com](https://dashcam.tesla.com) → 用 Tesla 帳號登入 → 上傳後在你瀏覽器**本機解密下載**（檔案不會上傳給 Tesla）。解密後的 MP4 即可丟進本檢視器。
- **不想再加密（之後的新影片）**：車機 **「控制 Controls」→「安全 Safety」→ 關閉「加密行車記錄影片 Encrypt Dashcam Recordings」**。註：只影響之後新錄的影片，已加密的舊檔仍需用上面工具解密。

（頁面最上方也有這段提示，可關閉並會記住。）

## 遙測（SEI）說明與限制

遙測資料是 Tesla 直接內嵌在影片位元流裡的 **SEI metadata（protobuf）**，本檢視器重用 Tesla 官方開源解析器
（[teslamotors/dashcam](https://github.com/teslamotors/dashcam)）在瀏覽器內解析，**完全本地、不上傳**。

只有同時滿足以下條件才有 SEI 遙測：

- 影片**未加密**（見上方，需在車上關閉加密）
- 韌體 **2025.44.25 以上**、**HW3 以上**
- 錄影當下**非停車**（停車哨兵模式通常沒有）
- 編碼為 **H.264（avc1）**；HW4 的 H.265 片段可能無法解析

若不符合，按鈕會顯示「無 SEI 遙測」。

## 檔案結構要求

Tesla 產生的標準結構：

```
TeslaCam/
├── RecentClips/
├── SavedClips/
│   └── 2024-06-10_14-22-33/
│       ├── 2024-06-10_14-22-33-front.mp4
│       ├── 2024-06-10_14-22-33-back.mp4
│       ├── 2024-06-10_14-22-33-left_repeater.mp4
│       └── 2024-06-10_14-22-33-right_repeater.mp4
└── SentryClips/
    └── ...
```

## 技術限制與相容性

- **最佳體驗**：Chromium 瀏覽器（Chrome, Edge, Arc, Brave…），因為使用 File System Access API
- Firefox / Safari：可使用「選擇檔案」fallback，但資料夾體驗較差
- 影片 codec：
  - **H.264**：瀏覽器原生支援良好（遙測解析也只支援 H.264）
  - **H.265 / HEVC**（新款 HW4）：需硬體解碼。Chrome 在 macOS 需開啟「使用圖形加速」且硬體支援；Safari 原生支援
  - **加密 (EncryptedClips)**：需先用 dashcam.tesla.com 解密（見上方）
- 匯出影片為 WebM 格式（相容性佳）

## 開發 / 部署到 GitHub Pages

- 直接 push 到 `main` 分支即可由 GitHub Actions 自動更新 Pages。
- 純靜態單檔（`index.html`），無需建置。

## 致謝

- 遙測解析重用 Tesla 官方開源工具 [teslamotors/dashcam](https://github.com/teslamotors/dashcam)（MP4/SEI 解析 + protobuf 規格）。
- 參考了社群許多優秀的開源 TeslaCam Viewer 實作。

## 授權

MIT
