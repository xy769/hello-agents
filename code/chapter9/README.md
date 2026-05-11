# Chapter 9 - 上下文工程示例代码

## 📋 概述

本目录包含第九章"上下文工程"的所有示例代码和演示文件，涵盖 ContextBuilder、NoteTool、TerminalTool、MemoryTool 四大核心组件的用法。

---

## 📁 目录结构

```
chapter9/
├── 01_context_builder_basic.py      # ContextBuilder 基础用法
├── 02_context_builder_with_agent.py # ContextBuilder 与 Agent 集成
├── 03_note_tool_operations.py        # NoteTool 基本操作
├── 04_note_tool_integration.py       # NoteTool 高级集成
├── 05_terminal_tool_examples.py      # TerminalTool 使用示例
├── 06_three_day_workflow.py          # 完整三天工作流演示
├── codebase_maintainer.py             # 代码库维护助手（核心组件）
├── codebase/                          # 示例代码库
│   ├── data_processor.py
│   ├── api_client.py
│   ├── utils.py
│   └── models.py
├── data/sales_2024.csv               # 示例销售数据（40+条记录）
└── logs/app.log                      # 示例应用日志
```

---

## 🚀 快速开始

### 0. 安装依赖

**本目录中的示例代码依赖于 `hello-agents` 包，需要先安装：**

```bash
pip install hello-agents
```

> ⚠️ **不要直接从本仓库目录运行示例**，否则会报 `ModuleNotFoundError: No module named 'hello_agents'`。请在任意目录下运行，或使用虚拟环境。

### 1. 配置嵌入模型（必需）

```python
import os
# 使用 TF-IDF（无需额外依赖或下载）
os.environ['EMBED_MODEL_TYPE'] = 'tfidf'
os.environ['EMBED_MODEL_NAME'] = ''  # 必须清空
```

### 2. 运行示例

```bash
# 无需 LLM
python 03_note_tool_operations.py
python 05_terminal_tool_examples.py

# 需要 LLM
python 06_three_day_workflow.py
```

---

## ⚙️ 嵌入模型配置方案

### 方案一：TF-IDF（推荐用于测试）

```python
os.environ['EMBED_MODEL_TYPE'] = 'tfidf'
os.environ['EMBED_MODEL_NAME'] = ''
```

| 优点 | 缺点 |
|------|------|
| ✅ 无需额外依赖 | ⚠️ 语义理解能力较弱 |
| ✅ 无需 API key | |
| ✅ 无需下载模型 | |

### 方案二：本地 Transformer（推荐离线使用）

```python
os.environ['EMBED_MODEL_TYPE'] = 'local'
os.environ['EMBED_MODEL_NAME'] = 'sentence-transformers/all-MiniLM-L6-v2'
os.environ['HF_TOKEN'] = 'your_huggingface_token'
```

**要求**：
- 安装：`pip install sentence-transformers`
- Hugging Face Token（从 https://huggingface.co/settings/tokens 获取）
- 首次运行下载模型（约 90MB）

**配置 Token 方式**：
```bash
# 方式一：使用 huggingface-cli（推荐）
pip install huggingface-hub
huggingface-cli login

# 方式二：环境变量
export HF_TOKEN="hf_your_token_here"
```

### 方案三：通义千问 DashScope（推荐生产环境）

```python
os.environ['EMBED_MODEL_TYPE'] = 'dashscope'
os.environ['EMBED_MODEL_NAME'] = 'text-embedding-v3'
os.environ['DASHSCOPE_API_KEY'] = 'your_dashscope_api_key'
```

**要求**：
- 注册：https://dashscope.aliyun.com/
- 安装：`pip install dashscope`

---

## 📖 示例文件说明

### 基础示例

| 文件 | 功能 |
|------|------|
| `01_context_builder_basic.py` | ContextBuilder 基本用法、上下文包创建和管理、Token 限制、上下文优先级 |
| `02_context_builder_with_agent.py` | ContextBuilder 与 SimpleAgent 集成、自动上下文管理、对话历史处理 |
| `03_note_tool_operations.py` | NoteTool CRUD 操作、笔记搜索、标签管理、笔记导出 |
| `04_note_tool_integration.py` | NoteTool 与 ContextBuilder 集成、长期项目追踪、基于历史笔记的建议 |
| `05_terminal_tool_examples.py` | TerminalTool 典型场景：文件分析、日志分析、代码库分析、安全特性 |

