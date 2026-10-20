# code-motion-video

一个给 AI 编程助手用的「导演层」Skill：让会写代码的 Agent 先当导演、再写代码，用代码（Remotion / Manim / 任意逐帧渲染器）生成不像"AI 幻灯片"的动效视频。

## 作用

AI 用代码生成动效视频时，质量差距不在模型，而在有没有喂给它完整的**导演层**信息：分镜、层级、物理、打光、节拍、音效、自检。缺了这一层，产出必然回归默认套路——居中大字、霓虹渐变、所有元素同步淡入，看一眼就是"AI 做的"。

这个 Skill 把导演层固化成一套强制流程：

1. **先写导演 Brief，后写代码** —— 8 个槽位（目标 / 分镜与层级 / 轨迹与镜头 / 运动与物理 / 材质与光 / 节拍与声音 / 禁忌清单 / 验收标准）全部填满才允许动手
2. **分层错相，永不同步** —— 主体先动、次级层滞后、镜头再晚几帧、阴影最后；同一时刻只有一个主事件
3. **运动必须有物理因果** —— 每个动作写全链条：预备 → 加速/拉伸 → 撞击 → 挤压 → 过冲 → 稳定 → 次级响应，动量前后衔接
4. **先对齐节拍网格，再做微偏移** —— 重动作落在拍点上，音效长在运动里（速度→呼啸、冲量→撞击），在规划阶段就标好事件帧
5. **像导演一样自检** —— 渲染后整片看一遍，再逐帧步进弱点；出现"全层共用一条时间曲线、静止物体高光冻结、全屏乱晃、模板感发光/渐变"直接打回重做

## 应用场景

- **科普 / 讲解动画**：把一个概念讲清楚的知识类短视频（概念演示、原理动画、层层递进的讲解结构）
- **数据可视化动画**：图表生长、数字滚动、对比关系动态呈现
- **文字动效与片头**：标题动画、品牌片头、栏目包装
- **产品 / UI 演示**：界面操作演示、功能亮点动效展示
- **抢救"扁平感"成片**：已生成的代码动效视频看起来平、假、像模板时，用它重新补导演层再返工

不适用：剪辑已有实拍/录屏素材（那是 [existing-footage-editing](https://github.com/wuji5201314/existing-footage-editing) 的事）；纯实拍拍摄与调色。

## 配套 Skill（重要：本 Skill 必须搭配使用）

本 Skill 是**导演层**，只负责"拍什么、怎么动、什么标准算过"——它不写渲染代码，也不做后期混音。单独使用它，Agent 只能给出导演方案，落地执行会退回它自己的默认水平。完整的代码动效生产链是三层配合，缺一层都会掉质量：

```
code-motion-video（导演层 · 本仓库）
    │  产出：8 槽导演 Brief、分镜顺序、每层的时序/物理/打光、节拍与音效事件表、验收标准
    ▼
remotion-best-practices（实现层）
    │  按 Brief 写 Remotion 代码：逐层时序函数、弹簧/物理、合成与渲染
    ▼
ffmpeg-skill（后期层）
       混音与音效对齐事件帧、BGM 避让、响度标准化（约 −14 LUFS）、字幕、平台规格导出与交付检查
```

| 配套 Skill | 在链条中的作用 | 仓库 | 安装 |
|---|---|---|---|
| **remotion-best-practices**（Remotion 官方 Agent Skills 中的总入口，含 create / markup / captions / render 等子 Skill） | 实现层：把导演 Brief 翻译成可渲染的 Remotion 工程——组合结构、动画时序、字幕、渲染参数都按官方最佳实践走，避免 Agent 凭记忆乱写 API | [remotion-dev/skills](https://github.com/remotion-dev/skills) | `npx skills add remotion-dev/skills` |
| **ffmpeg-skill**（kajisho5） | 后期层：给 Agent 一套本地 FFmpeg 工具链（probe → 剪辑 → 检查 → 验证），负责成片后的混音、音效、BGM 侧链避让、响度、字幕烧录与多平台导出 | [kajisho5/ffmpeg-skill](https://github.com/kajisho5/ffmpeg-skill) | `npx ffmpeg-skill` |

配合方式（Agent 的实际工作顺序）：

1. 本 Skill 先行：填满 8 槽导演 Brief，用户确认后才进入下一步
2. `remotion-best-practices` 接手写码渲染：Brief 里的逐层时序、物理链、节拍事件表直接写进代码，禁止一个全局缓动糊全片
3. `ffmpeg-skill` 收尾：按 Brief 的音效事件表混音、做响度标准化与交付检查
4. 回到本 Skill 自检：对照 Brief 的验收标准看成片，不合格是导演层缺失——重新 Brief，而不是盲目重渲染

> 换渲染器（如 Manim）时只需替换实现层，导演层与后期层不变；本 Skill 不绑定任何具体渲染器。

## 使用方法

把本仓库放进你的 Agent 的 skills 目录，并**同时装好上面两个配套 Skill**，例如：

```bash
git clone https://github.com/wuji5201314/code-motion-video.git ~/.claude/skills/code-motion-video
npx skills add remotion-dev/skills   # 实现层
npx ffmpeg-skill                     # 后期层
```

然后对 Agent 说「用代码做一个 XX 动效视频」，它会先产出 8 槽导演 Brief 给你确认，再开始写代码。

## 文件结构

```
├── SKILL.md                  # Skill 主文件：工作流、产出契约、操作规则
├── skill.yaml                # Skill 元信息
└── references/
    └── techniques.md         # 技巧详解：Brief 模板、物理链、节拍网格、自检信号
```

## 来源

方法论提炼自观默 @guanmo_ai 的长文《用 Codex 自动生成顶尖动效视频》（2026-09-30，https://x.com/guanmo_ai/status/2105146205915283737），AI 动效作品参考画廊：https://github.com/guanmo-ai/awesome-ai-motion

在本作者的实际项目（AI 科普视频、自律主题短视频）中验证使用。

## License

MIT
