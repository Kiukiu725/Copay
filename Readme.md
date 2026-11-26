# Copay — 日常夾錢與海外旅遊分帳神器 💰✈️

Copay 是一個專為朋友聚會、飯局與海外旅遊設計的實時協作分帳 Web App，採用單一 `index.html`（React 18 + Babel + Tailwind CSS）前端架構，搭配 Firebase Realtime Database 與 Firebase Authentication，免安裝原生 App 即可透過連結即時同步記帳與結算。

## 🌟 核心功能

* **雙房間模式：** 支援 `daily`（Copay Lite 日常夾錢，主打 HKD）與 `travel`（Copay Travel 海外旅遊，支援多國貨幣、即時／自訂匯率、信用卡手續費與「公數錢包」 `🏦 公數錢包`）。
* **房間級 Pro 共享（`isProRoom`）：** Pro 會員開立或進入旅遊房間時會將房間標記為 `isProRoom: true`，同一房間內的免費用戶可在該房間內使用進階拆帳與匯出 CSV；離開該房間或自行建立新 Pro 房間則受個人 `isPro` 狀態限制。
* **Email 帳號與雲端 Pro 同步：** 支援以 Email 註冊／登入（Firebase Auth），並將 Pro 狀態同步至 `copay_users/{uid}`，方便換機或清除瀏覽器紀錄後登入恢復。
* **智能對沖結算與匯出：** 自動計算債務對沖並一鍵複製 WhatsApp 過數清單；Pro 權限生效時可匯出 UTF-8 CSV 報表。
* **自動清理閒置房間：** 超過 60 日無活動的房間會由系統清理以保持資料庫整潔。

## 🛠️ 技術與部署架構

* **前端：** HTML5、React 18（CDN）、Babel Standalone、Tailwind CSS、玻璃擬物與水彩背景樣式。
* **後端服務：** Firebase Realtime Database、Firebase Authentication（Email/Password）、Firebase Hosting（自訂網域 `https://letscopay.com`）。
* **商業化：** Google AdSense（`ca-pub-5112577779634426`）及 Stripe Payment Link（HK$38 一次性解鎖，支援 `?vip=COPAY38` 自動解鎖與 VIP 兌換碼）。

## 🚀 部署與同步指令

在部署目錄（`Desktop\Copay-web`）更新 `index.html` 後執行：

```cmd
cd Desktop\Copay-web
firebase deploy
```

部署完成後，亦建議將最新的 `index.html` 同步至本機備份資料夾與 GitHub 儲存庫，避免誤改舊版本。