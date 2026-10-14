---
name: package-talking-head-video
description: Use when packaging center-presenter talking-head content from SRT, optionally with source video, into time-aligned motion graphics or a transparent overlay.
---

<!-- user-video-editing-policy -->
## 本用户剪辑执行规则（先读）

作为本用户视频剪辑的一环使用时，必须先读取并遵循 已安装的 `video-editing-playbook`（从当前宿主的 skill 目录定位；未安装时按本文件的剪辑规则工作），再执行本技能的制作部分。直接使用正式素材与当前已配置工程。agent 完成正式素材制作与同步准备，并负责导入当前素材库（剪映任务导入当前剪映工程素材库）；导入完成立即告知并停止。拖入、添加到时间线及摆放由用户自行完成，不弹确认、不等待拖入回复，也不通过按钮、快捷键或 API 代替用户放入时间线。后文中默认“完成时间线装配后才交付”的要求改为“正式素材导入素材库后交付”；只有用户另行明确要求 agent 执行时间线放置时才执行。后文、引用文档和示例中的初始/阶段确认、试听选样、测试/小样、质量验收、备份原始素材、主动保存/另存/重开、成片导出均为可选流程，只有用户另行明确要求对应操作时才执行；不得以“后台必要检查”补回这些步骤。

获取下一步操作必需的界面状态、转录时序、异步任务完成状态与工具内置参数校验属于实际制作，不追加成片测试。旧脚本若捆绑自动测试、保存、备份或导出，改用能直接完成制作的接口或编辑器操作，不能照搬整套旧流水线。正式动画/音频等工程必需的派生素材可以生成；单独要求素材生成、字幕文件或格式转换时只生成所要求的产物。仅真实缺失的信息或实际失败阻塞执行时提问；此偏好不扩大任务范围，也不绕过服务自身的权限、付费或身份同意要求。
正式视觉素材从素材时间 0（frame 0）就显示实际内容，首帧不能黑、白或全透明；入场动画从可见状态开始，不能把全部主体同时隐藏、缩为零、放在画面外或完全遮罩。此要求在制作时落实，不新增测试或验收流程。
交付前由 agent 把做好的正式视频动效素材在 Codex 中打开并直接展示，再告知完成；同时保留原有剪映素材库导入责任。只给路径或导入教程不算完成交付，不让用户自行导入 Codex，不为展示生成测试版或额外成片。正式交付素材保留在项目交付目录，不等于备份原片或主动保存工程。
剪映导入次数限制：全部正式素材与同步准备完成后，只在最后交付阶段执行一次批量导入；三个或更多素材可同批全部导入。制作中途不导入，不逐个开启新导入流程。提交后仅等待该批完成，不重复提交、不自动重试、不补导修订版；失败或部分成功如实告知，再次导入须由用户另行授权。导入后只展示交付成果并通知完成，轨道拖动和摆放由用户完成。
新增叠加动效必须输出真实 Alpha 透明文件：主体之外透明，拖入后露出原底片，不能用黑底、白底、绿幕待抠图或不透明 MP4 替代。首帧有可见主体，背景仍透明；制作、编码、音效封装和展示均保留 Alpha。除用户明确要求的全屏替换段外，不烘焙底片或铺整帧不透明背景。无法制作透明素材时报告实际阻塞，不导入不透明替代品；不追加测试或再次导入。
<!-- /user-video-editing-policy -->

# Package Talking-Head Video

Turn center-presenter talking-head content plus SRT into a reproducible, semantically directed packaging project. Accept either SRT-only transparent-overlay output or video-plus-SRT composite output. Preserve the source edit, voice, and burned-in captions unless the user explicitly requests otherwise.

For this user's video edits, automatically apply the installed `video-editing-playbook` skill (look it up through the host skill directory). For dense animation or material demonstrations, read its motion/editor reference. The user's default for applicable animation work is HyperFrames; current explicit authorization governs execution without repeated skill-selection confirmation.

For picture-in-picture requests, a bottom-right circular presenter, multiple presenter windows, or presenter resizing by screen information density, automatically read the installed `talking-head-pip` skill (look it up through the host skill directory). Its layout and source synchronization rules govern those scenes; the center-presenter protected area below applies only while the presenter remains centered.

For this user's caption and packaging work, read the installed `koubo-cut` reference `references/large-type-and-occlusion.md`; if unavailable, keep essential text outside the presenter area and use behind-presenter layers only with a usable synchronized matte. Prefer larger new captions and use selected behind-presenter text/animation beats. This shared reference governs sizing, subject mattes, synchronized layers, and motion QA; inspect renderer support before promising the effect.

## Direct editing workflow

