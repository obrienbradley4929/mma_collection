# Codex 实战系列｜零剪辑自动生成爆款视频

> **系列**：Codex 实战系列（第 4 期）
> **作者**：Miles Ma（@miles_mazy，认证账号）
> **原文**：X 长文（Article） · https://x.com/miles_mazy/status/2097177704282136838
> **发布时间**：2026-09-07 21:18（PDT）
> **抓取时数据**：44 回复 · 365 转发 · 2.1K 点赞 · 4.2K 收藏 · 868.2K 阅读
> **转录说明**：以下为原文全文转录（含图片与代码块的版式位置），图片已存档至 `assets/` 目录。原图外链见文末「媒体清单」。文中代码块的语言标签均为 text。

---

![封面图：Codex 实战系列 | 零剪辑自动生成爆款视频 —— 大标题「自动生成爆款视频」，标语「无需手动剪辑，运行工作流即可生成可发布的爆款视频」，标签「脚本·出图·配音·合成·发布」，页眉「AI CODING | VIDEO CREATION | PRACTICE」，「2026」；右侧三块等距 UI 面板：01 脚本输入 / 02 画面生成·配音合成 / 03 视频发布](assets/cover.webp)

*（原文位置：文章标题上方封面图）*

---

之前做一条视频，脚本、配音、剪辑和字幕，每一个都要花费大量的时间去学习，直接从入门到放弃。

现在这些工作可以直接交给 Codex。你给它一篇文章，或者只给一个主题，再把对应的视频 Skills 交给它，Codex 就能继续完成口播、声音、分镜、字幕、动效和成片。你不用先学会剪辑，也不用看懂代码，能把自己想要的视频说清楚，就可以开始。

这篇文章，我会把整条流程从头到尾讲一遍。先做一条不出镜的文章视频，再补上真人口播的剪辑方法。第一次接触视频的人，也可以跟着跑出自己的第一版。

我的流程是：文章或主题 → 口播稿 → 配音 → 时间轴 → 分镜 → HyperFrames → 检查成片

![手绘流程图：文章 → 口播 → 声音 → 分镜 → 视频，中间卡通人物](assets/img-01-pipeline.jpg)

*（原文位置：引言，流程说明之后）*

这篇教程就从最简单的一条路开始。你只要会打开 Codex、创建一个文件夹、把文章放进去，剩下的可以一句一句告诉它做。

## 先把需要的工具交给 Codex

Skill 可以理解成一份给 Codex 看的工作说明书。它告诉 Codex，遇到口播、配音、分镜或剪辑任务时，该调用什么工具、按什么顺序完成、最后检查哪些问题。

安装方法可以统一成一句话：把下面的 GitHub 链接复制给 Codex，让它阅读项目说明、完成安装并检查能否使用。具体命令交给 Codex 处理，你不需要自己抄。

这套视频流程会用到的工具，可以按环节放在一起看：

![工具分工表：环节/工具或Skill/它负责什么/怎么选 —— 口播/Codex，口播润色/humanizer-zh，普通配音/edge-tts，个人声音/CosyVoice，动效成片/HyperFrames，固定栏目/Remotion Agent Skills，真人剪辑/video-use](assets/img-02-tools-table.jpg)

*（原文位置：「先把需要的工具交给 Codex」一节，安装方法说明之后）*

HyperFrames 里面已经带有几种分工：/faceless-explainer 负责把文章或主题整理成不出镜讲解，/hyperframes-creative 负责口播节奏和分镜，/media-use 管理图片、声音和字幕，/hyperframes-animation 负责画面变化。平时只需要告诉 Codex"使用 /hyperframes 做这条视频"，它会继续选择需要的部分。

画面样式也可以按需要补充。想做拼贴式知识视频，可以把 HyperFrames Community Skills 里的 vox-explainer 交给 Codex；想做手写或绘制出现的效果，可以选同一个仓库里的 p5-paint-animation；想找产品介绍、界面演示和宣传片的镜头，可以让 Codex 参考 video-shotcraft。这些都属于样式扩展，第一条视频不用全装。

