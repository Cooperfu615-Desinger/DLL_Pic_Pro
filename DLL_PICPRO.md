# 專案名稱：DLL_PIC Pro (Personal Web Edition)
**版本**：1.1 (GitHub Actions CI/CD 版) | **日期**：2026-01-16
**類型**：Serverless 靜態網頁應用 (Single Page Application)

## 1. 專案概述
開發一個專供個人使用的 AI 圖像生成網頁工具。目標是利用 Google Gemini (NanoBanana) 模型的高速與低成本特性，實現類似 Midjourney 的生圖體驗。
應用程式採用**純前端架構**，並透過 **GitHub Actions** 實現自動化部署 (CI/CD)。

## 2. 技術架構 (Tech Stack)

* **託管平台 (Host)**：GitHub Pages
* **部署流水線 (CI/CD)**：**GitHub Actions** (自動化構建與發布)
* **核心語言**：HTML5, JavaScript (ES6+), CSS3
* **UI 框架**：Tailwind CSS (透過 CDN 引入)
* **API 通訊**：原生 `fetch` API (Client-side Request)
* **數據存儲**：瀏覽器 `localStorage` (僅用於存儲 API Key 與用戶偏好設定)

## 3. 核心功能需求

### 3.1 設置與認證 (BYOK - Bring Your Own Key)
* **安全原則**：嚴禁將 API Key 寫入 GitHub Secrets 或代碼中（防止洩漏）。
* **交互邏輯**：
    1.  用戶首次進入網頁，檢測 `localStorage` 是否有 Key。
    2.  若無，彈出設置視窗供用戶貼上 Google Gemini API Key。
    3.  提供「更換 Key」與「選擇模型 (Google/Grok)」的下拉選單。

### 3.2 圖像生成 (Text-to-Image)
* **核心模型**：Google Gemini 2.5 Flash (NanoBanana) / Imagen 3。
* **功能**：Prompt 輸入、生圖、預覽、下載圖片。

### 3.3 圖片反推提示詞 (Image-to-Prompt)
* **核心模型**：Google Gemini 2.5 Flash (Vision)。
* **功能**：上傳圖片 -> 分析 -> 自動填入 Prompt。

## 4. 部署規範 (GitHub Actions) **[新增章節]**

### 4.1 Workflow 設定
* 需建立 `.github/workflows/deploy.yml` 文件。
* **觸發條件**：當 `main` 分支有代碼 push 時觸發。
* **權限設定**：
    ```yaml
    permissions:
      contents: read
      pages: write
      id-token: write
    ```
* **構建步驟 (Job Steps)**：
    1.  Checkout 代碼。
    2.  Setup Pages。
    3.  Upload Artifact (上傳靜態文件)。
    4.  Deploy to GitHub Pages。

## 5. UI/UX 設計規範
* **風格**：極簡主義、深色模式 (Dark Mode)。
* **響應式**：支援 Desktop 與 Mobile。

## 6. API 接口規範 (多模型預留)
* **Google Gemini**: `POST` `.../gemini-2.5-flash:generateContent`
* **xAI Grok**: `POST` `https://api.x.ai/v1/chat/completions` (預留)

---

## 給 AI 開發者 (Antigravity) 的指令：

> "請基於上述 v1.1 規範執行開發：
> 1. 編寫 `index.html` (含 CSS/JS)。
> 2. **新增**：編寫一份標準的 `.github/workflows/deploy.yml` 文件，用於將靜態頁面自動部署到 GitHub Pages。
> 3. 保持 BYOK 架構，不要在 Workflow 中注入任何 Secrets。"