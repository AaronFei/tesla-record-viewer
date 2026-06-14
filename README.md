# Tesla Record Viewer

一個純前端、100% 本地處理的 Tesla 行車記錄器 (Dashcam / Sentry Mode) 網頁檢視器。

- 在 GitHub Pages 直接使用
- 使用者「匯入資料夾」後自動掃描 TeslaCam 結構
- 列出事件 (SentryClips / SavedClips / RecentClips)
- 多個攝影機角度**同時同步播放**
- 加上自訂**浮水印**，並可**匯出單一帶浮水印的影片** (完全在瀏覽器內，使用 Canvas + MediaRecorder)
- **所有資料絕不離開你的電腦**，沒有任何上傳

## 線上 Demo

**https://aaronfei.github.io/tesla-record-viewer/**

（首次部署後可能需要 1~3 分鐘建置時間）

## 特色

- 支援最新 6 鏡頭 (front / back / left_repeater / right_repeater / left_pillar / right_pillar)
- File System Access API (Chrome/Edge 最佳體驗) + webkitdirectory fallback
- 事件自動分組 + 時間排序
- 同步播放控制 (播放/暫停/進度/速度)
- 浮水印即時預覽 + 一鍵匯出合成影片
- 純靜態，適合 GitHub Pages
- 中文介面

## 使用方式 (本地測試)

1. 用 Chrome / Edge 開啟 `index.html` (或用任何靜態 server)
2. 點「選擇 TeslaCam 資料夾」
3. 選擇你 USB 裡的 `TeslaCam` 資料夾 (或直接選 SentryClips / SavedClips)
4. 等待掃描完成 → 點擊事件即可播放
5. 在播放器中可開啟浮水印，並按「匯出帶浮水印影片」

## 檔案結構要求

Tesla 產生的標準結構：

```
TeslaCam/
├── RecentClips/
├── SavedClips/
│   └── 2024-06-10_14-22-33/
│       ├── 2024-06-10_14-22-33-front.mp4
│       ├── 2024-06-10_14-22-33-left_repeater.mp4
│       ├── 2024-06-10_14-22-33-right_repeater.mp4
│       └── 2024-06-10_14-22-33-back.mp4
└── SentryClips/
    └── ...
```

## 技術限制與相容性

- **最佳體驗**：Chromium 瀏覽器 (Chrome, Edge, Arc, Brave...)，因為使用 File System Access API
- Firefox / Safari：可使用「選擇檔案」fallback，但資料夾體驗較差
- 影片 codec：Tesla 主要用 H.264，瀏覽器原生支援良好。新款車的 H.265 可能需硬體解碼支援
- 非常大量的 clip 時，建議先複製小部分測試

## 開發 / 部署到 GitHub Pages

本專案已由 GitHub CLI 自動建立 repo 並啟用 Pages（來源：main 分支根目錄）。

如需手動重新部署：
1. 直接 push 到 main 分支即可自動更新 Pages。
2. 或在 GitHub repo Settings → Pages 確認 Source 為 main / (root)。

### 本地開發

```bash
# 如果之後改用 Vite
npm install
npm run dev
```

## 貢獻 / 自訂

歡迎改進 grouping 邏輯、加入 event.json 解析、更好同步演算法、telemetry overlay 等。

## 授權

MIT (或你喜歡的)

## 致謝

本專案參考了社群許多優秀的開源 TeslaCam Viewer 實作 (NateMccomb/TeslaCamViewer、DeaglePC/TDashcamStudio 等)。目的是提供一個輕量、可輕鬆自訂浮水印、容易部署在 github.io 的版本。