把工具放回流程里以后，Codex 会先做内容编辑，再处理声音、分镜和成片检查。你只需要在每一步确认结果是否符合自己的意思。

![手绘四角色流程图：内容编辑 → 声音助理 → 视觉导演 → 成片检查](assets/img-03-four-roles.jpg)

*（原文位置：「先把需要的工具交给 Codex」一节末尾，工具放回流程说明之后）*

现在新建一个文件夹，例如 codex-video，在 Codex 里打开它。把文章、配图和后面生成的音频都放进来，再把 HyperFrames 的仓库链接交给 Codex，让它完成第一条主线的准备工作。

## 为什么有些文章视频能带来持续流量

Serena 做过一条很具体的尝试：她没有泛泛地讲"AI 可以做视频"，而是选了"图书拆解"这个内容领域，再用 Codex 和 HyperFrames 做成适合抖音发布的视频。

读者点进去，是因为他看见了一条完整路径：选一个已经有人看的领域，找到一本书，写成口播，做出视频，再把同样的结构重复用于下一本书。视频制作只是中间的执行方法，开头真正抓人的，是"这套方法能不能帮我把账号做起来"。

她的实际制作顺序也很值得照着走：先确定 30 秒、竖版和发布平台；单独写口播；把口播拆成几段；生成最终声音；按声音取得字幕时间；再让 HyperFrames 安排画面；预览、修改，最后渲染。

这里有一个很重要的顺序：先把要说的话和最终声音定下来，再做画面。声音改一句，字幕和镜头时间都会跟着变。过早做动效，只会让后面反复返工。

你不一定要做图书号。你可以拆解 AI 工具、解释工作方法、讲行业案例，也可以把自己的长文做成系列视频。先把内容范围固定下来，每条视频继续使用同一个结构，Codex 才能帮你越做越快。

## 第一步：把文章或主题变成口播稿

口播稿是整条视频的骨架。画面再漂亮，开头没有抓住人，后面的方法也说不明白，视频很难被看完。

这里有三条路。

### 方案一：只有一个主题，直接让 Codex 写

比如你准备做一条"Codex 如何做视频"，先告诉 Codex 四件事：讲给谁听、解决什么问题、视频多长、希望观众看完做什么。

```text
我要做一条 60 秒竖版视频，主题是"第一次用 Codex 做视频"。
观众只会使用 Codex，没有写过口播稿，也没有剪过视频。
开头先告诉他最终能做出什么，再讲清楚：生成口播、选择声音、用 HyperFrames 做画面、检查成片。
每句话尽量能直接念出来，不写书面化小标题，不堆概念。
结尾让他先用一个主题跑通第一条视频。
现在只交付口播稿，暂时不要做分镜和视频。
```

主题越具体，口播越容易写。把"做 AI 视频"缩成"把一篇工具测评做成 60 秒竖版讲解"，Codex 才知道哪些内容该留下。

### 方案二：已经有文章，让 Codex 改成能说出口的话

把文章保存为 article.md，连同文章里的图片一起放进项目文件夹。然后告诉 Codex：

```text
请读取 article.md，把它改成一条 90 秒口播稿。
保留文章的核心判断和最有用的案例，不要逐段缩写全文。
开头先说读者能得到什么，中间只保留三步方法，每一步都用能听懂的句子讲。
文章里的链接、脚注和排版符号不要读出来。
请把最后需要我核对的事实单独列出来。
先只生成 narration.md。
```

文章转口播时，最容易出现的问题是"字都认识，念起来不像人说的"。把 humanizer-zh 的链接交给 Codex，让它安装后处理 narration.md：保留事实和个人判断，只调整翻译腔、机械排比和过于书面的句子。

改完仍然要自己读一遍。嘴巴会替你发现很多眼睛看不出来的问题。

### 方案三：让 HyperFrames 接手整条不出镜视频

如果你只有材料，还没有想好口播和画面，可以从 /hyperframes 进入 /faceless-explainer。它会把主题、文章或笔记继续整理成讲解方向、旁白和分镜。

