# copilot-catch

自動抓取 **Google Gemini CLI** 與 **GitHub Copilot CLI** 的指令參考文件，透過 **Google Gemini API** 翻譯成繁體中文，並整理成 Markdown 文件。

## 功能

- 抓取 Gemini CLI [`commands.md`](https://github.com/google-gemini/gemini-cli/blob/main/docs/reference/commands.md)（GitHub raw markdown）
- 抓取 GitHub Copilot CLI [CLI command reference](https://docs.github.com/en/copilot/reference/cli-command-reference)（HTML 解析轉 markdown）
- 使用 Gemini API（`gemini-2.0-flash`）翻譯成繁體中文，指令名稱原樣保留
- 比對上次執行的原始內容，**內容無變更時跳過翻譯**，節省 API 呼叫
- 有變更時將 diff 記錄 prepend 至 `changelog.md`，完整保留歷次更新

## 輸出檔案

| 檔案 | 說明 |
|------|------|
| `output.md` | 最新的繁體中文指令參考文件 |
| `changelog.md` | 每次有內容變更時的 diff 紀錄（新的在上方） |
| `_cache.json` | 上次抓取的原始內容快取（供 diff 比對用） |

## 安裝

需要 [uv](https://github.com/astral-sh/uv)。

```bash
git clone <this-repo>
cd copilot-catch
uv sync
```

## 設定

複製範例並填入你的 [Gemini API Key](https://aistudio.google.com/app/apikey)：

```bash
cp .env.example .env
# 編輯 .env，填入 GEMINI_API_KEY=your-api-key
```

## 使用

```bash
uv run python fetch_and_translate.py
```

查看結果：
- `output.md`：完整繁體中文文件
- `changelog.md`：本次與上次的指令差異（若有變更）

## 執行流程

```
抓取原始內容
    ↓
與 _cache.json 比對
    ↓ 有變更              ↓ 無變更
prepend changelog.md    跳過，直接結束
    ↓
呼叫 Gemini API 翻譯
    ↓
寫出 output.md
```
