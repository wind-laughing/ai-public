# 跨境电商 AI 运营平台

作者 刘林阳  📱18976426325   📧  liulinyang85@yeah.net           
## 项目概况

一套为跨境电商卖家打造的 AI 运营辅助系统，覆盖从选品素材（多语种文案生成）、客服、风控到数据分析的全链路运营场景。兼具命令行（CLI）与 Web 可视化（FastAPI）双模式。

**技术栈**：Python 3.12 · FastAPI · PyTorch · DeepSeek API · Plotly · Jinja2  

**项目目录**：`$eks = D:\agent-projects\eks-demo`

![](../05-Annex/eks-demo-home-screen.png)

---

## 五大场景

### 场景 1：多语种文案生成

**职责覆盖**：AI 内容智能化 + 选品素材

按"产品 × 语种"矩阵批量生成多语种营销文案，支持 9 种语言（中文、英文、日文、韩文、法文、德文、西班牙文、葡萄牙文、阿拉伯文）。输出三种格式（TXT / MD / HTML），按产品建子目录存放，每次运行保存独立历史。

**用户操作流程**：
1. 选择产品（复选框去除，纯高亮选中）
2. 选择语种
3. 点击"生成文案"→ 调用 DeepSeek API 批量生成
4. 查看结果并下载/打开对应格式文件

![](../05-Annex/eks-demo-01-multi-lang.png)

![](../05-Annex/eks-demo-01-multi-lang-his.png)

![](../05-Annex/eks-demo-01-multi-lang-example.png)

![](../05-Annex/eks-demo-01-multi-lang-example-2.png)

![](../05-Annex/eks-demo-01-multi-lang-example-3.png.png)



### 场景 2：智能客服问答

**职责覆盖**：AI 智能客服

模拟跨境电商场景下的客户咨询对话。用户输入问题，系统调用大模型生成回答，并记录会话历史。支持多轮对话上下文感知。

![](../05-Annex/eks-demo-02-aichat-main.png)

### 场景 3：订单风控系统

**职责覆盖**：数据建模 + 风险控制 + 模型调优

基于 PyTorch 全连接神经网络（3 层，8 维特征）的订单欺诈检测系统。

- **特征工程**：金额、注册天数、改址次数、30天订单数、30天均价 + 金额偏离比 + 下单时段 + 距今天数
- **时间平移机制**：自动将 CSV 中的订单时间戳对齐当前时间，保证距今天数特征不随时间退化
- **训练 + 推理闭环**：每次运行加载数据→特征工程→训练→批量推理→输出风险看板
- **风险看板**：汇总卡片 + Top20 高风险清单 + 时段分布条图 + 欺诈vs正常维度对比
- **实时演示**：4 个典型 Case 的单条推理展示，新账号大额异常→BLOCK
- **模型**：PyTorch，F1=1.0（测试数据），~21KB，200轮训练

![](../05-Annex/eks-demo-03-order-control-main.png)

![](../05-Annex/eks-demo-03-order-control-main2.png)

![](../05-Annex/eks-demo-03-order-control-report.png)

![](../05-Annex/eks-demo-03-order-control-report1.png)

![](../05-Annex/eks-demo-03-order-control-report2.png)

![](../05-Annex/eks-demo-03-order-control-report3.png)

![](../05-Annex/eks-demo-03-order-control-reoprt4.png)

![](../05-Annex/eks-demo-03-order-control-report5.png)


### 场景 4：选品分析

**职责覆盖**：运营数据分析

基于销售趋势数据的可视化分析看板。

- **销售数据区**：滑动窗口表格（固定表头 + 6 行），中文列名（日期/品类/销量/收入/客单价/新客/退货率）
- **执行分析**：生成销售趋势报告（TXT/MD/HTML 三种格式）
- **历史管理**：按时间戳子目录输出，24 小时内报告标记 NEW

![](../05-Annex/eks-demo-04-product-pick-report0.png)

![](../05-Annex/eks-demo-04-product-pick-report1.png)

![](../05-Annex/eks-demo-04-product-pick-report2.png)

![](../05-Annex/eks-demo-04-product-pick-report3.png)

![](../05-Annex/eks-demo-04-product-pick-report4.png)

![](../05-Annex/eks-demo-04-product-pick-report5.png)

![](../05-Annex/eks-demo-04-product-pick-report6.png)

### 场景 5：运营复盘

**职责覆盖**：数据驱动决策

从选品分析延伸的运营复盘场景，覆盖销售数据查看、复盘报告生成与历史管理。

- **销售数据区**：同场景4中文列名滑动窗口
- **复盘报告**：生成运营复盘报告（TXT/MD/HTML 三种格式）
- **历史管理**：按时间戳子目录输出，NEW 标记

