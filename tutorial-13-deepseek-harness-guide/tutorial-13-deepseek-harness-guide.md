# DeepSeek Harness 从 0 到 1详细拆解

> **系列**：Codex 实战系列（第 13 期）
> **作者**：Miles Ma（@miles_mazy，认证账号）
> **原文**：X 长文（Article） · https://x.com/miles_mazy/status/2088163035521437982
> **发布时间**：2026-08-14 00:17（PDT）
> **抓取时数据**：13 回复 · 40 转发 · 179 点赞 · 290 收藏 · 121.8K 阅读
> **转录说明**：以下为原文全文转录（含图片与代码块的版式位置），图片已存档至 `assets/` 目录。原图外链见文末「媒体清单」。原文中加粗语句以 **粗体** 呈现。

---

![封面插图：漫画人物站在工作台前，把一颗标有 "AI" 的发光粉色脑芯芯片插入服务器机柜；旁边一张标注卡 "DAD'S TOOLBOX: PLUGIN MODULES"，列出 Files / Shell / Perms / Sessions / Sub-Agent / WebUI 等可插拔插件模块 —— 视觉隐喻「模型是大脑，Harness 是外面可拆装的执行系统 / Everything is a plugin」](assets/cover.webp)

*（原文位置：文章标题上方封面图，1983×793）*

---

DeepSeek V4 Pro 已经发布了。官方主打的是 Agentic Coding，我用下来的感受却很直接：**效果很差，至少离宣传带来的期待差得很远**。

8 月 13 日，DeepSeek 又放出了 DeepSeek Harness。这次发布的重点不在模型参数，而是模型外面的那套执行框架。

我把官方文档和代码过了一遍，又在本地跑了文件写入、Shell 验证、权限拦截和会话轨迹。我的判断是：**架构值得研究，当前版本不适合普通人拿来当主力工具**。只想让 AI 写网页、做 PPT，WorkBuddy、Codex 之类的成品更省事；正在做 Agent 或企业 AI 落地，才有必要把 DeepSeek Harness 拆开看。

## V4 Pro 是模型，Harness 是模型外面的执行系统

DeepSeek V4 在 4 月 24 日发布，分成 V4 Pro 和 V4 Flash。官方给 V4 Pro 的定位很高：1.6T 总参数、49B 激活参数、1M 上下文，并且强调 Agentic Coding 能力。官网、App 和 API 当天都可以使用，API 里的模型名就是 deepseek-v4-pro。

V4 Pro 已经在官网、App 和 API 正式开放，并非只存在于 Harness 的演示里。实际效果仍然要看任务，官方榜单不能代替使用结论。

Harness 解决的是另一层问题。

模型收到一句任务以后，可以决定自己想调用哪个工具，却不会自己把工具执行掉。谁来读取项目规则，谁来写文件，谁来运行命令，权限不够时谁来拦，任务中断后从哪里继续，这些都需要一套外部系统。

可以把模型理解成大脑。Harness 是外面的手、工作台、门禁和行车记录仪。只有 V4 Pro，你得到的是推理和生成能力；把它接进 Harness，模型才有机会在真实目录里连续做事。

Codex、Claude Code、WorkBuddy 也都有自己的 Harness。DeepSeek 这次比较特别的地方，是把这一层单独开源了，而且不是只放一个几十行的 Agent Loop。模型适配、工具、文件系统、Shell、权限、会话、子 Agent、界面，甚至 Agent 怎么循环，都能拆换。

![插图：年轻人把一颗发光的脑（模型）插入模块化工作台 —— 机械臂（工具/手）、带锁的笼子（权限/门禁）、文件抽屉（文件系统）、视频录制设备（会话轨迹/行车记录仪）](assets/img-01-brain-workbench.jpg)

*（原文位置："都能拆换"段落之后，1200×480）*