这条路省事，适合快速做第一版。前两种方案更适合你已经有明确观点，想牢牢控制口播内容的情况。

![手绘五步图：钩子 → 问题 → 方法 → 下一步 → 目标用户，中间卡通人物](assets/img-04-five-steps.jpg)

*（原文位置：方案三之后，口播四段说明之前）*

无论选哪一种，口播最好都有这四段：

1. 钩子：先把结果或具体问题摆出来。
2. 问题：告诉观众，他现在卡在哪里。
3. 方法：用两三步把过程讲清楚。
4. 下一步：给一个看完就能做的动作。

例如这篇教程的视频开头，可以直接说："装好 Codex 不知道干什么？你可以先用它做一条视频。给它一篇文章，它能继续处理口播、声音、字幕和画面。今天先跑通最短的一条流程。"

## 第二步：选择普通 TTS，还是复刻自己的声音

口播定稿以后，再决定由谁来念。第一次做，普通 TTS 已经够用；等内容能够稳定更新，再处理个人声音。

![双路径插画：普通 TTS（音箱设备）与我的声音（持麦克风的人物）汇入最终旁白，配音频波形](assets/img-05-voice-paths.jpg)

*（原文位置：第二步开头，标题与引言之后）*

### 最省事：使用现成 TTS

TTS 就是输入文字，得到 MP3 或 WAV。你可以使用手边已有的文字转语音功能，也可以把开源的 edge-tts 交给 Codex，让它完成安装、选择中文声音并生成音频和字幕。

先听十几秒，确认声音、语速和停顿，再生成全文。普通 TTS 不会变成你的声音，它的好处是快，适合验证内容和画面流程。

### 想长期使用自己的声音：CosyVoice

CosyVoice 是一套开源语音生成工具。给它一段干净的参考录音，再提供这段录音逐字对应的文本，它就可以用相近的说话人特征生成新的口播。

这里有三个东西不能混：

- reference.wav：你本人或已获得授权的参考录音。
- reference.txt：参考录音里实际说出的每一个字。
- narration.md：这次视频要生成的新口播。

参考文本必须和录音一致。你临时换了一个词，文本也要跟着改。背景音乐、回声和很重的降噪都会影响参考质量，尽量在安静环境录一段清楚、自然的声音。

CosyVoice 的本地安装比普通 TTS 多几步。纯小白可以把官方仓库直接交给 Codex：

```text
请帮我安装 CosyVoice： https://github.com/QwenAudio/CosyVoice
请先阅读官方 README，再检查当前电脑环境并完成安装。
安装好后只生成一条短测试音频，不要直接生成整篇口播。
```

它会在本地准备运行环境并下载语音模型，所需时间会比普通 TTS 长一些。你不用逐条执行命令；某一步失败时，把完整报错留在 Codex 对话里，让它继续检查。安装结束后仍然要亲自听测试音频，判断声音像不像、咬字是否自然。

生成正式口播时，不要一次把整篇文章塞进去。按语义拆成若干段，每段先试听，确认专有名词、英文和数字的读法，再合成 final.wav。其中一段有问题，只重做这一段就够了。

如果你暂时不想复刻声音，直接跳过 CosyVoice。后面的 HyperFrames 和 Remotion 只认最终音频文件，不在意它来自哪一种 TTS。

## 第三步：让画面跟着最终声音走

现在你手上应该有两个定稿文件：narration.md 和 final.wav。从这一刻开始，final.wav 才是视频的真实时间尺。

让 Codex 转写最终音频，取得每句话实际开始和结束的时间，再生成字幕。口播稿决定字幕写什么，音频转写负责告诉字幕什么时候出现。

不要按字数平均分配时间。同样是十个字，有的半秒说完，有的中间停了两次。字幕、关键词和图片都应该跟随真实声音出现。

![三轨同步插画：旁白（波形）/ 字幕（字幕卡）/ 画面（视频帧）三条轨道对齐，同步徽章](assets/img-06-sync-tracks.jpg)

*（原文位置：第三步，时间轴说明之后，代码块之前）*

可以直接这样交代：