### 高级示例

#### `06_three_day_workflow.py` - 完整长程智能体工作流

**包含**：
- 第一天：探索代码库
- 第二天：分析代码质量
- 第三天：规划重构任务
- 一周后：检查进度
- 跨会话连贯性演示
- 三大工具协同演示

**示例代码库内容**（`./codebase`）：
- `data_processor.py` - 数据处理模块（含多个 TODO）
- `api_client.py` - API 客户端（需要改进错误处理）
- `utils.py` - 工具函数（需要优化）
- `models.py` - 数据模型（需要补充验证）

#### `codebase_maintainer.py` - 核心组件

集成了四大工具：

```python
self.memory_tool = MemoryTool(
    user_id=project_name,
    memory_types=["working"]  # 只使用工作记忆，避免需要 Qdrant
)
```

---

## 🧪 演示数据说明

### `data/sales_2024.csv`

| 字段 | 说明 |
|------|------|
| date | 日期 |
| product | 产品 |
| category | 类别：Electronics, Furniture |
| quantity | 数量 |
| price | 价格 |
| customer_id | 客户ID |
| region | 地区：North, South, East, West |

### `logs/app.log`

- 时间：2024-01-19 14:00 到 23:30
- 日志级别：INFO, WARNING, ERROR
- 错误类型：DatabaseConnectionError, ValidationError 等

### `codebase/`

- 4 个 Python 模块
- 10+ 个 TODO 注释
- 支持代码分析、TODO 查找、函数搜索、代码统计

---

## 🐛 常见问题

### Q1: ModuleNotFoundError: No module named 'hello_agents'

**原因**：直接在本仓库目录运行代码，而没有安装 `hello-agents` 包。

**解决**：
```bash
pip install hello-agents
# 然后在任意目录运行示例，不要在仓库目录下运行
```

### Q2: RuntimeError - 所有嵌入模型都不可用

**原因**：嵌入模型配置不正确

**解决**：
```python
os.environ['EMBED_MODEL_TYPE'] = 'tfidf'
os.environ['EMBED_MODEL_NAME'] = ''  # 必须设置！
```

### Q3: Qdrant 连接失败

**解决方案**：
1. 使用只需 working 记忆的配置（`codebase_maintainer.py` 已配置）
2. 或启动 Qdrant：`docker run -p 6333:6333 qdrant/qdrant`

### Q4: Hugging Face 模型下载失败

**解决方案**：
1. 配置 HF Token
2. 使用镜像：`export HF_ENDPOINT=https://hf-mirror.com`
3. 改用 TF-IDF

### Q5: TerminalTool 提示"不允许的命令"

**允许的命令白名单**：
- 文件操作：`ls`, `cat`, `head`, `tail`, `grep`, `find`
- 文本处理：`awk`, `sed`, `cut`, `sort`, `uniq`, `wc`
- 其他：`pwd`, `cd`, `tree`, `stat`

---

## 📚 LLM 配置

```python
from hello_agents import HelloAgentsLLM

# 默认配置（需要设置 OPENAI_API_KEY）
llm = HelloAgentsLLM()

# 或明确指定
llm = HelloAgentsLLM(
    api_key="your_api_key",
    base_url="https://api.openai.com/v1",
    model="gpt-4"
)
```

> 建议在 `.env` 文件中设置

---

## 🎯 学习路径建议

### 运行顺序

1. **先运行无需 LLM 的示例**：
   - `03_note_tool_operations.py` → 了解 NoteTool
   - `05_terminal_tool_examples.py` → 了解 TerminalTool

2. **配置嵌入模型后运行**：
   - `01_context_builder_basic.py` → 理解上下文管理

3. **配置 LLM 后运行**：
   - `02_context_builder_with_agent.py` → Agent 集成
   - `04_note_tool_integration.py` → 高级集成
   - `06_three_day_workflow.py` → 完整工作流

### 推荐学习顺序

1. 基础概念 → `01_context_builder_basic.py`
2. 工具使用 → `03_note_tool_operations.py`, `05_terminal_tool_examples.py`
3. Agent 集成 → `02_context_builder_with_agent.py`
4. 高级应用 → `04_note_tool_integration.py`
5. 实战案例 → `06_three_day_workflow.py`
