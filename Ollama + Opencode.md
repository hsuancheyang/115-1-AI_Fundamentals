# Windows + Ollama 地端模型 + OpenCode

設置指南，分為三大階段：安裝 Ollama → 拉取並運行模型 → 安裝並配置 OpenCode。

---

## ▸ 目標

- 完全離線/本地運行 LLM，零雲端 API Key
- Windows 環境使用 Ollama 管理地端模型
- OpenCode 連接地端模型，直接用於專案開發

---

## 階段一：安裝 Ollama（地端模型管家）

### 方法 A — 一行指令（推薦）

以 PowerShell（管理員或一般皆可），貼上以下命令後執行：

```powershell
irm https://ollama.com/install.ps1 | iex
```

### 方法 B — winget

```powershell
winget install --id Ollama.Ollama
```

### 方法 C — 直接下載安裝檔

前往 [https://ollama.com/download/windows](https://ollama.com/download/windows) 下載 `OllamaSetup.exe`，雙擊安裝。

---

### 安裝確認

打開 PowerShell，執行：

```powershell
ollama --version
```

成功後 Ollama 會自動在背景服務化，API 監聽於：

```
http://localhost:11434
```

可在瀏覽器驗證：開啟 [http://localhost:11434](http://localhost:11434) 確認無錯。

> GPU 支援：Ollama 為原生 Windows 應用，內建 NVIDIA / AMD Radeon GPU 加速。安裝時若偵測到獨立顯卡會自動啟用 CUDA/Vulkan。

---

## 階段二：拉取並運行地端模型

### 選擇模型名稱（常見推薦）

| 模型         | 大小指令               | 適合場景           |
| ------------ | ---------------------- | ------------------ |
| **Qwen2.5**  | `ollama pull qwen2.5`  | 綜合最佳、中英兼備 |
| **Llama3.2** | `ollama pull llama3.2` | Meta 官方模型      |
| **Gemma3**   | `ollama pull gemma3`   | Google 輕量級      |
| **Phi3.5**   | `ollama pull phi3.5`   | Microsoft 小型快速 |

### 拉取模型

假設選擇 Qwen2.5：

```powershell
ollama pull qwen2.5
```

> 此指令會從網路下載模型權重至本地資料夾（預設存放在 `C:\Users\<你的使用者>\AppData\Local\Programs\Ollama\models`）。

### 驗證已安裝的模型

```powershell
ollama list
```

應列出剛才拉取的模型名稱與大小。

### 測試運行（可選）

```powershell
ollama run qwen2.5
```

進入模型對話模式後，輸入一句話測試是否正常運作，然後用 `Ctrl+D` 離開。

---

## 階段三：Install OpenCode CLI

四種方法 **選一即可**：

### A — Scoop（推薦）

```powershell
scoop install opencode
```

### B — npm

```powershell
# 需已安裝 Node.js
npm i -g opencode-ai
```

### C — Chocolatey

```powershell
choco install opencode
```

### D — 安裝腳本

```powershell
# 需有 bash/shell（Git Bash、WSL 等）
curl -fsSL https://opencode.ai/install | sh
```

安裝完畢驗證：

```powershell
opencode --version
```

---

## 階段四：將 OpenCode 連接地端 Ollama

有兩種設定路徑：**一鍵指令**（最快）與 **手動組態**。

---

### 路徑 A — 使用 Ollama 的一鍵整合（推薦）

Ollama 提供 `launch` 指令可自動為 OpenCode 完成配置：

```powershell
ollama launch opencode
```

此指令會：

1. 提示你選擇要用的本地模型（例如 qwen2.5）
2. 自動設定 context length（需 ≥ 64k）
3. 啟動 OpenCode TUI，連結地端模型

> **注意**：確保你在專案資料夾內執行此指令。

---

### 路徑 B — 手動建立 `opencode.json`

若偏好完全控制配置，在專案根目錄或使用者層級建立組態檔。

#### 步驟 1：建立 OpenCode 組態目錄

在 PowerShell 中執行：

```powershell
mkdir -Force $env:USERPROFILE\.config\opencode
```

#### 步驟 2：建立 `opencode.json`

在 `$env:USERPROFILE\.config\opencode\opencode.json` 加入以下 JSON（用記事本或編輯器即可）：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen2.5": {
          "name": "Qwen 2.5"
        }
      }
    }
  }
}
```

#### 步驟 3：檢查 API 端點是否可達

在 PowerShell 中確認 OpenAI-compatible endpoint 正常：

```powershell
curl http://localhost:11434/v1/models
```

應回應包含你所拉取模型的 JSON。

---

## 階段五：啟動 OpenCode

### 方式一 — CLI 直啟

進入你的專案目錄並執行：

```powershell
cd C:\path\to\your_project
opencode
```

開啟 TUI 後，你會看到系統自動連結到 Ollama 地端模型。可透過 `/models` 確認可用模型列表。

### 方式二 — Ollama launch（同上）

```powershell
ollama launch opencode
# 或手動選型
ollama launch opencode --config
```

---

## 完成檢查清單 ✅

| #   | 檢查項目                           | 指令確認法                                           |
| --- | ---------------------------------- | ---------------------------------------------------- |
| 1   | Ollama 已安裝且服務中              | `ollama --version` + 瀏覽器開 http://localhost:11434 |
| 2   | 模型已拉取至本機                   | `ollama list`（應列出該模型）                        |
| 3   | OpenCode CLI 已安裝                | `opencode --version`                                 |
| 4   | opencode.json 包含 Ollama provider | 檢查檔案內容有 `baseURL: http://localhost:11434/v1`  |
| 5   | API endpoint 可連                  | `curl http://localhost:11434/v1/models` 有回傳       |
| 6   | OpenCode 啟動成功並使用地端模型    | `opencode` → `/models` 應顯示本地模型                |