![](../05-Annex/eks-demo-05-operation-review-report0.png)

![](../05-Annex/eks-demo-05-operation-review-report1.png)

![](../05-Annex/eks-demo-05-operation-review-report2.png)

![](../05-Annex/eks-demo-05-operation-review-report3.png)


---

## 系统架构

### 双轨制

```
┌──────────────────────────────────────────────────────────────┐
│                      跨境电商 AI 运营平台                        │
├──────────────────────────┬───────────────────────────────────┤
│      CLI 命令行版          │         FastAPI Web 版              │
│    python main.py        │    python server.py → :8000         │
│                          │                                    │
│  终端直接执行，适合本地     │   可视化操作，适合演示/汇报              │
│  批量任务、脚本集成         │   场景卡片导航 + 实时交互               │
├──────────────────────────┴───────────────────────────────────┤
│                    共享业务逻辑层                                 │
│   scripts/  — 各场景独立分析引擎（CLI/Web 共用）                     │
│   utils/    — 图表工具（Plotly + Mermaid）                       │
│   data/     — CSV 数据源 + 模型文件                                │
│   config.py — 路径与 API Key 配置                                 │
└──────────────────────────────────────────────────────────────┘
```

### 目录结构

```
eks-demo/
├── 00-prd/           # 需求文档（PRD，用户编写）
├── 00-upgrade/       # 版本升级记录（AI 编写，与 PRD 版本号对齐）
├── 00-doc/           # 技术原理说明文档
├── templates/        # FastAPI Jinja2 模板（各场景独立页面）
│   ├── index.html         # 首页：5 场景卡片导航
│   ├── scene1.html        # 多语种文案生成
│   ├── scene2.html        # 智能客服
│   └── scene5.html        # 运营复盘
├── scripts/          # 各场景分析逻辑（CLI + Web 共用）
│   ├── scene01_copywriting.py
│   ├── scene02_customer_service.py
│   ├── scene03_fraud_detection.py
│   ├── scene04_product_selection.py
│   └── scene05_data_review.py
├── utils/            # 图表工具
│   └── charts.py          # Plotly + Mermaid 生成
├── data/             # 数据源
│   ├── products.csv
│   ├── orders.csv
│   └── sales_trend.csv
├── output/           # 运行输出（按场景 × 时间戳子目录）
├── server.py         # FastAPI Web 服务入口
├── main.py           # CLI 入口
└── config.py         # 全局配置
```

### 输出格式

每次运行产生三种格式的报告，存入时间戳子目录：

| 格式 | 后缀 | 用途 |
|------|------|------|
| **TXT** | `.txt` | 文本条形图 + 表格，终端/CLI 友好 |
| **MD** | `.md` | Obsidian/知识库友好，可嵌入双向链接 |
| **HTML** | `.html` | 彩色看板 + Plotly 交互图表，浏览器友好 |

---

## 版本历史

| 版本 | 内容 |
|------|------|
| v2.0 | 5 场景 CLI 基础版 |
| v3.0 | FastAPI Web 版，Plotly + Mermaid 图表 |
| v3.1 | 9 语种扩展，按产品子目录输出 |
| v3.1.4 | 订单风控 8 维特征升级 + 时间平移 + 风险看板 |
| v3.1.5 | 选品分析独立页面，历史报告管理 |
| v3.1.6 | 运营复盘独立页面，名称统一 |
| v3.1.7 | 场景 1 交互修复（去复选框，高亮选中） |

> 详细版本记录见 `00-upgrade/` 目录，按规则使用 `[[prd-vX.X.X]]` 双向链接关联 PRD。

---

## 项目核心设计理念

### 1. 一人全栈打穿 5 个职责

这个项目的定位跨境电商运营系统的小样，它的目标是用一套代码同时证明：
- AI 内容生成能力（DeepSeek API 调用 + 多语种模板）
- 数据处理与建模能力（PyTorch 神经网络 + 特征工程）
- Web 全栈工程能力（FastAPI + Jinja2 + Plotly）
- 产品设计思维（场景定义 + 交互流程 + 输出格式）

### 2. 双轨制：CLI 开发，Web 展示

CLI 模式适合开发调试和批量测试，Web 模式适合演示汇报。两者共享 `scripts/` 目录下的业务逻辑层，做到"写一次，两端跑"。

### 3. 版本对齐

用户写 PRD（`00-prd/`），AI 写升级记录（`00-upgrade/`），版本号对齐。PRD 与升级记录之间用 `[[prd-v3.x.x]]` 双向链接，形成需求→验收的追溯链。

### 4. 时间戳 + 历史管理

每次运行产出存入 `report_YYYYMMDD_HHMMSS/` 子目录，不覆盖历史。24 小时内新报告标记 NEW，方便演示时展示"持续运营"的效果。
