# Copay - 開發者與 AI 代理協作指南 (Agent Instructions)

本文件旨在為 AI 編程助手 (如 Gemini, Cursor, Copilot) 提供 Copay 專案的背景脈絡、底層架構限制及開發守則，確保未來的更新不會破壞現有架構。

## 🏗️ 核心架構限制 (CRITICAL RULES)

1. **絕對的「單一 HTML 檔案」原則**
   - 所有的代碼（HTML 結構、Tailwind CSS、React 邏輯、Firebase 呼叫）**必須**全部寫在 `index.html` 內。
   - **嚴禁**拆分 `.js`, `.css`, 或 `.jsx` 檔案。
   - 這是為了確保非技術用戶只需下載一個檔案就能部署或分享。

2. **Babel Standalone 語法限制**
   - 因為我們在瀏覽器端使用 `@babel/standalone` 即時編譯 JSX，**嚴禁使用任何需要 Node.js 打包工具 (Webpack, Vite) 的語法**。
   - ❌ 禁用：`import React from 'react'`, `export default App`, `require('...')`。
   - ✅ 必須使用解構賦值：`const { useState, useEffect, useMemo } = React;`。

3. **Firebase Compat SDK**
   - 目前使用的是 Firebase 10.14.1 的 **Compat 版本**。
   - ✅ 必須使用：`firebase.database()` 和 `db.ref('...')`。
   - ❌ 禁用：Firebase v9/v10 的 Modular 語法 (如 `getDatabase()`, `onValue()`, `ref()`)。

4. **URL 與路由管理**
   - 沒有 React Router。路由由 URL Query Parameter (`?room=XXXXXX`) 和前端的 `activeTab` State 控制。
   - **重要**：在更新 URL 時，必須包裹在 `try...catch` 中，避免在 Sandbox / 預覽 iframe 環境下觸發 `SecurityError` 導致 App 崩潰。

## 📝 專案當前狀態 (Current State)

* **品牌重塑完成**：已全面移除舊有的 "BillMate" 痕跡，統一更名為 "Copay" (分拆為 Copay Lite 與 Copay Travel)。
* **資料庫路徑更新**：Firebase 路徑已正式遷移至 `copay_rooms/...`，擁有完全乾淨的資料結構。
* **PWA 與 UI 完善**：已加入動態 Icon 繪製（Fallback）確保 iOS 加入主畫面時的體驗。
* **公數錢包 (Common Pool)**：Copay Travel 專屬功能，支援防超支檢查與多除小補結算機制。

## ✅ 最近完成的任務 (Completed Tasks)

1. **60 日自動清理機制**：
   - 在每次 App 載入時 (`useEffect`) 掃描 `localStorage` 內的房間。
   - 檢查 Firebase 中 `lastUpdated` 時間戳，若超過 60 日（60 * 24 * 60 * 60 * 1000 毫秒）無更新，自動觸發 `.remove()` 徹底銷毀房間，並同步清理本機清單。
2. **手動房間管理 (退出/刪除)**：
   - 於「房間列表」頁面加入「⚙️ (管理)」按鈕。
   - 支援「退出 (僅刪除本機 `localStorage` 關聯)」以及「徹底刪除 (呼叫 Firebase 移除整個房間資料)」。
3. **URL SecurityError 修復**：
   - `window.history.replaceState` 已加入 `try...catch` 保護。

## 🚀 待辦事項與未來規劃 (Pending / Next Steps)

下次開發時，可從以下功能著手：

1. **AI 掃描單據自動入帳 (規劃中)**
   - 規劃如何整合 Google Cloud Vision API 或 Gemini API 進行收據圖片識別。
   - 需要設計一個輕量級的 UI 上傳介面，並將識別出的項目 (Items)、金額 (Amount) 轉化為 Copay 的新增開支表單數據。
2. **UI 動畫與微互動**
   - 考慮在切換 Tab 或新增帳目時，加入 Framer Motion 的輕量平替方案（如純 CSS Transition 或 Tailwind `animate-*` 類別），提升 App 的精緻度。
3. **結算清單優化**
   - 支援生成結算圖檔 (Canvas 轉 PNG) 方便分享，而不僅限於純文字 WhatsApp 複製。