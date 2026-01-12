# Learn Claude Code

> **免責聲明**：這是一個由 [shareAI Lab](https://github.com/shareAI-lab) 開發的獨立教育專案。它與 Anthropic 沒有附屬關係，也未獲得其認可或贊助。"Claude Code" 是 Anthropic 的商標。

**透過從頭開始建構一個 AI Agent，學習現代 AI Agent 的運作原理。**

[簡體中文文件](./README_zh.md)

---

**給讀者的一段話：**

我們建立這個儲存庫是因為對 Claude Code 的欽佩 —— **我們認為它是世界上能力最強的 AI 程式碼 Agent**。起初，我們試圖透過行為觀察和推測來逆向工程它的設計。但我們發布的分析充滿了不準確之處、毫無根據的猜測和技術錯誤。我們向 Claude Code 團隊以及任何被該內容誤導的人深表歉意。

在過去的六個月裡，透過構建和迭代真實的 Agent 系統，我們對**「什麼構成了一個真正的 AI Agent」**的理解已經發生了根本性的轉變。我們想與您分享這些見解。所有先前的推測性內容已被刪除，並替換為原創的教育材料。

---

> 支援 **[Kode CLI](https://github.com/shareAI-lab/Kode)**、**Claude Code**、**Cursor** 以及任何支援 [Agent Skills Spec](https://github.com/anthropics/agent-skills) 的 Agent。

<img height="400" alt="demo" src="https://github.com/user-attachments/assets/0e1e31f8-064f-4908-92ce-121e2eb8d453" />

## 這是什麼？

一個漸進式的教學，旨在揭開像 Kode、Claude Code 和 Cursor Agent 這樣的 AI 程式碼 Agent 的神秘面紗。

**5 個版本，總共約 1100 行程式碼，每個版本增加一個概念：**

| 版本 | 行數 | 新增內容 | 核心見解 |
|---------|-------|--------------|--------------|
| [v0](./v0_bash_agent.py) | ~50 | 1 個 bash 工具 | Bash 就是一切 (Bash is all you need) |
| [v1](./v1_basic_agent.py) | ~200 | 4 個核心工具 | 模型即 Agent (Model as Agent) |
| [v2](./v2_todo_agent.py) | ~300 | 待辦事項追蹤 | 明確的規劃 (Explicit planning) |
| [v3](./v3_subagent.py) | ~450 | 子代理人 (Subagents) | 各個擊破 (Divide and conquer) |
| [v4](./v4_skills_agent.py) | ~550 | 技能 (Skills) | 按需獲取領域專業知識 |

## 快速開始 (Quick Start)

```bash
pip install anthropic python-dotenv

# 設定您的 API
cp .env.example .env
# 編輯 .env 並填入您的 API key

# 執行任何版本
python v0_bash_agent.py  # 極簡版
python v1_basic_agent.py # 核心 Agent 迴圈
python v2_todo_agent.py  # + 待辦事項規劃
python v3_subagent.py    # + 子代理人
python v4_skills_agent.py # + 技能
```

## 核心模式 (The Core Pattern)

每個程式碼 Agent 其實就只是這個迴圈：

```python
while True:
    response = model(messages, tools)
    if response.stop_reason != "tool_use":
        return response.text
    results = execute(response.tool_calls)
    messages.append(results)
```

就是這樣。模型呼叫工具直到完成為止。其他所有東西都是為了優化這個過程。

## 檔案結構 (File Structure)

```
learn-claude-code/
├── v0_bash_agent.py       # ~50 行：1 個工具，遞迴子代理人
├── v0_bash_agent_mini.py  # ~16 行：極限壓縮版
├── v1_basic_agent.py      # ~200 行：4 個工具，核心迴圈
├── v2_todo_agent.py       # ~300 行：+ TodoManager (待辦事項管理)
├── v3_subagent.py         # ~450 行：+ Task (任務) 工具，Agent 註冊表
├── v4_skills_agent.py     # ~550 行：+ Skill (技能) 工具，SkillLoader (技能載入器)
├── skills/                # 範例技能（用於學習）
└── docs/                  # 詳細說明文件 (英文 + 中文)
```

## 使用 Agent 建構技能 (Using the Agent Builder Skill)

這個儲存庫包含了一個「後設技能 (meta-skill)」，教導 Agent 如何構建 Agent：

```bash
# 建立一個新的 Agent 專案架構
python skills/agent-builder/scripts/init_agent.py my-agent

# 或者指定複雜度等級
python skills/agent-builder/scripts/init_agent.py my-agent --level 0  # 極簡版
python skills/agent-builder/scripts/init_agent.py my-agent --level 1  # 4 個工具 (預設)
```

### 安裝技能以供正式環境使用

```bash
# Kode CLI (推薦)
kode plugins install https://github.com/shareAI-lab/shareAI-skills

# Claude Code
claude plugins install https://github.com/shareAI-lab/shareAI-skills
```

請參閱 [shareAI-skills](https://github.com/shareAI-lab/shareAI-skills) 以獲取完整的生產級技能集合。

## 關鍵概念 (Key Concepts)

### v0: Bash 就是一切 (Bash is All You Need)
一個工具。遞迴自我呼叫以產生子代理人。證明核心是非常微小的。

### v1: 模型即 Agent (Model as Agent)
4 個工具 (bash, read, write, edit)。完整的 Agent 就在一個函式中。

### v2: 結構化規劃 (Structured Planning)
Todo 工具使計畫變得明確。限制條件 (Constraints) 使複雜任務成為可能。

### v3: 子代理人機制 (Subagent Mechanism)
Task 工具生成隔離的子代理人。上下文保持乾淨。

### v4: 技能機制 (Skills Mechanism)
SKILL.md 檔案按需提供領域專業知識。知識被視為一等公民。

## 深入探討 (Deep Dives)

**技術教學 (docs/):**

| 英文 | 中文 |
|---------|------|
| [v0: Bash is All You Need](./docs/v0-bash-is-all-you-need.md) | [v0: Bash 就是一切](./docs/v0-Bash就是一切.md) |
| [v1: Model as Agent](./docs/v1-model-as-agent.md) | [v1: 模型即代理](./docs/v1-模型即代理.md) |
| [v2: Structured Planning](./docs/v2-structured-planning.md) | [v2: 結構化規劃](./docs/v2-结构化规划.md) |
| [v3: Subagent Mechanism](./docs/v3-subagent-mechanism.md) | [v3: 子代理機制](./docs/v3-子代理机制.md) |
| [v4: Skills Mechanism](./docs/v4-skills-mechanism.md) | [v4: Skills 機制](./docs/v4-Skills机制.md) |

**原創文章 (articles/) - 僅中文，社群媒體風格：**
- [v0文章](./articles/v0文章.md) | [v1文章](./articles/v1文章.md) | [v2文章](./articles/v2文章.md) | [v3文章](./articles/v3文章.md) | [v4文章](./articles/v4文章.md)
- [上下文缓存經濟學](./articles/上下文缓存经济学.md) - Agent 開發者的 Context Caching 經濟學

## 相關專案 (Related Projects)

| 儲存庫 | 用途 |
|------------|---------|
| [Kode](https://github.com/shareAI-lab/Kode) | 全功能開源 Agent CLI (生產級) |
| [shareAI-skills](https://github.com/shareAI-lab/shareAI-skills) | 適用於 AI Agent 的生產級技能 |
| [Agent Skills Spec](https://github.com/anthropics/agent-skills) | 官方規範 |

### 作為範本使用 (Use as Template)

Fork 並自定義您自己的 Agent 專案：

```bash
git clone https://github.com/shareAI-lab/learn-claude-code
cd learn-claude-code
# 從任何版本等級開始
cp v1_basic_agent.py my_agent.py
```

## 設計哲學 (Philosophy)

> 模型佔 80%，程式碼佔 20%。

像 Kode 和 Claude Code 這樣的現代 Agent 之所以有效，不是因為巧妙的工程設計，而是因為模型被訓練成一個 Agent。我們的工作是給它工具，然後不要礙事。

## 授權條款 (License)

MIT

---

**Model as Agent. That's the whole secret.**
**(模型即 Agent。這就是全部的秘密。)**

[@baicai003](https://x.com/baicai003)