---

##常見問題與排除

<details>
<summary><b>Q：拉取模型時網路很慢或失敗</b></summary>
A：Ollama 預設從 HuggingFace/官方 CDN 下載。可設定代理環境變數，或在 Ollama Desktop App 的 Setting 中手動指定模型儲存路徑至快取資料夾。
</details>

<details>
<summary><b>Q：OpenCode 回報 API Error / connection refused</b></summary>
A：先確認三個環節：<br>1. `ollama --version` 有回傳 → Ollama 服務正常<br>2. `curl http://localhost:11434/v1/models` 有 JSON 回傳 → OpenAI-compatible endpoint 運作中<br>3. opencode.json 的 baseURL 正確為 `http://localhost:11434/v1`（非 `/api`）
</details>

<details>
<summary><b>Q：記憶體不足 / GPU 未啟用</b></summary>
A：模型大小需足夠 RAM。一般建議：<br>- 7B 以上 ≈ CPU + 8GB+ RAM<br>- 14B+ ≈ 需要 NVIDIA 顯卡（CUDA）或 32GB+ RAM<br>可改用較小模型（如 phi3.5、gemma3:2b）降低資源需求。
</details>

<details>
<summary><b>Q：上下文視窗預設太少</b></summary>
A：OpenCode 需要 ≥ 64k context window。在 Ollama Desktop App Settings 中將 Context Length 調整為 64K 或更高，或在 Modelfile 設定 `num_ctx 65536`。
</details>

---

## 快速一頁回顧（最簡流程）

```
1. ir m https://ollama.com/install.ps1 | iex      ← 裝 Ollama
2. ollama pull qwen2.5                             ← 拉模型
3. scoa install opencode                           ← 裝 OpenCode
4. （手動建立 opencode.json，baseURL = localhost:11434/v1）
5. cd your_project && opencode                     ← 開始用！
```

零 API Key、完全離線的本地 AI coding agent 就此完成。
