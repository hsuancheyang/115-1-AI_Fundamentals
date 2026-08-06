## Windows 環境安裝 OpenCode + 申請 Zen 金鑰＋啟動操作

---

### Step 1：執行安裝 opencode

在 PowerShell（以管理員身分）或終端機 (cmd) 命令提示字元 中選擇一種方法：

**方法 A — Scoop（Windows 推薦）**

```powershell
scoop install opencode
```

**方法 B — npm（Node.js 需已安裝）** ==課堂使用方法==

```powershell
npm install -g opencode-ai
```

**方法 C - 安裝桌面版而非 CLI：**

1. 至 [https://opencode.ai/download](https://opencode.ai/download) 下載 Windows (x64) 安裝檔
2. 執行 installer
3. Desktop 內建附帶本地 opencode-cli server，無需手動啟動

安裝完成後驗證：

```powershell
opencode --version
```

---

### Step 2：申請 OpenCode Zen 金鑰

1. **登入**：前往 [https://opencode.ai/auth](https://opencode.ai/auth)（Chrome/Edge 等瀏覽器）
2. **註冊帳號**並填寫帳單資訊
3. **複製 API Key**：從 Dashboard 或 Account 頁面複製你的 Zen API Key

---

### Step 3：在 OpenCode TUI 中使用 Zen金鑰啟動

```powershell
# 1) 進入你的專案資料夾，啟動 opencode
cd %PATH_TO_YOUR_PROJECT%
opencode

# 2) 在 opencode TUI 中輸入：
/connect

# 3) 選擇 "OpenCode Zen" provider

# 4) 貼上你在 Step 2 複製的 API key

# 5) 驗證模型列表是否正常載入：
/models
```

---

## Step 4：IDE 整合（可選）

OpenCode 也提供 VS Code / Cursor 擴充功能。在 Extension Marketplace 搜尋 "OpenCode" 安裝後，即可用 `Ctrl+Esc` 快捷鍵開啟 OpenCode 終端面板。

---

### 使用其他 Provider 的注意事項

Zen **不是必須的**。你也可以連接：

- **Anthropic API Key** — [console.anthropic.com](https://console.anthropic.com) 取得
- **OpenAI API Key** — [platform.openai.com](https://platform.openai.com) 取得
- **Google Gemini Key** — [aistudio.google.com](https://aistudio.google.com) 取得

連接方式同樣透過 `/connect` → 選擇 provider → 貼上 key。

---

現在你可以先從 **Step 1** 開始安裝，裝好後再回來看後續的 Zen 金鑰流程。
