# Copay — 日常夾錢與海外旅遊分帳神器 💰✈️

Copay 是一個專為朋友聚會、飯局以及海外旅行而設計的實時協作分帳 Web App。它免卻了繁瑣的下載與註冊過程，只需一個連結，全團朋友即可實時同步記帳。

## 🌟 核心功能 (Features)

### 1. 雙模式設計
*   **Copay Lite (日常版)：** 免費使用，專為日常飯局、搭車夾錢設計，介面極簡，快速輸入港幣 (HKD) 開支。
*   **Copay Travel (海外旅遊版 / Pro)：** 專為長途旅行設計，支援多國貨幣、即時匯率轉換、信用卡手續費計算及「公數錢包」管理。

### 2. 👑 Pro 房間共享機制 (Virus Growth Feature)
*   **一人課金，全房共享：** 只要房間的創建者或其中一位進入者是「Travel Pro」付費用戶，該房間即會自動標記為 `isProRoom: true`。
*   **免費用戶受惠：** 免費用戶進入 Pro 房間後，可無條件免費使用所有進階功能（包括進階拆帳、匯出 CSV 等），大幅降低群組使用門檻，達至最佳的病毒式口碑營銷。

### 3. 🏦 獨家「公數錢包」系統 (Pool Fund)
*   完美解決旅行「入公數」的痛點。
*   支援隨時注資、顯示剩餘公數餘額。
*   結算時系統會自動將剩餘的公數按注資比例退還給相應成員。

### 4. ⚡ 智能對沖結算 (Smart Settlement)
*   自動計算複雜的債務關係，並將多重欠款簡化為最少的過數次數（例如：A 欠 B $50，B 欠 C $50，系統會直接指示 A 過數 $50 給 C）。
*   一鍵複製 WhatsApp / PayMe 過數清單。

### 5. 📊 報表與匯出
*   內建圓環圖分析消費類別比例。
*   一鍵匯出完整 Excel (CSV) 報表，詳細列明每筆帳目的折合港幣、墊支人及拆帳備註。

## 🛠️ 技術棧 (Tech Stack)
*   **前端框架：** React 18 (Standalone via Babel), HTML5
*   **樣式設計：** Tailwind CSS, 自定義 CSS 玻璃擬物化特效 (Glassmorphism)
*   **後端與資料庫：** Firebase Realtime Database (Compat SDK)
*   **架構：** Single HTML File (無須 Build，開箱即用，方便部署於任何靜態伺服器或 GitHub Pages)
*   **外部 API：** ExchangeRate-API (實時匯率抓取)

## 🚀 如何運行 (How to Run)
1. 下載 `index.html`。
2. 雙擊在任何現代瀏覽器 (Chrome, Safari, Edge) 中打開。
3. （可選）將檔案上傳至 Vercel, Netlify, GitHub Pages 或 Firebase Hosting 即可獲取公開連結。

## 📂 資料庫結構 (Firebase Database Schema)
```json
copay_rooms: {
  "{roomId}": {
    "roomId": "A1B2C3",
    "groupName": "東京之旅 ✈️",
    "roomMode": "travel",
    "isProRoom": true,
    "members": ["我", "Alex", "Chloe"],
    "defaultCurrency": "JPY",
    "cardFeePercent": 1.95,
    "poolContributions": { "我": 1000, "Alex": 1000 },
    "expenses": {
      "{expenseId}": {
        "title": "居酒屋",
        "amount": 5000,
        "currency": "JPY",
        "payer": "我",
        "category": "🍱 食飯",
        "date": "2026-11-26",
        "splitWith": ["我", "Alex"],
        "splitAdjustments": { "Alex": 50 } 
      }
    }
  }
}
```