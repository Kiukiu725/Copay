# Copay App - AI Agent Instructions (.cursorrules / guidelines)

Hello AI Developer! When modifying or analyzing the Copay App codebase, please strictly adhere to the following rules to maintain the project's integrity, architecture, and unique UI/UX.

## 1. Codebase Architecture (CRITICAL)
*   **Single-File Architecture:** The entire application MUST remain within a SINGLE `index.html` file. 
*   **Do NOT** split the code into separate `.js`, `.css`, or component files.
*   **React + Babel:** The app uses React 18 via CDN and Babel standalone. All React code is inside `<script type="text/babel">`.
*   **Tailwind CSS:** Styling is handled via Tailwind CSS CDN. Use utility classes extensively.

## 2. UI/UX & Styling Guidelines
*   **Design Language:** The app uses a watercolor aesthetic with "Glassmorphism" UI components.
*   **Custom CSS Classes:** Always use the predefined custom classes for consistent styling:
    *   `glass-dark`: For main cards and modals (white translucent with blur).
    *   `teal-pill`: For primary action buttons (gradients of teal/blue).
    *   `gold-pill`: For premium/Pro features or warning actions (gradients of dark blue to gold text).
    *   `ios-uniform-field`: For native-looking select/input fields.
*   **Animations:** Use predefined micro-interactions for dynamic rendering:
    *   `animate-fade-in-up`: For list items rendering.
    *   `animate-scale-in`: For modals and pop-up tooltips.
    *   `animate-fade-in`: For subtle text or state changes.
*   **Icons:** Use standard Emojis as icons to keep the app lightweight (e.g., ✈️, 💰, 🍱).

## 3. Business Logic & Premium Features
*   **Room Modes:** `daily` (Copay Lite - HKD only) and `travel` (Copay Travel - Multi-currency + Pool fund).
*   **Pro Room Sharing Logic (IMPORTANT):** 
    *   Pro features are unlocked on a **Per-Room Basis**, not purely per-user.
    *   If a user with local Pro status (`isPro === true`) enters or creates a travel room, they must set `isProRoom: true` on the Firebase database for that room.
    *   If a free user enters a room where `isProRoom === true`, they automatically gain access to all Pro features (Advanced Split, CSV Export) *for that specific room only*.
    *   Paywalls should ONLY trigger if `!isPro && !isProRoom`.
*   **The Pool Fund (公數):** Identified by `POOL_PAYER_KEY = "🏦 公數錢包"`. It tracks contributions per member and validates if there are enough funds before allowing a pool expense.

## 4. State Management & Real-time Sync
*   **Firebase:** Uses Firebase Realtime Database. Always use `on("value")` for real-time synchronization.
*   **Local Storage:** Used for tracking user's recent rooms (`copay_brand_rooms_v1`) and personal Pro status (`copay_pro_travel_unlocked`).
*   **Performance:** Use `useMemo` for heavy calculations (like `settlement`, `poolStats`, `chartAndReportData`) to prevent UI lag on re-renders.

## 5. Modifying Code Example Protocol
*   When asked to add a feature, locate the specific component or function inside the large `App` function.
*   Ensure variables and states depend correctly on the current context (e.g., passing dependencies to `useMemo` arrays).
*   Never remove the `setupAppIcons` IIFE at the top; it's required for iOS PWA fallbacks.