```text
请以 final.wav 为准建立时间轴。
用 narration.md 核对字幕文字，用音频转写结果取得每句话的真实开始和结束时间。
输出 captions.srt 和 timing.json。
如果口播稿与实际声音不一致，请列出差异，不要自行猜测。
```

## 第四步：把口播拆成分镜

分镜不需要先学专业术语。把口播切成 4 到 8 段，每段只回答一个问题：观众听到这句话时，画面上最需要看到什么？

文章里本来就有截图、照片、图表或角色卡，优先使用这些素材。先让观众看清全貌，再放大正在讲的部分。没有现成图片的句子，可以使用关键词、步骤卡、对比、时间线和简单的信息图。

![分镜流程插画：原文配图（图表、照片）→ 挑选装盘 → 视频分镜（四格）](assets/img-07-storyboard.jpg)

*（原文位置：第四步，分镜说明之后，代码块之前）*

给 Codex 的要求可以很短：

```text
请根据 narration.md、timing.json 和 assets 文件夹制作分镜表。
每个镜头写清楚：对应口播、开始时间、结束时间、主要画面、需要的素材、字幕位置。
原文已有图片必须安排实际展示；缺少素材的地方先标记，不要用无关图片填满。
```

如果不知道动效长什么样，可以看两个公开的效果库：

- HyperFrames Launches：里面是完整的 HyperFrames 项目，可以查看成片怎样拆成分镜。
- video-shotcraft：提供 Remotion 镜头配方和动态预览，更适合产品介绍、界面演示和宣传片。

video-shotcraft 是可选的镜头参考库，不是文章视频的必装项。把仓库链接交给 Codex，它就可以读取里面的镜头配方。先把信息讲清楚，再选一两个合适的动效，画面会更稳。

## 第五步：用 HyperFrames 做出第一版视频

到这里，口播、声音、字幕时间和分镜都已经准备好。现在把这些文件交给 HyperFrames：

```text
使用 /hyperframes，把当前项目做成一条 9:16 竖版讲解视频。
请读取 narration.md、final.wav、captions.srt、timing.json、storyboard.md 和 assets 文件夹。
画面跟随真实音频时间，字幕不要挡住主要内容。
优先使用 assets 里的原图和角色卡，缺少素材的镜头用简洁的信息图。
先完成前 15 秒样片并打开预览，我确认方向后再继续全片。
```

先看前 15 秒，可以尽早发现三个问题：开头慢不慢，字幕看不看得清，画面有没有真的在解释口播。方向对了，再继续全片。

项目生成以后，直接告诉 Codex："打开 HyperFrames 本地预览，把预览地址和项目文件夹告诉我。"

修改时不要只说"高级一点""再好看一点"。告诉 Codex 具体对象和位置，例如："第 6 秒的标题太小""这张截图至少停留 3 秒""字幕挡住了人物""第一个镜头进入太慢"。这种反馈它更容易执行。

检查完成后，再告诉 Codex："把当前确认的版本渲染成 final.mp4，完成后检查文件能否正常播放。"

看到预览页面，只能说明项目能打开。真正完成的标志是 final.mp4 已经生成，并且你从头到尾看过一遍，确认声音没有截断、字幕没有错位、图片没有被裁掉。

## HyperFrames 和 Remotion 怎么选

![对比插画：HyperFrames 先做第一条（素材组装成首条视频）vs Remotion 复用固定栏目（模板长期复用）](assets/img-08-hf-vs-remotion.jpg)

*（原文位置：「HyperFrames 和 Remotion 怎么选」标题之后）*

![决策表：你的情况/更适合的选择 —— 第一次做视频希望尽快看到画面→HyperFrames；每条内容图片卡片节奏都变化→HyperFrames；已固定栏目样式每期只替换内容→Remotion；准备长期批量做同一种视频→Remotion](assets/img-09-decision-table.jpg)

*（原文位置：上图之后，「HyperFrames 更像……」段落之前）*

两者都可以把文字、声音、图片和时间轴做成视频。你只需要选一个作为最后的成片工具。

