# Copay — 日常夾錢與海外旅遊分帳神器 💰✈️

Copay 是一個專為朋友聚會、飯局與海外旅遊設計的實時協作分帳 Web App，採用單一 `index.html`（React 18 + Babel + Tailwind CSS）前端架構，搭配 Firebase Realtime Database 與 Firebase Authentication，免安裝原生 App 即可透過連結即時同步記帳與結算。

## 🌟 核心功能

* **雙房間模式：** 支援 `daily`（Copay Lite 日常夾錢，主打 HKD）與 `travel`（Copay Travel 海外旅遊，支援多國貨幣、即時／自訂匯率、信用卡手續費與「公數錢包」 `🏦 公數錢包`）。
* **房間級 Pro 共享（`isProRoom`）：** Pro 會員開立或進入旅遊房間時會將房間標記為 `isProRoom: true`，同一房間內的免費用戶可在該房間內使用進階拆帳、匯出 CSV 並享有同房無廣告體驗。
* **PRO 零廣告體驗：** 僅在免費用戶且非 Pro 房間（`!isPro && !isProRoom`）時動態載入 Google AdSense 並顯示橫幅廣告；PRO 用戶與 Pro 房間完全不載入廣告。
* **帳號綁定與購買保護：** 透過 Firebase Auth 提供 Email 登入。為保障用戶權益，解鎖 PRO 前 **強制要求登入/註冊 Email**，確保 PRO 狀態同步至雲端（`copay_users/{uid}`），換機不遺失。
* **智能對沖結算與匯出：** 自動計算債務對沖並一鍵複製 WhatsApp 過數清單；Pro 權限生效時可匯出 UTF-8 CSV 報表。
* **隨緣樂助（Buy me a coffee）：** 設定頁底部設有獨立的 Stripe 贊助按鈕，讓覺得 App 好用的用戶可以隨緣課金支持開發者（HK$20 凍檸茶）。

## 🛠️ 技術與部署架構

* **前端：** HTML5、React 18（CDN）、Babel Standalone、Tailwind CSS、玻璃擬物與水彩背景樣式。
* **後端服務：** Firebase Realtime Database、Firebase Authentication（Email/Password）、Firebase Hosting（自訂網域 `https://letscopay.com`）。
* **商業化 (Stripe & AdSense)：** 
  * Google AdSense：`ca-pub-5112577779634426`（按需載入）
  * PRO 解鎖 (HK$38)：支援付款後跳轉 `?vip=COPAY38` 自動解鎖。
  * 隨緣樂助 (HK$20)：獨立 Stripe Payment Link。

## 🚀 部署與同步指令

在部署目錄（`Desktop\Copay-web`）更新 `index.html` 後，於 CMD 執行以下指令發布：

```cmd
cd Desktop\Copay-web
firebase deploy
```

> **開發者提示：** 部署完成後，請確保將最新的 `index.html`、`README.md` 與 `AGENTS.md` 同步至本機備份資料夾與 GitHub 儲存庫。