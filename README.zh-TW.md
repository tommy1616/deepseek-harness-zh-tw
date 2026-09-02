# DeepSeek Harness — 台灣繁體中文特化版

[English](README.md) | [简体中文](README.zh.md) | 繁體中文（台灣）

這是 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 的社群維護台灣繁體中文特化版本。核心功能與官方版本保持同步，並針對台灣使用者提供一致的繁體中文介面與回答語言。

## 特化內容

- 內建 `zh-TW`（繁體中文／台灣）介面語系。
- 全面採用台灣常用詞彙，例如「一般設定」、「引導傳送」、「設定檔」、「資料夾」、「日誌」與「工作階段」。
- 預設 System Prompt 會要求模型在使用者以中文提問時，優先使用台灣繁體中文回答。
- 保留官方版本的插件架構、Web UI、CLI 與 LLM 整合能力。

## 執行

請先安裝 Node.js，接著從本專案目錄執行：

```sh
pnpm install
pnpm run build
pnpm dsh web
```

啟動 Web UI 時可加上 `--no-open`，只啟動伺服器而不自動開啟瀏覽器：

```sh
pnpm dsh web --no-open
```

## 語系設定

啟動後，於 Web UI 的語言設定選擇「繁體中文（台灣）」。若模型沒有依預期使用台灣繁體中文，可在對話開頭明確指定「請使用繁體中文（台灣）回答」。

## 上游專案

- 官方專案：[deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)
- 官方文件：[DeepSeek Harness Documentation](https://deepseek-harness.github.io/deepseek-harness/)
- 問題回報與討論：[GitHub Discussions](https://github.com/deepseek-ai/deepseek-harness/discussions)

本版本的翻譯與用語調整屬於社群貢獻；上游版本更新後，會盡量同步功能並保留台灣繁體中文支援。