HyperFrames 更像让 Codex 根据这一期内容重新组织画面。Remotion 更像先做一套栏目模板，之后每一期往固定位置填内容。

想走 Remotion 路线，把 Remotion Agent Skills 的链接交给 Codex，让它完成安装。然后使用 /remotion-create 建立视频，使用 /remotion-studio 打开预览，最后用 /remotion-render 渲染。上游的 narration.md、final.wav、字幕、时间轴和图片都可以继续使用，不需要重新做一遍。

我的建议很简单：第一条先用 HyperFrames 跑通。连续做了几期以后，发现镜头结构已经稳定，再让 Codex 把它整理成 Remotion 的固定栏目。

## 如果你拍了真人口播，再加 video-use

前面的流程可以完全不出镜。等你愿意面对镜头时，把 narration.md 放在相机旁边，按段拍摄。说错了停一下，从这句话重新说，不必每次从头开始。

video-use 处理的是已经拍好的素材。它先转写视频，找出每一段说了什么，再和你确认哪些内容留下，之后才开始整理剪辑、字幕和补充画面。

![四步插画：真人拍摄（相机）→ 整理素材（删除废片）→ 补充画面（文字图片）→ 完成成片（手机上的成品视频）](assets/img-10-videouse-steps.jpg)

*（原文位置：video-use 介绍之后，安装代码块之前）*

安装时可以直接把这段话交给 Codex：

```text
请帮我安装 https://github.com/browser-use/video-use。
先阅读 install.md，完成依赖、FFmpeg 和 Codex Skill 注册。
安装后先不要转写任何视频，等我把素材放进文件夹。
```

video-use 需要完整安装整个仓库，因为它的剪辑助手和脚本都放在 Skill 旁边。它还需要 FFmpeg，这是实际处理视频和音频的底层工具。使用默认语音转写时，还要配置 ElevenLabs API Key，也就是让工具访问语音转写服务的一把密钥。把缺少的项目交给 Codex 检查即可。

拍摄完成后，把原视频放进一个单独文件夹，再告诉 Codex：

```text
请使用 video-use 处理这个文件夹里的真人口播。
先盘点素材并转写，找出重复、停顿、说错和重新开始的位置。
先给我一份剪辑方案，列出准备保留和删除的片段；我确认后再剪。
定稿后添加中文字幕，并在讲到步骤时使用 assets 里的对应图片补充画面。
最后输出预览版，检查无误后再输出 final.mp4。
```

这条支线的完整流程是：口播稿 → 真人拍摄 → video-use 整理有效片段 → 字幕和补充画面 → 成片。

HyperFrames 与 video-use 也可以一起用。video-use 先把真人口播剪顺，HyperFrames 再制作标题、步骤卡、截图演示和片头片尾。一个处理真人素材，一个处理信息画面，分工会更清楚。

## 最后，把视频变成一套内容获客系统

一条视频只能带来一次测试。真正有价值的部分，是把同一类问题持续做下去。Serena 的图书拆解能让人记住，是因为内容对象非常明确：一本书、一个值得讲的观点、一条短视频、下一本书继续使用同样的结构。你也可以换成自己的领域，例如每周拆一个 AI 工具、每次解决一个办公问题、每期解释一个行业案例。

这套内容循环可以很简单：

```text
目标用户的问题 → 选一个具体主题 → 写成口播稿 → 做成视频 → 看评论和完播反馈 → 决定下一条讲什么
```

获客的钩子也要落在具体结果上。"我用 AI 做了一条视频"只能说明你会使用工具；"我把一篇 3000 字的工具测评，做成一条 60 秒的新手教程"会让真正需要这件事的人停下来。

先不要急着搭一套庞大的自动化系统。选一个你能连续讲十期的主题，用 Codex 跑通第一条：口播定稿，声音生成，时间轴对齐，HyperFrames 出片。第二条继续复用文件结构，第三条开始保留固定片头和字幕样式。

做到这里，Codex 才真正从"帮你做过一条视频"，变成"陪你持续生产一类内容"。