官方把项目标成了 **Developer Preview**，同时明确提醒后续会有破坏性改动。它现在更像一套公开实验中的 Agent 底座，离稳定的企业产品还有距离。

## 我在本地是怎么跑的

有 Node.js 的电脑可以直接启动：

```bash
npx @deepseek-ai/dsh web
```

它会在本机打开一个 Web 页面。第一次启动会要求配置 API Key。

![Web UI 标注截图：「设置」对话框「模型」页，显示 DeepSeek（deepseek-official）提供方与「输入 API 密钥」输入框，取消/保存按钮，「+ 添加提供方 / + 添加自定义提供方」链接；背景为 "Harness smoke test" 会话](assets/img-02-settings-model.jpg)

*（原文位置："第一次启动会要求配置 API Key"之后，1200×480）*

这里很容易混淆：能在 DeepSeek 官网聊天，不等于已经有 Harness 可以调用的 API。使用 DeepSeek 官方模型时，需要在开放平台创建 API Key，费用按输入、输出和缓存命中计算。也可以接 OpenAI、Anthropic、Codex OAuth、公司网关或自建的兼容接口。

配置模型、选择工作区以后，页面看起来很像普通聊天工具。消息下面那几行工具记录，才是我这次测试一直盯着看的地方。

我做了一个很小的控制实验：让 Agent 新建 result.txt，写入指定内容，再用 Shell 确认文件确实存在。为了不把模型质量和 Harness 能力混在一起，我接的是本地确定性测试端点。它每一步都按固定顺序返回，这样可以单独看 Harness 是否真的执行了工具。

任务下去以后，页面依次出现两项操作：Write result.txt 和 Bash Verify file through sandboxed shell。文件也确实落到了测试目录里。

![Web UI 截图："Harness smoke test" 会话（标准模式），消息"写出 result.txt 然后用 shell 验证"，工具调用记录（上下文注入 AGENTS.md/CLAUDE.md、Write result.txt、Bash - Verify file through sandboxed shell），输出 DSH_HARNESS_TASK_COMPLETED，底部统计 "1 轮 · 3 步 | LLM 0s · 工具调用 0s | 首 token 平均 0s · 7000 tok/s | 缓存命中 0% | 输入 50 tok · 输出 28 tok"，权限下拉 "Workspace Write"，模型 "DeepSeek-V4-Flash High"](assets/img-03-smoke-test.jpg)

*（原文位置："文件也确实落到了测试目录里"之后，1200×480）*

这一步发生了三次模型请求：

1. 模型请求调用 write。
2. Harness 检查权限，写入文件，再把执行结果交回模型。
3. 模型请求调用 bash，Harness 执行验证命令，最后模型才回复任务完成。

如果直接调用模型 API，模型最多返回一个"我想调用 write"的结构。真正把文件写下去、把结果再交回模型的，是 Harness。

![轨迹页截图：对话/轨迹标签页，Duration/Turns/Calls 时间线条形图（含 Input/System/Model/Tools 分段），详细 turn 日志（SYSTEM/USER/CONTEXT/ASSISTANT/TOOL 行）—— Initial System Prompt、用户任务、system-reminder、runtime-context 快照、write 工具调用（file_path "result.txt"）、bash 调用、输出 DSH_HARNESS_BASH_OK、最终 ASSISTANT 消息 DSH_HARNESS_TASK_COMPLETED](assets/img-04-trajectory.jpg)

*（原文位置：三步模型请求编号列表之后，1200×480）*

轨迹页把这段过程摊开了。系统提示词、工作区里的 AGENTS.md、当前权限、每次模型请求、工具参数和返回值都能看到。做 Agent 调试时，这些信息比最后一句"已经完成"有用得多。

比如模型选错了工具，可以回到对应请求看它当时拿到了什么上下文；工具执行失败，可以继续查参数和返回值；上下文膨胀，也能看到系统提示、工具定义和历史消息分别占了多少。

