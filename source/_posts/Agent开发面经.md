---
title: Agent开发面经
tags:
  - AI
  - Agent开发
  - 面经
categories:
  - 面经
keywords:
  - 面经
description: Agent开发面经
abbrlink: 20001
date: 2026-06-22 22:43:58
updated: 2026-06-22 22:43:58
top_img: https://s2.loli.net/2024/05/27/ytcdAHzliRquNM2.png
comments:
cover: https://s2.loli.net/2024/05/27/6wWObXhdZL13pqo.png
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---


## 自我介绍

面试官您好，我叫陈温鹏，南京理工大学软件工程硕士，目前在阿里巴巴菜鸟集团担任 全栈开发工程师，这次应聘的是 **Agent 开发岗位**。

在菜鸟期间，我主要负责 核心人事系统与薪酬系统的迭代开发和稳定性保障，覆盖员工入离异、合同管理、审批流、社保等核心模块，日均百万级调用下保持大促零故障。同时深度参与了两个 AI Native 项目从 0 到 1 的落地：一是 HR 智能助理 Agent，；二是薪酬 AI 质检平台。

在 AI 工程化方面，我拥抱并适应 AI 编程范式，AI Coding 采纳率稳定在 90% 以上，并实现个人需求吞吐量翻倍。
除此之外，我熟练掌握 Java 基础、并发编程、以及DDD分层架构，熟练在日常开发中使用 Vibe Coding、SDD等 AI 编程范式，熟练使用MCP、Skill用于排查日常问题；另外，我对Agent开发相关知识也有一定了解。

最后感谢贵公司给我这次面试机会，我也十分希望能加入团队，在新的业务场景中学习，与团队共同成长！

## 工作经历

公司：阿里巴巴-菜鸟集团

时间：202506-至今

- 负责人事核心域（员工入离异、合同管理、审批流、组织人事主数据等）多个核心模块的迭代开发与稳定性维护，通过告警治理、热点接口缓存改造、链路压测与降级预案完善，保障核心人事链路在日均百万级调用下稳定运行，大促期间零故障。
- 负责薪酬域核心模块社保系统的稳定性维护与日常迭代，通过ISO合规性改造，支持海外准时发薪；同时参与薪酬AI质检平台一期模块的需求落地，通过集成AI自然语言解析、六大质检环节（提报/计算/调账/报表/薪资方案/其他）全覆盖、三大规则类型（条件逻辑校验/重复数据检测/一致性校验）体系化，大幅降低薪酬质检投入。
- 参与集团采购合规风控平台跨多仓改造，承担供应商围串标治理专项的标前布控开发
- 深度参与 CPO 域 AI Native 建设，参与 HR 智能助理 Agent 与薪酬 AI 质检平台两个核心场景从 0 到 1 落地；同时主动拥抱 AI 编程范式，将Vibe Coding/SDD深度融入日常研发流程，通过 AI 相关能力实现个人需求吞吐量翻倍，AI Coding 采纳率稳定维持在 90% 以上。
- 积极探索 AI 领域前沿技术并推动团队工程化沉淀，团队内最先部署 OpenClaw 并接入钉钉、沉淀为团队共享文档；2700 行代码复刻业界开源 OpenClaw 框架，覆盖 Agent Loop、Heartbeat、Workspace 契约文件、Skills 渐进式披露、Context Compaction、Multi-Agent Spawn 沙箱等核心原理；持续跟踪 AI 前沿动态（开源 Agent 框架、主流大模型版本迭代、业界落地案例等），在团队内定期输出技术分享与最佳实践沉淀。

### 1. 人事核心域稳定性保障
负责人事核心域多个核心模块的迭代开发与稳定性维护，覆盖员工入职（cn-hr-entry-server）、离职（cn-hr-dimission）、异动（cn-hr-transfer）、合同管理（cn-hr-contract-server）、审批流（cn-hrapprove）、组织人事主数据（cn-masterdata/cn-masterdata2）等系统，各系统均采用 DDD 分层架构（domain/application/infrastructure/scenario/workbench）。

**告警治理**：对核心人事链路进行告警降噪与分级治理，梳理 Sunfire/GOC 告警规则，将无效告警（如定时任务正常波动、非核心依赖超时等）从 P1/P2 降级或屏蔽，保留真正影响用户体验的核心告警；建立告警响应 SOP，明确各类告警的处理时效与升级机制，告警有效率从约 40% 提升至 85% 以上。

**热点接口缓存改造**：针对高频查询接口（如员工基本信息、组织架构、汇报线等）进行多级缓存改造。基于 TairCache + MultiCacheManager 实现多级缓存路由，支持按缓存名称前缀自动路由到不同 CacheManager（Tair 远程缓存 / Guava 本地缓存），缓存 Key 支持应用隔离（APP_NAME 前缀）和 MD5 摘要防超长；对主数据查询接口增加本地 LRU 缓存 + Tair 二级缓存双层防护，热点接口 RT 从平均 50ms 降至 5ms 以内，DB QPS 下降约 70%。

**链路压测与降级预案**：参与核心人事链路的全链路压测，覆盖入职办理、离职交接、合同签署等高频场景，通过影子库/影子表隔离压测流量；完善降级预案体系，基于 Diamond 配置中心实现开关化降级（如非核心通知异步化、审批流超时自动跳过非关键节点、主数据查询降级到缓存兜底等），大促期间核心人事链路零故障、零降级触发。

### 2. 薪酬域社保系统与AI质检平台
负责薪酬域核心模块社保系统（workspace-social-security / cn-insurance）的稳定性维护与日常迭代，同时深度参与薪酬 AI 质检平台（workspace-ai-check）一期模块的需求落地。

**社保系统 ISO 合规性改造**：针对海外发薪场景进行 ISO 合规性改造，包括社保基数计算逻辑适配多国法规、发薪日历与时区处理（支持 UTC+8 / UTC+9 / UTC-5 等多时区）、跨境数据传输脱敏等；通过 Diamond 配置中心实现各国合规规则的动态下发，新增国家/地区无需发版即可上线，保障海外员工准时发薪。

**薪酬 AI 质检平台**：集成 AI 自然语言解析能力，将业务人员用自然语言描述的质检规则通过 Self-Refine 自洽循环（LLM-as-Generator × Groovy 编译器-as-Verifier，最多 3 轮）转换为可执行的 Groovy DSL 脚本；覆盖六大质检环节（数据提报 / 薪资计算 / 调账 / 报表 / 薪资方案 / 其他）和三大规则类型（条件逻辑校验 / 重复数据检测 / 一致性校验）；异步质检执行引擎支持多租户隔离（同租户分布式锁限并发）、容灾框架跨节点 Failover、Tair-DB 双层进度条；一期上线后月人工抽检工时下降 ≥50%，质检通过率 ≥95%，规则复用率 ≥60%。

### 3. 采购合规风控平台跨多仓改造
参与集团采购合规风控平台（workspace-purchase-risk）跨多仓改造，该平台包含 cn-cplm（采购全生命周期管理）、cn-purchase（采购执行）、cnei-cn-rc-kernel（风控内核）三个独立仓库，采用 DDD 分层架构 + Facade 接口隔离。

**跨多仓协同**：通过 HSF/Dubbo Facade 接口定义跨仓契约，风控内核（rc-kernel）提供标准化的风险识别与处置能力，采购执行（cn-purchase）和采购全生命周期（cn-cplm）通过 Facade 调用风控服务，实现风控能力与业务流程解耦；跨仓数据同步通过消息队列异步解耦，避免强依赖导致的级联故障。

**供应商围串标治理专项——标前布控**：承担标前布控模块开发，在招标/询价/竞价等采购流程发起前，自动触发风控规则引擎进行供应商围串标风险预判。核心实现在 `AbstractBusinessRuleImpl` 抽象类中，通过模板方法定义"规则加载 → 数据采集 → 风险判定 → 结果处置"标准流程，子类实现具体业务场景（公开招标 / 邀请招标 / 询价 / 竞价等）的差异化逻辑；风控规则通过 Diamond 配置中心动态下发，支持分钟级热更；风险结果分为"拦截 / 预警 / 放行"三级，拦截类风险直接阻断采购流程，预警类风险推送审批人人工复核。

### 4. CPO域AI Native建设与AI编程范式
深度参与 CPO 域 AI Native 建设，主导 HR 智能助理 Agent（hcm-ai / cn-work）与薪酬 AI 质检平台两个核心场景从 0 到 1 落地。

**HR 智能助理 Agent**：负责多源 RAG 系统（4 套异构知识源并发召回 + 三层线程池隔离 + 3s 熔断降级 + 两段 Rerank）、两段式路由引擎（规则前置拦截 + LLM Function Routing）、MCP Server 标准化开放、Dubbo Triple 流式推送等核心模块设计与开发，首字延迟降低约 60%，日均对话数万次。

**AI 编程范式落地**：主动拥抱 Vibe Coding / SDD（Spec-Driven Development）等 AI 编程范式，将其深度融入日常研发流程：需求分析阶段用 AI 辅助拆解技术方案与接口设计，编码阶段用 AI 生成 boilerplate 代码与单元测试，Code Review 阶段用 AI 辅助审查代码质量与安全漏洞；个人需求吞吐量实现翻倍，AI Coding 采纳率稳定维持在 90% 以上（基于 Aone Copilot 后台统计数据）。

### 5. AI前沿技术探索与团队工程化沉淀
积极探索 AI 领域前沿技术并推动团队工程化沉淀，在团队内建立 AI 技术影响力。

**OpenClaw 框架复刻与落地**：团队内最先部署 OpenClaw 并接入钉钉，沉淀为团队共享文档供全员使用；2700 行代码复刻业界开源 OpenClaw 框架（MiniClaw / MyMiniClaw 项目），覆盖 Agent Loop（ReAct 循环 + Tool Use）、Heartbeat（心跳保活 + 会话续期）、Workspace 契约文件（.claw/ 目录规范）、Skills 渐进式披露（按需加载技能描述减少 Token 消耗）、Context Compaction（上下文压缩 + 摘要保留关键信息）、Multi-Agent Spawn 沙箱（子 Agent 隔离执行 + 结果汇总）等核心原理，加深对 Agent 架构的理解。

**技术分享与最佳实践**：持续跟踪 AI 前沿动态（开源 Agent 框架演进、主流大模型版本迭代、业界 AI 落地案例等），在团队内定期输出技术分享（如 MCP 协议实践、Prompt Engineering 技巧、RAG 调优经验等），推动团队 AI 工程化能力建设；参与 Harness Engineering 理念在团队的落地实践，将 AI 能力从"个人提效"升级为"团队标准化生产力"。



## x小蜜-企业级HR智能助理Agent

项目介绍：面向集团内部数万员工的HR领域Agent，以自然语言对话替代传统表单与人工咨询，覆盖知识问答、智能请假、证明开具、人才档案、智能问数等高频场景，日均对话数万次，显著降低HR工作量、提升用户服务体验。

技术栈：PandoraBoot、RAG、Function Calling、MCP、RPA、Prompting Engineering

- 多源 RAG 系统：线程池隔离 + CompletableFuture编排并发召回 4 套异构知识源（政策库/知识平台/协同文档/草稿态），单源超时 3s 熔断降级，Rerank重排后取Top-K注入上下文，保障召回相关性与主链路可用性。
- 两段式路由引擎："规则前置拦截(本地策略模式匹配，零 LLM 调用) + LLM Function Routing" 两段式路由，保障LLM实时可用性；基于策略模式抽象 ToolExecutor，支撑 10+ 业务工具可插拔注册，新增技能零侵入主流程。
- RPA + 定时任务双链路知识同步：主链路通过 RPA 自动抓取协同文档变更增量同步至百炼向量库，Java 侧 SchedulerX 定时任务处理 RPA 无法覆盖的 FAQ 同步与文档删除场景，保障知识库实时性与完整性。
- 端到端流式体验：Dubbo Triple StreamObserver 实现服务端流式推送，基于 ReplayProcessor + 百炼智能体流式 API 做背压控制与中间态兜底（1.5s 无响应自动推送加载提示），首字延迟下降约 60%；智能体 AppId 不可用时自动降级到工作流 AppId，保障流式链路高可用。
- MCP Server 标准化对外开放：基于 MCP 协议暴露员工信息查询、知识召回、相似问匹配等标准 Tool，支持外部 Agent 通过统一协议编排调用，降低跨系统集成成本。
- Prompt 工程化与质量闭环：Prompt 与 Tool Schema 外置到配置中心，支持分钟级热更无需发版；离线 LLM-as-a-Judge 评估任务对答案打分回流，驱动 Prompt 与召回策略持续迭代。



### 1. 多源 RAG 系统
多源 RAG 系统：线程池隔离 + CompletableFuture编排并发召回 4 套异构知识源（政策库/知识平台/协同文档/草稿态），单源超时 3s 熔断降级，Rerank重排后取Top-K注入上下文，保障召回相关性与主链路可用性。

4 套知识源各有独立的 `AbstractKnowledgeRetrievalStrategy` 子类，通过模板方法定义标准流程（构建过滤标签 → 调用百炼 retrieve API → 文档鉴权），子类只需实现差异化逻辑。并发架构采用三层线程池隔离：外层 `RAG_STRATEGY_POOL`（core=30, max=50）驱动 4 个 Strategy 并发；内层 `RAG_RETRIEVE_POOL`（core=100, max=150）处理每个 Strategy 内多 query × 多知识库的百炼 API 调用；鉴权层 `KNOWLEDGE_AUTH_POOL`（core=200, max=300）处理每个知识切片的文档权限校验。三层必须分开，否则外层任务占满线程后内层任务排队，形成线程饥饿死锁。CompletableFuture 并发有一个关键陷阱：必须先 `.collect(Collectors.toList())` 收集所有 Future，再 `.stream().flatMap(f -> f.join())` 汇总结果——如果直接链式调用，由于 Stream 惰性求值会退化为串行。单源超时通过 `CompletableFuture.allOf(futures).get(3, TimeUnit.SECONDS)` 实现，超时后只收集已完成的鉴权结果，未完成的切片降级为无权限。Rerank 分两段：百炼内部先用 `qwen3-rerank-hybrid` 模型从 Top-100 收敛到 Top-20，应用层再用 `qwen3-rerank` 模型对文档名做 rerank，按 `原召回分 × 0.7 + 文档名 rerank 分 × 0.3` 加权融合后取 Top-K（由 Diamond 配置控制，默认 20）注入 Prompt。


- RAG（Retrieval Augmented Generation）：先从外部知识库**检索**相关信息，再把检索到的内容塞进 Prompt，让模型基于**真实数据**生成回答，而非仅凭记忆"编造"。 **为什么需要 RAG？** 模型参数知识有截止日期，且无法包含企业私有数据。RAG 让模型"查资料"而非"凭记忆"，大幅减少幻觉，是当前 Agent 开发中最常用的知识增强手段。
    ```
    【离线索引阶段】
     文档 ──→ 分块(Chunking) ──→ Embedding ──→ 存入向量数据库
    【在线查询阶段】
      Query ──→ Embedding ──→ 向量检索 Top-K ──→ 拼入 Prompt ──→ LLM 生成答案
    ```

- 线程池：线程池就是管理一系列线程的资源池。当有任务要处理时，直接从线程池中获取线程来处理，处理完之后线程并不会立即被销毁，而是等待下一个任务。
- 死锁：死锁是多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。
- CompletableFuture：`CompletableFuture` 类是 Java 8 中引入的一个增强版 `Future`，它不仅可以代表一个异步计算的结果，还提供了强大的方法链和回调机制，用于构建复杂的异步逻辑和并行操作。与 `Future` 相比，`CompletableFuture` 更加灵活和强大，支持函数式编程、异步任务编排组合等能力。


#### RAG具体是如何切片的？
菜小蜜的文档切片**不在应用层手动实现**，而是通过百炼平台 `addFile` API 上传文档时指定 `DASHSCOPE_DOCMIND` 解析器，由百炼自动完成解析、切片和向量化。应用层只需关注三件事：**①上传前的文档准备（命名规范、标签）②检索时的过滤条件（tags）③召回后的鉴权和重排序**。

- **文档解析**：百炼根据文档类型自动选择解析方式——PDF 用 OCR+版面分析，Word 解析 XML 结构保留标题层级，Markdown 按语法树以标题为切分边界，钉钉文档通过 API 导出结构化内容，纯文本按段落分隔符切分。
- **智能切片**：采用语义感知+长度约束双重策略。默认切片大小约 500-800 tokens，相邻切片有 10%-20% 重叠窗口保证上下文连贯；切分边界优先级为：标题边界（H1>H2>H3）> 段落边界 > 句子边界 > 固定长度兜底；表格整表或按行拆分，代码块保持完整不内部切分，图片提取 alt 文本或 OCR 文字作为切片内容。
- **向量化与元数据**：每个切片独立用 `text-embedding-v3` 模型向量化（1536维），并附加 docId、docName、sliceId、tags、categoryId、chunkIndex 等元数据，用于检索过滤和鉴权。
- **与菜小蜜的对接点**：`BailianClient.addFile()` 指定 DASHSCOPE_DOCMIND 解析器 → `retrieveIndex()` 设置 tags 过滤 + `EnableReranking=true`（qwen3-rerank-hybrid 精排）→ 先粗召回 Top-100 再精排取 Top-20 → 从切片元数据的 docName 中提取 docKey 做文档级鉴权。

#### AbstractKnowledgeRetrievalStrategy是策略模式吗？
是的，这是**策略模式 + 模板方法模式**的组合。AbstractKnowledgeRetrievalStrategy 是抽象策略基类，定义了 4 个抽象方法（buildFilterTags / retrieve / authSlice / getStrategyName），3 个子类分别实现不同知识库的召回策略：HrPolicyRetrievalStrategy（政策平台）、DingDocRetrievalStrategy（钉文档）、KnowledgePlatformRetrievalStrategy（知识平台）。调用方不关心具体用哪个策略，只通过父类引用统一调用——这是典型的策略模式。

#### 这个标准流程是SOP吗？
更准确地说是**模板方法模式（Template Method Pattern）**。父类的 ragRetrieveKnowledgeSlice() 方法定义了固定的三步流程：①调用子类的 buildFilterTags() 获取标签 → ②调用子类的 retrieve() 召回知识 → ③调用父类统一的 authSliceList() 做文档鉴权。流程骨架由父类控制，可变步骤由子类实现——这就是模板方法模式。SOP（标准操作流程）是业务层面的概念，模板方法模式是它在代码层面的实现手段。

#### 知识切片的文档权限校验是什么意思？意思是文档切片后，召回的时候还需要根据谁有权限查看权限，来进行知识切片的权限校验吗？如果没文档权限，则没有这个知识切片的权限？是这样吗？
是的，你的理解基本正确。RAG 召回的知识切片（chunk）来自不同的原始文档，而不同文档有不同的访问权限。鉴权层做的事情是：对每个召回的知识切片，根据它所属的原始文档，校验当前提问用户是否有权查看该文档。代码中 authSlice(workNo, empNo, docName) 就是传入工号和文档名进行权限校验。不同策略子类的鉴权方式不同：政策平台通过 DocumentOpenService 校验、钉文档通过钉钉 API 校验、知识平台通过 KnowledgeAuthenticateFacade 校验。如果用户对某文档没有权限，该文档下的所有知识切片都会被过滤掉，不会注入到 Prompt 中——防止通过 AI 助理绕过文档权限体系泄露敏感信息。

#### 三层线程池是否存在前后依赖关系？否则无法形成死锁
是的，三层线程池存在严格的**嵌套依赖关系**，这正是必须分开的原因。调用链是：RAG_STRATEGY_POOL（外层）并发执行 3 个策略的 ragRetrieveKnowledgeSlice() → 每个策略内部用 RAG_RETRIEVE_POOL（内层）并发调用多个百炼 retrieve API → 召回结果返回后用 KNOWLEDGE_AUTH_POOL（最内层）并发做文档鉴权。如果三层共用一个线程池，外层 3 个策略任务占据了所有线程，内层的 retrieve 调用和鉴权调用就无法获得线程执行，但外层任务又在等内层结果——外层持有线程等内层、内层等线程被外层占着，形成经典的**线程饥饿死锁**。代码注释也明确写了"不能与 RAG_RETRIEVE_POOL 共用，否则会有线程饥饿死锁风险"。

#### 3秒熔断只是针对切片鉴权吗？还是针对整个召回过程？
根据代码，3秒熔断是**针对切片鉴权阶段**的。在 AbstractKnowledgeRetrievalStrategy 的 collectAuthResults() 方法中，对所有鉴权 Future 设置 CompletableFuture.allOf(futures).get(3, TimeUnit.SECONDS) 总超时。超时后只收集已完成的鉴权结果，未完成的切片被视为无权限直接丢弃。之所以只针对鉴权阶段设超时，是因为鉴权需要调用外部权限服务（如钉钉API、DocumentOpenService），这些外部调用延迟不可控，是整个 RAG 链路中最容易成为瓶颈的环节。召回阶段（retrieve）的超时由百炼 SDK 自身的 HTTP 超时控制。

#### 3s熔断过程中，如果鉴权超时，是否可能会导致召回过程丢失了包含关键信息的切片，现有实现中有考虑这种情况吗？如果没有，你觉得后面可以做什么优化？
**是的，3s熔断确实可能导致丢失关键切片。** 现有代码 `collectAuthResults()` 中，3秒超时后只收集已完成的鉴权结果，未完成的切片直接被视为"无权限"被丢弃——但实际上这些切片可能是有权限的，只是鉴权服务响应慢了。
现有实现**没有特别的补偿机制**，采用的是"可用性优先"策略：宁可丢失部分切片，也要保证主链路不卡死。这在大多数场景下是合理的——Rerank 从 Top-100 收敛到 Top-K（默认20），即使丢失几条鉴权超时的切片，剩余的切片通常仍能覆盖用户问题。
**后续可做的优化方向**：
① **二次异步补鉴权**——超时被降级为"无权限"的切片，后台异步补做鉴权，结果写入缓存。下次同用户提问时，缓存中已有该文档的鉴权结果，避免重复超时。
② **分档超时**——对不同文档类型设不同超时阈值（政策平台文档通常鉴权快，可设2s；钉文档鉴权链路更长，可设4s），而非一刀切3s。
③ **部分放行策略**——鉴权超时的切片先标记为"未鉴权"，如果 Rerank 后 Top-K 切片数量不足（如只有3条），则把"未鉴权"切片临时放行注入 Prompt，同时在回答末尾标注"以下内容未经权限校验，仅供参考"。


### 两段式路由引擎
两段式路由引擎："规则前置拦截(本地策略模式匹配，零 LLM 调用) + LLM Function Routing" 两段式路由，保障LLM实时可用性；基于策略模式抽象 ToolExecutor，支撑 10+ 业务工具可插拔注册，新增技能零侵入主流程。

第一段规则前置拦截按固定优先级执行：先通过 `SpecialCommandTool` 匹配特殊指令（如"开启新话题"），再通过 `SimilarQuestionTool` 在 FAQ 库中做精确匹配，命中即返回、不走 LLM，保证确定性场景的低延迟（<50ms）。前置拦截器返回 null 表示"不处理"，流程继续到第二段 LLM 路由。第二段 `aiRouting()` 并发执行 4 个 LLM 调用（实际含嵌套最多 6 个）：技能路由（将工具列表序列化为 Prompt，用 qwen-flash 快速模型识别意图）、语言识别、问题重写（又并发 3 路：上下文重写 / 关键词同义词扩展 / 用户身份信息重写，多角度扩展提高 RAG 召回率）、知识库标签识别。这 4 个调用无数据依赖，并发执行将路由延迟从串行累加的 ~2s 压缩到 ~500ms。ToolExecutor 注册通过 Spring 自动收集：`ToolExecutorRegistry` 构造器注入 `List<ToolExecutor>`，过滤掉前置拦截器后按 operationId 建映射表。新增技能只需实现接口 + 加 `@Component` + 在 Diamond 配置工具描述，对主流程零侵入。路由失败时自动降级到知识问答（最安全的兜底技能）。每个 Tool 还支持圈人配置（`crowdRuleId`），通过人群校验决定该工具是否出现在路由 Prompt 中。


