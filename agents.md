# Copay App — AI Agent Instructions

## 1. 架構與檔案規則

* **單一檔案架構：** 核心應用程式必須嚴格維持在單一 `index.html`（React 18 + Babel + Tailwind CSS），絕對不能拆分成多個 `.js` 或 `.css` 檔案。
* **iOS / PWA 支援：** 必須保留頂部 `setupAppIcons` 圖示回退腳本，以及 `manifest.json` 與 `/sw.js` 的 Service Worker 註冊代碼。
* **Firebase 完整設定：** 必須保留包含 `apiKey`、`authDomain`、`databaseURL`、`projectId`、`storageBucket`、`messagingSenderId`、`appId` 的完整 `firebaseConfig`，避免 `firebase.auth()` 失效。

## 2. 帳號、權限與防漏機制 (Critical)

* **強制登入保護 (Auth-Gated Payment)：** 
  * 觸發付費牆（Paywall）時，**必須檢查 `currentUser`**。
  * 若用戶未登入，嚴禁顯示 Stripe PRO 付款按鈕，必須顯示警告提示並引導用戶先進行 Email 註冊/登入。
* **Email 登入與雲端同步：** 透過 Firebase Auth 處理登入，並於 `copay_users/{uid}` 同步 `isPro` 狀態與 `email`。
* **房間級 Pro 共享（`isProRoom`）：** 免費用戶進入 `isProRoom === true` 的房間時，享有進階拆帳、CSV 匯出及無廣告體驗。離開該房間後恢復免費限制。

## 3. 廣告與收費整合規範

* **雙 Stripe 連結維護：**
  * `STRIPE_PAYMENT_LINK` (PRO 解鎖): 必須連接至 HK$38 一次性付款連結，且保留 `?vip=COPAY38` 自動解鎖邏輯與 `VALID_VIP_CODES` 陣列。
  * `STRIPE_DONATION_LINK` (隨緣樂助): 必須保留在設定頁底部的「請飲凍檸茶」按鈕，連接至 HK$20 贊助連結。
* **PRO 用戶零廣告 (Dynamic AdSense)：** 
  * 嚴禁在 `<head>` 靜態硬編碼載入 `adsbygoogle.js`。
  * 必須維持在 React `useEffect` 中依據 `hasNoAds = isPro || isProRoom` 動態載入腳本。一旦升級或進入 PRO 房間，需自動移除 DOM 中的廣告節點與 `.adsbygoogle` 元素。
  * 橫幅預留位應由 `{!hasNoAds && <AdBanner />}` 嚴格控制。