![轨迹页截图：Request #1 详情面板打开，Summary 页显示 Status Completed、Provider deepseek-official、Model deepseek-v4-flash、Tool calls 1、Result Assistant Message；Summary/Options/Usage/Timing 标签页](assets/img-05-request-detail.jpg)

*（原文位置：轨迹页段落之后，1200×480）*

截图里的模型标签虽然显示 DeepSeek V4 Flash，后端实际是本地测试端点。它验证的是 Harness 的执行过程，不代表 V4 Flash 能达到图里的速度，也不是模型效果评测。

## 权限能拦住动作，但拦不住模型胡说

我又把同一个任务放到只读环境里跑了一次。

写文件时，Harness 返回 FS_SANDBOX_DENIED；后面的 Shell 验证也因为文件不存在而失败。这个限制发生在执行层，不是在提示词里劝模型"请不要修改文件"。模型再坚持也写不进去。

Web 界面里可以直接看到三档权限：Read Only、Workspace Write 和 Full access。

![Web UI 截图：权限下拉菜单打开，显示三档：Read Only、Workspace Write（已勾选）、Full access](assets/img-06-permissions.jpg)

*（原文位置："三档权限"段落之后，1200×480）*

测试里也暴露了一个边界：工具已经报错，本地测试模型仍然回复任务完成。Harness 把越权操作拦住了，却没有替人判断最后那句话是真是假。

企业做 Agent 时，权限和验收要分开。权限控制它能不能做，验收判断它做对了没有。单元测试、评测集、业务规则和人工复核，该加的一样都不能少。

## DeepSeek Harness 到底开源了哪一层

它的设计原则写得很直白：Everything is a plugin。

我一开始觉得这句话有点宣传味，继续看代码和界面以后，才发现它确实做得比较彻底。插件页面里，终端、Agent 循环和网页搜索都可以单独配置；仓库里还能找到模型适配、凭据、沙箱、审批、会话持久化、子 Agent 和 Web UI 等插件。

![设置对话框截图：「插件」页 → 插件配置，列出终端（限制 agent 运行的每一条命令）、Agent 循环（Agent 如何派发工具调用）、网页搜索（DeepSeek 搜索提供方）；左侧导航 通用设置/模型/插件/Agent 预设；「打开配置文件」按钮](assets/img-07-plugins.jpg)

*（原文位置："Everything is a plugin"段落之后，1200×480）*

它自带四套 Agent 预设：

- 标准模式提供文件、Shell、检索、Skills、计划、目标、子 Agent 和工作流。
- PTC 模式让模型用 TypeScript 程序组合多步工具调用。
- 极简模式只保留持久 Bash 和编辑器。
- 创造模式用来检查运行时、实验插件和创建自定义预设。

![设置对话框截图：「Agent 预设」页，四张预设卡片：标准模式（当前使用：文件、Shell、检索、Skills、计划、目标、子 Agent 和工作流）、PTC 模式（通过 TypeScript 程序灵活组合多步工具调用）、极简模式（只保留持久 Bash 和编辑器，适合简单脚本类任务）、创造模式（查看运行时上下文，实验插件和创建自定义预设）](assets/img-08-presets.jpg)

*（原文位置：四套预设列表之后，1200×480）*

预设没有把 V4 Pro 变成四个模型。变化的是模型随身携带哪些工具、提示词和运行能力。

这套设计对做企业 Agent 的人很有吸引力。公司只允许通过审计接口查询数据库，可以换数据访问插件；密钥必须进入 KMS，可以换凭据提供方；高风险动作要走内部审批，也可以把审批接进去。开发者不必因为其中一层不合适，就把整套 Agent 推倒重写。

麻烦同样来自这里。能换的东西越多，需要做的技术决定就越多。插件版本、兼容性、缓存、权限组合和第三方供应链都要有人负责。成品工具替用户做掉了这些选择，DeepSeek Harness 把选择权和维护成本一起交了出来。

## 它和 WorkBuddy 的区别，不是有没有 Skills 和 MCP

WorkBuddy 可以切模型，可以装 Skills、接 MCP，也有 Hooks、自定义 Agent、subagent、CLI 和 SDK。把两边的功能名列出来，确实会觉得它们很像。

两者的产品边界不同。

WorkBuddy 先是一款办公和开发工作台。用户给出任务，它负责交付文档、PPT、数据处理、研究结果或者代码。模型和连接器可以扩展，默认的会话、界面和执行机制已经替用户选好了。

DeepSeek Harness 提供的是可拆装的运行时，当前 Web 页面更像参考产品。开发者可以继续改文件系统、Shell、权限、持久化、Agent 循环和界面，然后做出一款面向自己行业的工作台。

![插图：服务器机柜（大脑模块）旁，一名年轻人手持小脑芯片，站在模块化工作台/插件墙旁 —— 可插拔的彩色插件模块、抽屉与控制面板](assets/img-09-plugin-rack.jpg)

*（原文位置：WorkBuddy 对比一节内，1200×480）*

如果目标是今天把 PPT 做出来，我会选 WorkBuddy；如果目标是开发一套自己的 WorkBuddy，我才会认真考虑 DeepSeek Harness。

普通用户并不会因为"Everything is a plugin"获得更好的成品。大部分时候，他只是多了 Node.js、API Key、配置和排错。对 Agent 开发者来说，能改底层才有价值。

## 它也没有因为开源就自动超过 Codex

DeepSeek Harness 和 Codex 都能读项目、改文件、运行命令，也都处理 Skills、MCP、权限、会话和子任务。

Codex 已经把重点放在任务完成率、代码质量、Diff、终端、并行工作和长任务体验上。它同样开源，不能粗暴地写成"DeepSeek 开源，Codex 闭源"。

DeepSeek Harness 现在的优势是模型更容易替换、运行部件更容易改、轨迹暴露得更完整。Codex 的优势是拿来就能干活，而且产品完成度高得多。

只比"帮我做出一个网页"，Harness 没有给 V4 Pro 自动加分。网页好不好，仍然受模型能力、上下文、工具反馈和验收循环影响。

## V4 Pro 做出来的小游戏，说明了另一个问题

GitHub Discussions 里有一个很直观的公开测试。用户要求 V4 Pro High 生成一个单文件 Three.js《我的世界》小游戏。第一版思考接近 20 分钟，地图很小，人物进入游戏后还会一直下坠。修过以后可以玩，功能依然不多。缓存命中率倒是很高，99%，总费用不到五毛钱。

我把他上传的 HTML 下载下来，在 Chrome 里实际打开了。页面有地形、树木、第一人称操作提示、FPS、坐标和九格物品栏。它不是一张摆拍截图，修复后的文件确实能运行。

![浏览器截图：V4 Pro 生成的单文件 Three.js《我的世界 · Web 版》运行中 —— 方块地形与树木、第一人称操作提示（WASD 移动、空格跳跃、鼠标左键破坏方块、鼠标右键放置方块、数字键/E 切换方块、重新开始）、FPS 60 指示、坐标 8.0, 6.0, 8.0、九格物品栏、「点击进入」开始按钮](assets/img-10-minecraft.jpg)

*（原文位置：小游戏描述段落之后，1200×480）*

这个例子不能证明 V4 Pro 在所有任务上都很差，也不能证明 Harness 没用。它至少能说明一件事：**工具流程跑通，不代表产物已经合格**。

官方给 V4 Pro 的 Agent 能力评价很高，具体任务里仍然可能出现碰撞错误、需求遗漏和长时间思考。Harness 可以继续把报错送回模型，让模型修；如果没有好的测试和验收，它也可能带着一个半成品宣布完成。

我对这次发布的判断因此分成两部分：V4 Pro 的实际效果低于官方宣传带来的预期；DeepSeek Harness 的当前使用体验也不成熟，但它开放出来的运行机制，比眼前这个 Web 产品更有长期价值。

## 普通人现在要不要装

如果只是想试个热点，看看页面长什么样，装起来并不难。准备长期使用，就要看自己需要的到底是结果还是改造权。

我建议这几类人现在就研究：

- 正在开发 Agent、编码助手或内部 AI 工作台。
- 需要接公司网关、本地模型和自定义权限系统。
- 想看清工具调用、上下文和会话状态怎么流动。
- 能接受版本变化，愿意自己排查插件与配置问题。

下面几种情况，先别折腾：

- 只想一句话拿到 PPT、网页、报告或代码。
- 需要成熟的桌面体验、产物预览和远程协作。
- 不想处理 Node.js、API Key、命令行和插件。
- 准备直接接生产系统，却没有安全、测试和运维团队。

它默认只监听本机地址，这个限制我反而认可。当前 Web API 可以运行 Bash，又没有完整的远程认证，随便绑定到公网地址，相当于给别人留了一个远程执行入口。安全默认值会麻烦一点，总比为了方便把电脑暴露出去好。

## 企业拿它落地，还差多少东西

把 DeepSeek 三个字拿掉，企业 Agent 需要的 Harness 至少要处理六件事：上下文、工具、权限、状态、恢复和验收。

MCP 负责连接外部工具和数据源，可以成为 Harness 的一部分。Skills 告诉 Agent 某类任务应该怎么做，也是一部分。Agent 是当前执行者，subagent 是它派出去处理子任务的协作者。谁能派、能看哪些信息、能调用什么工具、结果怎么收回来，仍然要由 Harness 管。

凭据管理管 API Key、Token 和证书。配置文件里最好只保留引用，真正的密钥交给环境变量、Keychain 或企业 KMS。它和"每条结论有没有证据"不是一回事。后者属于证据治理，需要来源记录、证据等级和发布门禁。

比如做一个自动处理 GitHub Issue 的 Agent。它要读取问题、修改分支、运行测试、创建 PR，再根据 CI 结果继续修。GitHub 连接器只是入口，后面还需要短期 Token、分支权限、任务状态、测试门禁、失败重试和审计日志。

再比如监控企业邮箱。读信和提取任务可以自动完成，给客户发信、修改交付日期和确认报价不能随便自动化。谁审批、等待期间状态放在哪里、几小时后怎么继续、客户回复怎么对应回原任务，都要在 Harness 里设计。

DeepSeek Harness 可以承担中间的 Agent 运行时，安装完成并不会自动长出一套 GitHub、邮箱、CRM 和项目管理系统。外部触发、业务连接器、账号体系、多租户、长期状态和验收规则，还得实施团队补。

这也是 FDE 应该关注它的原因。客户说"让 AI 自动跟进项目"时，FDE 不能只接一个模型再写几段提示词。哪些信息能读，哪些动作能自动做，哪里必须审批，失败以后重试还是转人工，最后拿什么证明任务完成，这些才是落地方案。

项目结束后，现场验证过的连接器、权限模板、评测集和验收规则要带回研发后台，做成下一次可以复用的资产。下一个客户再来，团队改业务差异，不必重新从零搭一遍。

![插图：一人抱着上锁的凭证箱（锁+钥匙+清单图标），从备品齐全的工作台走向一排上锁的各色凭证柜 —— 隐喻 FDE 把现场验证过的连接器/权限模板/评测集/验收规则带回研发后台做成可复用资产](assets/img-11-credential-cases.jpg)

*（原文位置："下一个客户再来"段落之后，1200×480）*

我做过算法优化和部署，现在越来越确定：模型只决定了一部分上限。Agent 进了业务以后，能不能稳定做事、会不会越权、出了错能不能查，更多取决于外面的 Harness。

我现在的结论是：**普通用户不用急着装，Agent 开发者应该尽早研究**。现在拿它替代 WorkBuddy 或 Codex，我不看好；把它当作一套公开的 Agent 底盘来拆，我会继续看。

我是 Miles，一名从大厂转型 FDE 的 AI 算法专家，做过模型优化部署，也做过企业内部沟通与培训。

![作者横幅插图：Miles 手持带勾选标记的写字板，立于 "AI MODEL" 立方体、服务器机柜与带网络图的展示板之间；文字 "Miles / AI 算法专家 × FDE / 企业 AI 培训 · 落地 · 部署 / 关注我"](assets/img-12-author-banner.jpg)

*（原文位置：作者介绍之后，「关注我」之前，1200×480）*

关注我 @miles_mazy（链接 https://x.com/miles_mazy），后面我会继续把模型、Agent 和企业落地之间那些真正影响交付的工程问题讲清楚。

**一起成长，一起赚钱**。

---

## 附：媒体清单（共 13 项，按原文出现顺序）

| # | 类型 | 原文位置 | 本地文件 | 原始链接 |
|---|------|----------|----------|----------|
| 封面 | 图片 | 文章标题上方 | `assets/cover.webp` | https://pbs.twimg.com/media/HPqk_QVbYAERcqV?format=webp&name=large |
| 1 | 图片 | 「都能拆换」段落之后 | `assets/img-01-brain-workbench.jpg` | https://pbs.twimg.com/media/HPqisj5bwAAu2kI.jpg |
| 2 | 图片 | 「第一次启动会要求配置 API Key」之后 | `assets/img-02-settings-model.jpg` | https://pbs.twimg.com/media/HPqiskAbMAAAKSV.jpg |
| 3 | 图片 | 「文件也确实落到了测试目录里」之后 | `assets/img-03-smoke-test.jpg` | https://pbs.twimg.com/media/HPqisvcbcAAh33J.jpg |
| 4 | 图片 | 三步模型请求列表之后 | `assets/img-04-trajectory.jpg` | https://pbs.twimg.com/media/HPqi0d0a0AAe8Tg.jpg |
| 5 | 图片 | 轨迹页段落之后 | `assets/img-05-request-detail.jpg` | https://pbs.twimg.com/media/HPqi7hraEAAKCls.jpg |
| 6 | 图片 | 「三档权限」段落之后 | `assets/img-06-permissions.jpg` | https://pbs.twimg.com/media/HPqi-THbIAEWNmH.jpg |
| 7 | 图片 | 「Everything is a plugin」段落之后 | `assets/img-07-plugins.jpg` | https://pbs.twimg.com/media/HPqjAtxbEAEdXAg.jpg |
| 8 | 图片 | 四套预设列表之后 | `assets/img-08-presets.jpg` | https://pbs.twimg.com/media/HPqjD_pa0AA83IO.jpg |
| 9 | 图片 | WorkBuddy 对比一节内 | `assets/img-09-plugin-rack.jpg` | https://pbs.twimg.com/media/HPqjWg_bcAAVvvP.jpg |
| 10 | 图片 | 小游戏描述段落之后 | `assets/img-10-minecraft.jpg` | https://pbs.twimg.com/media/HPqjNaSaQAATJq0.jpg |
| 11 | 图片 | 「下一个客户再来」段落之后 | `assets/img-11-credential-cases.jpg` | https://pbs.twimg.com/media/HPqja__aYAEjeYg.jpg |
| 12 | 图片 | 作者介绍之后 | `assets/img-12-author-banner.jpg` | https://pbs.twimg.com/media/HPqj1KZbcAA4_m2.jpg |

## 附：代码清单（共 1 个代码块）

- 代码块 1（bash）：`npx @deepseek-ai/dsh web`

---

## 系列导航

- 上一篇：[第 12 期 WorkBuddy 从入门到精通](../tutorial-12-workbuddy-guide/tutorial-12-workbuddy-guide.md)
- 下一篇：无（目前最新一期）
