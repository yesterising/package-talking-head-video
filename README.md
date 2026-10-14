# 口播视频语义包装 Skill

根据口播 SRT 的语义与时间码规划标题、侧卡、流程、对比、证据和简笔画，生成与口播对齐的动效工程或透明叠加素材。支持已有视频与 SRT，或仅提供 SRT 并指定画布尺寸和帧率。

本版本由 yesterising 维护，基于 [Nana AI 的上游项目](https://github.com/zhouyuechuan2025-ui/ai-self-media-video-packaging-skill) 在本机安装的 **0.2.0 代码快照**，保留 MIT 许可证和作者署名。它不代表上游最新版本。

## 默认 Agent 工作流

沿用用户当前工程和已对齐的字幕，直接制作正式素材；动画工作优先使用已安装的 HyperFrames 能力。叠加素材保留真实 Alpha，首帧有可见主体，避开人物与原字幕。全部素材与同步准备完成后一次批量导入剪映素材库，在 Codex 展示，由用户自行拖入和摆放时间线。默认不备份原片、不制作测试样片、不主动保存工程、不导出最终成片；用户本次明确指令优先。

本仓库包含规划、Remotion 渲染与 HyperFrames 适配源码，但不包含剪映控制工具或外部生成服务。实际导入、展示与直接 HyperFrames 渲染需要宿主提供对应能力。完整规则见 [SKILL.md](SKILL.md)。

## 能力

- 10 种语义结构：双侧信息轨、论点与证据、双向流、命令面板、四阶段流程、前后对比、证据面板、数值展示、信号路径与语义简笔画。
- 帧驱动、可 seek 的动效；人物和字幕安全区；已经固定在原画面的字幕不重复生成。
- 透明叠加 MOV 或显式选择的合成 MP4；不重剪或重排原口播。
- Remotion 源码、可选 HyperFrames 项目适配器、合成示例与运行逻辑测试。

## 安装

需要 Node.js 20+、FFmpeg 和 ffprobe。将完整仓库放入宿主技能目录：

```bash
git clone https://github.com/yesterising/package-talking-head-video.git "${CODEX_HOME:-$HOME/.codex}/skills/package-talking-head-video"
cd "${CODEX_HOME:-$HOME/.codex}/skills/package-talking-head-video"
npm ci
```

让宿主重新加载技能目录，提供准确的 SRT 和可选视频，例如：

> 使用 package-talking-head-video，为这段口播制作透明重点字幕和语义动效，全部做好后一次导入剪映，时间线由我摆放。

只提供 SRT 时，需要已知的尺寸、帧率及人物/字幕保留区；不能声称已经核对真实人物位置。原视频字幕已固定在画面中时使用 `burned-in`，需要新字幕时使用 `generated`，只要动效时使用 `none`。

## 可选四阶段 CLI

源码中仍保留上游的四阶段审核与导出 CLI。它是用户明确选择该模式时的路径，不作为日常直接剪辑的必经流程。各审核参数只代表用户对该具体任务的实际批准，不能用参数伪造批准。命令与输出见 [CLI 使用说明](docs/USAGE.md)，合成示例见 [示例](docs/EXAMPLES.md)，工具问题见 [排障](docs/TROUBLESHOOTING.md)。

## Companion skills

`video-editing-playbook`、`talking-head-pip`、`koubo-cut`、`hyperframes-core` 与 `hyperframes-animation` 按需安装，不随本仓库打包。缺少时只执行当前可用能力；人物后方文字需要实际可用的同步主体蒙版，不能仅凭提示词保证。

## 发布整理与开发验证

公开版保留源码、运行逻辑测试与合成 SRT，整理了本机绝对路径和安装说明。原作者的历史真实素材示例、预览图、营销联系方式及二维码不在本发布包内；原版锁定这些文案/素材的两份文档测试也未沿用，渲染器、规划器和 CLI 行为测试继续保留。

```bash
npm test
npm run typecheck
npm run build
npm run verify:public
```

MIT，见 [LICENSE](LICENSE) 和 [NOTICE](NOTICE)。Remotion、HyperFrames、第三方字体及外部素材遵循各自许可，本仓库的 MIT 不授予它们的使用权。
