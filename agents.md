# Copay App — AI Agent Instructions

## 1. 架構與檔案規則
* **單一檔案架構：** 核心應用程式維持在單一 `index.html`（React 18 + Babel + Tailwind CSS），勿隨意拆分成多個 `.js` 或 `.css` 檔案，並保留頂部 `setupAppIcons` 圖示回退腳本。
* **部署來源：** 正式上線以 `Desktop\Copay-web\index.html` 透過 `firebase deploy` 發布至 Firebase Hosting（`https://letscopay.com`）；修改後應同步本機備份與 GitHub 版本。

## 2. 權限、帳號與房間邏輯
* **房間級 Pro 共享（`isProRoom`）：** 免費用戶進入 `isProRoom === true` 的房間時，僅在**該房間內**可使用進階拆帳與 CSV 匯出；自行建立新的 Pro 旅遊房或於非 Pro 房間使用付費功能時，須檢查個人 `isPro` 狀態（`!isPro && !isProRoom` 觸發 Paywall）。
* **Email 登入與雲端同步：** 透過 Firebase Auth 處理 Email 登入／註冊，並於 `copay_users/{uid}` 同步 `isPro` 狀態；免費用戶登入 Email 不會自動獲得 Pro 權限。
* **公數錢包：** 維持 `POOL_PAYER_KEY = "🏦 公數錢包"` 的注資、餘額驗證與結算退還計算邏輯。

## 3. 變現與外部整合
* **Google AdSense 與 Stripe：** 保留 `<head>` 內的 Google AdSense 腳本、`STRIPE_PAYMENT_LINK`、付款後 `?vip=COPAY38` 自動解鎖邏輯及 `VALID_VIP_CODES` 兌換流程。
* **效能與同步：** 維持 Firebase Realtime Database 監聽與 `useMemo` 計算，修改 `index.html` 後提供明確的 `firebase deploy` 驗證步驟。