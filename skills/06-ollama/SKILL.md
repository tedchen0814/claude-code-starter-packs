---
name: cc-ollama
description: Claude Code 安裝本地 AI Ollama。說「安裝 Ollama」「本地 AI」時載入。
---

# 安裝本地 AI Ollama（Claude Code 版）

1. 下載安裝：https://ollama.com/download
2. `ollama --version`
3. 拉取模型：`ollama pull llama3.2:latest` 或 `ollama pull gemma3:latest`
4. 如需 GPU 加速確認：`ollama run llama3.2:latest "hello"`

## 讓 Claude 或自製工具呼叫本地模型
Ollama 啟動後會在本機提供 HTTP API（`http://localhost:11434`），不需要另外裝 MCP。測試：
```bash
curl http://localhost:11434/api/generate -d '{"model":"llama3.2:latest","prompt":"hello","stream":false}'
```

回報：Ollama 版本、模型清單、GPU 狀態。