我是 Miles，一名从大厂转型 FDE 的 AI 算法专家，做过算法研发、优化部署，也做过企业培训、落地交付，我的 X 账号，15 天做到 1 万粉，一周写出三篇百万曝光长文，其中两篇后来超过 200 万。

关注我 @miles_mazy，一起成长，一起赚钱。

![作者介绍卡：Miles 卡通形象手持剪贴板，AI 模型立方体、服务器、白板图标；文字「Miles / AI 算法专家 × FDE / 企业 AI 培训 · 落地 · 部署 / 关注我」](assets/img-11-author-card.jpg)

*（原文位置：文末，作者简介与关注引导之后）*

---

## 附：文中提到的外部链接

- HyperFrames Community Skills：https://github.com/heygen-com/hyperframes-community-skills
- video-shotcraft：https://github.com/Vincentwei1021/video-shotcraft
- humanizer-zh：https://github.com/ai-zixun/humanizer-zh
- edge-tts：https://github.com/rany2/edge-tts
- CosyVoice：https://github.com/QwenAudio/CosyVoice
- HyperFrames Launches：https://github.com/heygen-com/hyperframes-launches
- Remotion Agent Skills：https://github.com/remotion-dev/skills
- video-use：https://github.com/browser-use/video-use
- 关注作者：https://x.com/miles_mazy

## 附：媒体清单（共 12 项，按原文出现顺序）

| # | 类型 | 原文位置 | 本地文件 | 原始链接 |
|---|------|----------|----------|----------|
| 封面 | 图片 | 文章标题上方 | `assets/cover.webp` | https://pbs.twimg.com/media/HRqhnl_akAEpySt?format=webp&name=medium |
| 1 | 图片 | 引言 | `assets/img-01-pipeline.jpg` | https://pbs.twimg.com/media/HRqfaDvaQAAC2TC.jpg |
| 2 | 图片 | 「先把需要的工具交给 Codex」一节 | `assets/img-02-tools-table.jpg` | https://pbs.twimg.com/media/HRqfbg6bUAAwDk-.jpg |
| 3 | 图片 | 「先把需要的工具交给 Codex」一节末尾 | `assets/img-03-four-roles.jpg` | https://pbs.twimg.com/media/HRqfayBaAAANvq7.jpg |
| 4 | 图片 | 方案三 | `assets/img-04-five-steps.jpg` | https://pbs.twimg.com/media/HRqfaD4a0AAQRLH.jpg |
| 5 | 图片 | 第二步开头 | `assets/img-05-voice-paths.jpg` | https://pbs.twimg.com/media/HRqfaD7bwAACvDF.jpg |
| 6 | 图片 | 第三步 | `assets/img-06-sync-tracks.jpg` | https://pbs.twimg.com/media/HRqfazqbUAAQc9i.jpg |
| 7 | 图片 | 第四步 | `assets/img-07-storyboard.jpg` | https://pbs.twimg.com/media/HRqfaDybMAA2JgN.jpg |
| 8 | 图片 | 「HyperFrames 和 Remotion 怎么选」 | `assets/img-08-hf-vs-remotion.jpg` | https://pbs.twimg.com/media/HRqfa0macAAEQy_.jpg |
| 9 | 图片 | 「HyperFrames 和 Remotion 怎么选」 | `assets/img-09-decision-table.jpg` | https://pbs.twimg.com/media/HRqfbiIbkAAeCX6.jpg |
| 10 | 图片 | video-use 一节 | `assets/img-10-videouse-steps.jpg` | https://pbs.twimg.com/media/HRqfa1fbIAEfpeN.jpg |
| 11 | 图片 | 文末 | `assets/img-11-author-card.jpg` | https://pbs.twimg.com/media/HRqh8KiaMAA3Mom.jpg |

---

## 系列导航

- 上一篇：[第 3 期 Codex 实战系列｜零代码画出复杂架构图](../tutorial-03-codex-architecture-diagrams/tutorial-03-codex-architecture-diagrams.md)
- 下一篇：[第 5 期 FDE 落地实战｜Codex 自动生成企业经营报告](../tutorial-05-fde-business-reports/tutorial-05-fde-business-reports.md)
