# 🤖 Copay - AI Agent Handover Document

Hello AI Developer! If you are reading this, you are tasked with assisting the human developer in maintaining, debugging, or expanding the **Copay** web application. Please read these constraints and architectural notes carefully BEFORE generating any code.

## 🏗️ Architecture Context
- **Single-File Setup**: The entire application (React components, state management, CSS, Firebase logic) is currently housed within a single `index.html` file.
- **In-Browser Compilation**: We use `@babel/standalone` to compile JSX into JavaScript on the fly in the browser. 

## ⚠️ Critical Danger Zones (Why the App Crashes)
The app has a history of encountering the **"White Screen of Death"** (where the React app fails to mount, showing only the background color). This is almost exclusively caused by Babel failing to parse the JSX. 

**RULES TO PREVENT FATAL CRASHES:**
1. **No Complex Inline SVGs or Filters**: Avoid heavy SVG filters (like `feTurbulence` or `feDisplacementMap`) embedded in the CSS or JSX. They have previously caused browser memory exhaustion and Babel parsing errors.
2. **Watch Your JSX Syntax Strictness**: `@babel/standalone` is extremely unforgiving. 
   - Never use unescaped characters like `>` or `<` inside JSX text nodes. Always wrap them like `{"<"}` or `{"->"}`.
   - Do not use complex array destructuring directly inside JSX mapping if it can be avoided (e.g., `Object.entries(obj).map(([, val]) => ...)` caused crashes). Map it cleanly.
3. **Truncation Risk**: Because the HTML file is extremely long (>2000 lines), AI outputs often get truncated. **Always ensure your code blocks are complete.** If you must modify a small part, provide specific instructions to replace a specific function rather than outputting the entire file, OR make sure the user knows to combine parts.
4. **localStorage Access**: Always wrap `localStorage.getItem()` and `localStorage.setItem()` in `try-catch` blocks. Browsers in Private/Incognito mode will block access and crash the entire app if unhandled.

## 💾 Firebase Realtime DB Structure
- Path: `billmate_rooms/{roomId}`
- The app uses real-time listeners (`ref.on('value')`). Be careful with state updates inside the listener to prevent infinite re-rendering loops.
- **Data Slimming Requirement**: Storage on the Free Spark Plan (1GB) is precious. Always truncate strings (e.g., `title.substring(0, 80)`) and use `toFixed(2)` for numbers before writing to Firebase.

## 🚀 Current Unfinished Business / WIP Tasks
1. **60-Day Auto-Cleanup Logic**: 
   - We need a reliable mechanism to delete rooms from Firebase if `Date.now() - updatedAt > 60 days`. We attempted client-side cleanup, but it needs robust error handling to prevent deleting active rooms by mistake.
2. **Manual Room Deletion**:
   - The UI needs a persistent trash bin icon in the "Rooms List" tab allowing owners to delete rooms locally and from Firebase.
3. **PWA / iOS Home Screen Logo Issue**:
   - iOS Safari aggressively caches `apple-touch-icon`. A fallback script drawing the logo on a `<canvas>` and injecting the Data URL was attempted to bypass GitHub Pages case-sensitivity and cache issues. This needs monitoring to ensure the logo always appears when adding to the home screen.
4. **Codebase Migration (Future)**:
   - The single-file HTML approach has reached its maintainability limit. The next major architectural step should be breaking this into a proper Node.js project (e.g., using Vite + React). Suggest this to the human developer if they ask for massive new features.

## 🎯 Your Directives
- **Do not break the existing UI**: The app uses a delicate mix of Tailwind CSS and custom glass-morphism classes (`.glass-dark`, `.teal-pill`, etc.). Keep the aesthetic intact.
- **Always think about Babel**: Before giving code, ask yourself: "Will `@babel/standalone` parse this without throwing a SyntaxError?"
- **Collaborative Tone**: Be concise, direct, and protective of the app's stability.