- 常用设计模式：[常用设计模式](https://www.cnblogs.com/cwp0/p/20729842)
- 策略模式：将一组可互换的算法封装为独立类，使它们可以相互替换，算法的变化不影响使用它的客户端。核心是"面向接口编程 + 组合优于继承"。适用于支付方式选择、排序策略切换、RAG 检索策略等场景。


#### 怎么匹配特殊指令的？
通过**精确字符串匹配**实现。代码中直接用 equals 判断用户输入是否为固定关键词，如 `"开启新话题".equals(userInput) || "new chat".equals(userInput)`。匹配成功则调用 cxmSessionHelper.refreshSessionInfo() 刷新会话并发送提示卡片，返回 ExecutorResult.success("已开启新话题")；不匹配则返回 null，表示"本拦截器不处理"，流程继续往下走。这种设计非常轻量，无需 LLM 调用，延迟几乎为零。

#### 如何匹配相似问题的？是语义检索还是精确匹配？
调用 `SimilarQuestionService.getSimilarQuestion(userInput, empNo)` 进行匹配。根据代码实现，当前**并非通过百炼语义检索**，而是**数据库精确匹配**：`similarQuestionMapper.queryByQuestion(removeSymbol(question))` 直接用用户输入文本在 question 表中做精确查询，匹配到记录后再通过 crowdRuleId 做圈人过滤，取 gmtModified 最新的一条返回。命中则直接返回预设答案（SimilarQuestionVO），未命中返回 null 继续走 LLM 路由。
这样做的好处是：高频标准问题（如"怎么查工资条""年假还剩几天"）绕过 LLM 直接给出确定性答案，延迟 <50ms 且准确率 100%。代价是用户输入必须与预设问题文本一致才能命中。如果未来需要升级为语义匹配，可以考虑：① 在百炼上为 FAQ 库单独建知识库索引，调用 retrieve API 做语义检索取 Top-1；② 引入向量数据库（如 Milvus）对 FAQ 问题做 embedding 存储，用余弦相似度匹配。

#### 为什么相似问题不用语义检索？
- **性能优先**：相似问题是前置拦截器（`isPreInterceptor()=true`），在 AI 路由之前执行。精确匹配的耗时是 O(n) 字符串比较，毫秒级完成；而语义检索需要调用向量数据库，至少几百毫秒延迟
- **准确性保障**：相似问题库是人工标注的标准 Q&A，要求 100% 准确。语义检索可能召回"看起来像但实际不对"的问题，反而引入错误答案
- **数据规模小**：相似问题库通常只有几百到几千条，全量内存遍历完全可行，无需向量索引
- **兜底设计**：如果相似问题未命中，才会进入后续的 RAG 语义检索流程。两者是互补关系，不是替代关系

#### 4个LLM调用详细说下
aiRouting() 方法通过 CompletableFuture.supplyAsync() 并发启动 4 个独立的 LLM 调用：
- ① **技能路由**（aiRoutingForTool）：将当前用户可用的工具列表（经过圈人过滤后）序列化为 Prompt，用 qwen-flash 快速模型识别用户意图，返回匹配的 operationId。
- ② **语言识别**（cxmLangDetector.detect）：识别用户输入的语言（中文/英文等），后续用于多语言回答。
- ③ **问题重写**（aiRoutingForUserInputRewrite）：这一路内部又并发 3 个子 LLM 调用——上下文重写（结合历史对话补全指代和省略）、关键词同义词扩展（扩展搜索关键词提高召回率）、用户身份信息重写（结合员工工号/部门等信息补充问题上下文）。所以实际最多 6 个 LLM 调用。
- ④ **知识库标签识别**（aiRoutingForKnowledgeBaseFilterCode）：判断问题所属的知识库分类标签，用于 RAG 召回时做知识库过滤，提高召回精准度。
- 这 4 个调用之间无数据依赖，并发执行后通过 CompletableFuture.allOf().join() 等待全部完成，将路由总延迟从串行 ~2s 压缩到 ~500ms（取决于最慢的那一路）。

#### 这里ToolExecutor的介绍与解释我觉得太过于专业了，我都看不太懂了？或者你需要介绍更详细一点
用大白话解释——ToolExecutor 就是一个**"技能插件"的统一接口**。每个业务技能（比如"查工资""查年假""查组织架构"）都是一个实现了 ToolExecutor 接口的 Java 类。这个接口定义了几个关键方法：
- `getOperationId()`：返回技能的唯一标识（如 "queryPayslip"），相当于技能的"身份证号"
- `execute()`：技能被选中后执行的核心逻辑
- `isPreInterceptor()`：标记是否为前置拦截器（SpecialCommandTool / SimilarQuestionTool 返回 true）

ToolExecutorRegistry 是"技能注册中心"。Spring 启动时，会自动发现所有加了 @Service/@Component 注解的 ToolExecutor 实现类，收集成一个列表注入到 ToolExecutorRegistry 的构造器中。Registry 把前置拦截器过滤掉，剩下的按 operationId 放进一个 Map（字典）。当 AI 路由说"用户想查工资，对应 operationId=queryPayslip"时，Registry 就能从 Map 中快速找到对应的技能类去执行。

#### 具体现在有什么 Tool？
菜小蜜的 Tool 分为两类，均通过实现 `ToolExecutor` 接口 + `@Component` 注解自动注册到 `ToolExecutorRegistry`，新增技能无需改主流程：

- **前置拦截器**（零 LLM 调用，优先执行）：`SpecialCommandTool`（特殊指令如"开启新话题"）、`OnlineSupportTool`（转人工客服）、`SimilarQuestionTool`（相似问题精确匹配）
- **AI 路由技能**（LLM Function Calling 调度）：`CxmKnowledgeQATool`（RAG 知识问答兜底）、`AILeaveTool` / `AILeaveRecordTool` / `AILeaveQuotaTool`（请假三件套：发起/记录/额度）、`AICInsuranceTool`（商保咨询）、`AiCertificateTool2`（证明办理）、`CxmDataQueryTool`（HR 数据查询）、`TalentProfileTool`（人才画像）、`CreateCase`（创建工单）、`CallHotline`（拨打热线）、`AITransferCheck` / `AITransferSubmit`（转岗资格检查与申请提交）

所有技能的可见性还可通过 Diamond 配置中的 `crowdRuleId` 按人群动态控制。




#### 详细介绍新增Tool的流程？
新增一个技能的完整流程分为代码侧和配置侧两步：
- **代码侧**：① 创建一个新的 Java 类，加上 @Service 或 @Component 注解；② 实现 ToolExecutor 接口；③ getOperationId() 返回唯一标识（如 "queryAttendance"）；④ execute() 方法中编写业务逻辑（如调用考勤服务查询数据、组装回答）。Spring 容器启动时会自动扫描到这个类，ToolExecutorRegistry 构造器自动将它注册到映射表中。
- **配置侧**：在 Diamond 配置中心的 AiAssistantConfigData.tools 列表中添加一条工具描述，包含 operationId（与代码中一致）、title（工具名称）、description（功能描述，供 AI 路由判断时参考）、exampleQueries（示例问题，帮助 AI 识别意图）、parameters（参数定义）、crowdRuleId（可选，圈人配置）。Diamond 配置推送后立即生效，新技能就出现在 AI 路由的工具列表中了。
- 全程**不需要修改 CxmAiEngine 或任何主流程代码**——这就是"零侵入"的含义。

#### 如何圈人的？
每个 Tool 在 Diamond 配置中可以设置 crowdRuleId（人群规则 ID）。AI 路由构建工具列表时，通过 shouldIncludeTool() 方法逐个校验：如果 crowdRuleId 为空，则所有用户可见；如果配置了 crowdRuleId，则调用 CrowdWrapper.validateMatched(empNo, crowdRuleId) 向 HCM 人群服务发起校验，判断当前用户的工号是否命中该人群规则。只有校验通过的工具才会出现在路由 Prompt 的工具列表中，AI 才有机会选中它。这样就实现了**按人群灰度开放技能**——比如新技能可以先配置只对某个 BU 的员工可见，验证稳定后再去掉 crowdRuleId 全量开放。


### RPA + 定时任务双链路知识同步
RPA + 定时任务双链路知识同步：主链路通过 RPA 自动抓取协同文档变更增量同步至百炼向量库，Java 侧 SchedulerX 定时任务处理 RPA 无法覆盖的 FAQ 同步与文档删除场景，保障知识库实时性与完整性。

双链路设计的原因是能力互补：RPA 擅长处理需要渲染/下载的普通文档（alidoc / pdf），但无法处理钉钉 FAQ 格式（able 表格）的导出和文档删除操作。Java 侧 `DingDocSyncProcessor`（SchedulerX 定时任务）专门处理这两个场景——`dealDeleteTask()` 循环取出删除任务调用百炼 API 删除文件，`dealFaqTask()` 将钉钉 FAQ 文档导出为临时文件后上传百炼。知识库变更检测由独立的 `DingDocScanProcessor` 扫描任务完成：遍历钉钉文档空间目录树生成快照，与百炼现有文件列表对比，生成变更日志（ADD / MODIFY / DELETE），检测维度包括文件名变化、修改时间 tag 变化、able 文件超过 30 天需强制刷新（因内部图片 URL 有过期风险）。扫描与同步解耦——扫描只生成变更日志，同步任务消费日志。百炼 API 有频率限制，通过 Guava RateLimiter 控制（查询 5 QPS、删除 10 QPS、上传 10 QPS）。上传时从文档路径提取圈人标签，注入为百炼文件 tag，实现基于标签的文档权限过滤。


- RPA/Robot Process Automation：机器人流程自动化 是指用软件机器人模拟人类操作界面（点击、填表、复制粘贴等）来自动执行繁琐、重复性业务流程，无需改造底层系统，提高效率、减少错误。


#### 为什么使用 RPA？
核心原因：**钉钉开放 API 无法导出普通文档内容**，RPA 通过模拟浏览器操作弥补了这一能力缺口。具体分工如下：

- **Java API 能做的**：列出文档目录/元信息、变更检测与增量对比、FAQ 文档导出（`exportFAQ()`）、百炼文件删除同步、任务调度与鉴权
- **Java API 做不了的**：普通钉钉文档（doc/sheet/pdf）的内容抓取——钉钉开放 API 不提供直接的"导出为文件"接口
- **RPA 的价值**：模拟用户"打开→渲染→下载"的浏览器操作，恰好覆盖了 API 的能力盲区

因此采用 **"各取所长"的双链路设计**：RPA 负责普通文档内容抓取，Java 侧 `DingDocSyncProcessor` 负责变更检测、FAQ 导出、删除同步和状态管理，两者通过 `ToolController` 的 HTTP 接口衔接，形成完整的知识同步闭环。


#### 为什么RPA无法处理导出和文档删除操作？
RPA（机器人流程自动化）本质是模拟人在浏览器/客户端上的点击操作来完成任务。它擅长的是有明确 UI 交互流程的操作（如打开文档页面→点击下载→保存文件）。但有两类操作 RPA 做不了：① **FAQ 导出**——钉钉 FAQ 文档是 able 表格格式，它没有像普通文档那样的"下载"按钮，需要通过钉钉开放平台 API 编程导出，RPA 无法模拟 API 调用；② **文档删除**——删除百炼向量库中的文件需要调用百炼 API（bailianClient.deleteFile()），这是后端 API 操作而非前端 UI 操作，RPA 无法执行。所以这两个场景由 Java 定时任务（DingDocSyncProcessor）通过编程方式直接调用 API 处理。

#### dealDeleteTask() 这个定时任务的具体流程？
dealDeleteTask() 的具体流程：① 从数据库中查询状态为"待删除"的任务记录 → ② 循环取出每条删除任务 → ③ 调用 bailianClient.deleteFile(fileId) 通过百炼 API 删除向量库中的文件 → ④ 删除成功后更新数据库任务状态为 SUCCESS → ⑤ 如果删除失败则记录错误日志，任务状态标记为 FAILED，等待下次定时任务重试。整个过程受 Guava RateLimiter 控制（删除操作 10 QPS），防止瞬间大量删除触发百炼 API 限流。

#### dealFaqTask() 这个定时任务的具体流程？
dealFaqTask() 的具体流程：① 从数据库查询状态为"待同步"的 FAQ 任务记录 → ② 循环取出每条 FAQ 任务 → ③ 调用 aliDingClientHelper.exportFAQ() 通过钉钉开放平台 API 将 FAQ 文档（able 表格格式）导出为临时文件 → ④ 调用 uploadDingDoc() 将临时文件上传到百炼向量库，上传时从文档路径提取圈人标签注入为百炼文件 tag → ⑤ 上传成功后更新任务状态为 SUCCESS → ⑥ 清理本地临时文件。上传操作同样受 RateLimiter 控制（上传 10 QPS）。

#### Guava是啥？
Guava 是 Google 开源的 Java 核心工具库，提供了集合、缓存、并发、字符串处理等大量实用工具。RateLimiter 是 Guava 中的**令牌桶限流器**——它以固定速率往"桶"里放令牌，每次调用前先 acquire() 获取一个令牌，桶里没令牌就阻塞等待。代码中定义了三个 RateLimiter：`RateLimiter.create(5.0)` 表示每秒放 5 个令牌（即查询限 5 QPS）、`RateLimiter.create(10.0)` 表示每秒放 10 个令牌（即删除/上传限 10 QPS）。这样即使程序并发很高，实际对百炼 API 的调用频率也不会超过这些阈值，避免触发百炼的服务端限流返回 429 错误。


### 端到端流式体验
端到端流式体验：Dubbo Triple StreamObserver 实现服务端流式推送，基于 ReplayProcessor + 百炼智能体流式 API 做背压控制与中间态兜底（1.5s 无响应自动推送加载提示），首字延迟下降约 60%；智能体 AppId 不可用时自动降级到工作流 AppId，保障流式链路高可用。

HSF 接口使用 Triple 协议（基于 HTTP/2 的 gRPC 兼容协议）暴露服务端流式接口，声明为 `void callStream(ChatRequest request, StreamObserver<ChatResponse> response)`。内部核心是 `ReplayProcessor`（RxJava 热流）作为桥梁：上游百炼流式 API 产生的每一帧通过 `Flowable` 推入 ReplayProcessor，下游通过 `processor.subscribe()` 订阅并逐帧转发到 `StreamObserver.onNext()`，每帧实时更新钉钉卡片，用户体验类似 ChatGPT 逐字出现。选择 `ReplayProcessor` 而非 `PublishProcessor` 是因为它能缓存所有历史帧（带 1 分钟时间窗口），晚加入的订阅者可以回放历史数据，避免丢帧。百炼 API 设置 `incrementalOutput=true` 增量输出，每帧只传输新增内容减少带宽。1.5s 加载提示通过 Spring `TaskScheduler` 延迟调度实现：检查 `finalResult`（AtomicReference）是否仍为空，是则推送灰色加载文案，百炼第一帧到达后直接覆盖。RAG 兜底策略：如果 RAG 答案包含"暂时没有相关内容"，通过 RxJava `concatWith(Flowable.defer(...))` 无缝拼接一次通识 LLM 问答流，调用方完全透明。AppId 降级逻辑：优先使用百炼智能体 AppId，未配置或不可用时降级到工作流 AppId。


#### 介绍下Dubbo Triple StreamObserver
Dubbo Triple 是 Dubbo 3.0 引入的新协议，基于 HTTP/2 实现，兼容 gRPC 协议。它最大的特点是支持**服务端流式推送（Server Streaming）**——传统 RPC 是请求-响应一对一，而 Triple 允许一次请求返回多次响应。StreamObserver<ChatResponse> 是流式响应的接口，它有三个方法：onNext(data) 推送一帧数据、onError(e) 推送错误、onCompleted() 标记流结束。在代码中，CxmOpenClientImpl 用 @HSFProvider(protocols="tri") 声明 Triple 协议，callStream() 方法接收请求后，将 CxmAiEngine 返回的 ReplayProcessor 热流订阅到 StreamObserver 上：每产生一帧 ChatResponse 就调用 response.onNext() 推给调用方，流结束时调用 response.onCompleted()。这样调用方就能像 ChatGPT 一样逐字接收 AI 回答，而不是等全部生成完才一次性返回。

#### 1.5s加载提示是什么文案：正在加载中吗？实现的具体原理是什么？
加载提示文案是"正在搜索知识..."（灰色样式）。实现原理是利用 Spring TaskScheduler 的延迟调度：在发起百炼 API 调用的同时，调用 taskScheduler.schedule(() -> { ... }, Instant.now().plusMillis(1500)) 注册一个 1.5 秒后执行的定时任务。这个任务执行时检查 finalResult（AtomicReference<String>）是否仍为空（即百炼还没返回任何内容），如果为空则通过 cxmRobotClient.updateCardDateStream() 推送灰色加载文案到钉钉卡片。当百炼第一帧到达时，流式更新会直接覆盖掉加载文案，用户看到的效果就是：1.5 秒内如果 AI 没响应就先看到"正在搜索知识..."，AI 开始输出后立即看到实际内容逐字出现。这种设计避免了用户在等待时看到空白页面的焦虑感。

#### 模拟下finalResult为空/不为空的情况，以及RAG兜底/不兜底的情况
四种场景模拟如下：
- **场景A：finalResult 不为空 + RAG 不兜底（正常流程）**
用户问"年假还剩几天"→ RAG 召回到相关政策切片 → 百炼流式 API 正常返回 → finalResult 被逐帧填充 → 1.5s 定时任务检查时 finalResult 已有值，不推送加载提示 → 用户直接看到逐字出现的答案"根据您的考勤记录，您的年假余额为..."
- **场景B：finalResult 为空（超时/失败）**
用户问"xxx"→ 百炼 API 调用卡住或失败 → 1.5s 后定时任务检查 finalResult 为空 → 推送"正在搜索知识..."加载文案 → 如果最终超时失败，走 answerWithFallback() 返回兜底内容
- **场景C：finalResult 不为空 + RAG 兜底**
用户问"今天天气怎么样"（知识库无相关内容）→ RAG 召回到的切片不相关 → 百炼正常返回但答案包含"暂时没有相关内容" → 系统检测到这个关键词 → 通过 RxJava concatWith(Flowable.defer(...)) 在当前流后面无缝拼接一次通识 LLM 问答流 → 用户看到的效果是先显示"暂时没有相关内容"然后自动接上通识回答（如"今天杭州天气晴，25°C..."）
- **场景D：finalResult 为空 + RAG 兜底**
百炼 API 调用失败 → 1.5s 推送加载文案 → 最终走 fallback → 直接用通识 LLM 回答用户问题，跳过 RAG

#### 具体的AppId降级逻辑是什么
代码中通过 determineAppId() 方法实现降级。优先读取 Diamond 配置中的 bailianAgentAppId（智能体 ID），如果该字段不为空且非空字符串则使用它；如果未配置或为空，则降级使用 bailianAppId（工作流 ID）。智能体 AppId 对应的是百炼平台上的"智能体应用"（功能更丰富，支持插件/工具调用），工作流 AppId 对应的是"工作流应用"（功能较基础，但更稳定）。这种设计保证了：正常情况下用功能更强的智能体，智能体不可用时自动退回到稳定的工作流，保障流式链路始终可用。


### MCP Server 标准化
MCP Server 标准化对外开放：基于 MCP 协议暴露员工信息查询、知识召回、相似问匹配等标准 Tool，支持外部 Agent 通过统一协议编排调用，降低跨系统集成成本。

使用 `@alibaba/mcp-lite` 框架，通过 `@Tool` 注解声明 3 个 MCP Tool：`getCxmEmployeeInfo`（获取用户身份信息）、`knowledgeRecall`（知识检索，指定三个知识源并发召回，返回鉴权后的原始知识切片列表）、`similarQuestion`（相似问题匹配）。`@McpContextAware` 注解自动注入 MCP 上下文，通过 `McpContext.getUser()` 获取调用者身份。设计上只暴露原子能力，不包含 LLM 总结步骤——外部 Agent（如集团悟空平台）有自己的 System Prompt 和回答策略，Tool 内部做总结会限制调用方的二次推理能力。`knowledgeRecall` 内部复用 `RecallServiceImpl` 和 RAG 策略链，避免重复建设。会话标识加 `wukong_` 前缀区分来源，便于后续的流量分析和问题排查。


- MCP(Model Context Protocol)：MCP 是 Anthropic 提出的**工具接入标准协议**，类似 AI 世界的 USB-C 接口——让任何工具以统一方式被任何 Agent 发现和调用。


#### @alibaba/mcp-lite，简要介绍下这个框架的原理，以及怎么能通过@Tool注解声明MCP Tool
MCP（Model Context Protocol）是 Anthropic 提出的一套标准协议，用于 LLM 与外部工具/数据源的标准化交互。@alibaba/mcp-lite 是阿里内部对 MCP 协议的轻量级 Java 实现框架。原理是：框架在 Spring 容器启动时扫描所有带 @Tool 注解的方法，自动将方法签名（方法名、参数列表、描述信息）转换为 MCP 协议定义的 Tool Schema（JSON 格式），注册到 MCP Server 中。当外部 Agent（如集团悟空平台）通过 MCP 协议发起 Tool 调用时，框架根据 tool name 路由到对应的 Java 方法，自动做参数反序列化、调用方法、序列化返回值。@McpContextAware 注解让框架自动注入 MCP 上下文对象，通过 McpContext.getUser() 可以拿到调用者身份信息。开发者只需在方法上加 @Tool(name="xxx", description="xxx") 注解，就完成了一个 MCP Tool 的声明，不需要手动编写协议层代码。


### Prompt 工程化与质量闭环
Prompt 工程化与质量闭环：Prompt 与 Tool Schema 外置到配置中心，支持分钟级热更无需发版；离线 LLM-as-a-Judge 评估任务对答案打分回流，驱动 Prompt 与召回策略持续迭代。

所有 Prompt 模板通过阿里 Diamond 配置中心管理，每个 Prompt 是一个独立的 `@DiamondListener` 类（如 `PromptToolRouting`、`PromptRagAnswerInstruction` 等，共 17 个独立 dataId），收到推送后更新 `volatile` 静态变量，调用方通过 `buildPrompt()` 做占位符替换获取最新模板。Tool Schema 同理，`AiAssistantConfigData.tools` 列表配置在 Diamond，包含 operationId、title、description、exampleQueries、parameters 和 crowdRuleId，热更新即可添加/修改/禁用工具。LLM-as-a-Judge 评估流程：SchedulerX 定时任务触发 `CxmAnswerScoreServiceImpl.evaluateAnswerScore()`，从云灵知识平台采集标准 Q&A 对作为评估基线；5 线程固定池并发处理——每条记录先用灰度环境的 AI 助理回答标准问题，再用 LLM（评分 Prompt 也外置在 Diamond）比对标准答案和 AI 助理答案给出分数（BigDecimal 存储）；下一轮只重新评估 4 小时前且准确率低于 0.85 的记录，持续改进低分项。低分记录按类目聚合，针对性调整该类目的 RAG 召回策略或 Prompt 模板。



#### 为什么需要多个Prompt模板
因为 AI 助理的不同环节需要**完全不同的指令和输出格式**，一个万能 Prompt 无法胜任。代码中有 17 个独立的 Prompt 模板，各司其职：
- **PromptToolRouting**：技能路由——告诉 LLM "这些是可用工具列表，判断用户问题属于哪个工具"，输出格式是 operationId
- **PromptQueryRewriteWithContext**：上下文重写——告诉 LLM "结合历史对话补全指代和省略"，输出格式是重写后的完整问题
- **PromptQueryRewriteWithKeywords**：关键词扩展——告诉 LLM "对问题做同义词扩展"，输出格式是扩展后的关键词列表
- **PromptQueryRewriteWithUserInfo**：用户信息重写——告诉 LLM "结合员工信息补充问题"
- **PromptKnowledgeFilterTag**：知识库标签识别——告诉 LLM "判断问题属于哪个知识库分类"
- **PromptRagAnswerInstruction**：RAG 回答指令——告诉 LLM "根据以下知识片段回答用户问题"
- **PromptLangDetect**：语言识别——告诉 LLM "识别用户输入的语言"
- **PromptEvaluateAnswerScore**：答案评分——告诉 LLM "对比标准答案和 AI 答案打分"
- 每个 Prompt 的 System Prompt、输出契约、Few-shot 示例都不同，拆分成独立模板才能各自独立迭代优化，互不干扰。

#### LLM-as-a-Judge这个评估具体的流程是什么？
完整流程如下：① SchedulerX 定时任务触发 CxmAnswerScoreProcessor → ② 调用 CxmAnswerScoreServiceImpl.evaluateAnswerScore() → ③ 从数据库查询需要评估的记录（未评估过的 + 4小时前且低于0.85分的） → ④ 用 5 线程固定池并发处理每条记录：先调用灰度环境的 AI 助理（caixiaomiGrayAssistantClient.talk()）用标准问题提问，获取 AI 的回答 → ⑤ 拼装评分 Prompt（PromptEvaluateAnswerScore.buildPrompt()），将标准答案和 AI 回答送入百炼 LLM 做对比评分 → ⑥ LLM 返回 0-1 之间的分数（BigDecimal 存储），写入 cxm_answer_score 表 → ⑦ 低分记录按知识类目聚合，反馈给运营人员针对性优化该类目的 Prompt 或召回策略。

#### 定时任务的触发频率？
定时任务的具体触发频率在 **SchedulerX 控制台**配置，代码中没有硬编码 cron 表达式。项目通过 auto-config.xml 配置了 SchedulerX 2.0 的 groupId（cn-work）和 appKey，CxmAnswerScoreProcessor 实现 MapJobProcessor 接口作为任务处理器。触发频率由运维在 SchedulerX 控制台设置（通常是每天定时跑一次或每隔几小时跑一次），代码侧只负责实现任务逻辑。

#### 线程为5的线程池吗？为什么要这样设置线程池？maxPoolSize、corePoolSize是怎么设置的？
是的，使用 `Executors.newFixedThreadPool(5)` 创建固定线程池。对于 FixedThreadPool，**corePoolSize = maxPoolSize = 5**，工作队列是无界的 LinkedBlockingQueue（容量 Integer.MAX_VALUE）。设置为 5 个线程的考虑：每个线程都要调用灰度环境的 AI 助理 + 再调一次百炼 LLM 评分，两次 LLM 调用每次耗时约 5-10 秒，5 个并发已经能提供足够的吞吐量（约每分钟处理 30-60 条），同时不会对灰度环境的 AI 助理造成过大压力。设太多线程会打爆灰度环境的并发限制，设太少则评估速度太慢。

#### 灰度环境和正式环境的AI助理的RAG召回策略等信息是用的相同的配置吗？如果不是，二者的同步策略是什么？
灰度环境和正式环境使用**不同的 Diamond 配置空间**。Diamond 配置中心按环境隔离：daily（日常）、pre（预发/灰度）、online（正式）各有独立的配置集。代码通过 EnvUtil 判断当前运行环境加载对应配置。评估任务专门调用灰度环境的 AI 助理（caixiaomiGrayAssistantClient），是因为灰度环境通常部署了最新的 Prompt 和召回策略，评估的目的就是验证这些变更的效果。二者的同步策略是**人工发布流程**：先在灰度环境调整 Prompt/策略 → 用评估任务验证效果 → 评分达标后再手动将配置同步推送到正式环境的 Diamond。

#### 为什么只评估4小时前且准确率低于0.85的记录，这样设置的依据是什么？
这两个条件各有其设计依据：
- **4小时前**：避免重复评估刚处理过的记录。评估任务每次触发时，刚在上一轮被评过分的记录（即使分数低）不会立即被再次评估，需要等 4 小时。这个时间窗口留给运营人员查看低分记录、调整 Prompt 或召回策略，调整生效后下次评估才有意义。如果不设时间窗口，同一条低分记录会被反复评估、反复产出相同的低分，浪费 LLM 调用配额。
- **准确率低于 0.85**：这是质量基线阈值。得分 ≥0.85 的记录被认为答案质量合格，无需持续关注；低于 0.85 的才是需要持续改进的"问题记录"。0.85 这个值是经验值——在实际评估中，0.85 分以上的回答用户体验较好，低于这个值通常意味着回答有明显错误或遗漏。此外，未评估过的记录（ai_answer 或 accuracy_rate 为 null）也会被选入评估。

#### 菜小蜜如何评估回答的准确性？除了LLM-as-a-Judge还有哪些评估机制？
除了离线的 LLM-as-a-Judge 标准Q&A对比评分外，菜小蜜还有一套**三层实时评估体系**：用户反馈 → AI自动打标 → 人工复核，形成闭环质量保障。
- **用户实时反馈**：用户在钉钉卡片上点击"有用/没用"按钮，通过 `FeedbackStrategy` 写入 `feedback_record` 表。点"没用"时展示反馈链接引导填写具体意见。**用户反馈具有最高优先级**——在 AI 打标流程中，如果用户对某条回答点了"没用"，会强制覆盖 LLM 打标结果，将该条标记为 FAIL。
- **AI 自动打标**：`MessageAiMarker` 使用 qwen3.6-plus 模型对历史对话进行批量准确性评估。采用多轮对话窗口打标（待评估记录最多10条 + 上下文已打标记录5条），按用户分组用 `AI_MARK_POOL` 线程池并行处理。Prompt 体系包含角色设定（PromptAiMarkRole）、业务域选项（PromptAiMarkCategory）、评估规则（PromptAiMarkRule）三部分，LLM 返回 JSON 数组包含 markResult（true/false）和 categoryCode（业务域分类）。打标结果优先级：用户反馈(USELESS) > 人工打标 > AI打标。
- **人工复核打标**：运营人员通过管理后台（`MessageMarkServiceImpl`）对 AI 打标结果进行复核修正，支持列表查询、单条/批量修改、导出 Excel（上限50W条）。FAIL 记录自动进入跟进流程（INIT→PROCESSING），解决后转回 INIT。
- **准确率指标**：`finalResult = 人工打标 ?? AI打标`，准确率 = SUCCESS / (SUCCESS + FAIL)。周报卡片（`weeklyReport()`）自动推送 UV、PV、准确率、failCount、历史未解决问题数等指标，驱动 Prompt 与召回策略持续迭代。



### 面试深挖 Q&A

#### 请介绍一下这个项目，你在其中主要负责什么工作？

A：这是面向集团内部数万员工的 HR 领域 Agent，以自然语言对话替代传统表单与人工咨询，覆盖知识问答、智能请假、证明开具等高频场景，日均对话数万次，显著降低 HR 工作量。该 Agent 系统包含多源 RAG 知识检索、两段式意图路由、端到端流式输出、MCP 标准化开放、Prompt 工程化与质量闭环等核心模块。整个系统采用 DDD 分层架构：hcm-ai-application 层承载 RAG 策略编排（如 `AbstractKnowledgeRetrievalStrategy` 及其 4 个子类）和工具执行器（`ToolExecutor` 接口及 10+ 实现）；hcm-ai-infrastructure 层封装百炼 API 调用、流式客户端（`CxmOpenClientImpl`）、Guava RateLimiter 等外部依赖；hcm-ai-interfaces 层负责 HSF Triple 协议接口暴露、SchedulerX 定时任务（知识同步/评估）和 MCP Tool 声明；hcm-ai-domain 层定义领域对象和枚举（如 `KnowledgeSourceEnum` 4 种知识源枚举）。这种分层使得 RAG 策略变更不影响接口协议，基础设施切换（比如百炼 API 升级）不侵入业务逻辑，各层可独立演进。我主要参与了多源 RAG 系统的并发召回与 Rerank 重排、两段式路由引擎中的策略模式抽象与工具注册机制、MCP Server 标准化对外开放、以及 Prompt 外置热更与 LLM-as-a-Judge 离线评估体系的落地。



#### 做项目中遇到的最大挑战是什么？

最大的挑战是 如何提高首字响应延迟。我们的链路很长——路由、改写、多源召回、Rerank、再调 LLM 生成，用户发完消息可能要等好几秒才看到第一个字，体验很差。

核心做了四件事：
1. 路由阶段并行化。 原来工具路由、Query Rewrite、语言检测、知识库分类是串行的，改成 5 路 CompletableFuture 并行，其中 Rewrite 内部又套了 3 路并行，相当于 8 个 LLM 调用同时跑。为此我们做了 线程池隔离，路由、检索、鉴权分三个独立线程池，防止嵌套异步导致线程饥饿死锁。
2. 端到端流式推送。 用 RxJava 的 ReplayProcessor 做中间桥梁——上游接 DashScope 的 Flowable 流式返回，下游对钉钉走卡片流式更新、对 Web 走 Dubbo Triple StreamObserver，LLM 每吐一个 token 就实时推到前端，不用等全部生成完。
3. 1.5 秒兜底提示。 即使做了并行，路由+召回阶段仍需要几秒。我们用 taskScheduler.schedule 设了一个 1.5 秒定时任务，如果这时候 LLM 还没返回第一个 token，就先推一帧"正在搜索知识库..."的加载提示，让用户知道系统在工作，体感上消除了等待焦虑。首字体感延迟下降约 60%。
4. 降级保可用。 智能体 AppId 不可用时自动降级到工作流 AppId；路由阶段任何一个并行任务异常，直接降级到默认知识问答技能，不阻塞主链路。钉钉 API 触发限流时，自动随机退避 1-2 秒重试。

总结就是：并行压缩前置耗时、流式消除生成等待、兜底提示优化体感、降级保障可用性。

#### 你们是怎么准备向量数据并最终存入数据库的？

整体是一条 扫描→抽取→上传 的流水线。

扫描阶段： SchedulerX 定时任务递归遍历钉钉文档空间，和百炼已有文件做 diff，通过文件名、修改时间戳、路径标签三个维度检测变更，产出 ADD/MODIFY/DELETE 事件写入变更日志表。

抽取阶段，两条链路互补： 主链路是 RPA 机器人，通过 REST 接口轮询领取任务，打开钉钉文档抽取内容后回传文件，处理普通文档和表格。辅链路是 SchedulerX 定时任务，处理 RPA 覆盖不了的：FAQ 多维表格通过 AliDing SDK 读取后导出为 Excel，以及文档删除操作。

上传阶段： 调百炼 API 上传文件，指定 DASHSCOPE_DOCMIND 解析器。切片、Embedding、向量索引全部由百炼平台完成，我们不自己做。 上传时带两类标签：修改时间戳（用于下次变更检测）和人群权限标签（从文档路径中提取，检索时做预过滤）。

另外有个监控任务，检测待处理文档积压时自动钉钉告警。


#### 准备向量数据时候没有进行数据清洗吗？那么你如何保障数据的质量？
坦白说，我们在数据清洗这块做得相对轻量，主要有几个原因和对应的措施：

做了的部分：
- FAQ 链路有基础清洗：过滤掉标准问题为空的记录、合并换行符、提取 Markdown 纯文本、替换钉钉临时图片链接为长期 OSS 链接防止过期
- 文档级去重：基于 docKey 做变更检测，避免重复入库
- 文档切片、解析这块我们没有自己造轮子，交给百炼平台的 DocMind 解析器处理，它本身有格式识别和智能分段能力

没有自己做重度清洗的原因：
我们的知识源主要是公司内部钉钉文档，来源可控、格式相对规范，不像爬取互联网数据那样需要大量去噪。而且 DocMind 在解析侧已经处理了格式转换的问题。

数据质量更多是靠下游保障的：
- 检索阶段：双重 Rerank + 最低分 0.2 过滤，低质量切片自然被排到后面
- 权限过滤：每个文档做鉴权，无权限的直接剔除
- 离线评估闭环：LLM-as-a-Judge 定时对标准 QA 打分，如果某个知识源的文档质量导致回答分数下降，我们能快速定位到是哪个文档有问题，反向推动数据源治理

如果要继续优化，我觉得可以在上传前增加 内容完整性校验（过滤空文档、过短文档）和 语义去重（相似内容的文档合并），这是后续可以迭代的方向。

#### 如何提高RAG的准确率？

分五个层面迭代提升：

1. 查询优化： 并行跑三路 Query Rewrite——上下文消歧、关键词同义词扩展、用户身份注入。同时用 LLM 做话题分类预筛知识库，减少噪声源。

2. 召回增强： 7 个异构知识源 CompletableFuture 并行召回，单源 3 秒超时熔断。每个知识源 × 每个改写查询做笛卡尔积并发检索，最大化召回量。

3. 双重 Rerank： 检索层用 qwen3-rerank-hybrid 从 top-100 重排取 top-20；应用层再对文档名做 rerank，混合打分 内容分×0.7 + 文档名分×0.3，优先选来自更相关文档的片段。

4. 前置短路： 维护了一张人工审核的 FAQ 表，精确匹配直接返回标准答案，绕过整个 RAG 链路，高频问题准确率 100%。

5. 兜底 + 闭环： RAG 答不出时无缝降级到通用 LLM + 联网搜索；离线用 LLM-as-a-Judge 对标准 QA 对自动评分，低于阈值的自动标记，驱动 Prompt 和召回策略持续迭代。

总结就是：让问题更精准、让召回更全、让排序更准、让高频问题短路、让质量可度量。


#### 多源 RAG 系统中，4 套异构知识源分别是什么？为什么要做多源而不是统一成一个向量库？

A：4 套知识源分别是：政策库（公司制度文档，权威性最高、更新频率最低）、知识平台已审核内容（经审核的标准知识条目）、知识平台未审核草稿态内容（时效性强但未经人工审核）、钉钉协同文档（业务团队实时编辑的工作文档）。之所以不统一成一个向量库，核心原因有三：一是数据来源和更新频率差异大，政策库可能数月更新一次但权威性最高需要高权重，钉钉文档可能每天变更但内容质量参差不齐，草稿态内容需要明确标注"未审核"提醒用户谨慎采信；二是鉴权模型不同，政策库按文档路径标签做圈人鉴权，知识平台按审核状态区分可见性，钉钉文档按知识库空间权限控制访问范围，统一存储会丢失这些细粒度权限语义——假设把所有文档混在一个向量库里，就无法在 retrieve 阶段通过 filterTags 做权限预过滤，只能召回后再鉴权，浪费大量无效召回；三是召回策略差异，每个知识源的 filterTags 构建逻辑不同（比如钉钉文档要按用户所属圈人标签过滤，而政策库需要按文档路径层级匹配），需要各自的 Strategy 子类实现差异化逻辑。代码上通过 `AbstractKnowledgeRetrievalStrategy` 模板方法抽象，定义了标准流程：`buildFilterTags()` 构建过滤条件 → 调用百炼 retrieve API 检索 → `authSlice()` 文档鉴权。子类只需实现这三个差异点即可接入新知识源，这种设计也方便独立调优每个知识源的召回参数（如 Top-K、相似度阈值），互不影响。面试中可以进一步讨论：这本质上是"领域驱动的知识分层"设计，每个知识源对应一种领域语境。

#### CompletableFuture 并发召回时，你提到要"先 collect 再 join"，为什么？如果直接 stream().map(future -> future.join()) 会怎样？

A：如果写成 `stream().map(strategy -> CompletableFuture.supplyAsync(...)).flatMap(f -> f.join().stream())`，由于 Java Stream 的惰性求值特性，每个元素会走完整个 pipeline 再处理下一个——也就是说第一个 Future 创建后立刻 join 阻塞等待完成，然后才创建第二个 Future。这就退化成了串行执行，完全丧失了并发的意义。根本原因在于 Stream 的中间操作（map/flatMap）是惰性的，只有遇到终端操作（collect/forEach）才触发实际执行，而且默认是单线程顺序处理管线中的每个元素。正确做法是先 `.collect(Collectors.toList())` 作为第一个终端操作，强制触发所有 Future 的创建——此时 4 个异步任务已经同时提交到线程池开始执行——再用第二个 `.stream().flatMap(future -> future.join().stream())` 依次收集结果。这样总耗时约等于最慢的那个知识源（通常 1-2s），而非所有知识源的耗时之和（可能 4-8s）。代码注释里也明确标注了"先 collect 收集所有 Future，否则变成串行"。这个陷阱在 CompletableFuture + Stream 组合使用中非常常见，本质是开发者忽略了 Stream 的惰性语义。面试中如果被追问，可以进一步延伸：parallelStream 虽然能并行，但它使用 ForkJoinPool.commonPool，无法控制线程池隔离，且 commonPool 大小默认等于 CPU 核心数减一，对 IO 密集型任务严重不足。所以我们选择显式使用 CompletableFuture + 自定义线程池的方式，对并发度、超时、异常处理都有完全的控制权。

#### 你提到用了三层线程池隔离，具体怎么设计的？为什么不用一个线程池？

A：三层线程池分别是：外层 `RAG_STRATEGY_POOL`（core=30, max=50）驱动 4 个 Strategy 并发召回；内层 `RAG_RETRIEVE_POOL`（core=100, max=150）处理每个 Strategy 内部多 query × 多知识库的并发百炼 API 调用；鉴权层 `KNOWLEDGE_AUTH_POOL`（core=200, max=300）处理每个知识切片的文档权限校验。不能共用一个线程池的根本原因是防止线程饥饿死锁（Thread Starvation Deadlock）：假设只有一个 100 线程的池，外层 4 个 Strategy 占用 4 个线程后，每个 Strategy 内部又要提交 N 个 retrieve 子任务到同一个池。当并发请求稍多（比如 QPS=3 同时来了 3 个请求，占用 12 个外层线程，每个要提交 5-10 个内层任务），线程池很快被占满，新提交的子任务全部排队等待，但持有线程的外层任务又在 `future.join()` 等内层任务完成，形成循环等待——经典的死锁。三层隔离后，每层有独立的线程资源，外层等内层时不会阻塞内层的线程分配。线程池参数是基于 QPS=3 推导的：外层 4 个 Strategy × 3 QPS = 12 并发，core=30 留了 2.5 倍余量应对突发流量；内层每个 Strategy 可能并发 5-8 个百炼 API 调用（多 query × 多知识库组合），4 Strategy × 8 × 3 QPS ≈ 96，所以 core=100；鉴权层每个知识切片独立鉴权，Top-20 切片 × 4 知识源 × 3 QPS = 240，core=200 接近满负荷，max=300 兜底。所有线程池都配置了自定义的 ThreadFactory 设置线程名前缀（如 `rag-strategy-`），方便在线程 dump 和日志中快速定位问题。面试中可以延伸讨论：这种分层线程池模式与 Bulkhead Pattern（舱壁隔离模式）思想一致，核心原则是"不同优先级/不同生命周期的任务必须使用独立资源"。

#### 单源超时 3s 熔断降级是怎么实现的？超时后未完成的 Future 怎么处理？

A：在 `AbstractKnowledgeRetrievalStrategy.collectAuthResults()` 中用 `CompletableFuture.allOf(futures).get(3, TimeUnit.SECONDS)` 设置鉴权阶段的总超时。catch 到 `TimeoutException` 后，遍历所有 Future，通过 `isDone()` 判断哪些已完成，只收集已完成的鉴权结果，未完成的切片标记为无权限（降级处理），相当于"宁可少展示、不可慢响应"。3 秒阈值的选取基于两个考量：一是用户体验——HR 咨询场景下，3 秒内首字不出会导致用户明显感知到卡顿；二是鉴权接口的 P99 延迟统计，正常情况下单次鉴权 200-500ms，3 秒足够覆盖绝大多数请求，超时意味着下游服务可能出了问题。未完成的 Future 本身不会被取消——`CompletableFuture` 不支持真正的中断（`cancel(true)` 只是设置中断标志，如果底层是 HTTP 调用并不会中断 Socket），它们会在后台继续执行直到完成，但结果被丢弃。这在当前场景下是可接受的，因为鉴权操作本身是幂等的读操作，不会有副作用，只是浪费了一些线程资源。如果要做得更精细，可以考虑在 OkHttp 层设置 per-call timeout 来真正中断网络 IO，但目前的 QPS 水平下不值得增加这层复杂度。超时降级后会在日志中记录哪些切片超时、原始 query 和知识源类型，方便后续排查是某个知识源普遍慢还是偶发抖动。

#### Rerank 重排的具体策略是什么？为什么要做两段 Rerank？

A：Rerank 分两段：第一段在百炼 retrieve API 内部，使用 `qwen3-rerank-hybrid` 模型对向量检索结果做初步排序，从 Top-100 收敛到 Top-20——这里选 hybrid 模型是因为它同时结合了向量语义相似度和稀疏词频匹配（类似 BM25），在 HR 领域存在大量专业术语（如"N+1 赔偿"、"竞业限制"、"试用期转正"）的场景下，纯向量检索可能因语义相似但概念不同而产生误召回（如"年假"与"产假"语义接近但政策完全不同），hybrid 模式通过精确词匹配弥补了这个短板；第二段在应用层 `SliceHelper.reRank()` 中，用 `qwen3-rerank` 模型对文档名（而非正文）做 rerank，然后与原始召回分加权融合：`最终得分 = 原召回分 × 0.7 + 文档名 rerank 分 × 0.3`。做两段 Rerank 的必要性在于：第一段是在单个知识库内排序，每个知识源独立返回 Top-20；第二段是跨 4 个知识源的全局排序——4 个知识源各自返回的 Top-20 合并后可能有 80 条，需要统一标准重排取真正的 Top-K。为什么第二段不直接用正文 rerank 而是用文档名？因为正文 rerank 在第一段已经做过了，第二段的核心目的是引入"来源权威性"信号：文档名包含了知识来源、分类等元信息（如"公司差旅报销政策 V3.0"），文档名与 query 的匹配度能反映知识源的相关性维度。权重 0.7/0.3 是经过离线评测调优的经验值——正文语义相似度是主信号，文档名匹配是辅助信号，过高的文档名权重会导致标题党文档排名虚高。最终的 Top-K 数量由 Diamond 配置 `knowledgeSliceTopK` 控制（默认 20），注入到 RAG 答案生成的 Prompt 上下文中，这个值太小会丢失相关信息，太大会导致 Context 超长增加 Token 成本和推理延迟。

#### 两段式路由引擎中，"规则前置拦截"和"LLM Function Routing"的分工标准是什么？

A：分工标准是确定性 vs 模糊意图。前置拦截器处理两类确定性场景：一是特殊指令（如"开启新话题"、"清除历史"等固定文本指令），通过 `SpecialCommandTool` 直接匹配关键词，零 LLM 调用，延迟 <5ms；二是相似问题精确匹配（FAQ），通过 `SimilarQuestionTool` 在预构建的 FAQ 库中查找高相似度匹配，命中后直接返回标准答案。这两类场景不需要 LLM 理解意图，本地规则就能判定，走 LLM 反而浪费时间和 Token。只有前置拦截器全部返回 null（表示"我不处理"），才进入 LLM Function Routing 阶段。架构上的关键设计：前置拦截器通过 `isPreInterceptor()` 方法标识自己，`ToolExecutorRegistry` 在构造映射表时会过滤掉所有 `isPreInterceptor() == true` 的实现——这意味着前置拦截器不会出现在 LLM 路由的工具列表 Prompt 中，LLM 完全不知道它们的存在。执行顺序通过 `CxmAiEngine.executeToolForRobot()` 中的硬编码保证：先按固定优先级依次执行前置拦截器，只有全部不命中才走 `aiRouting()`。这种设计的好处不仅是低延迟，还能防止 LLM 路由误判——比如用户说"开启新话题"，如果交给 LLM 路由，它可能误识别为知识问答意图；前置拦截能保证 100% 的确定性命中率。面试中可以延伸讨论：这本质是一种分层决策架构，与推荐系统中"粗排 → 精排 → 重排"的思路类似——用低成本手段先过滤确定性 case，把不确定的留给高成本的 LLM。

#### LLM Function Routing 阶段并发了几个 LLM 调用？为什么要并发而不是串行？

A：并发了 4 个 LLM 调用：技能路由（将工具列表序列化为 Prompt，用 LLM 识别用户意图匹配哪个工具）、语言识别（判断用户输入是中文/英文/繁体，用于后续回答的语言适配）、问题重写（内部又嵌套并发 3 路：上下文重写——结合对话历史补全指代/省略、关键词+同义词扩展——增加召回覆盖面、用户身份信息重写——注入用户所属公司/部门等上下文）、知识库标签识别（判断该查哪个知识子库，用于 RAG 阶段的 filterTags 构建）。实际最多 6 个并发 LLM 调用（4 主路 + 问题重写内嵌 3 路，重写算 1 主路）。之所以并发，是因为这 4 个任务之间没有数据依赖——路由不需要等重写结果（路由基于原始 query，重写基于扩展后的 query 用于 RAG），语言识别不依赖标签识别。串行执行需要累加每个调用的延迟（每个约 200-500ms），总延迟可能达 2 秒以上，严重影响用户对首字响应的感知。并发执行后总延迟只取决于最慢的那个调用，通常在 500ms 以内。路由阶段统一使用 `qwen-flash` 模型（qwen 系列中速度最快的版本），选它而非 qwen-plus/qwen-max 的原因是：路由和分类任务的复杂度远低于生成任务，flash 模型的准确率与 plus 无明显差异（内部评测路由准确率均 >95%），但推理速度快 3-5 倍且 Token 成本低一个量级。如果某个并发 LLM 调用失败，不会阻塞整体——比如语言识别失败就默认中文，标签识别失败就查全部知识库，保证主链路的容错能力。

#### ToolExecutor 策略模式的注册机制是怎么实现的？新增一个技能需要改多少代码？

A：注册通过 Spring 自动依赖注入实现：`ToolExecutorRegistry` 构造器注入 `List<ToolExecutor>`，Spring 容器启动时自动收集所有实现了 `ToolExecutor` 接口且标注 `@Component` 的 Bean，注入到这个列表中。Registry 在初始化时做两件事：一是过滤掉所有 `isPreInterceptor() == true` 的实现（它们走前置拦截路径，不参与 LLM 路由）；二是按 `getOperationId()` 建立 `Map<String, ToolExecutor>` 映射表，路由结果返回 operationId 后直接查表获取对应的执行器。新增技能分两步：代码侧只需新写一个类实现 `ToolExecutor` 接口（约定 `getOperationId()` 返回唯一标识、`execute()` 方法实现业务逻辑），加 `@Component` 注解即可在下次发版时自动注册——注意这里不需要修改 Registry 或 Engine 的任何代码，完全开闭原则；配置侧在 Diamond 的 `AiAssistantConfigData.tools` 列表中添加该工具的 JSON 描述（operationId、title、description、exampleQueries、parameters、crowdRuleId），LLM 路由 Prompt 会自动将新工具的描述序列化进去。配置是热更新的，意味着工具的描述文案、参数定义甚至圈人规则都可以在不发版的情况下调整。整个过程对 `CxmAiEngine` 主流程零侵入。目前已注册 10+ 工具，包括知识问答（兜底技能）、智能请假、拨打热线、智能问数、人才档案、智能证明、智能商保、假期余额、请假记录、调动等。面试深入点：可以讨论为什么用 Spring 自动注入而不是 SPI 机制——Spring 自动注入天然支持条件装配（`@Conditional`），未来某个技能要按环境/租户动态启停只需加注解，不需要改 Registry 逻辑。

#### Dubbo Triple StreamObserver 流式推送是怎么实现的？ReplayProcessor 起什么作用？

A：HSF 接口声明为 `void callStream(ChatRequest request, StreamObserver<ChatResponse> response)`，使用 Triple 协议（基于 HTTP/2 的 gRPC 兼容协议）暴露服务端流式接口。选择 Triple 而非 WebSocket 或 SSE 的原因是：Triple 原生支持双向流、自带序列化（Protobuf/Hessian）、与公司 HSF 生态无缝集成，不需要额外搭建 WebSocket 网关。内部核心是 `ReplayProcessor`（RxJava 热流）作为桥梁连接上下游：上游百炼流式 API 通过 `CxmOpenClientImpl` 发起调用，设置 `incrementalOutput=true` 启用增量输出模式（每帧只传输新增文本而非全量累积），百炼返回的 `Flowable` 流每产生一帧就 push 到 ReplayProcessor；下游通过 `processor.subscribe()` 订阅，在回调中逐帧封装为 `ChatResponse` 并调用 `StreamObserver.onNext()` 推送到客户端。选择 `ReplayProcessor` 而非 `PublishProcessor` 的关键原因是：ReplayProcessor 带有 `createWithTime(1, TimeUnit.MINUTES)` 配置的时间窗口缓存，晚加入的订阅者可以回放窗口内的所有历史帧。这在我们的场景中至关重要——因为百炼流可能在 subscribe 之前就已经开始产生数据（异步调度的时序问题），PublishProcessor 会丢弃 subscribe 之前的帧，导致用户看到的回答缺少开头部分。1 分钟的时间窗口足够覆盖一次完整对话的流式输出周期。背压控制方面，`Flowable` 原生支持 Reactive Streams backpressure 规范，当下游消费速度慢于上游产生速度时会自动缓冲，避免数据丢失。

#### 1.5s 无响应自动推送加载提示是怎么实现的？如果和第一帧数据竞争怎么办？

A：通过 Spring `TaskScheduler` 在请求发起时调度一个 1500ms 延迟任务。延迟任务执行时检查 `finalResult`（`AtomicReference<String>`）是否仍为初始空字符串，如果是，说明 1.5 秒内百炼还没返回任何内容，就推送一条灰色的"正在为您检索知识库..."加载提示到钉钉卡片，给用户以视觉反馈。当百炼第一帧数据到达时，会通过 `updateCardDateStream()` 更新同一张卡片的 content 字段，直接覆盖加载提示文案。为什么选 1.5 秒？这是基于用户体验研究的经验值——Web 端超过 1 秒无反馈用户开始焦虑，超过 3 秒用户可能放弃，1.5 秒在"避免过早显示加载提示（大部分请求 1 秒内出首字）"和"不让用户等太久"之间取了平衡。竞争问题的处理分三种情况分析：第一种，百炼在 1.5s 内返回了第一帧，`finalResult` 已通过 `compareAndSet` 被设为非空，延迟任务检查后直接跳过不推送——不存在竞争。第二种，百炼恰好在 1.5s 左右返回，延迟任务和数据帧几乎同时到达——由于 `AtomicReference.compareAndSet` 是 CAS 原子操作，只有一方能成功将空字符串设为非空，保证了状态转换的原子性；即使加载提示先推送了，数据帧紧随其后覆盖卡片内容，钉钉卡片更新是最终一致的（以最后一次更新为准），用户看到的短暂闪烁在实际网络延迟下几乎不可感知。第三种，百炼始终没有返回（如 API 超时），加载提示会持续展示直到超时兜底逻辑触发错误提示。这种设计的核心思路是"乐观并发"——不加锁、不互斥，通过原子变量 + 覆盖语义保证最终一致性。

#### MCP Server 暴露了哪些 Tool？为什么只暴露原子能力而不包含 LLM 总结？

A：暴露了 3 个 MCP Tool：`getCxmEmployeeInfo`（获取用户身份信息，包括工号、姓名、部门、职级等）、`knowledgeRecall`（知识检索，指定政策库/知识平台/协同文档三个知识源并发召回，返回鉴权后的原始知识切片列表）、`similarQuestion`（相似问题匹配，在 FAQ 库中查找最相似的标准问答对）。只暴露原子能力是刻意的设计选择，核心原因是"不越权"：MCP 的定位是让外部 Agent（如集团悟空平台）编排调用，外部 Agent 有自己的 System Prompt、上下文窗口和回答策略——如果我们在 Tool 内部就做了 LLM 总结，外部 Agent 收到的就是一段被"加工"过的文本，无法基于原始知识切片做二次推理（如交叉引用多个切片）、格式定制（如生成表格对比）或引用溯源。把 LLM 总结留给调用方，保持了最大的灵活性，符合 MCP 协议"Tool 做能力、Agent 做决策"的哲学。实现细节上，使用 `@alibaba/mcp-lite` 框架，通过 `@Tool` 注解声明 MCP Tool，`@McpContextAware` 注解自动注入 MCP 上下文——`McpContext.getUser()` 获取调用者身份用于鉴权，不需要额外的身份参数透传。`knowledgeRecall` 内部复用 `RecallServiceImpl` 和 `AbstractKnowledgeRetrievalStrategy`，与自有对话入口共享同一套 RAG 链路，避免重复建设。会话标识加 `wukong_` 前缀区分外部 MCP 调用和自有入口调用，便于后续的流量分析、问题排查和计费统计。面试延伸点：可以讨论 MCP 的 Tool vs Resource 原语选择——我们用 Tool 而非 Resource，因为知识检索是有参数的主动操作（需要传 query），不是被动的数据暴露。

#### Prompt 和 Tool Schema 外置到配置中心具体怎么做的？怎么保证热更新的安全性？

A：每个 Prompt 模板是一个独立的 `@DiamondListener` 类，通过阿里 Diamond 配置中心管理。比如 `PromptToolRouting` 监听 dataId `com.cainiao.management.cnwork:prompt:toolsRouting`，收到推送后更新 `volatile` 静态变量，调用方通过 `buildPrompt()` 静态方法获取最新模板做占位符替换（如 `{tools}`、`{userInput}`、`{conversationHistory}` 等）。目前有 17 个独立 Prompt dataId，涵盖路由、RAG 答案生成、问题重写、语言识别、标签识别、评分等各环节。为什么用 17 个独立 dataId 而不是一个大 JSON？因为不同 Prompt 的修改频率和负责人不同，分开管理可以独立灰度推送、独立回滚，减少变更影响面。`volatile` 关键字保证了多线程场景下的可见性——Diamond 回调线程更新变量后，业务请求线程能立即读到最新值，无需加锁。Tool Schema 同理，`AiAssistantConfigData.tools` 列表配置在 Diamond，每个工具是一个 JSON 对象，包含 operationId、title、description（给 LLM 看的自然语言描述）、exampleQueries（示例问题列表，用于路由 Prompt 的 few-shot）、parameters（参数定义）和 crowdRuleId（圈人规则 ID），热更新即可添加/修改/禁用工具。安全性方面，Diamond 本身有灰度推送（先推一台观察再全量）、版本回滚能力（每次推送保留历史版本）；代码层面目前依赖 Diamond 的审计日志做变更追踪。这个项目未在应用侧做推送 lint 校验，是一个已知的改进点——在薪酬 AI 质检项目中我们补上了这个短板，推送前做长度下限 + 关键词白名单 lint，防止空 Prompt 上线导致全链路故障。

#### LLM-as-a-Judge 评估的具体流程是什么？怎么驱动 Prompt 迭代？

A：评估流程分三步：首先从云灵知识平台采集标准 Q&A 对（`collectQAFromXl` 方法），按类目分页拉取写入本地数据库作为评估基线——标准 Q&A 由 HR 业务方维护，每条包含标准问题、标准答案、所属类目（如"请假政策"、"入职流程"、"差旅报销"等）；然后 SchedulerX 定时任务触发 `CxmAnswerScoreServiceImpl.evaluateAnswerScore()`，用 5 线程固定池（`Executors.newFixedThreadPool(5)`）并发处理——每条记录先调用灰度环境的 AI 助理 callStream 接口获取对标准问题的回答（注意必须用灰度环境而非线上，避免评测流量污染真实用户数据），再用独立的 LLM 调用（评分 Prompt 也外置在 Diamond）比对标准答案和 AI 助理的实际答案，按相关性、完整性、准确性三个维度给出综合分数，分数用 `BigDecimal` 存储（避免浮点精度问题，便于后续做阈值比较）。最后分数写入数据库，下一轮评估时只重新评估"4 小时前评估且准确率低于 0.85"的记录——这个策略实现了"持续改进低分项"的效果：高分记录（>0.85）视为已达标不再重复评估（节省 LLM 调用成本），低分记录持续跟踪直到改善。驱动 Prompt 迭代的闭环是：低分记录按类目聚合后，分析哪个类目的平均准确率持续偏低——如果是"请假政策"类目低分，可能是 RAG 没有召回正确的请假政策文档，需要调整该类目的 filterTags 或知识库标签；如果是回答质量问题（信息正确但表述不好），则调整 RAG 答案生成的 Prompt 模板。5 线程的选择是权衡评估速度和灰度环境负载——太多并发会影响灰度环境的其他测试活动。

#### RAG 答案不满意时有什么兜底策略？

A：有两层兜底，形成"RAG 知识回答 → 通识 LLM 兜底 → 路由降级兜底"的三级容错体系。第一层是 RAG 到通识问答的无缝切换：在 `answerWithFallback()` 中，系统会监听 RAG 流式输出的累积内容，如果检测到包含"暂时没有相关内容"这类特定话术（说明 RAG 没有召回到有效知识），通过 RxJava 的 `concatWith(Flowable.defer(...))` 无缝拼接一次通识 LLM 问答流——`defer` 的作用是延迟创建 Flowable，只有前序流触发了兜底条件才会实际发起通识 LLM 调用，不浪费资源。`concatWith` 保证前序流完成后再拼接后续流，对调用方来说感知不到流的切换，用户看到的是一个连续的回答。第二层是路由层面的降级：如果 `aiRoutingForTool()` 路由失败（LLM 调用超时/异常）、返回的 operationId 为空或在注册表中找不到对应执行器，自动降级到 `caixiaomiKnowledgeQA`——知识问答是最安全的兜底技能，即使知识库也没有相关内容，至少能通过第一层兜底给出通识回答而不是报错。还有一个隐含的第零层：AppId 降级——`CxmOpenClientImpl` 在调用百炼时优先使用智能体 AppId（支持更复杂的编排），如果未配置或调用失败，自动降级到工作流 AppId（功能简单但更稳定），保证了即使百炼智能体版本出问题，流式链路本身不会中断。面试中可以讨论这种分层降级设计的原则：每层降级都是有损的（信息质量递减），但保证了系统在各种异常场景下的可用性——宁可给用户一个"不够完美但有用"的回答，也不能返回空白页面或错误提示。

#### 项目的 DDD 分层架构是怎么设计的？各层的具体职责和边界是什么？

A：项目按照 DDD 四层架构划分为四个 Maven 模块。hcm-ai-interfaces 是最外层的接口适配层，负责协议适配和对外暴露：包括 HSF Triple 流式接口（`CxmStreamServiceImpl`）、SchedulerX 定时任务（`DingDocSyncProcessor` 知识同步、`CxmAnswerScoreServiceImpl` 评估任务、`DingDocScanProcessor` 变更扫描）、以及 MCP Tool 声明（`CxmMcpTool` 3 个工具）。这一层的核心原则是"薄"——不包含业务逻辑，只做参数校验、协议转换和调用编排，比如接收 HSF 请求后组装 `ChatRequest` 对象交给 application 层处理。hcm-ai-application 是应用服务层，承载核心业务编排逻辑：RAG 策略（`AbstractKnowledgeRetrievalStrategy` 及 4 个子类的策略选择与并发编排）、工具执行器（`ToolExecutorRegistry` 注册 + `ToolExecutor` 10+ 实现的策略模式路由）、路由引擎（`CxmAiEngine` 两段式路由编排）、评估服务等。这一层协调多个领域对象和基础设施完成完整业务流程，但不直接操作外部系统。hcm-ai-infrastructure 是基础设施层，封装所有外部依赖的技术实现：百炼 API 封装（`CxmOpenClientImpl` 流式/非流式调用、retrieve 检索）、Guava RateLimiter（查询 5QPS/删除 10QPS/上传 10QPS）、钉钉 API 调用、Diamond 配置监听等。这一层实现 application 层定义的 Repository/Gateway 接口，上层通过接口依赖倒置、不直接依赖具体实现。hcm-ai-domain 是领域模型层，定义纯领域对象：`KnowledgeSourceEnum`（4 种知识源枚举）、`KnowledgeSlice`（知识切片值对象）、对话上下文等，没有任何技术依赖，是最稳定的内核。分层的实际收益举例：当百炼 API 从 V1 升级 V2 时，只需修改 infrastructure 层的 `CxmOpenClientImpl`，application 层和 interfaces 层完全不受影响；当新增一个 MCP Tool 时，只需在 interfaces 层加注解声明，复用 application 层已有的服务逻辑。

#### 对话历史为什么用 XML 格式注入而不是用多轮 message？这样做有什么优势？

A：代码中 `buildConversationMessage()` 将历史对话包装成 XML 文本注入为单条 USER message，而不是将每轮对话拆成独立的 user/assistant message 交替传入。这样做的核心原因是防止角色混淆（Role Confusion）。如果将历史对话以标准的多轮 message 格式传入，LLM 会将历史中的 assistant 回复视为"自己之前真正说过的话"，产生两个问题：一是当历史对话中包含错误信息或过时内容时，LLM 会倾向于保持与"自己之前说过的话"的一致性，即使当前轮次的 RAG 知识切片提供了更准确的信息，也不愿意自我纠正——这在 HR 政策场景中特别危险，政策可能随时更新；二是历史对话中的 assistant 回复可能包含格式化指令或特殊格式（如 markdown 表格、引用标签），这些内容被当作 assistant 角色的上下文时可能干扰当前轮次的输出格式。用 XML 格式（`<conversation><turn role="user">...</turn><turn role="assistant">...</turn></conversation>`）注入为单条 USER message 后，LLM 将整个对话历史视为"用户提供的参考资料"而非"自己的记忆"，可以客观地审视历史内容并以当前 RAG 知识为准回答。XML 格式的选择是因为它的标签语法对 LLM 来说有清晰的结构边界（相比 JSON 或纯文本分隔符），LLM 能很好地理解 `<turn role="user">` 和 `<turn role="assistant">` 的语义。这种设计还有一个附带好处：单条 USER message 中包含所有历史，简化了 Prompt 模板的组装逻辑，不需要动态构建 message 数组。面试中可以进一步讨论：这种做法在 OpenAI 和 Anthropic 的官方最佳实践中都有类似建议——在需要"引用但不认同"历史对话时，用嵌入式文本而非角色消息。

#### 圈人机制（crowdRuleId）在路由中是怎么起作用的？

A：圈人机制的作用是控制每个工具对特定人群的可见性——不是所有用户都能使用所有技能。在 Diamond 配置的 `AiAssistantConfigData.tools` 列表中，每个工具 JSON 对象包含一个 `crowdRuleId` 字段，关联到公司内部的人群圈选系统（类似 A/B 测试的受众圈选）。路由阶段的工作机制是这样的：在 `CxmAiEngine.executeToolForRobot()` 进入 LLM Function Routing 之前，系统会先遍历所有注册的工具配置，对每个配置了 crowdRuleId 的工具调用 `crowdWrapper.validateMatched(crowdRuleId, userId)` 校验当前用户是否在目标人群中。只有校验通过的工具才会被序列化进路由 Prompt——也就是说，不在某个工具圈人范围内的用户，LLM 在路由时根本看不到这个工具的存在，自然不会路由到它。这种设计的应用场景包括：新技能灰度上线（比如"智能证明"功能先对 IT 部门开放，验证稳定后再全量）、差异化能力（比如"智能问数"只对有数据权限的管理层开放）、临时下线（将 crowdRuleId 设为空集等价于禁用该工具）。相比在 ToolExecutor 内部做权限校验，圈人机制在路由前置过滤有两个优势：一是节省 Token——不展示不可用工具的描述，路由 Prompt 更短；二是避免用户困惑——如果 LLM 路由到一个用户无权使用的工具再报"无权限"，用户体验很差。圈人规则的更新不需要发版，只需在人群圈选系统中修改规则，工具配置的 crowdRuleId 保持不变，实现了权限策略与代码的完全解耦。

#### 问题重写 3 路并发具体是怎么设计的？对 RAG 召回率有多大提升？

A：问题重写通过 `aiRoutingForUserInputRewrite()` 实现，内部并发发起 3 路 LLM 调用，每路从不同角度扩展原始 query：第一路是上下文重写——结合对话历史补全指代和省略，比如用户第二轮问"那它的报销标准呢"，上下文重写会扩展为"差旅机票的报销标准是什么"，这对多轮对话场景的召回率提升最大；第二路是关键词+同义词扩展——提取 query 中的核心概念并生成同义词/近义词，比如"年假"扩展为"年假 年休假 带薪假期"，增加召回覆盖面，对长尾专业术语场景贡献最大；第三路是用户身份信息重写——注入用户所属公司（如菜鸟/淘天/本地生活）、职级、城市等上下文信息，比如将"请假政策"重写为"菜鸟集团杭州P7员工的请假政策"，让 RAG 检索能命中针对特定公司/地域的政策文档。3 路重写的结果都会作为独立的 query 输入到 RAG 检索阶段——也就是说，一个用户问题会变成 3-4 个检索 query（原始 + 3 路重写），每个 query 都会在 4 个知识源中检索，大幅增加了召回的覆盖面。3 路并发使用 `CompletableFuture.supplyAsync()` 提交到 LLM 调用线程池，由于三路之间无依赖关系，总耗时只取决于最慢的一路（通常 200-400ms）。在内部评测中，3 路重写相比不重写，RAG 召回率（Recall@20）从约 72% 提升到约 85%，提升约 13 个百分点。3 路而非更多路的原因是边际收益递减——增加第 4 路（如 query 分解）在评测中只额外提升 2%，但多一路 LLM 调用增加延迟和 Token 成本，性价比不划算。

#### 百炼 retrieve API 的具体参数和调优经验是什么？

A：百炼 retrieve API 是 RAG 系统的核心检索接口，关键参数和调优经验如下。检索模型方面，使用 `qwen3-rerank-hybrid` 作为 rerank 模型，hybrid 意味着同时结合向量语义相似度和稀疏词频匹配（类似 BM25），在 HR 领域存在大量专业术语（如"N+1 赔偿"、"竞业限制"、"试用期转正"）的场景下，纯向量检索可能因语义相似但概念不同而产生误召回（如"年假"与"产假"语义接近但政策完全不同），hybrid 模式通过精确词匹配弥补了这个短板。检索数量方面，设置向量检索 Top-100 → 经 rerank 模型重排后取 Top-20。Top-100 的选取逻辑是：向量检索的初始召回噪声较多，但要保证 rerank 模型有足够的候选集做精排——太小（如 Top-30）可能漏掉相关文档，太大（如 Top-500）会显著增加 rerank 延迟。Top-20 是 rerank 后保留的高质量切片数量，这些切片会进入应用层的二次 rerank。相似度阈值设为 0.2，低于此阈值的切片直接丢弃——0.2 是一个很宽松的阈值，目的是"宁可多召回一些不太相关的、也不能漏掉可能相关的"，精排交给 rerank 模型处理。filterTags 参数支持在检索阶段做文档级过滤，我们按知识源类型注入不同标签：政策库按文档路径标签过滤（只看用户有权限的文档目录），钉钉文档按圈人标签过滤（只看用户所属团队的文档空间）。调优经验总结：阈值从默认的 0.5 下调到 0.2 后，召回率提升了约 15%，但准确率略有下降（从 88% 到 85%），通过第二段应用层 rerank 弥补了准确率损失；Top-100 的最佳值是通过离线评测网格搜索确定的（测试了 50/100/200 三档），100 在召回率和延迟之间取得了最佳平衡。

#### 知识切片的鉴权是怎么做的？按文档名去重的优化是什么？

A：知识切片鉴权发生在 RAG 检索返回结果之后、注入 Prompt 之前，确保用户只能看到自己有权限访问的知识内容。具体实现在 `AbstractKnowledgeRetrievalStrategy.collectAuthResults()` 中：对 retrieve API 返回的每个知识切片，异步提交到 `KNOWLEDGE_AUTH_POOL` 线程池（core=200, max=300），调用对应知识源的鉴权接口（如钉钉文档空间权限校验、知识平台审核状态检查）。鉴权结果为布尔值——有权限的切片保留，无权限的过滤掉。核心优化是按文档名去重鉴权：一个文档可能被切成 5-10 个知识切片（chunk），如果对每个切片都独立鉴权，实际上对同一个文档的权限查了 5-10 次，而鉴权结果是相同的——权限是文档级别的，不是切片级别的。优化逻辑是：先按文档名（`documentName`）对切片分组，每组只取第一个切片调用鉴权接口，结果缓存后应用到同组所有切片——"单个文档只鉴权 1 次"。这个优化在实际场景中效果显著：一次 RAG 检索可能返回 80 条切片（4 知识源 × Top-20），如果来自 20 个不同文档，鉴权调用从 80 次降到 20 次，减少了 75% 的鉴权 API 调用量，直接降低了鉴权阶段的延迟和下游服务的压力。去重使用 `LinkedHashMap` 保证遍历顺序与原始召回分排序一致，避免去重后打乱 rerank 排序。鉴权结果不做跨请求缓存（如 Redis），因为文档权限可能随时变更（如员工调部门），使用过期缓存会导致权限泄漏的安全风险。超时处理方面，通过 `CompletableFuture.allOf(futures).get(3, TimeUnit.SECONDS)` 设置总超时，未完成的鉴权直接标记为无权限——安全降级，宁可少展示也不能泄漏未鉴权内容。

#### 钉钉卡片流式更新的具体实现是什么？每帧如何渲染？

A：钉钉卡片流式更新的完整链路是：百炼流式 API 每产生一帧增量文本 → Flowable 推入 ReplayProcessor → 订阅回调中累积到 `StringBuilder` → 调用钉钉卡片 OpenAPI `updateCardDateStream()` 更新卡片内容。具体的帧处理逻辑：百炼 API 设置了 `incrementalOutput=true`，每帧只包含新增的文本片段（如"根据"、"公司差旅"、"政策规定"），订阅回调收到每帧后 append 到 `StringBuilder`，然后用完整的累积文本（`sb.toString()`）调用卡片更新接口。钉钉互动卡片（Interactive Card）支持 markdown 渲染——卡片模板中定义了一个 content 变量，每次更新传入最新的累积文本，钉钉客户端自动做 markdown 到富文本的渲染。用户看到的效果类似 ChatGPT 的逐字出现：每隔 50-200ms（取决于百炼的输出速度和网络延迟）卡片内容刷新一次，文本逐渐变长。但并非每一帧都触发卡片更新——为了避免过高频率的 API 调用导致钉钉服务端限流，实际实现中有节流机制：只有当累积文本相比上次更新增加了一定长度（或距上次更新超过 200ms）才触发 API 调用。流式输出结束时（百炼返回 `finish_reason=stop`），触发最后一次卡片更新确保内容完整，同时更新卡片底部的操作按钮（如"有用/没用"反馈按钮从隐藏变为可见）。异常处理方面：如果某一帧更新失败（网络抖动），不影响后续帧——下一帧会带上完整的累积文本覆盖，天然具有自愈能力。如果整个流式过程异常中断（如百炼 API 超时），通过 `onError` 回调推送一条错误提示更新到卡片，避免卡片停留在半成品状态。

#### 整个请求链路的延迟分布是什么样的？哪些环节是性能瓶颈？

A：一次完整的对话请求链路的延迟分布大致如下：前置拦截阶段 5-50ms（特殊指令匹配和 FAQ 精确匹配，命中即返回不走后续环节）→ LLM 路由阶段 300-500ms（4 路并发 LLM 调用取最慢值，qwen-flash 模型单次调用 200-400ms）→ RAG 检索阶段 1000-2500ms（4 知识源并发 retrieve + 鉴权 + 两段 Rerank）→ 流式生成阶段首字 500-1000ms + 逐字输出 3-8s（百炼智能体流式调用，取决于回答长度）。端到端首字延迟（用户发送问题到看到第一个字）约 2-3 秒，完整回答 5-12 秒。性能瓶颈分析：最大瓶颈是 RAG 检索阶段，占总延迟的 40-50%。其中百炼 retrieve API 单次调用 500-800ms（含向量检索 + rerank），4 知识源并发但需要等最慢的那个返回；鉴权阶段 500-1000ms，虽然已经做了按文档名去重优化，但当切片来自大量不同文档时鉴权调用仍然较多。第二大瓶颈是 LLM 路由阶段，虽然已经并发化到 500ms 以内，但如果某一路 LLM 调用抖动（偶发 800ms+），会拖慢整体。已做的优化包括：三层线程池隔离消除线程饥饿、问题重写 3 路并发化、RAG 检索 4 知识源并发化、单源 3s 超时熔断降级、路由使用 flash 快速模型、1.5s 加载提示改善感知体验。还在考虑的进一步优化方向：RAG 检索和 LLM 路由阶段做流水线重叠（路由结果出来后不等所有重写完成就开始部分知识源的检索）、百炼 retrieve 结果对热门问题做短时缓存（相同 query 5 分钟内复用）、鉴权结果做用户维度的会话级缓存（同一会话内用户权限不会变化）。


## 薪酬AI质检项目

项目介绍：面向集团薪酬全生命周期（薪资方案 / 提报 / 计算 / 调账 / 报表）的 AI 驱动质检平台，用 LLM 把自然语言批量解析为可执行的 Groovy DSL 规则脚本，配合强弱卡控双链路与异步执行引擎，把传统"人工抽检 + 发薪后修正"升级为"发薪前自动拦截"闭环，一期实现月人工抽检工时下降 ≥50%、质检通过率 ≥95%，规则复用率 ≥ 60%。

技术栈：PandoraBoot、Self-Refine、Groovy、Redis、TDDL、MySQL、Prompt Engineering

- Self-Refine 自洽循环（LLM-as-Generator × 编译器-as-Verifier）：每条规则最多 3 轮"生成 → 预编译校验 → 失败反馈再生成"，错误收敛为调用失败/格式异常/编译失败/超时四态，把幻觉约束在可被编译器证伪的边界内。
- Prompt 工程 SRE 化 + LLM 调用安全护栏：apiKey / model / systemPrompt 全外置配置中心分钟级热更，推送前做长度下限 + 关键词白名单 lint 防空 Prompt 上线；出栈日志三类正则脱敏（API Key 前缀 / 键值对 / Bearer Token）杜绝凭证落盘被检索。
- 异步质检执行引擎（多租户隔离 + 容灾 + 双层进度）：容灾框架跨节点 Failover、同租户分布式锁限并发防打爆分片库；流式分页拉取数据后分批送入规则引擎并桶级并发执行；进度走 Tair-DB 双层，Tair 实时写给前端轮询、每 N 页持久化降 DB 压力，前端封顶 99 防误显 100。
- 推理成本 + 限流双控：规则解析显式关闭深度思考开关省 thinking token；信号量控异步并发、同步路径独立隔离；指数退避（1s/2s/4s）仅在限流类异常重试，业务异常 fail-fast 不浪费配额。
- 多 Agent 协同载体 + 跨仓 DDD 协同：异常分发接入协同表格作为"任务空间"，复制模板后分页写入异常明细，由业务方通过工作流催办跟进；分库分表非分片键查询通过联合索引把候选数据从十万级收敛到万级。



### Self-Refine 自洽循环
Self-Refine 自洽循环（LLM-as-Generator × 编译器-as-Verifier）：每条规则最多 3 轮"生成 → 预编译校验 → 失败反馈再生成"，错误收敛为调用失败/格式异常/编译失败/超时四态，把幻觉约束在可被编译器证伪的边界内。

核心实现在 `RuleBatchParseExecutor.parseSingleInternal()` 中，用 `for (attemptNo = 1; attemptNo <= 3; attemptNo++)` 硬编码 3 轮上限。每轮流程：获取信号量许可 → `buildUserMessage()` 拼装 Prompt（含规则上下文 + 字段字典 + 上一轮错误反馈）→ `callBailianApi()` 调百炼 SDK 非流式接口 → `parseAiResponse()` 校验 JSON 格式（必须含 `parseSuccess`、`content`、`ruleName`、`checkType` 等字段）→ `tryPrecompile()` 调 `GroovyUtils.precompile()` 做 Groovy 预编译语法校验。成功则返回，失败则记录 `lastAiResponse` / `lastErrorType` / `lastErrorMessage`，在下一轮 Prompt 中追加 `[上次尝试反馈]` 区块，让 LLM 看到自己的错误和编译器反馈定向修正。四种错误类型定义在 `ParseErrorType` 枚举中：`AI_CALL`（SDK 调用失败）、`AI_FORMAT`（返回非 JSON 或缺字段）、`GROOVY_COMPILE`（脚本语法错误）、`TIMEOUT`（HTTP 超时 90s）。如果 AI 主动判定不可解析（`parseSuccess=false`），直接跳出循环不浪费重试。字段字典按质检环节分路由：数据提报阶段从 Excel 模板表加载列定义，薪资计算阶段从薪酬项表按子串匹配过滤。AI 返回的 `content` 字段是 Groovy 脚本，Base64 编码后存入 `PrRule.content`。

- Self-Refine 自洽循环：


#### 获取的信号量许可是什么？
信号量（Semaphore）是 Java 并发包中的一种限流工具，可以理解为"停车场的车位"。Semaphore(10) 就是有 10 个车位的停车场。`acquire()` 是"开车进去占一个车位"——如果有空位就直接进入，没有空位就排队等；`release()` 是"开走了腾出一个车位"。在这里，信号量许可控制的是"同时有多少条规则在调用百炼 AI API"。批量解析 50 条规则时，不是 50 条同时发给 AI（会触发 API 限流），而是最多 10 条同时调用，第 11 条必须等前面某条完成释放许可后才能开始。这样既保证了并发效率，又不会超过 AI API 的并发限制。

#### 为什么是非流式接口？
使用非流式接口（`stream(false)`）而非流式接口，原因有三：① **Self-Refine 需要完整响应**——每条规则要经过"生成→Groovy预编译校验→失败则反馈再生成"的循环，必须拿到 AI 的完整 JSON 响应后才能做 Groovy 预编译校验（`tryPrecompile()`），流式接口是逐字返回的，无法在中途做完整性校验；② **qwen3 深度思考兼容**——显式设置 `enable_thinking=false` 后，非流式调用能确保 content 字段一次性返回完整内容，流式模式下可能出现 content 为空只返回 reasoning_content 的问题；③ **超时控制更简单**——非流式调用在 90s 超时后直接抛异常触发 Self-Refine 重试，流式调用的超时处理更复杂。

#### AI判定的是Json格式是否可解析吗？这是AI判断的吗？
不是判断 JSON 格式是否可解析，而是 **AI 判断这条业务规则是否能被转化为 Groovy 脚本**。AI 返回的 JSON 中有一个 parseSuccess 字段，这是 Prompt 中要求 AI 必须输出的字段。当 AI 分析规则后认为该规则无法自动化（比如涉及跨表关联、跨期比对、需要外部数据源等超出 Groovy 脚本能力范围的场景），它会主动返回 `{"parseSuccess": false, "parseErrorMessage": "该规则涉及跨期数据比对，无法转化为单行校验脚本"}`。后端 parseAiResponse() 解析到 parseSuccess=false 后，直接跳出 3 轮重试循环，返回"不可解析"结果给前端——因为这种情况重试也没用，规则本身就超出了自动化能力范围。

#### Base64编码是相当于加密吗？
**不是加密，是编码**。Base64 和加密是两个完全不同的概念：加密需要密钥，没有密钥无法还原；Base64 只是一种编码方式，任何人都可以用 `Base64.getDecoder().decode()` 还原原文，没有任何保密性。这里用 Base64 编码的原因是：Groovy 脚本中可能包含特殊字符（如引号 `"`、换行符 `\n`、花括号 `{}`、大于小于号 `<>` 等），直接存入数据库字段或通过 JSON 传输时可能导致转义问题、SQL 注入风险或 JSON 解析错误。Base64 编码后，脚本内容变成纯粹的字母+数字+少量符号（A-Z, a-z, 0-9, +, /, =）的字符串，可以安全地在 JSON、数据库、HTTP 请求中传输存储。代码注释也说明了："content 是 Base64 编码的 Groovy。下游 F4 入库 / 前端预览据此解码"。



### Prompt 工程 SRE 化
Prompt 工程 SRE 化 + LLM 调用安全护栏：apiKey / model / systemPrompt 全外置配置中心分钟级热更，推送前做长度下限 + 关键词白名单 lint 防空 Prompt 上线；出栈日志三类正则脱敏（API Key 前缀 / 键值对 / Bearer Token）杜绝凭证落盘被检索。


两个 Diamond 配置类分别管理不同阶段：`DiamondRuleGeneratorConfig`（数据提报阶段）和 `DiamondRuleGeneratorRuntimeConfig`（薪资计算阶段，内含 `salaryResultSystemPrompt` 和 `payeeListSystemPrompt` 两个独立 Prompt）。`callBailianApi()` 方法按 stage + ruleType 路由选择对应的 systemPrompt。lint 校验在 Diamond 回调 `received()` 中实现：长度校验——systemPrompt 必须超过 1000 字符（有效 Prompt 含完整规则模板 + 输出契约 + 示例，不可能低于此阈值）；关键词白名单——必须包含"输出契约"、"checkType"、"parseSuccess"等核心关键词，缺少任何一个都会导致解析失败。校验不通过时保留旧值不更新并打印告警。日志脱敏在 `sanitizeForLog()` 中用三类正则处理：`sk-[A-Za-z0-9]{6,}` 匹配百炼 API Key 前缀、`(api[_-]?key)\s*[=:]\s*...` 匹配通用键值对 Key、`(authorization|bearer)\s*[:=]?...` 匹配认证头。脱敏在百炼 API wrapper 层统一处理，对上层透明，保留 Key 前缀用于区分不同 Key。


#### Prompt工程SRE化中的SRE具体是什么意思？
**SRE（Site Reliability Engineering，站点可靠性工程）** 是 Google 提出的工程实践体系，核心思想是**用软件工程的方法来解决运维和可靠性问题**，强调自动化、可观测性、变更安全和故障预防。

在 **“Prompt 工程 SRE 化”** 这个语境下，意思是把 Prompt 管理从“手工编辑、直接上线”的粗放模式，升级为**像管理生产代码一样管理 Prompt**，具体包括：

- **配置外置 + 热更新**：Prompt 不硬编码在代码里，而是放到 Diamond 配置中心，支持分钟级推送无需发版
- **推送前 lint 校验**：类似代码 CI 中的静态检查，推送前做长度下限 + 关键词白名单校验，防止空 Prompt 或恶意内容上线导致全链路故障
- **灰度发布**：先推一台机器观察效果，确认无误再全量推送
- **版本回滚**：每次推送保留历史版本，出问题可一键回退
- **日志脱敏**：出栈日志做三类正则脱敏（API Key / 键值对 / Bearer Token），杜绝凭证落盘被检索
- **可观测性**：Prompt 变更有审计日志，调用有 Trace 追踪

> **一句话总结**：Prompt SRE 化 = 把 Prompt 当作“生产级配置”而非“文案”来管理，用工程化手段保障 Prompt 变更的安全性和可追溯性。


#### 分钟级热更体现在哪方面？
分钟级热更体现在三个方面：① **apiKey 热更**——当百炼 API Key 需要轮换（比如旧 Key 即将过期或被泄露）时，在 Diamond 配置中心修改 apiKey 字段，`@DiamondListener` 回调 `received()` 方法被触发，自动更新 volatile 字段，下一次 AI 调用立即使用新 Key，无需重启服务或发版；② **model 热更**——需要切换模型（如从 qwen-plus 换到 qwen-max）时，修改 model 配置即刻生效；③ **systemPrompt 热更**——这是最频繁需要调整的，优化 Prompt 指令、修改输出契约、增加 Few-shot 示例等，都可以在 Diamond 推送后分钟级生效。"分钟级"指的是 Diamond 配置推送到所有服务节点生效的延迟通常在秒级到分钟级，远快于传统的"改代码→编译→部署"流程（通常需要 30 分钟以上）。同时 `received()` 中的 lint 校验（长度下限 + 关键词白名单）保证了不会误推空 Prompt 或残缺 Prompt 到生产环境。


### 异步质检执行引擎
异步质检执行引擎（多租户隔离 + 容灾 + 双层进度）：容灾框架跨节点 Failover、同租户分布式锁限并发防打爆分片库；流式分页拉取数据后分批送入规则引擎并桶级并发执行；进度走 Tair-DB 双层，Tair 实时写给前端轮询、每 N 页持久化降 DB 压力，前端封顶 99 防误显 100。

核心类 `QualityCheckAsyncTask` 通过 `@Failover(bizType = "QUALITY_CHECK_TASK", runMode = RunMode.NEW_THREAD)` 注解接入容灾框架。任务参数类 `QualityCheckParam` 实现 `ReliableCallParam<String>` + `Serializable`，框架将参数持久化到 DB，节点宕机后其他存活节点自动从 DB 取出参数重新执行。多租户隔离分两层：`TenantContext.call(tenant, ...)` 包裹执行逻辑确保租户信息透传；每租户最多 2 个并发（`MAX_CONCURRENT_PER_TENANT = 2`），通过"DB 查 PROCESSING 任务数 + Redis 分布式锁 `SET NX EX 30min`"双重检查实现。分布式锁使用 Redis SET NX EX 原子加锁，Tair CAD（Compare And Delete）保证只有持锁者能释放。数据处理流程：分页拉取（`PAGE_SIZE = 1000`）→ 分批送入规则引擎（`VALIDATE_BATCH_SIZE = 200`）→ `ValidationOption.setBucketNum(50)` 50 桶并发执行。进度设计：Redis 层每页处理完立即 `SET qc:progress:{taskId} {count} EX 3600`，前端轮询接口优先读 Redis（延迟 <1ms）；DB 层每 10 页持久化一次降写入压力；PROCESSING 状态优先 Redis fallback DB，最终状态以 DB 为准；前端封顶 99 防止进度先于最终状态到 100 造成误导。


#### 分布式锁是什么？项目中是怎么用的？
分布式锁是在分布式系统中保证"同一时刻只有一个节点能执行某段代码"的互斥机制。与单机锁（synchronized/ReentrantLock）不同，分布式锁需要跨进程、跨机器协调，通常基于 Redis/Tair/ZooKeeper 等外部存储实现。项目中薪酬AI质检平台使用 **Redis SET NX + Tair CAD** 组合：加锁用 Redis `SET key value NX EX`（原子性设置+过期时间），释放锁用 Tair CAD（Compare And Delete，只有 value 匹配才删除），防止误删其他持有者的锁。应用场景是同租户限并发——每个租户最多2个质检任务同时运行，通过分布式锁防止同一租户重复提交导致打爆分片库。

#### 流式分页是什么？和普通分页有什么区别？
流式分页是一种**边拉取边处理**的数据读取模式，区别于传统分页的"先查总数→按页码查询→全部加载完再处理"。核心思想是用游标（cursor）或滚动ID代替 OFFSET，每次只拉取一小批数据（如50条），处理完立即拉下一批，内存中始终只保留当前批次。项目中薪酬AI质检平台对 pr_base_data 表（可能十万级数据）采用流式分页：按 id 升序游标翻页，每页50条拉取后立即送入规则引擎桶级并发执行，避免一次性加载全量数据导致 OOM；同时配合 Tair-DB 双层进度条，Tair 实时写前端轮询、每N页持久化到 DB 降低写入压力。相比普通分页的优势：①内存占用恒定不随数据量增长 ②无深分页性能问题（OFFSET越大越慢）③支持中断恢复（记录最后处理的游标即可续跑）。


#### 三种常用缓存读写策略（Tair-DB 双层）
- **旁路缓存模式（Cache Aside Pattern）**：适合读多写少场景，以 DB 为准。读：先查 cache，命中直接返回；未命中则查 DB 并回填 cache。写：先更新 DB，再删除 cache（而非先删缓存再更新DB，避免窗口期内旧数据被重新写入缓存导致不一致）。缺陷是首次请求必穿透、写频繁时缓存命中率下降，可通过预热热点数据或切换 Write Through 缓解。项目中薪酬质检的 Tair-DB 双层进度条即采用此模式：Tair 实时写供前端轮询，每 N 页持久化到 DB 兜底。
- **读写穿透（Read/Write Through Pattern）**：应用只与 cache 交互，cache 服务负责同步读写 DB。读：cache 命中直接返回，未命中则由 cache 从 DB 加载并回填。写：cache 存在则同步更新 cache+DB，不存在则直接写 DB。优点是应用逻辑简单、数据一致性强；缺点是写操作同步落库延迟较高，且仍有首次穿透问题。
- **异步缓存写入（Write Behind Pattern）**：与 Read/Write Through 类似，但写操作只更新 cache，DB 由后台异步批量刷新。优点是 DB 写性能极高；缺点是 cache 宕机可能丢失未刷盘数据，一致性挑战最大。典型应用：消息队列异步刷盘、MySQL InnoDB Buffer Pool。




#### 容灾框架是什么？作用是重试吗？还是？
容灾框架（`com.cainiao.cpo.failover`）是阿里内部的**跨节点任务容灾框架**，作用**不仅是重试，更核心的是跨节点 Failover（故障转移）**。工作原理：① 任务参数类 QualityCheckParam 实现了 ReliableCallParam + Serializable 接口，框架在任务执行前将参数序列化持久化到数据库；② 如果当前执行节点宕机（如 JVM 崩溃、机器掉线），框架的健康检查机制会检测到该节点失联；③ 集群中其他存活节点自动从数据库取出未完成的任务参数，在新节点上重新执行；④ `RunMode.NEW_THREAD` 表示任务在独立线程中执行，隔离对主线程的影响。简单说：普通重试是"同一台机器再试一次"，容灾框架是"这台机器挂了，另一台机器自动接手继续干"，保证即使节点故障，质检任务也不会丢失。

#### 每个租户最多两个并发任务吗？这是考虑了性能？
是的，每个租户最多 2 个并发质检任务。这是从**多个维度**考虑的：① **防止打爆分片库**——质检任务涉及分页拉取 pr_base_data（可能十万级数据）、大量规则引擎校验、批量写入 validation_result，对数据库读写压力极大。一个租户如果不限制，可能连续发起多个质检任务同时跑，瞬间把该租户所在分片库的连接池和 IO 打满；② **多租户公平性**——系统是多租户共享的，一个租户疯狂发任务不应该影响其他租户的正常使用；③ **资源隔离**——2 个并发已经能覆盖正常业务场景（比如一个"数据提报阶段"质检 + 一个"薪资计算阶段"质检同时跑），超过 2 个通常是误操作或异常情况。并发控制通过"DB 查 PROCESSING 状态任务数 + Redis 分布式锁"双重检查实现，既保证准确性又保证性能。

#### Tair CAD（Compare And Delete）具体是如何保证只有持有者才能释放锁的？
分两步理解——**加锁**和**释放锁**：
- **加锁**：使用 `SET lockKey requestId NX EX 1800` 原子命令。NX 表示"仅当 key 不存在时才设置"（防止覆盖他人的锁），EX 1800 表示"30分钟自动过期"（防止死锁）。加锁成功时，锁的 value 是当前线程生成的唯一 requestId（UUID）。
- **释放锁**：使用 Tair 的 `CAD（Compare And Delete）` 原子操作。`cad(lockKey, requestId)` 的语义是："只有当 lockKey 的当前值等于 requestId 时，才删除这个 key"。举个例子：线程 A 加锁时设置 value="uuid-A"，线程 B 加锁时设置 value="uuid-B"。如果线程 A 执行完要释放锁，调用 `cad(lockKey, "uuid-A")`——此时锁的值确实是 "uuid-A"，匹配成功，删除锁，返回 1。但如果线程 A 因为超时，锁已经过期被线程 B 重新获取（value 变成了 "uuid-B"），线程 A 再调用 `cad(lockKey, "uuid-A")` 时，值不匹配，不会删除，返回 0。这就防止了"线程 A 误删线程 B 的锁"的经典分布式锁问题。CAD 是 Tair 提供的原子操作（对比+删除不可分割），比用 Lua 脚本实现的方案性能更好。

#### 为什么既用了Tair，也用了Redis，这两者有什么区别？为什么实现进度条不使用Tair？
**Tair 和 Redis 的关系**：Tair 是阿里基于 Redis 开发的**增强版 Redis**，兼容所有 Redis 原生命令，同时扩展了 Tair 专属命令（如 CAD/CAS/EXSET 等）。在项目中两者共用同一个 JedisCluster 连接实例——代码中 `@Resource JedisCluster jedisCluster` 和 `@Resource TairStringCluster tairStringCluster` 连的是同一套 Tair 集群。所以"用了 Tair 也用了 Redis"不是用了两个不同的系统，而是**对同一套 Tair 集群，分别用了 Redis 原生命令和 Tair 扩展命令**。**具体分工**：
- **Redis 原生命令（jedisCluster）**：用于加锁（`SET NX EX`）和进度条（`setex` / `get` / `del`）——这些是标准 Redis 命令，所有 Redis 实例都支持
- **Tair 扩展命令（tairStringCluster）**：仅用于释放锁（`cad`）——这是 Tair 独有的原子操作，普通 Redis 不支持
- **为什么进度条不用 Tair 专属命令？** 因为进度条只需要简单的 `set/get/del` 操作，这些是 Redis 原生命令完全能胜任的，用 Tair 扩展命令没有任何额外收益。Tair 扩展命令的优势在于原子性的 CAS/CAD 操作，适合分布式锁这类需要"比对+操作一体化"的场景，进度条只是存取一个数字，不需要这种原子保障。

#### 什么叫桶并发执行？依据项目介绍下？为什么要设置为50
"桶并发执行"是薪酬规则引擎内部的并行化机制。具体来说：质检任务分页拉取 1000 条数据后，分成 200 条一批送入规则引擎的 `validateByRules()` 方法。规则引擎内部将这 200 条数据分配到 50 个"桶（bucket）"中，每个桶约 4 条数据，50 个桶**并行执行**规则校验。可以理解为"把一批数据分成 50 份，50 个工人同时干活"。设置为 50 的考虑：① **充分利用多核 CPU**——服务器通常有 16-64 核，50 个并发桶能充分利用多核并行能力；② **平衡吞吐与开销**——桶数太少（如 5 个），并发度不够，200 条数据要串行处理很久；桶数太多（如 500 个），线程上下文切换开销大，反而降低效率；③ **与项目中其他质检任务（CalenderValidationTask）保持一致**，统一配置便于运维和性能调优。

#### 什么叫每页？哪个数据的每页？是质检数据的每页？
是的，"每页"指的是**质检基础数据（pr_base_data）的每页**。质检任务需要遍历所有符合条件的员工薪酬数据进行规则校验，数据量可能达到十万级，不可能一次全部加载到内存。所以采用分页拉取策略：`PAGE_SIZE = 1000`，每次从 DMC 远程服务查询 1000 条 pr_base_data 记录，校验完这 1000 条后再拉下一页的 1000 条，直到最后一页不足 1000 条为止。"每页处理完"就是指每处理完 1000 条数据后，立即更新 Redis 中的进度计数，让前端轮询时能看到实时进展。



### 推理成本 + 限流双控
推理成本 + 限流双控：规则解析显式关闭深度思考开关省 thinking token；信号量控异步并发、同步路径独立隔离；指数退避（1s/2s/4s）仅在限流类异常重试，业务异常 fail-fast 不浪费配额。

qwen3 系列模型默认开启深度思考（Deep Thinking），会在 `reasoning_content` 字段输出推理过程（3-5 倍额外 token），且不显式关闭时 `content` 字段可能为空导致解析失败。通过 `ChatCompletionParam.builder().parameter("enable_thinking", false)` 显式关闭，同时使用非流式调用确保一次性获取完整结果用于预编译校验。并发控制用两个独立信号量：批量解析（F4 功能）`Semaphore(10)` 最多 10 个并发 AI 调用，单条重生成（F5 功能）`Semaphore(1)` 独立许可，隔离原因是防止大批量挤占单条操作——50 条规则批量解析时不应阻塞另一用户的单条重生成。选信号量而非线程池是因为控制的是"同时等待 AI 响应的请求数"（AI API 有并发限制），颗粒度比线程池更精确。指数退避在 `backoffIfRateLimited()` 中实现：`sleepMs = 2^(attemptNo-1) * 1000`（1s / 2s / 4s），仅在 `isRateLimitException()` 检测到错误消息含 "429" / "rate limit" / "throttl" / "too many requests" 时触发，非限流的业务异常直接 fail-fast。HTTP 超时配置：connect 10s / write 30s / read 90s，通过 `ConnectionOptions` 注入 dashscope SDK 的 OkHttp 客户端。

- 信号量：


#### 信号量具体是如何控制并发的？介绍下
信号量（Semaphore）控制并发的原理可以用"游泳池限流"来理解：Semaphore(10) 就是一个只允许同时容纳 10 人的游泳池。具体流程：① 每条规则在调用百炼 AI 之前，先调用 `semaphore.acquire()` ——相当于"在门口取号排队"，如果池子里不满 10 人就直接进入，如果已经满了就阻塞等待；② 调用百炼 AI 完成后（无论成功还是失败），在 finally 块中调用 `semaphore.release()` ——相当于"出来了腾出一个位置"；③ 此时等待队列中的下一条规则被唤醒，进入池子开始调用 AI。选信号量而非线程池的原因是：这里控制的不是"有多少线程在工作"，而是"有多少个 AI API 请求在同时等待响应"。线程可能在做其他事（比如拼装 Prompt、解析 JSON），但只有真正调用 AI API 时才需要限流。信号量的 acquire/release 可以精确地只包裹 API 调用这一段，颗粒度比线程池更细。

#### 补充一个简要的semaphore代码示例
以下是项目中 Semaphore 的核心用法（简化版）：
```java
// 批量解析：Semaphore(10) 控制最多 10 个并发 AI 调用
Semaphore semaphore = new Semaphore(10);

for (BatchParseRuleItemDTO item : rules) {
    CompletableFuture.supplyAsync(() -> {
        // ① acquire：占一个许可位，满 10 人则阻塞等待
        semaphore.acquire();
        try {
            // ② 真正调用百炼 AI API（只有拿到许可才能调用）
            String aiResponse = callBailianApi(userMessage);
            return parseAiResponse(aiResponse);
        } finally {
            // ③ release：释放许可位，让等待队列中的下一个任务进入
            semaphore.release();
        }
    }, threadPool);
}

// 单条重生成：Semaphore(1) 独立许可，防止大批量挤占单条操作
Semaphore singlePermit = new Semaphore(1);
singlePermit.acquire();
try {
    String result = callBailianApi(userMessage);
} finally {
    singlePermit.release();
}
```
**关键设计点**：acquire 在 AI API 调用之前，release 在 finally 中确保异常时也能释放——否则一条规则异常会导致许可永远不归还，后续规则全部阻塞。

#### 这个超时配置是调用百炼API的超时配置吗？还是质检模块前端访问后端的超时配置？
是**调用百炼 AI API 的 HTTP 超时配置**，不是前端访问后端的超时。代码中通过 `ConnectionOptions.builder().connectTimeout(10s).readTimeout(90s).writeTimeout(30s).build()` 注入到 dashscope SDK 内部的 OkHttp 客户端。三个超时分别控制：connectTimeout 10s 是建立 TCP 连接的超时（网络不通时 10 秒快速失败）；writeTimeout 30s 是向百炼 API 发送请求体的超时；readTimeout 90s 是等待百炼 AI 返回响应的超时（AI 生成 Groovy 脚本可能需要 20-30 秒，90s 覆盖正常长尾场景，同时防止真正挂死时无限等待）。代码注释说明："dashscope SDK 默认不设上限，曾出现单条规则 AI 响应卡 2 分钟拖慢整批的情况。90s 既覆盖正常长尾（典型 20-30s），又能在真正挂死时尽快触发 Self-Refine 重试。"


### 多 Agent 协同载体
多 Agent 协同载体 + 跨仓 DDD 协同：异常分发接入协同表格作为"任务空间"，复制模板后分页写入异常明细，由业务方通过工作流催办跟进；分库分表非分片键查询通过联合索引把候选数据从十万级收敛到万级。

异常分发完整流程在 `QualityCheckAiSheetHandler.handle()` 中：工号 → 钉钉 userId → unionId 三级转换 → `createDocumentFromTemplate()` 基于模板创建 AI 表格文档 → `getAllSheets()` 定位"发薪前异常审核-汇总"Sheet → 分页查 `validation_result` 表按 bizId 聚合异常规则 → 分页查 `pr_base_data` 仅保留有异常的记录 → `AiSheetFieldMapper` 映射 15 个字段 → 按 500 条分批 `insertRecords()` 写入（钉钉 API 单次上限）。选协同表格而非邮件/Excel 是因为支持多人实时协作和工作流催办，比静态附件效率高。跨仓架构：三仓（cn-payroll 主仓 / cn-payroll-dmc 数据管理中心 / cn-hrwork 工作台）独立 Git 仓库但同一部署单元。主仓通过 `Class.forName()` 反射加载 DMC 仓的 Handler 类，避免编译期依赖引入大量无关类定义、保持仓间解耦和独立发版节奏。调用链路：前端 → cn-hrwork Controller（HTTP）→ cn-payroll-dmc Gateway Facade（HSF）→ cn-payroll Core Facade（HSF）→ Application Service。分库分表优化：非分片键查询在 SQL 中始终带上分片键（租户 ID）作为第一条件，配合 `(tenant_id, business_field)` 联合索引，每个分片内从全表扫描变为索引查询，候选数据从十万级收敛到万级。


#### DDD（领域驱动设计）是什么？项目中是怎么落地的？
DDD（Domain-Driven Design）是一种以业务领域为核心的软件设计方法论，强调用统一的领域语言（Ubiquitous Language）建模，将复杂业务拆分为限界上下文（Bounded Context），每个上下文内部按分层架构组织。项目中两个 AI 项目均采用 DDD 四层架构：**interfaces 层**（协议适配、HSF/MCP 接口暴露、SchedulerX 任务）→ **application 层**（业务编排、RAG 策略调度、工具路由）→ **domain 层**（纯领域模型、枚举、值对象，无技术依赖）→ **infrastructure 层**（外部依赖封装、百炼API、Redis、DB）。核心收益是各层可独立演进：百炼 API 升级只改 infrastructure，新增 MCP Tool 只加 interfaces 注解，RAG 策略调整只动 application，domain 层始终稳定。采购风控平台的跨多仓改造也基于 DDD：cn-cplm、cn-purchase、rc-kernel 三仓通过 Facade 接口定义跨仓契约，实现风控能力与业务流程解耦。

#### 分库分表是什么？项目中是怎么做的？
分库分表是将单库单表的数据按某个维度（分片键）拆分到多个库/表中，解决单机存储和性能瓶颈。常见策略有哈希取模（均匀但扩容难）和范围分片（扩容易但可能热点）。项目中薪酬AI质检平台的 pr_base_data 表按 **tenant_id（租户ID）** 做分片键，使用 TDDL 中间件自动路由：写入时根据 tenant_id 哈希定位到具体分片，查询时带 tenant_id 条件可直接路由到目标分片。**非分片键查询优化**：当需要按其他字段（如质检批次号）查询时，通过联合索引（tenant_id + batch_no）把候选数据从十万级收敛到万级，避免全分片扫描。分库分表的核心权衡是分片键选择——tenant_id 天然适合多租户隔离场景，保证同租户数据在同一分片内，跨租户查询极少，既满足业务隔离又避免分布式事务。



#### 这里哪里体现了多Agent协同载体？
"多 Agent 协同载体"体现在**钉钉 AI 表格（DingTalk Notable）作为多个角色/Agent 的协作平台**。具体来说：① **质检 Agent**（QualityCheckAiSheetHandler）自动将质检异常数据写入钉钉 AI 表格——基于模板创建文档、映射 15 个字段、按 500 条分批 insertRecords()；② **业务方/HR**通过同一个 AI 表格查看异常明细、标注处理结果、通过工作流催办跟进；③ **审批流 Agent**可以基于表格中的异常数据发起审批流程。AI 表格在这里不只是数据展示工具，而是一个**多方协作的"任务空间"**——质检系统自动产出异常数据 → HR 在同一个文档中审核处理 → 工作流催办确保闭环。选协同表格而非邮件/Excel 附件，正是因为它支持多人实时协作、数据实时同步、可接入工作流自动化，是连接多个 Agent（自动化系统 + 人 + 工作流）的协同载体。

#### 这里是如何进行分库分表的？分片键又是什么？具体是如何将数据从十万级收敛到万级的？通过每个分片内部建立索引吗？没太明白。。。
分三个层次来理解：

- **一、分库分表是什么？**
当单表数据量太大（比如百万级以上），查询性能会急剧下降。分库分表就是把一张大表按某个规则拆成多张小表，分散到多个数据库实例上。比如 pr_base_data 表按"租户 ID"拆分——租户A的数据在分片库1，租户B的数据在分片库2，这样每个分片库只存一部分数据，查询压力分散。

- **二、分片键是什么？**
分片键（Sharding Key）就是决定"这条数据该放在哪个分片库"的字段，在这个项目中分片键是**租户 ID（tenant）**。分库分表中间件（如阿里的 TDDL）根据 tenant 的值做哈希或取模，决定路由到哪个分片库。查询时如果 SQL 中带了分片键，中间件能精确路由到对应的分片库；如果不带分片键，中间件只能**广播查询所有分片库**再合并结果，性能极差。

- **三、如何从十万级收敛到万级？** 分两步收敛：
**第一步——分片路由收敛**：SQL 中始终带上 `WHERE tenant_id = ?` 作为第一条件，分库分表中间件根据 tenant_id 精确路由到该租户所在的分片库（比如从 16 个分片库缩小到 1 个），数据量从全量百万级收敛到单分片的十万级。
**第二步——联合索引收敛**：在每个分片库内部，对 `(tenant_id, business_field)` 建了联合索引。查询 `WHERE tenant_id = ? AND bill_cycle = ? AND data_usage = ?` 时，数据库引擎通过联合索引快速定位到符合条件的记录，而不是扫描分片内的全部十万条数据。经过 tenant + bill_cycle + data_usage + employee_type 等多条件过滤后，候选数据从十万级收敛到万级。
简单总结：**第一步靠分片路由"选对库"，第二步靠联合索引"快速查"**，两步叠加实现了从全量百万级 → 单分片十万级 → 最终万级的三级收敛。



### 面试深挖 Q&A

#### 请介绍一下这个项目，你在其中主要负责什么工作？

A：这是面向集团薪酬全生命周期的 AI 驱动质检平台，核心问题是传统薪酬质检完全依赖人工抽检——业务方在 Excel 中定义几十甚至上百条质检规则，由 HR 逐条核对发薪数据，效率低且容易遗漏，错误往往要等发薪后才发现，修正成本极高。我们的解决方案是用 LLM 把业务人员用自然语言描述的质检规则批量解析为可执行的 Groovy DSL 脚本，配合强弱卡控双链路（强卡控 STRONG 直接阻断发薪流程、弱卡控 WEAK 仅展示警告提示）与异步执行引擎，把"人工抽检 + 发薪后修正"升级为"发薪前自动拦截"闭环。一期覆盖六大质检环节（QualityCheckStepEnum 枚举定义：REPORT 数据提报、CALCULATE 薪资计算、ADJUST 调账、REPORT_FORM 报表、SALARY_PLAN 薪资方案、OTHER 其他），三大规则类型（PrRuleType 枚举定义：EXCEL_VALIDATOR 条件逻辑校验、SALARY_RESULT_VALIDATOR 薪资结果校验、PAYEE_LIST_VALIDATOR 算薪名单校验），规则通过 `mapRuleType(stage, scope)` 方法按质检环节和作用范围自动路由到对应类型。项目上线后实现月人工抽检工时下降 50% 以上，质检通过率 95% 以上，规则复用率 60% 以上。我主要负责五大核心模块：第一是 Self-Refine 自洽循环的设计实现，这是整个 AI 解析链路的核心——LLM 生成 Groovy 脚本后通过 GroovyClassLoader 预编译校验，失败时将编译器错误反馈注入下一轮 Prompt 让 LLM 定向修正，最多 3 轮收敛；第二是异步质检执行引擎，包含多租户隔离（每租户最多 2 个并发任务、TenantContext 透传）、基于 `@Failover` 注解的跨节点容灾、Redis-DB 双层进度追踪；第三是 Prompt 工程 SRE 化，将 apiKey / model / systemPrompt 全部外置到 Diamond 配置中心支持分钟级热更新，同时实现推送前 lint 校验（长度下限 + 关键词白名单）和出栈日志三类正则脱敏（API Key / 键值对 / Bearer Token）；第四是推理成本与限流双控，显式关闭 qwen3 深度思考开关节省 3-5 倍 thinking token，用双信号量隔离批量（Semaphore(10)）和单条（Semaphore(1)）解析的并发，指数退避仅在限流异常触发；第五是协同表格 OpenAPI 集成的异常分发链路，通过钉钉 Notable API 将质检异常写入协同表格供业务方实时协作跟进。项目采用三仓协作架构：cn-payroll 主仓负责质检任务和异步引擎，cn-payroll-dmc 数据管理中心仓负责规则引擎和 AI 解析，cn-hrwork 工作台仓负责网关 Controller 转发。三仓独立 Git 仓库但同一部署单元，主仓通过 `Class.forName()` 反射加载 DMC 仓的 Handler 类保持编译期解耦和独立发版节奏。

#### 做项目中遇到的最大挑战是什么？

A：最大挑战是**异常分发链路中协同表格 OpenAPI 的对接与数据写入**。具体来说有三个难点：
**第一层：钉钉 Notable API 的分批写入与单次上限**。钉钉协同表格的 `insertRecords()` API 单次最多写入 500 条记录，但一次质检任务可能产生数千条异常数据，必须按 500 条分批写入。每批写入前要先通过 `getAllSheets()` 定位目标 Sheet（按名称匹配"发薪前异常审核-汇总"），再调用写入接口。更棘手的是，如果 Sheet 中已有数据（比如上次质检的结果没清掉），需要先清空再写入，但 Notable API 没有"清空 Sheet"的原子操作，只能逐行删除或重建文档——我们选择了基于模板重建文档（`createDocumentFromTemplate()`），每次质检异常分发都创建全新的表格文档，避免历史数据干扰。
**第二层：工号 → 钉钉 userId → unionId 的三级转换链路**。质检异常数据中的员工标识是工号（empNo），但钉钉 Notable API 的文档创建和分享权限需要 unionId。中间必须先调 HCM 主数据服务将工号转为钉钉 userId，再调钉钉开放平台将 userId 转为 unionId。问题在于这两次转换都有一定的失败率（员工未绑定钉钉、数据同步延迟等），且每条异常记录都需要独立转换。我们通过 `parallelStream()` 并发转换 + 失败记录降级处理（跳过该条但不中断整批写入）解决了性能问题，最终写入成功率保持在 98% 以上。
**第三层：跨仓反射加载与 Handler 解耦**。异常分发链路涉及三个仓：cn-payroll 主仓发起质检任务 → cn-payroll-dmc 仓的 `QualityCheckAiSheetHandler` 执行表格写入 → cn-hrwork 仓的 Controller 接收前端请求。DMC 仓的 Handler 不能通过 Maven 依赖直接引入主仓（否则会把 DMC 仓大量无关类定义拉进编译期依赖），所以用了 `Class.forName()` 反射加载。反射加载本身不难，难点在于 Handler 内部依赖的 Spring Bean（如 JedisCluster、TairStringCluster、钉钉客户端）必须在 DMC 仓的 Spring 容器中正确注入，而主仓启动时不会自动初始化 DMC 仓的 Bean——必须确保 DMC 仓的 `@Component` 类被 Spring 扫描到，这涉及到三仓共享部署单元时的 ComponentScan 包路径配置，配置错误会导致反射加载的 Handler 内部 Bean 全部为 null，运行时 NullPointerException。

#### Self-Refine 自洽循环的具体流程是什么？为什么最多 3 轮？

A：每条规则的解析流程是一个最多 3 轮的"生成 → 校验 → 反馈"循环，核心实现在 `RuleBatchParseExecutor.parseSingleInternal()` 中，用 `for (attemptNo = 1; attemptNo <= MAX_ATTEMPTS; attemptNo++)` 硬编码循环，MAX_ATTEMPTS 常量值为 3。每轮流程分五步：第一步获取信号量许可控制并发——批量解析（F4 功能）走 `Semaphore(10)` 最多 10 个并发 AI 调用，单条重生成（F5 功能）走独立的 `Semaphore(1)`，两者隔离防止批量任务挤占单条操作的响应；第二步 `buildUserMessage()` 拼装 User Message，包含规则上下文（质检环节 QualityCheckStepEnum、作用范围 scope、涉及字段列表、卡控规则原文、提醒文案）加上该环节对应的字段字典（数据提报阶段从 Excel 模板表加载列定义，薪资计算阶段从薪酬项表按子串匹配过滤），如果不是第一轮还会追加 `[上次尝试反馈]` 区块注入上一轮的错误信息；第三步 `callBailianApi()` 调用百炼 SDK 的非流式接口，传入 System Prompt（从 Diamond 配置中心按 stage + ruleType 路由选择，数据提报走 DiamondRuleGeneratorConfig，薪资计算走 DiamondRuleGeneratorRuntimeConfig 中的 salaryResultSystemPrompt 或 payeeListSystemPrompt）和拼装好的 User Message，显式设置 `enable_thinking=false` 关闭深度思考；第四步 `parseAiResponse()` 校验返回的 JSON 格式——AI 必须返回包含 `parseSuccess`、`content`（Groovy 脚本 Base64 编码）、`ruleName`、`description`、`checkType`（STRONG/WEAK）、`isGlobal` 等字段的合法 JSON，缺少任何必要字段都归类为 `AI_FORMAT` 错误；第五步 `tryPrecompile()` 调用 `GroovyUtils.precompile()` 用 `GroovyClassLoader.parseClass()` 做 Groovy 预编译语法校验，这一步是"编译器作为 Verifier"的核心——只有通过预编译的脚本才能写入规则库。预编译通过则循环成功返回，任何步骤失败则记录三个状态变量：`lastAiResponse`（AI 原始输出全文）、`lastErrorType`（ParseErrorType 四态之一）、`lastErrorMessage`（具体错误详情如"unexpected token at line 5"），注入到下一轮 Prompt 的 `[上次尝试反馈]` 区块。另有一个短路逻辑：若 AI 主动返回 `parseSuccess=false`（表示 AI 判定该规则无法解析为可执行脚本），直接跳出循环不浪费后续轮次。3 轮上限基于两方面考量：统计数据——第 1 轮成功率约 85%，加上第 2 轮错误修正累计达 95% 以上，第 3 轮边际提升仅 2-3 个百分点；成本分析——每多一轮约增加 2000-3000 个 token（`[上次尝试反馈]` 注入和 AI 重新生成），3 轮以上成功率提升已不足以覆盖额外成本。选择 for 循环而非递归是因为循环更直观、状态变量（lastAiResponse 等）在循环体外声明每轮更新，递归在固定深度场景下无优势。面试中可以延伸对比 Reflexion 模式：Reflexion 有独立的 Reflection 步骤让 LLM 先分析错误原因再修正，我们的设计更简洁——直接把编译器错误作为 Feedback 注入，省去额外的反思调用，在有明确 Verifier（编译器）的场景下效率更高。

#### Self-Refine 中的错误反馈是怎么注入到下一轮 Prompt 的？与普通重试有什么区别？

A：核心区别在于"有状态反馈"vs"无状态重复"。普通重试（如 Spring Retry、Guava Retryer）只是在相同输入上重复执行相同的逻辑，每次调用的参数完全一样，依赖的是"非确定性操作偶然成功"（比如网络波动恢复、限流窗口过去）；而 Self-Refine 的每一轮都会把上一轮的完整 AI 响应和具体错误信息注入到新的 Prompt 中，让 LLM 能看到自己上次生成了什么、错在哪里、编译器给出了什么反馈，从而做定向修正而非随机重试。具体实现在 `buildUserMessage()` 方法中：方法检测 `lastAiResponse` 和 `lastErrorType` 两个参数是否非空，如果是（说明不是第一轮），就在 User Message 末尾追加 `[上次尝试反馈]` 区块。这个区块的结构包含三个子字段：第一个是"上次输出"，放入 AI 上一轮的原始响应全文（包括可能格式错误的 JSON），让 LLM 完整看到自己上次的生成结果；第二个是"错误类型"，放入四态之一（AI_CALL / AI_FORMAT / GROOVY_COMPILE / TIMEOUT），帮助 LLM 判断问题出在哪个层面——比如看到 GROOVY_COMPILE 就知道是自己写的脚本语法有问题，而不是网络或格式问题；第三个是"错误信息"，放入具体的错误详情——最有价值的是 GROOVY_COMPILE 类型的编译错误，包含具体行号、列号和原因描述（如 "unexpected token 'def' at line 5, column 12"），LLM 能据此精确定位并修正语法问题。这种设计不用 Spring Retry 的根本原因是标准重试框架的抽象模型是"调用 → 成功/失败 → 等待 → 再调用"，没有在重试间传递和转化上下文状态的能力。要实现我们的需求，需要在每次失败后提取错误信息、格式化为 Prompt 文本、注入到下一次调用的输入中——这是业务逻辑而非基础设施逻辑，用重试框架的 `@Recover` 方法也无法优雅表达。从 AI 范式角度看，这就是 Self-Refine 论文定义的 Generator-Verifier-Feedback 三元组：LLM 是 Generator 负责生成 Groovy 脚本，Groovy 编译器是 Verifier 负责验证脚本正确性（编译器的判定是确定性的、零成本的，不像人类 review 有主观性），编译器的错误输出是 Feedback 回流到 Generator 驱动下一轮修正。这种"把编译器作为 Verifier"的思路特别适合代码生成场景——编译器是最精确、最低成本的代码正确性验证器。面试中可以延伸对比 Reflexion 模式：Reflexion 有独立的 Reflection 步骤（LLM 先分析错误原因再修正），我们的设计更简洁——直接把编译器错误作为 Feedback 注入，省去额外的反思调用，在有明确 Verifier 的场景下效率更高。

#### 四种错误类型分别是什么？为什么要做这种分类？

A：四种错误类型定义在 `ParseErrorType` 枚举中，按失败阶段从外到内依次是：`AI_CALL`（百炼 SDK 调用层面的失败，包括网络异常、DNS 解析失败、鉴权错误、SDK 内部异常等，意味着请求根本没有到达模型）；`AI_FORMAT`（AI 确实返回了结果，但格式不合预期——可能是返回了纯文本而非 JSON、JSON 缺少必要字段如 parseSuccess 或 content、content 字段为空、Base64 解码失败等，说明 Prompt 的输出契约约束不够严格或模型出现了幻觉）；`GROOVY_COMPILE`（AI 返回的 JSON 格式正确、Groovy 脚本也成功提取，但 `GroovyClassLoader.parseClass()` 预编译失败——语法错误、引用了不存在的类或方法、括号不匹配等，这是最有价值的错误类型因为编译器的错误信息足够精确具体）；`TIMEOUT`（单次 AI 调用超过 HTTP 超时设置 90 秒未返回，通常是因为模型负载过高或规则描述过于复杂导致生成时间过长）。分类设计服务于三个目的：第一是反馈策略差异化——GROOVY_COMPILE 的错误信息（含具体行号、列号和错误原因）对 LLM 最有可操作性，注入下一轮 Prompt 后 LLM 大概率能精确修正；AI_FORMAT 的错误信息也有一定价值（如"缺少 checkType 字段"），LLM 可在下一轮补上；而 AI_CALL 是基础设施问题，把网络错误反馈给 LLM 毫无意义，但仍值得重试因为下次网络可能恢复；TIMEOUT 类似，可能下次模型负载降低就能成功。第二是退避策略差异化——只有 `isRateLimitException()` 检测到限流类异常（错误消息包含 "429" / "rate limit" / "throttl" / "too many requests" 关键词）才触发指数退避（`2^(attemptNo-1) * 1000` 毫秒，即 1s / 2s / 4s），因为限流意味着短时间内再请求一定失败；其他类型的异常直接进入下一轮或 fail-fast，不做无意义等待。第三是可观测性——每种错误类型对应不同的监控指标和告警阈值，AI_CALL 告警阈值最低因为通常意味着大面积故障、GROOVY_COMPILE 比例突然上升可能说明 Prompt 模板被错误推送、TIMEOUT 比例上升提示需要扩容或降低并发。从生产数据看，GROOVY_COMPILE 最常见（约 60%），说明 LLM 在代码生成时的语法准确率还有提升空间；AI_FORMAT 约 20%，主要因为极少数边缘规则描述导致模型偏离 JSON 格式；AI_CALL 和 TIMEOUT 各约 10%。这些分布数据反向驱动 Prompt 优化——比如增加更多 Groovy 语法示例和常见错误避坑指引来降低 GROOVY_COMPILE 比例，增强输出契约中的 JSON Schema 描述来降低 AI_FORMAT 比例。

#### 什么是 QLExpress？什么是 DSL 规则？项目中的规则引擎具体用的是什么？

A：QLExpress 是阿里开源的轻量级业务规则引擎，支持用类 Java 语法编写业务规则脚本，特点是上手门槛低、可动态加载执行，在集团内部很多业务系统中用于营销规则、风控规则等场景。DSL（Domain-Specific Language）是面向特定领域的专用语言，在这个项目中指的是业务人员用自然语言描述的质检规则被 LLM 转换后生成的可执行脚本代码。项目初版设计考虑过 QLExpress，但最终落地使用的是 Groovy 脚本引擎，原因有三方面：第一是 Groovy 语法与 Java 完全兼容，表达能力更强——质检规则可能涉及复杂的日期计算、字符串处理、集合操作，Groovy 可以直接使用所有 Java 类库（如 BigDecimal 精确计算、LocalDate 日期处理），而 QLExpress 的内置函数库有限，遇到复杂逻辑需要扩展操作符比较麻烦；第二是预编译校验能力——`GroovyClassLoader.parseClass()` 提供了完整的语法校验，编译错误信息包含精确的行号、列号和错误原因，这对 Self-Refine 循环中的"编译器验证"环节至关重要（编译器错误信息越精确，LLM 在下一轮修正的成功率越高），QLExpress 的语法检查能力相对弱，错误信息不够精确；第三是 LLM 的生成质量——主流 LLM（如 qwen3）对 Groovy/Java 语法的训练数据远多于 QLExpress 这种小众 DSL，生成正确 Groovy 代码的第一轮成功率（约 85%）显著高于 QLExpress 脚本。代码中 `PrRule.execute()` 按 `language` 字段路由执行，支持三种模式：GROOVY 路径调用 `GroovyUtils.executeScript()` 执行 Groovy 脚本，是 AI 生成规则的标准路径；JAVA 路径通过反射调用 Java 类的方法，用于早期手写的硬编码规则（上线初期业务紧急时直接写 Java 实现，后续逐步迁移到 Groovy）；JPATH 路径用 JsonPath 表达式做简单的 JSON 字段提取和比较，用于最轻量的检查场景（如"字段 A 不为空"、"字段 B 的值在枚举列表中"），这类规则简单到不需要生成 Groovy 脚本。AI 生成的规则统一走 Groovy 路径，因为 Groovy 能表达所有复杂逻辑且有完善的预编译校验。如果面试官问为什么不用 JavaScript（Nashorn/GraalJS），原因是 JVM 上的 JavaScript 引擎性能和调试体验不如 Groovy，Java 互操作性更差（跨语言类型映射复杂），且团队技术栈以 Java 为主，Groovy 的学习成本最低。

#### Groovy 脚本引擎有什么安全风险？你们是怎么防护的？

A：Groovy 最大的安全风险是它可以执行任意 Java 代码——理论上一段恶意脚本可以调用 `Runtime.exec()` 执行系统命令、读写文件系统、通过反射突破沙箱、建立网络连接外传数据，甚至可以通过 `System.exit()` 直接杀死 JVM 进程。在我们的项目中，安全防护依赖三层策略：第一层是 AI 生成端的 Prompt 约束——System Prompt 中通过输出契约严格规定了脚本结构（必须是一个返回 Boolean 的闭包）和可用 API 范围（只允许使用白名单工具类的方法），同时给出了大量正确示例和明确的禁止清单（禁止 Runtime/ProcessBuilder/反射/System.exit 等），LLM 生成的脚本主要是 if/else 条件判断、字段取值比较、日期差值计算等安全操作。第二层是运行时上下文隔离——`GroovyContextEnhancer` 模块控制脚本运行时可以访问的对象白名单，只向脚本上下文注入 `StringUtils`（字符串处理）、`PrDateUtils`（日期计算）、`LineMapUtil`（行数据字段映射）等预定义的工具类，脚本通过这些受控入口访问业务数据，而非直接操作底层系统。注入方式是通过 Groovy 的 `Binding` 对象设置变量名到工具类实例的映射，脚本中只能通过绑定的变量名访问对应的工具类。第三层是编译期的结构校验——`tryPrecompile()` 阶段通过 GroovyClassLoader 编译脚本，虽然不做 AST 级别的安全扫描，但编译过程本身能排除语法错误的脚本。之所以没有用 Groovy 原生的 SecureASTCustomizer 做正式沙箱（它可以配置允许/禁止的语句类型、方法调用、import 白名单等），是因为我们的场景是 AI 生成的脚本而非用户直接输入——攻击面是"AI 意外生成危险代码"而非"用户恶意构造攻击脚本"，Prompt 约束已经足够控制生成内容的安全性，加沙箱的复杂度（维护允许/禁止的 AST 节点白名单，且可能误拦正常的业务逻辑）带来的收益有限。此外，`GroovyUtils` 使用 Caffeine 缓存（容量 1024）按脚本内容的 MD5 缓存编译后的 `ScriptingFunction`，避免重复编译。这个缓存设计还解决了一个重要的安全隐患：Groovy 每次编译都会通过 GroovyClassLoader 创建新的 Class 对象，这些 Class 持有对 ClassLoader 的引用导致 MetaClass 元数据无法被 GC 回收，大量编译会导致 Metaspace 内存泄漏（`OutOfMemoryError: Metaspace`）。通过 MD5 去重 + Caffeine 缓存，同一脚本内容只编译一次，从根本上避免了这个问题。

#### 异步质检执行引擎的多租户隔离是怎么实现的？

A：多租户隔离分为两层，从逻辑隔离到资源隔离逐层加固。第一层是租户上下文传递：通过 `TenantContext.call(tenant, () -> { ... })` 包裹整个执行逻辑，这个方法会在当前线程的 ThreadLocal 中设置租户标识，确保后续所有操作（包括 HSF 跨进程调用）都能自动透传租户信息。在分库分表场景下，TDDL 的路由规则会读取 TenantContext 中的租户 ID 来选择对应的分库分片，所有 SQL 查询都会带上租户过滤条件，天然实现数据隔离。当使用 `@Failover` 容灾框架在新线程中执行任务时，需要特别注意 ThreadLocal 的传递——QualityCheckParam 中序列化存储了 tenantId，在新线程启动时通过 `TenantContext.call()` 重新设置，避免 ThreadLocal 丢失导致跨租户数据泄露或路由到错误的分库。第二层是租户级并发控制：每个租户最多同时执行 2 个质检任务（`MAX_CONCURRENT_PER_TENANT = 2`）。获取并发许可的逻辑是"双重检查"——第一步查 DB 中该租户 PROCESSING 状态的任务数，如果已达上限直接返回错误码，前端提示"当前有正在执行的质检任务，请稍候"；第二步如果 DB 检查通过，尝试获取 Redis 分布式锁（`SET NX EX 30min`），锁的 key 格式包含 tenantId 和 taskId 保证任务级唯一。之所以需要双重检查而非只用 Redis 锁，是因为 DB 查询能感知到历史状态（包括 Failover 重新执行的任务），而 Redis 锁只反映当前时刻的加锁状态——如果只依赖 Redis 锁，当 Redis 发生主从切换丢失锁数据时，可能允许超出限制的并发任务启动。设置每租户 2 个并发上限的原因是：质检任务涉及大量分页查询（PAGE_SIZE=1000）和规则执行（VALIDATE_BATCH_SIZE=200、bucketNum=50），单个任务已经会产生显著的数据库读压力，不限制并发的话单租户的多个任务会打爆该租户所在的分库分片，影响同库其他租户的正常业务——薪酬系统是多租户共享分库分片的架构，一个租户的异常负载会级联影响其他租户。为什么是 2 而不是 1 或 3？1 个太严格，用户可能需要同时运行数据提报和薪资计算两个环节的质检；3 个在压测中观察到分片 DB 的 CPU 利用率超过 70% 阈值，存在影响在线业务的风险。如果面试官问"超出限制时用户怎么办"——前端会显示友好提示，告知当前有任务正在执行，可以等待完成或手动取消后重新发起。

#### 跨节点 Failover 容灾框架是怎么工作的？

A：使用了公司内部的 `cpo.failover` 框架，通过 `@Failover` 注解声明在 `QualityCheckAsyncTask` 类上。核心机制分为三个阶段：任务提交阶段——任务参数类 `QualityCheckParam` 实现 `ReliableCallParam<String>` 和 `Serializable` 两个接口，前者定义了 `getKey()` 方法返回任务唯一标识（taskId），后者确保参数可序列化。框架在接收到任务调用时，先将序列化后的参数（包含 taskId、tenantId、qualityCheckStepEnum、bizId 等关键字段）持久化到 DB，同时记录执行节点的 IP 和时间戳。任务执行阶段——通过 `RunMode.NEW_THREAD` 配置在新线程中执行，与调用方线程解耦，调用方提交后立即返回不阻塞。选择 NEW_THREAD 而非 CALLER_THREAD 是因为质检任务可能运行数十分钟（取决于数据量和规则数），在调用方线程中执行会阻塞 HSF 线程池，影响其他 RPC 请求的处理。容灾恢复阶段——框架通过心跳机制感知节点存活状态，如果执行节点超过一定时间没有更新心跳（通常是节点宕机或 FullGC 假死），框架判定该节点失活，由集群中其他存活节点的 Failover 调度器从 DB 取出未完成的任务参数，反序列化后重新执行。这解决了长时间运行的质检任务在节点宕机时中断无法恢复的问题——没有容灾的话，一个运行了 20 分钟的任务因宕机中断，用户需要手动重新触发并等待再次执行。Failover 恢复与分布式锁的协同设计：原节点宕机后其持有的 Redis 锁会因 EX 过期自动释放（锁的 TTL 设置为 30 分钟），新节点重新执行时会获取新的锁。锁的续约通过 Tair CAS（Compare And Swap）原子操作实现——每隔一段时间（通常是 TTL 的 1/3）更新锁的过期时间，确保长时间运行的任务不会因锁过期被误释放。释放锁使用 Tair CAD（Compare And Delete），只有 lockValue 匹配的持锁者才能释放，防止因锁过期后被其他节点获取导致的误删。如果面试官问"原节点恢复后会不会重复执行"——框架通过 DB 状态标记保证幂等性，任务被新节点接管后状态更新为"已转移"，原节点恢复后发现状态已变更不会重复执行。进度恢复方面，Failover 后新节点从 DB 中读取最近一次持久化的进度断点（每 10 页持久化一次），从断点位置继续执行而非从头开始，最多损失 10 页数据的重复处理，对最终结果无影响（规则校验是幂等操作）。

#### Redis-DB 双层进度的设计思路是什么？为什么不全用 Redis 或全用 DB？

A：双层设计的核心考量是"实时性 vs 持久性"的平衡，同时兼顾写入频率与系统压力。Redis 层承担实时进度的高频写入和快速读取：每页数据（PAGE_SIZE=1000）处理完后立即写入 Redis（`SET qc:progress:{taskId} {count} EX 3600`），key 按 taskId 唯一标识，value 是已处理的记录数，TTL 1 小时自动过期避免残留数据。前端通过轮询接口读取进度时优先查 Redis，响应延迟 < 1ms，对用户来说进度条几乎是实时更新的。DB 层承担进度的持久化和最终一致性保障：每处理 10 页（即 10000 条记录）才持久化一次，将当前处理进度写入质检任务表的 progress 字段。10 页这个阈值是在写入频率和数据安全之间的折中——假设每页处理耗时约 2 秒，10 页即 20 秒持久化一次，即使 Redis 故障最多丢失 20 秒的进度，Failover 重新执行时从 DB 记录的断点继续即可。全用 Redis 的问题有三个：一是 Redis 故障（如主从切换、内存淘汰）后进度数据丢失，任务重新执行时不知道从哪里继续，只能从头开始浪费大量计算；二是没有事务保障，任务状态（PROCESSING → SUCCESS/FAILED）和进度可能出现不一致；三是 Redis 数据不可追溯，无法事后分析任务执行的历史进度变化。全用 DB 的问题也有三个：一是写入频率过高——每页写一次 DB 意味着一个万级数据的质检任务会产生几十次 UPDATE，在分库分表场景下每次 UPDATE 需要先路由到正确分片再执行，高频更新对数据库压力大；二是查询延迟——数据库的 SELECT 延迟通常在 5-10ms，前端每秒轮询 2 次的话每次都打 DB 既浪费连接资源又造成可感知的卡顿；三是对在线核心业务的干扰——质检任务和在线薪酬业务共享分库分片，高频写进度会影响正常业务的 SQL 执行延迟。查询优先级设计：PROCESSING 状态的任务优先读 Redis（拿到实时进度），Redis 缺失时 fallback 到 DB（拿到最近一次持久化的进度）；最终状态（SUCCESS/FAILED）以 DB 为准，因为 Redis 的进度只是过程数据、DB 的状态才是权威数据。前端额外做了封顶 99 的处理——即使 Redis 进度计数已经到达总数据量（所有页处理完），前端也只显示 99%，直到 DB 的任务状态字段更新为 SUCCESS 后才切换到 100% 完成态并展示结果。这是防止"进度条已到 100% 但还没有结果"的用户困惑，因为 Redis 进度先于 DB 最终状态到达可能有几秒的时间差（最后一页处理完后还需要做结果汇总、状态更新等收尾操作）。

#### Prompt 工程 SRE 化中，推送前的 lint 校验具体做了什么？为什么需要这一步？

A：lint 校验在 Diamond 配置回调 `received()` 方法中实现，是配置推送到达应用实例后、实际更新内存中 Prompt 变量之前的一道守门检查。具体做了两项检查：第一项是长度校验——systemPrompt 必须超过 1000 字符。这个阈值不是随意设定的，一个有效的规则解析 Prompt 至少包含以下组成部分：角色定义（约 100 字）、任务描述（约 200 字）、输出契约（定义 AI 必须返回的 JSON 结构，包含 parseSuccess / content / ruleName / checkType / description / isGlobal 等字段说明，约 300 字）、Groovy 脚本模板和示例（约 200 字）、字段引用说明和约束（约 200 字），加起来远超 1000 字符，低于这个阈值说明推送了空内容、残缺配置或完全错误的文本。第二项是关键词白名单校验——推送的 Prompt 必须包含"输出契约"、"checkType"、"parseSuccess"等核心关键词。这些关键词对应 Prompt 中定义 AI 输出格式的关键约定：缺少"输出契约"说明 JSON 格式定义部分被删除、缺少"checkType"会导致 AI 不输出强弱卡控类型使得 `parseAiResponse()` 解析时找不到该字段报 AI_FORMAT 错误、缺少"parseSuccess"会导致 AI 不输出解析成功标志使得系统无法判断 AI 是否认为规则可解析。两项校验任一不通过时保留旧值不更新（`volatile` 静态变量不赋新值，继续使用上一个版本的 Prompt），同时打印 WARN 级别告警日志，日志中包含推送内容的前 200 字方便快速定位问题原因。这一步的必要性来自实际运维经验：Prompt 是通过 Diamond 配置中心人工编辑推送的，存在多种操作风险——手误清空内容、粘贴时没有全选导致只粘贴了一半、从其他格式（如 Word、Confluence）粘贴时引入不可见字符导致 JSON 模板损坏、多人同时编辑覆盖彼此的修改等。没有 lint 兜底的话，一次错误推送会导致线上所有新的规则解析请求因 Prompt 缺陷而批量失败，影响面等同于 P0 故障——且这种故障很隐蔽，从 AI 调用的报错信息很难直接定位到"是 Prompt 被改坏了"。有了 lint 之后，错误推送被拦截在生效前，旧的正确 Prompt 继续生效，运维人员看到告警后修正再推送即可，对线上零影响。项目的两个 Diamond 配置类（DiamondRuleGeneratorConfig 管理数据提报阶段的 Prompt、DiamondRuleGeneratorRuntimeConfig 管理薪资计算阶段的 salaryResultSystemPrompt 和 payeeListSystemPrompt）各自独立做 lint 校验，确保每个 Prompt 模板都受保护。

#### 日志脱敏的三类正则具体是什么？为什么要做出栈日志脱敏？

A：三类正则定义在 `sanitizeForLog()` 方法中，按匹配的凭证类型分类：第一类匹配百炼 API Key 前缀 `sk-[A-Za-z0-9]{6,}`，替换为 `sk-***`，保留前缀 `sk-` 是为了在脱敏后的日志中仍能区分不同用途的 Key（比如不同环境或不同模型的 Key 前缀可能不同，保留前缀有助于问题定位）；第二类匹配通用键值对形式的 API Key `(api[_-]?key)\s*[=:]\s*[A-Za-z0-9_\-]{6,}`，替换为 `$1=***`，使用 `$1` 捕获组保留 key 名称（如 `api_key` 或 `apiKey`），只脱敏值部分，这样日志中仍能看到是哪个配置项、只是值被隐藏了；第三类匹配 Authorization/Bearer Token `(authorization|bearer)\s*[:=]?\s*[A-Za-z0-9_\-\.]{8,}`，替换为 `$1=***`，覆盖 HTTP Authorization 头和 Bearer Token 两种常见的认证信息格式。三类正则的设计是逐步积累的——第一类覆盖百炼 SDK 特有的 Key 格式，第二类覆盖通用的 API Key 配置项，第三类覆盖 HTTP 认证头，三者组合基本覆盖了 AI 调用链路中可能泄露的所有凭证类型。做出栈日志脱敏的背景是：AI 调用的请求和响应日志中必然携带 API Key（在请求头或 URL 参数中）和认证信息，这些日志会被收集到公司的日志服务（SLS），并离线归档到 ODPS 数据仓库，被索引为全文搜索。这意味着如果不脱敏，任何有日志查看权限的员工都能通过搜索 "sk-" 前缀直接获取百炼 API Key——在大公司内部，日志权限通常比 API Key 权限宽泛得多（日志权限按应用维度授权，API Key 按团队维度管控），这就形成了权限绕过。更严重的是，日志数据通常会保留数月甚至数年，一旦凭证落盘就很难彻底清除——即使事后轮换了 Key，历史日志中的旧 Key 如果还有效就仍然是风险点。脱敏的注入点选择在百炼 API 的 wrapper 层（调用百炼 SDK 前后的日志记录方法），对上层业务代码完全透明——业务代码正常使用完整 Key 进行 API 调用，只有写日志时才触发脱敏。这种设计保证了脱敏逻辑的集中管理（只需维护一处正则），且不影响运行时功能。正则的编写有一个注意点：不能过于宽泛否则会误脱敏正常的业务数据（如 Base64 编码的 Groovy 脚本 content 字段），也不能过于狭窄否则新增凭证类型时需要补充正则。

#### 深度思考开关是什么？为什么要显式关闭？

A：深度思考（Deep Thinking）是 qwen3 系列模型的特性，等同于 OpenAI o1/o3 的 Chain-of-Thought 推理模式。开启后模型会先在 `reasoning_content` 字段输出详细的推理过程（类似人类的"先想后答"），然后在 `content` 字段输出最终结果。qwen3 系列的一个关键行为差异是：深度思考默认开启，不显式关闭时 SDK 返回的 `content` 字段可能为空——所有推理内容都在 `reasoning_content` 中，而我们的 `parseAiResponse()` 解析逻辑只读取 `content` 字段，这会直接导致解析失败被归类为 AI_FORMAT 错误。显式关闭的方式是 `ChatCompletionParam.builder().parameter("enable_thinking", false)`，通过 dashscope SDK 的 parameter 扩展字段传入。关闭深度思考的核心考量有三个：第一是 Token 成本——深度思考的 reasoning_content 通常是正式输出的 3-5 倍 token 量。以一条中等复杂度的质检规则为例，正式输出（JSON + Groovy 脚本）约 500-800 token，reasoning_content 可能达到 1500-3000 token。在批量解析 50 条规则、每条可能重试 3 轮的场景下，开启深度思考会额外消耗大量 token，成本增加数倍。规则解析是高频操作（业务方经常调整规则后重新批量生成），Token 成本持续累积影响项目整体预算。第二是 API 行为的确定性——关闭深度思考后 `content` 字段保证有值，`reasoning_content` 字段为空，解析逻辑简单可靠；开启深度思考时两个字段的内容分布在不同模型版本和不同推理复杂度下可能有差异，增加了不确定性和维护成本。第三是延迟——深度思考会增加模型的推理时间（需要先生成 reasoning 再生成 content），在已有 90 秒 HTTP 超时的约束下，延迟增加可能导致更多 TIMEOUT 错误，影响 Self-Refine 循环的整体耗时。同时使用非流式调用（`stream(false)`）的原因是：规则解析需要一次性获取完整的 JSON 结果用于后续的 `parseAiResponse()` 格式校验和 `tryPrecompile()` 预编译——流式调用返回的每一帧是增量文本片段，需要在客户端拼接完成后才能做 JSON 解析和 Groovy 编译，增加了额外的拼接逻辑复杂度且没有用户体验上的收益（解析过程是后台异步执行的，用户不需要看到逐字输出）。如果面试官问"有没有考虑用深度思考来提高解析质量"——实测对比过，对于规则解析这种结构化输出任务，关闭深度思考的一轮生成质量（成功率 ~85%）和开启后的差异不大（~88%），3% 的提升不足以覆盖 3-5 倍的 Token 成本，且 Self-Refine 循环已经能将最终成功率提升到 97% 以上。

#### 信号量并发控制为什么要区分批量和单条两个独立许可？

A：批量解析（F4 功能，一次解析整个 Excel 中的所有规则，可能有 50+ 条）使用 `Semaphore(10)` 控制最多 10 个并发 AI 调用；单条重生成（F5 功能，用户对单条规则不满意重新生成）使用独立的 `Semaphore(1)`。两者用独立信号量隔离，核心原因是防止"大批量挤占小操作"：假设共用一个 `Semaphore(10)`，当用户 A 触发 F4 批量解析 50 条规则时，10 个许可会被前 10 条规则的并发调用全部占满，此时用户 B 想 F5 重新生成单条规则就必须排队等待，直到批量解析的某条规则完成释放许可。50 条规则在 10 并发下需要 5 轮才能完成（每轮最多 3 次 Self-Refine 重试），用户 B 可能要等待数十秒甚至数分钟，体验极差。独立信号量保证 F5 单条操作有自己的专属许可（`Semaphore(1)` 意味着同一时刻最多 1 个单条重生成请求在等待 AI 响应），不受批量任务影响，实现即时响应。`Semaphore(1)` 只允许 1 个并发而非更多，是因为 F5 是用户手动触发的低频操作，同一用户不太可能同时重生成多条规则，1 个许可够用且避免了单条操作过多消耗 AI API 配额。选择信号量而非线程池来控制并发的原因是语义精确度：我们要控制的是"同时等待 AI 响应的请求数"（百炼 API 有账户级别的并发限制，超出会返回 429 限流），而非"正在执行代码的线程数"。一个 AI 调用的生命周期是：获取信号量许可 → 发 HTTP 请求 → 等待 AI 响应（数秒到数十秒的 IO 等待）→ 解析响应 → 释放许可。真正占用"并发配额"的是 IO 等待阶段，线程池的大小控制的是"有多少线程在运行"而非"有多少请求在等待 IO"，两者不等价——比如线程池大小为 10 但每个线程内部又异步提交了 AI 请求，实际并发可能超过 10。信号量直接控制"持有许可的数量"，精确对应 AI API 的并发限制。许可获取使用 `tryAcquire(timeout)` 带超时而非阻塞的 `acquire()`，超时后抛出异常通知调用方"系统繁忙请稍后重试"，避免无限等待。信号量使用非公平模式（默认），因为规则解析请求没有严格的先后顺序要求，非公平模式的吞吐量优于公平模式（减少了唤醒线程的上下文切换开销）。

#### 协同表格 OpenAPI 是怎么用的？完整的异常分发流程是什么？

A：异常分发的完整流程在 `QualityCheckAiSheetHandler.handle()` 中实现，是质检任务执行完成后将异常结果推送给业务方跟进的链路。第一步是身份转换：将操作人的工号转换为钉钉 unionId，经过工号 → 钉钉 userId → unionId 的三级转换（工号到 userId 通过内部花名册 API、userId 到 unionId 通过钉钉开放平台 API）。之所以需要 unionId 而非直接用工号，是因为钉钉 Notable API 的文档权限体系基于 unionId 标识用户，工号是企业内部标识在钉钉体系中无法识别。第二步创建文档：调用钉钉 Notable API `createDocumentFromTemplate()` 基于预设模板创建新的 AI 表格文档。模板中预先定义了表头结构（工资月份、所属仓、姓名、工号、异常规则名称、异常详情等 15 个字段）、条件格式（如异常等级用不同颜色标记）和工作流催办配置，创建后新文档自动继承模板的所有格式和配置。第三步定位 Sheet：`getAllSheets()` 获取新文档中所有 Sheet，通过名称匹配找到"发薪前异常审核-汇总"表的 sheetId——模板中可能有多个 Sheet（如汇总表、明细表、说明表），需要精确定位数据写入的目标 Sheet。第四步聚合异常数据：分页查询质检校验结果表 `validation_result`，按员工 bizId 聚合异常规则——一个员工可能触发多条规则的异常，需要合并展示为一行记录，异常规则名称和详情用分隔符连接。第五步过滤有效记录：分页查询基础数据表 `pr_base_data`，仅保留存在异常的员工记录，过滤掉全部通过质检的数据。第六步字段映射：通过 `AiSheetFieldMapper` 将 Java 对象的 15 个字段映射为 AI 表格的记录格式，字段映射包含数据类型转换（如日期格式化为"yyyy-MM"、金额保留两位小数、枚举值到中文的转换如 STRONG→"强卡控"）。第七步分批写入：按 500 条分批调用 `insertRecords()` 写入 AI 表格，500 是钉钉 Notable API 的单次 insert 上限，超过会返回参数错误。写入完成后，业务方会收到钉钉工作流通知，点击即可打开协同表格开始处理。选择协同表格而非邮件/Excel 附件的原因有三：一是协同表格支持多人实时编辑，HRBP 可以直接在表格中标记处理状态、添加备注、@相关人员；二是内置工作流催办能力，超时未处理的记录自动发送钉钉消息提醒；三是数据可追加更新——如果后续补充了新的质检结果，可以追加写入同一张表而非重新发送。如果面试官问"写入失败怎么办"——每一步都有异常捕获和日志记录，createDocumentFromTemplate 失败会重试 1 次，insertRecords 单批失败会记录失败批次的起止行号到日志供人工补录，不影响其他批次的写入。

#### 跨仓 Facade 反射加载是怎么做的？为什么不直接 Maven 依赖？

A：在 `QualityCheckServiceImpl.loadExportHandlerClass()` 中，通过 `Class.forName("com.cainiao.cn.payroll.dmc.xxx.QualityCheckAiSheetHandler")` 反射加载 DMC 仓的 Handler 类。三个仓虽然是独立的 Git 仓库和 Maven 模块，但运行时部署在同一个 JVM 进程中（同一部署单元），所有仓的 class 文件最终被打包到同一个 fatjar 中，共享同一个 ClassLoader，所以 `Class.forName()` 能找到 DMC 仓的类。具体反射调用方式是 `Class.forName()` 获取 Class 对象 → `getConstructor()` 获取构造器 → `newInstance()` 创建实例 → 转型为公共接口类型调用方法。之所以不直接 Maven 依赖（即 cn-payroll-core 不通过 `<dependency>` 引入 cn-payroll-dmc-core），原因有四个层面：第一是编译期解耦——DMC 仓包含大量的 DO 对象（数据库实体类）、MyBatis Mapper 接口、规则引擎逻辑、AI 调用组件（百炼 SDK 集成、Prompt 管理等），如果主仓编译期依赖 DMC-core 模块，就会引入上百个无关的类定义，增加编译时间和 IDE 的索引负担。第二是发版节奏独立——三个仓的开发节奏不同，DMC 仓可能因为 AI 解析逻辑优化（如 Prompt 调优、新增规则类型支持）频繁发版，如果主仓有编译期依赖，DMC 的 API 变更会导致主仓编译失败，必须同步修改同步发版，在多团队协作中这种同步协调的沟通成本很高。反射加载只依赖类名字符串和公共接口，DMC 内部实现随意调整不影响主仓编译。第三是循环依赖避免——主仓和 DMC 仓之间有双向调用需求（主仓调 DMC 的 Handler 做异常分发、DMC 调主仓的 Service 查询任务状态），Maven 不允许循环依赖，虽然可以抽出 -api 模块解耦但增加了模块管理复杂度。反射加载天然无编译期依赖，绕过循环依赖问题。第四是 DDD 分层合理性——在 DDD 视角下，三个仓分别对应不同的限界上下文，上下文之间应通过防腐层或接口交互而非直接类引用。反射加载的代价是失去了编译期类型检查——如果 DMC 仓重命名了 Handler 类但没有同步修改主仓的反射字符串，运行时会报 ClassNotFoundException。为此做了两重保障：一是启动时的 smoke test 验证反射类可加载，应用启动阶段失败直接拒绝部署、发布流程自动回滚；二是集成测试覆盖完整的跨仓调用链路。完整调用链路是：前端 → cn-hrwork Controller（HTTP）→ cn-payroll-dmc Gateway Facade（HSF RPC）→ cn-payroll Core Facade（HSF RPC）→ Application Service。

#### 分库分表场景下，非分片键查询是怎么优化的？

A：分库分表后，TDDL 中间件的路由规则基于分片键（通常是租户 ID 或 tenant_id + 业务 ID 的组合）决定数据存储在哪个分库的哪个分表中。如果查询条件不包含分片键，TDDL 会将 SQL 广播到所有分库的所有分表上执行然后合并结果——这就是全表扫描，在分库分表规模较大时（如 16 库 x 64 表 = 1024 个分片）性能极差。在质检场景中，某些查询需要按非分片键字段（如工资月份 salary_month、质检环节 check_step、任务 ID task_id）过滤数据，优化策略的核心原则是"始终在 SQL 的 WHERE 条件中带上分片键作为第一个条件"。具体做法：查询 SQL 写成 `WHERE tenant_id = ? AND salary_month = ? AND check_step = ?` 而非 `WHERE salary_month = ? AND check_step = ?`。有了 tenant_id 后 TDDL 能定位到具体的分库（按租户分库），虽然仍需扫描该分库内的所有分表（因为分表键可能不是 tenant_id），但候选分片数从全部 1024 大幅收敛。在每个分片内，配合 `(tenant_id, salary_month)` 或 `(tenant_id, check_step)` 联合索引，查询从全表扫描变为索引查询，把每分片的候选数据从十万级收敛到千级甚至百级。联合索引设计的关键是把分片键 tenant_id 放在最左列——即使分片键已经用于路由选库，在分片内的 SQL 执行中它仍然是索引的组成部分，能有效利用 B+ 树索引的最左前缀匹配原则。数据处理采用流式分页而非传统的 offset 分页：每页 `PAGE_SIZE=1000`，使用"上一页最后一条记录的主键 ID"作为下一页的起始条件（`WHERE id > #{lastId} ORDER BY id LIMIT 1000`），避免了 `OFFSET N LIMIT M` 在 N 很大时需要扫描并跳过前 N 行的深分页性能问题。每页 1000 条数据再分批送入规则引擎（`VALIDATE_BATCH_SIZE=200`），配合 `ValidationOption.setBucketNum(50)` 设置 50 个桶并发执行。50 个桶的含义是规则引擎内部将 200 条数据按 hash 分到 50 个桶中并发校验，每个桶内串行执行所有规则——这种设计比"每条规则并发"更高效，因为同一个桶内的数据共享规则上下文（如字段字典映射、GroovyContextEnhancer 注入的工具类实例），减少了重复的上下文初始化。分页查询与分批校验的组合保证了内存可控——任何时刻 JVM 堆中最多只有 1000 条待处理数据 + 200 条正在校验的数据，不会因大数据量导致 OOM。

#### 强弱卡控在技术上是怎么区分的？业务上分别适用什么场景？

A：技术上通过规则元数据中的 `checkType` 字段区分，取值由常量定义为 `STRONG`（强卡控）或 `WEAK`（弱卡控）。AI 生成规则时会根据 Prompt 中的输出契约和规则原文语义判断卡控类型并写入返回 JSON 的 checkType 字段——System Prompt 中对两种类型的语义做了明确定义和示例：强卡控用于"违反后必须修正才能继续的硬约束"，弱卡控用于"需要提醒但可以选择忽略的软提示"。AI 生成后用户也可以在规则编辑界面手动调整 checkType，覆盖 AI 的判定——因为有些规则的卡控强度取决于具体的业务上下文（比如同一条"金额超过阈值"规则在关键岗位可能是强卡控、在普通岗位可能是弱卡控），AI 无法完全感知这些上下文，人工校准保证了灵活性。执行时，校验框架在 `PrRule.execute()` 返回校验结果后，根据 `checkType` 决定后续行为：强卡控校验失败直接阻断流程，通过 HSF 返回阻断标识给前端，前端展示红色错误提示并将提交按钮置灰，用户必须修正数据使其通过所有强卡控规则后才能继续提交；弱卡控校验失败通过 HSF 返回提醒标识，前端展示黄色警告弹窗，列出所有未通过的弱卡控规则和对应的提醒文案，用户可以选择"返回修改"修正数据，也可以选择"确认继续"跳过警告直接提交。弱卡控的"确认继续"操作会记录审计日志（包含操作人、操作时间、被忽略的规则列表），便于事后追溯和合规审查。业务上，强卡控适用于合规性硬约束：比如"基本工资不能为负数"——负工资在任何场景下都是错误的；"社保基数不能低于当地最低标准"——低于最低标准违反劳动法规；"应发合计与各项明细之和不一致"——数值不平衡说明计算逻辑有 bug。这类规则违反后如果放行，轻则导致发薪错误需要退回重算（高成本且严重影响员工体验），重则引发法律纠纷和合规风险。弱卡控适用于经验性规则或异常提示：比如"绩效系数超过历史平均值 200%，请确认"——可能是正常的晋升调薪或超额完成目标；"本月调薪人数异常偏多"——可能是年度普调或组织架构调整；"新入职员工的工资低于 offer 约定"——可能是试用期折扣。这类情况可能是合理的业务操作，强制阻断会导致正常流程无法进行。从生产数据看，强卡控规则约占总规则数的 30%，弱卡控约占 70%；命中率方面强卡控因为阈值明确命中率较低（约 2-3%），弱卡控因为是经验判断命中率较高（约 15-20%），这个分布也验证了设计的合理性。

#### 自然语言到 Groovy 脚本的转换具体是怎么做的？Prompt 里包含哪些关键信息？

A：自然语言到 Groovy 脚本的转换是整个 AI 质检平台的核心能力，由 `RuleBatchParseExecutor` 驱动，System Prompt 和 User Message 各承担不同职责。System Prompt 通过 Diamond 配置中心外置管理（按质检环节和规则类型路由选择：数据提报阶段走 DiamondRuleGeneratorConfig 中的 systemPrompt，薪资计算阶段走 DiamondRuleGeneratorRuntimeConfig 中的 salaryResultSystemPrompt 或 payeeListSystemPrompt），内容主要包含五个部分：第一是角色定义，声明 AI 是一个"薪酬质检规则脚本生成器"，明确任务边界——只负责将自然语言规则转换为 Groovy 脚本，不做额外分析或解释；第二是输出契约，严格定义 AI 必须返回的 JSON 结构，包含 parseSuccess（是否成功解析，Boolean 类型）、content（Groovy 脚本的 Base64 编码，String 类型）、ruleName（规则名称）、description（规则描述）、checkType（STRONG/WEAK 强弱卡控类型）、isGlobal（是否全局规则）等字段，每个字段都有类型说明和示例值，这是 lint 校验中"必须包含输出契约/checkType/parseSuccess"关键词的来源——缺少任何字段定义都会导致 `parseAiResponse()` 解析失败；第三是 Groovy 脚本模板，规定了脚本的标准结构——必须是一个接收 context（Map 类型）参数的闭包，返回 Boolean 值（true 表示校验通过、false 表示不通过），示例展示了如何通过 `context.get("fieldName")` 获取业务数据、如何使用白名单工具类；第四是可用 API 清单和约束，列出脚本中可以使用的工具类和方法签名（如 `StringUtils.isEmpty()`、`PrDateUtils.daysBetween()`、`LineMapUtil.getBigDecimal()`），同时明确禁止 Runtime.exec()、System.exit()、反射等危险操作；第五是常见错误示例和避坑指引，列举 LLM 容易犯的 Groovy 语法错误（如空值未判断导致 NPE、字符串比较误用 == 而应该用 equals()、BigDecimal 比较误用 > 而应该用 compareTo()），帮助 LLM 在第一轮就生成更健壮的代码。User Message 由 `buildUserMessage()` 方法动态拼装，包含：规则上下文（质检环节、作用范围、涉及字段列表）、卡控规则原文（业务人员写的自然语言描述）、提醒文案（校验失败时展示的文字）、该环节对应的字段字典（告诉 AI 有哪些可用字段和含义）。字段字典是 Prompt 中最关键的动态部分——没有它 AI 只能猜测字段名，大概率生成不存在的字段引用。

#### 规则执行的运行时上下文是什么？GroovyContextEnhancer 注入了哪些工具类？

A：规则执行的运行时上下文是一个 `Map<String, Object>` 结构的 context 对象，由 `GroovyContextEnhancer` 负责初始化和增强，作为 Groovy 脚本执行时的输入参数。context 中包含两类内容：第一类是业务数据——当前被校验的数据行（如一条员工的薪资记录），以 key-value 形式存放，key 是字段名（如 "basic_salary"、"employee_id"、"salary_month"），value 是字段值。在数据提报场景（EXCEL_VALIDATOR）下，每行数据来自 Excel 解析后的 `Map<String, String>` 行数据，key 对应 Excel 列名；在薪资计算场景（SALARY_RESULT_VALIDATOR）下，来自 `pr_base_data` 表查询结果的字段映射；在算薪名单场景（PAYEE_LIST_VALIDATOR）下，来自 `pr_payee_list` 表的人员属性数据。第二类是工具类实例——由 `GroovyContextEnhancer` 通过 Groovy 的 `Binding` 机制注入，脚本中可以直接通过变量名引用这些工具类。注入的白名单工具类包括：`StringUtils`——Apache Commons Lang3 的字符串工具类，提供 isEmpty / isBlank / isNotEmpty / equals / equalsIgnoreCase / contains / trim 等方法，是规则脚本中最高频使用的工具（几乎每条规则都需要做空值判断和字符串比较）；`PrDateUtils`——项目自定义的日期工具类，封装了 daysBetween（计算两个日期的天数差）、monthsBetween（月数差）、parseDate（字符串解析为日期）、formatDate（日期格式化）、isAfter / isBefore（日期先后比较）等方法，薪酬规则中大量涉及入职日期、转正日期、工资月份等日期计算；`LineMapUtil`——行数据字段映射工具类，封装了从 context map 中按字段名安全获取值的逻辑，提供 getString / getBigDecimal / getInteger / getDate 等类型安全的访问方法（如 `LineMapUtil.getBigDecimal(context, "basic_salary")` 获取并转换为 BigDecimal），内部做了空值保护和类型转换异常捕获，避免脚本中直接做类型转换的空指针和 ClassCastException 风险；`BigDecimal`——Java 精确数值计算类，薪酬金额计算必须用 BigDecimal 而非 double/float 避免浮点精度问题（如 0.1 + 0.2 != 0.3），工具类注入让脚本可以直接 `new BigDecimal("100.00")` 而无需写 import 语句。白名单机制的设计意图是双重的：一是安全防护——只注入预审过的安全工具类，限制脚本的能力边界；二是提高 AI 生成质量——System Prompt 中列出了所有可用工具类的方法签名和使用示例，AI 只需从这个有限集合中选择方法组合，比从整个 Java 类库中搜索更容易生成正确代码。

#### Groovy 脚本的缓存机制是怎么设计的？为什么用 Caffeine + MD5？

A：Groovy 脚本的缓存机制在 `GroovyUtils` 工具类中实现，使用 Caffeine 缓存框架以脚本内容的 MD5 摘要为 key、编译后的 `ScriptingFunction` 为 value，缓存容量上限 1024 条。设计这个缓存有两个核心原因：第一是避免重复编译的性能开销——Groovy 的编译过程（通过 `GroovyClassLoader.parseClass()` 将脚本源码编译为 JVM 字节码）是一个相对重量级的操作，涉及词法分析、语法分析、AST 构建、字节码生成等步骤，单次编译耗时通常在 50-200ms。在质检执行场景中，同一条规则可能需要对上万条数据逐行执行——如果每次执行都重新编译脚本，万条数据 x 200ms = 2000 秒的纯编译开销完全不可接受。有了缓存后，每条规则只在首次执行时编译一次（cache miss），后续执行直接从缓存获取已编译的 ScriptingFunction 调用（cache hit），编译开销从 O(N) 降为 O(1)。第二个更关键的原因是避免 GroovyClassLoader 的 MetaClass 内存泄漏——这是 Groovy 的一个经典陷阱。每次 `GroovyClassLoader.parseClass()` 都会创建一个新的 Class 对象并注册到 Groovy 的 MetaClass 注册表（MetaClassRegistry）中，这些 Class 持有对 GroovyClassLoader 的强引用，而 GroovyClassLoader 又持有对所有已编译 Class 的引用，形成循环引用导致 GC 无法回收。如果不做缓存去重，对同一脚本的反复编译会不断创建新的 Class，Metaspace 内存持续增长直到 `OutOfMemoryError: Metaspace`。通过 MD5 去重，同一脚本内容只编译一次、复用同一个 Class 对象，从根本上避免了泄漏。选择 MD5 而非脚本全文作为缓存 key 的原因有二：一是性能——MD5 摘要固定 32 字符，HashMap 的 key 比较（equals）和 hashCode 计算效率远高于可能几百甚至上千字符的脚本全文；二是避免大 key——Caffeine 内部使用 ConcurrentHashMap，用全文作为 key 会占用大量堆内存，1024 条规则 x 平均每条 500 字符 = 约 1MB 额外内存仅用于存储 key。MD5 的碰撞概率在 2^128 量级，在我们最多 1024 条规则的场景下可以完全忽略。选择 Caffeine 而非 Guava Cache 的优势在于：Caffeine 使用 W-TinyLFU 淘汰算法（结合了 LRU 的快速响应和 LFU 的频率感知），在缓存命中率上显著优于纯 LRU。容量 1024 基于业务预估设定——目前活跃的质检规则总数约 500-800 条，1024 留了足够余量。如果未来规则数增长超过 1024，W-TinyLFU 淘汰策略会自动保留最常执行的热门规则、淘汰低频冷门规则，命中率下降可控。

#### 六大质检环节和三大规则类型的对应关系是什么？

A：六大质检环节定义在 `QualityCheckStepEnum` 枚举中：REPORT（数据提报，HR 通过 Excel 上传员工薪资基础数据）、CALCULATE（薪资计算，系统根据薪酬规则自动计算各薪酬项金额）、ADJUST（调账，对已计算的薪资做手动调整修正）、REPORT_FORM（报表，生成对外提交的薪酬报表如个税申报表）、SALARY_PLAN（薪资方案，定义不同岗位序列的薪酬结构和计算规则）、OTHER（其他，不归入前五类的杂项校验）。三大规则类型定义在 `PrRuleType` 枚举中：EXCEL_VALIDATOR（条件逻辑校验——对结构化数据做行级的条件判断，如"基本工资不能为负数"、"入职日期不能晚于工资月份"）；SALARY_RESULT_VALIDATOR（薪资结果校验——对薪资计算结果做交叉校验，如"应发合计 = 各薪酬项之和"、"个税计算结果与预期差异不超过 1 元"）；PAYEE_LIST_VALIDATOR（算薪名单校验——对参与算薪的人员名单做完整性校验，如"所有在职员工都应出现在名单中"、"离职超过 N 个月的员工不应出现"）。两者的映射关系通过 `mapRuleType(stage, scope)` 方法路由：REPORT 环节默认映射到 EXCEL_VALIDATOR，因为这个环节的质检对象是 Excel 上传的提报数据，规则需要逐行校验每条记录的字段值；CALCULATE 环节根据 scope 参数进一步细分——scope 指向薪资结果相关字段时映射到 SALARY_RESULT_VALIDATOR，scope 指向算薪名单时映射到 PAYEE_LIST_VALIDATOR，这种细分是因为薪资计算环节同时产出"计算结果"和"参与计算的人员名单"两类输出，需要不同的校验逻辑；ADJUST / REPORT_FORM / SALARY_PLAN / OTHER 四个环节根据具体业务场景灵活映射，通常走 EXCEL_VALIDATOR。这种映射设计的意义在于：不同规则类型的执行逻辑和数据来源完全不同——EXCEL_VALIDATOR 的数据来自 Excel 解析后的 `Map<String, String>` 行数据，通过 `LineMapUtil` 按列名取值；SALARY_RESULT_VALIDATOR 的数据来自薪资计算引擎的输出（`pr_salary_result` 表），字段是薪酬项编码；PAYEE_LIST_VALIDATOR 的数据来自算薪名单（`pr_payee_list` 表），字段是人员属性。对应地，每种类型使用不同的字段字典——EXCEL_VALIDATOR 从 `pr_excel_template → pr_excel_sheet → pr_excel_cell` 三级表加载 Excel 列定义，SALARY_RESULT_VALIDATOR 和 PAYEE_LIST_VALIDATOR 从 `pr_salary_subject` 薪酬项表按子串匹配过滤——不同的字典注入到 AI 解析 Prompt 中，确保 AI 针对不同类型生成引用正确字段的 Groovy 脚本。此外，不同规则类型的 System Prompt 也不同——DiamondRuleGeneratorConfig 管理数据提报阶段的 Prompt，DiamondRuleGeneratorRuntimeConfig 管理薪资计算阶段的 salaryResultSystemPrompt 和 payeeListSystemPrompt。

#### 项目的三仓协作架构是怎么设计的？各仓的职责边界？

A：三仓协作架构是基于 DDD 限界上下文划分的，三个仓分别是 cn-payroll（主仓）、cn-payroll-dmc（数据管理中心仓）、cn-hrwork（HR 工作台仓），独立 Git 仓库、独立 Maven 模块、独立开发分支和 Code Review 流程，但运行时部署在同一个 JVM 进程中（同一部署单元，所有仓的产物打包为一个 fatjar）。各仓职责清晰分离：cn-payroll 主仓是质检执行的核心，对应"质检执行"限界上下文，包含质检任务管理（任务创建、状态流转 PENDING→PROCESSING→SUCCESS/FAILED、结果持久化到 validation_result 表）、异步执行引擎（QualityCheckAsyncTask + @Failover 容灾 + RunMode.NEW_THREAD 异步执行）、多租户隔离与并发控制（TenantContext 透传 + MAX_CONCURRENT_PER_TENANT=2 + 分布式锁）、进度追踪（Redis 实时进度 + DB 每 10 页持久化）、HSF Core Facade（对外暴露的核心服务接口）。cn-payroll-dmc 数据管理中心仓是 AI 解析和规则引擎的核心，对应"规则管理"限界上下文，包含 AI 解析链路（RuleBatchParseExecutor + Self-Refine 3 轮循环 + 百炼 SDK 集成 + 信号量并发控制）、规则引擎（PrRule.execute() 按 language 路由 + GroovyUtils 执行 + GroovyContextEnhancer 上下文增强 + Caffeine 缓存）、字段字典管理（从 Excel 模板表和薪酬项表加载不同环节的字段定义）、Diamond 配置管理（Prompt 外置 + lint 校验 + 日志脱敏）、协同表格集成（QualityCheckAiSheetHandler 异常分发）、HSF Gateway Facade（网关层 Facade，做参数校验、权限验证和路由后转发到主仓 Core Facade）。cn-hrwork 工作台仓是用户交互层，对应"用户交互"限界上下文，包含网关 Controller（接收前端 HTTP 请求，做登录认证和权限点校验后通过 HSF 调用 DMC 仓的 Gateway Facade）、前端页面的 BFF（Backend For Frontend）接口。调用链路是：前端 → cn-hrwork Controller（HTTP）→ cn-payroll-dmc Gateway Facade（HSF RPC）→ cn-payroll Core Facade（HSF RPC）→ Application Service。三仓间运行时交互有两种方式：一是 HSF RPC 调用用于服务间接口调用；二是 `Class.forName()` 反射加载用于主仓调 DMC 仓的 Handler 类（因为同一部署单元共享 ClassLoader）。三仓独立 Git 仓库的好处是开发团队可以独立迭代、各自保持清晰的代码边界；同一部署单元的好处是运行时调用走 JVM 内部方法调用而非网络 RPC，延迟更低且不存在跨进程调用的分布式事务问题。

#### 分布式锁的加锁和释放为什么用不同的原语（Redis SET NX vs Tair CAD）？

A：加锁使用 Redis `SET key value NX EX 1800`（NX 表示 key 不存在时才设置、EX 1800 表示 30 分钟过期），释放使用 Tair CAD（Compare And Delete）命令，续约使用 Tair CAS（Compare And Swap）命令。三个操作使用不同原语的核心原因是保证分布式环境下的正确性，防止常见的锁安全问题。加锁用 SET NX EX 是标准做法——NX 保证互斥性（只有一个客户端能成功设置 key），EX 保证锁不会因持锁者宕机而永远无法释放（自动过期兜底）。value 存储的是 lockValue（通常是 UUID 或 requestId 的组合），用于后续释放和续约时验证身份。释放锁用 Tair CAD 而非直接 DEL 的原因是防止误删其他客户端的锁——这是分布式锁最经典的安全问题。考虑以下场景：客户端 A 加锁成功（lockValue=A），由于 FullGC 暂停或长时间计算导致锁超时过期（30 分钟 EX 到期），此时客户端 B 成功加锁（lockValue=B），然后客户端 A 恢复执行并调用 DEL 释放锁——此时 A 实际上删除了 B 的锁，导致锁失效、第三个客户端 C 也能加锁成功，两个客户端同时持锁违反了互斥性。CAD（Compare And Delete）命令是 Tair 扩展的原子操作，语义是"当且仅当 key 的当前 value 等于指定 value 时才删除"——客户端 A 调用 CAD 时传入 lockValue=A，由于 key 当前 value 已经是 B（客户端 B 的锁），比较不通过不会执行删除，保证了只有持锁者才能释放自己的锁。标准 Redis 中通常用 Lua 脚本实现相同语义（`if redis.call("get", KEYS[1]) == ARGV[1] then return redis.call("del", KEYS[1]) end`），但 Tair CAD 是服务端原生命令，性能更好（省去了 Lua 解释执行的开销，减少了一次网络往返）且语义更清晰。续约用 Tair CAS 的原因类似——CAS 语义是"当且仅当 key 的当前 value 等于 old_value 时才更新为 new_value 并重置 TTL"。续约是在质检任务长时间运行时定期延长锁的过期时间（每隔 TTL/3 即约 10 分钟执行一次），防止任务还没执行完锁就过期了。使用 CAS 而非直接 `EXPIRE key newTTL` 更新 TTL，是为了在续约时也验证身份——如果锁因某种原因已被其他客户端持有，CAS 比较不通过不会错误地延长别人锁的 TTL。三个操作的安全性小结：SET NX EX 保证加锁的互斥性和自动过期防死锁、CAD 保证释放时的身份验证防止误删、CAS 保证续约时的身份验证防止误续。

#### 规则解析的字段字典是怎么构建的？不同质检环节的字典来源不同？

A：字段字典是 AI 规则解析链路中最关键的动态上下文之一，它告诉 AI 当前质检环节有哪些可用字段、每个字段的代码名称（fieldName）和业务含义（displayName），AI 据此在生成的 Groovy 脚本中引用正确的字段名。没有字段字典的话，AI 只能根据规则原文猜测字段名（比如看到"基本工资"可能猜测字段名是 "basic_salary" 或 "baseSalary" 或 "jbgz"），大概率生成不存在的字段引用导致 `context.get()` 返回 null 进而运行时空指针。不同质检环节的字典来源确实不同，按数据来源分为两大类：数据提报阶段（REPORT 环节，规则类型为 EXCEL_VALIDATOR）的字段字典从 Excel 模板体系加载，采用三级表结构：`pr_excel_template`（模板表，存储模板 ID、名称和所属租户——不同租户有完全不同的 Excel 模板和列定义）→ `pr_excel_sheet`（Sheet 表，定义该模板中每个 Sheet 的名称和顺序——一个 Excel 可能有"员工基础信息"、"薪资明细"等多个 Sheet）→ `pr_excel_cell`（单元格定义表，存储每个 Sheet 中每列的字段名 fieldName、显示名 displayName、数据类型 dataType 如 STRING/NUMBER/DATE、是否必填 required 等属性）。构建过程是：根据当前规则关联的 Excel 模板 ID 查询 pr_excel_template 获取模板信息 → 获取模板下所有 Sheet 列表 → 获取规则涉及的 Sheet 中所有列定义 → 最终生成 `List<FieldDefinition>` 注入到 Prompt 的字段字典区块。三级表设计是因为不同租户有不同的 Excel 模板（集团内不同 BU 的薪酬结构和报表格式差异很大），同一模板中不同 Sheet 的列也完全不同，必须精确到 Sheet + 列级别才能给 AI 正确的字段信息。薪资计算阶段（CALCULATE 环节，规则类型为 SALARY_RESULT_VALIDATOR 或 PAYEE_LIST_VALIDATOR）的字段字典从 `pr_salary_subject` 薪酬项表加载。薪酬项表存储了所有薪酬项的编码（如 "base_salary"、"performance_bonus"、"social_insurance_personal"）和名称（如"基本工资"、"绩效奖金"、"社保个人部分"）。构建方式不是加载全量薪酬项（可能有上千个），而是按规则涉及字段做子串匹配过滤——比如规则原文提到"基本工资"，就用"基本工资"作为关键词去匹配薪酬项名称包含这个子串的记录，只把相关的薪酬项注入 Prompt。这种过滤策略控制了 Prompt 长度（全量注入可能产生数千 token 的字段列表，不仅增加成本还会稀释 AI 对规则核心逻辑的注意力），且子串匹配在实践中准确率足够——AI 结合规则原文语义能判断应使用哪个字段。字段字典注入 Prompt 的格式是结构化列表，每条包含 fieldName 和 displayName，AI 通过 displayName 理解含义、通过 fieldName 生成代码引用。

#### 预编译和真正执行有什么差异？通过编译但运行时失败的场景有哪些？

A：预编译（`tryPrecompile()` 调用 `GroovyUtils.precompile()`）和真正执行（`PrRule.execute()` 调用 `GroovyUtils.executeScript()`）是两个完全不同的阶段，发生在不同时间点、验证不同层面的正确性。预编译发生在 AI 解析阶段（规则生成时，Self-Refine 循环的第五步），通过 `GroovyClassLoader.parseClass(scriptSource)` 将 Groovy 源码编译为 JVM 字节码，验证的是静态语法正确性——括号是否匹配、关键字是否拼写正确、变量声明是否合法、方法调用的签名是否存在等。预编译通过只能说明脚本在语法层面是合法的 Groovy 代码，不保证运行时行为正确。真正执行发生在质检任务运行阶段（异步执行引擎处理数据时），通过从 Caffeine 缓存获取的 `ScriptingFunction.call(context)` 调用已编译的脚本并传入实际的业务数据上下文（包含字段值和工具类），验证的是运行时行为正确性——字段取值是否为 null、类型转换是否成功、业务逻辑是否按预期工作。通过编译但运行时失败的场景主要有以下几类：第一类是空指针异常（NullPointerException，最常见约占运行时错误的 60%）——脚本中写了 `context.get("salary_month").equals("202601")`，但实际数据中该字段缺失或值为 null，null.equals() 抛出 NPE。预编译不检查运行时数据值，`Map.get()` 的返回类型是 Object（可以是 null），编译器无法发现这类问题。正确写法应该是 `Objects.equals(context.get("salary_month"), "202601")` 或先判空。第二类是类型转换异常（NumberFormatException / ClassCastException）——脚本中写了 `new BigDecimal(context.get("amount").toString())`，但实际数据中 amount 字段的值包含非数字字符（如 "N/A" 或 "10,000" 含逗号），BigDecimal 构造器抛出 NumberFormatException。预编译只检查构造器签名合法，不检查运行时参数值。应该使用 `LineMapUtil.getBigDecimal()` 安全方法替代直接构造。第三类是字段名不匹配——脚本引用了 `context.get("basic_salary")` 但实际数据中的字段名是 `basicSalary`（驼峰 vs 下划线命名不一致），get 返回 null 导致后续逻辑失败。这类问题的根源通常是字段字典中的 fieldName 与实际数据源中的 key 存在格式差异。第四类是业务逻辑错误——脚本编译和执行都不报错，但校验结果不符合预期（如比较方向写反了、阈值引用错了字段）。第五类是集合操作异常——脚本对列表做索引访问（如 `list.get(0)`）但列表为空抛出 IndexOutOfBoundsException。为应对这些运行时风险，系统设计了多重兜底：规则引擎对每条规则的执行做 try-catch 包裹，运行时异常不中断整个质检任务，只将该规则对该条数据标记为"执行异常"并记录异常堆栈；Self-Refine 的 Prompt 中通过"常见错误避坑"示例引导 AI 生成更健壮的代码（如始终先判空、使用 LineMapUtil 安全方法）；字段字典的注入从源头降低字段名不匹配的概率。


## 专业技能

TODO 完善专业技能

- 熟练掌握 Java 基础与集合（HashMap / ConcurrentHashMap 等底层原理）、并发编程（线程池、JUC 锁、CAS 与 JMM 内存模型、ThreadLocal 上下文传递）、JVM 内存区域与 G1 / CMS GC 调优；熟悉 Spring（IOC、AOP、SpringBoot自动装配等）和 MyBatis（动态 SQL、拦截器机制等）；
- 熟练使用 MySQL（索引、事务、存储引擎、行/间隙锁与MVCC）和 Redis（数据类型、线程模型、持久化与过期淘汰策略），熟悉缓存穿透 / 击穿 / 雪崩防护与分布式锁实现；熟练使用企业级分布式中间件生态（RPC 框架、分库分表与流式分页、分布式缓存、消息队列异步解耦、分布式定时调度与跨节点容灾、配置中心分钟级热更、全链路 Trace、限流降级、离线数仓），并基于 DDD 分层完成多仓协同研发；熟悉计算机网络与操作系统等基础知识；
- 适应全栈研发模式，覆盖「页面 Schema 拖拽编排 → 网关接口发布与权限点配置 → 后端 Facade / Controller 接口」端到端链路，能独立交付从页面到底层数据的完整需求；
- 熟练使用 Vibe Coding、SDD等 AI 编程范式，了解 Harness Engineering 工程化理念；掌握 Prompt Engineering、Function Calling / Tool Use、A2A / Skills 渐进式披露等 LLM 核心能力与 Agent 协议生态，熟练在日常开发与运维中使用MCP、Skills等相关能力；
- 熟悉 ReAct、Reflexion等Agent主流架构模式，了解LangChain框架、Agent调试技巧、Token 成本调优、RAG等相关内容。



## MiniClaw — OpenClaw 核心架构最小复刻

项目介绍：用 ~2,700 行 Python 复刻 OpenClaw（43 万行 TS，30w+ Star Agent 框架）核心架构，沉淀为 16 模块 / 11 项核心原理的可运行最小实现，支持 CLI / Discord / HTTP API 三端接入，配套 179 单测 + 22 集成测试。

技术栈：Python asyncio、Anthropic / OpenAI / 阿里百炼 SDK、EventBus、ReAct、Function Calling、Prompt Engineering

- 自主性三角：Heartbeat 周期觉察 + `HEARTBEAT_OK` 静默协议避免告警疲劳，Cron 精确定时调度，Hooks（EventBus + 12 种事件 + 优先级 + 错误隔离）事件驱动响应，让 Agent 从"问答 chatbot"升级为"无人输入也能干活"的角色。
- Hub-and-Spoke Gateway：统一协调 Channel / Router / Agent 生命周期 / Heartbeat / Cron，新增 Channel 或 Agent 零侵入；JSONL 持久化 + 启动自动恢复最近 20 轮对话，进程崩溃重启不丢上下文。
- Agent Loop + 多 LLM 抽象：实现 Brain → Hands → Brain 工具循环（最多 10 轮防死循环）；Brain 抽象层屏蔽 Anthropic `tool_result` 嵌套与 OpenAI 独立 `tool` 角色的协议差异，上层统一 `Message` / `ToolCall` 数据结构。
- Workspace 契约文件 + 自动反思：将人格 / 身份 / 记忆外置为 SOUL.md / IDENTITY.md / MEMORY.md，按固定顺序注入 system prompt 利用 Recency Bias 保证规则优先级；每 N 轮 LLM-as-Reflector 提炼用户画像追加到 MEMORY.md，`NOTHING_TO_REMEMBER` 协议过滤无效记忆。
- Skills 渐进式披露 + Context Compaction：Skill 索引始终可见、命中后才注入完整 body，开局节省数千 token；历史超 70% 阈值时 LLM 摘要替换旧消息，并对 `tool_call` / `tool_result` 配对做安全分割，杜绝压缩后协议错位。
- Multi-Agent Spawn + 安全沙箱：Agent 间独立 workspace / Brain / 上下文，可通过 `spawn_agent` 委派子任务；`spawn_depth` 到上限直接不注册 spawn 工具，比工具内报错更优雅地防递归爆炸；Hands 对文件操作做路径遍历校验，杜绝越权读写。





> 暂存这里
## 面试怎么考查 AI Coding 能力
工作需要，跟候选人聊AICoding，总结一些心得：一看做品，二看他写过的最牛逼的 Prompt。
首先：最主要是看作品，让他讲完整的VibeCoding经历，讲 + 演示作品。没作品的话至少也要讲一个完整的 Case，而且要完完全全带入产品和PMO视角，因为AICoding代替了体力劳动，一定会有大量的脑力思考从代码细节中抽离出来到产品设计、架构、方法和理念上，这部分脑力活动占据整个YOLO过程的至少一大半以上，所以按理说要对自己作品的形态、理念、目标、演示和完整性这些信息描述很清晰才对。
再者：剩下一半的精力是被死磕模型和提示词消耗掉的，看候选人都触碰到过哪些大模型的能力边界，即某个难题让模型解决，一次不行，二次不行，三次四次，最后终于成功，怎么引导大模型从失败走向成功的，但凡有过这类触碰过模型极限的场景，很容易沉淀出方法和经验，这种“摸高”的编程体验很重要！