1. Read the shared video-editing playbook. Use the current editor configuration, supplied source and existing SRT timing; obtain only the information needed to author and place this edit.
2. Choose semantic structures and write the requested composition directly. Use HyperFrames for this user's animation work; preserve unrelated existing speech, captions and cuts.
3. Generate only the formal animation assets needed for the edit, prepare timing and layers, and import them into the current editor media library. User performs timeline dragging and placement. Default to overlay assets for an existing project, rather than a flattened composite MP4.
4. When the formal assets are prepared and imported, immediately tell the user they are ready in the media library and stop. User handles timeline placement, testing, acceptance and export.

The legacy four-gate CLI combines approval flags, review stills, artifact QA and final rendering. Do not run that bundled pipeline for routine edits or pass approval flags to simulate approvals. Use the direct HyperFrames authoring/rendering path for needed animation assets and the editor interface for assembly. Consult legacy gate commands only when the user explicitly requests that review/export workflow.

## Output scope

Default delivery is the prepared formal assets imported into the current media library; the user places them on the timeline. For this user, a generated overlay must also be imported into the current media library; timeline placement is the user's part. An explicit request for a standalone overlay or composite file authorizes only that requested file; it does not add source backup, review stills, tests or approval rounds. Infer dimensions and timing from the active project rather than presenting an initial confirmation form.

## V2 semantic structures

Select by spoken meaning, never by palette rotation:

- `editorial-dual-rail`: two-sided facts, categories, or action lists.
- `thesis-and-proof`: one claim plus its reason, attribution, or proof boundary.
- `bidirectional-flow`: input/output, human/AI, or generate/review loops.
- `command-palette`: literal operations, checks, approvals, or run steps.
- `four-stage-pipeline`: three or four ordered production stages.
- `before-after-scrub`: exactly two anchored states and one comparison criterion.
- `evidence-panel`: a real approved screenshot or document plus source and interpretation.
- `metric-odometer`: one to three attributed or source-backed numbers.
- `signal-route`: three to five nodes connected by a traceable path.
- `semantic-doodle`: an integrated subject/action/outcome metaphor, never evidence.

## Non-negotiable visual rules

- Maintain one continuous source video/audio track. Do not silently recut or reorder it.
- While a 16:9 presenter remains centered, protect center x=35%–65%. Put ordinary critical copy in left x=5%–32% or right x=68%–95%, and reserve the bottom 18% for burned-in subtitles. In picture-in-picture scenes, protect the actual presenter window and caption bounds according to `talking-head-pip` instead.
- Every scene declares `presenter-safe`, `behind-presenter`, or `opaque-full-screen` mode.
- Presenter-safe scenes keep center x=35%–65% untouched by foreground graphics. Behind-presenter scenes may span the center only beneath a synchronized, verified subject matte; keep essential readable copy outside occluded areas. Without a usable matte, use presenter-safe or opaque full-screen mode.
- Side cards are content-fit: no fixed vertical fill, no decorative empty lower half, and no card taller than 64% of the frame. Prefer one compact rail over two sparse rails when the spoken content does not support two groups.
- Opaque full-screen scenes cover the complete 1920×1080 canvas with an opaque base. Keep critical copy above the caption reserve even when the source captions are hidden by the full-screen scene.
- Semantic palettes support structure; color alone never counts as a new layout.
- If captions are burned in, use `burned-in` and do not generate a second caption layer.
- Visual beats normally last 1.6–3.2 seconds and never exceed six seconds.
- Evidence panels require an actual source asset and readable label. Never invent metrics, logos, testimonials, or product capabilities.
- Every animation is frame-driven and seek-safe. No timers, random values, stateful CSS transitions, or poster-only tricks.
- Programmatic line illustrations must be integrated into the semantic layout, use explicit SVG `fill="none"`, and communicate subject, action, and outcome. They never prove a claim.
- Videos longer than 30 seconds require at least eight semantically valid structures; the same structure may not appear more than twice consecutively.
- Alternate ordinary left/right lanes. Do not place two ordinary overlays on the same side consecutively.
- A component gallery, HTML page or generated animation is not an assembled edit. Under the direct workflow, report prepared/imported assets accurately; place them on the timeline only when the user explicitly requests that step.
- Face safety, caption clearance and legibility guide authoring. Review stills, contact sheets and manual quality gates are used only when the user explicitly requests a review.

Read only the reference needed for the current step:

- Legacy gated review/export, only when explicitly requested: [references/gates.md](references/gates.md)
- Director and evidence rules: [references/director-rules.md](references/director-rules.md)
- Ten semantic structures: [references/visual-structures.md](references/visual-structures.md)
- Motion and integrated doodles: [references/motion-and-illustration.md](references/motion-and-illustration.md)
- Actual failure troubleshooting; QA only on explicit request: [references/qa-and-troubleshooting.md](references/qa-and-troubleshooting.md)
- Visual review criteria, only on explicit review request: [references/visual-quality-gates.md](references/visual-quality-gates.md)
