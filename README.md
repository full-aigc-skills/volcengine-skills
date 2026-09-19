# volcengine-skills

面向 Codex、ZCode、Kimi 及其他 Agent Skills 客户端的火山引擎可复用技能包。

本仓包含 6 个技能，覆盖豆包录音识别、语音合成、音频规格、Seedream 图像、Seedance 视频和任务路由。插件宿主专属的 `volcengine-harness` 留在插件仓。

## 安装

```bash
npx skills add full-aigc-skills/volcengine-skills
npx skills add full-aigc-skills/volcengine-skills --skill volcengine-asr-transcribe
```

## 维护门禁

```bash
python3 scripts/lint_skills.py
python3 scripts/trace_gate.py \
  --evaluator ../../full-stack-skills-repositories/agent-skills/skills/skill-trace-evaluation/scripts/trace_evaluate.py \
  --threshold 4.5
```

正式 `v*` Release 发布后，仓库会把不可变 tag 与 peeled commit SHA 发送给 `full-aigc-plugins/volcengine-design-plugin`，由插件自动创建技能升级 PR。

<!-- FULL_STACK_DOC_START -->
## 项目定位与边界

`volcengine-skills` 是包含 **6 个可独立安装 Agent Skill** 的源代码仓库，当前清单版本为 `0.1.0`。本仓负责技能的触发说明、工作流、references、examples 与质量门禁；宿主插件的 Hook、MCP、凭据注入和运行时脚本不属于本仓职责。

| 已确认事实 | 值 | 证据 |
|---|---|---|
| 安装包 | `full-aigc-skills/volcengine-skills` | `.claude-plugin/plugin.json`、仓库远端 |
| 可安装技能 | 6 | `skills/*/SKILL.md` |
| 当前版本 | `0.1.0` | `.claude-plugin/plugin.json` |
| 规格事实源 | OpenSpec | `openspec/config.yaml` |
| 许可证 | Apache-2.0 | `LICENSE` |

### 不负责

- 不替代消费插件中的可执行 Harness、MCP 服务、Hook 或供应商客户端；
- 不把 `SKILL.md` 被复制到目录视为宿主已经发现、触发或成功执行；
- 不自动授权网络调用、付费生成、文件覆盖、上传或发布；
- 不允许消费插件直接修改受 `skills.lock.json` 管理的副本。

## 一眼看懂

```text
用户任务
  │
  ▼
name / description 发现技能
  │
  ▼
读取完整 SKILL.md ──► 按需加载 references / examples / scripts
  │
  ▼
执行领域工作流 ──► 收集验证证据 ──► PASS / FAIL / UNVERIFIED
```

## 已验证的安装与发现

```bash
npx skills add full-aigc-skills/volcengine-skills
npx skills add full-aigc-skills/volcengine-skills --skill volcengine-asr-transcribe
npx skills list --json
```

固定发布版本时使用 GitHub Release/tag，不要把移动的 `main` 当成不可变版本。安装完成后应核对技能数量、名称、资源文件和目标 Agent 列表；Codex、ZCode、Kimi 的真实插件加载仍需分别验证。

### 可安装技能

| 技能 | 触发范围摘要 |
|---|---|
| `volcengine-asr-transcribe` | Transcribe speech audio with Doubao bigmodel ASR: submit → wait → millisecond word-level timeline. This is the slicing base for 口播/short-form video editing — word timestamps drive  |
| `volcengine-audio-spec` | Spec knowledge for 豆包语音 credential families, audio formats, limits, and speaker resources. Use when debugging auth failures, choosing audio formats, or picking speakers. |
| `volcengine-design-use` | Router for Volcengine (火山引擎) creation workflows: Doubao ASR transcription with millisecond word timestamps, TTS narration, Seedream image generation, and Seedance video generation  |
| `volcengine-image-generation` | Generate images with Doubao Seedream via Ark /images/generations: text2image, reference-guided editing, sequential 组图. Use for 生图 tasks on Volcengine. |
| `volcengine-tts-narration` | Generate narration audio with Doubao TTS v3: speaker selection, long-text chunking, deterministic download. Use when a video needs 解说配音/旁白 or any text-to-speech. |
| `volcengine-video-generation` | Generate video with Doubao Seedance via Ark task API: t2v, i2v (first frame), FL2VA (first+last frame — white-model previs anchoring), and reference-image mode; submit-once with pe |

## 包结构与加载规则

```text
volcengine-skills/
├── .claude-plugin/plugin.json   # 包名、版本与技能清单
├── skills/<name>/SKILL.md       # 触发条件与主工作流
├── skills/<name>/references/    # 按任务加载的领域知识
├── skills/<name>/examples/      # 请求、验收与恢复示例
├── scripts/                     # 仓库级生成和质量门禁（若存在）
├── openspec/                    # 规格与归档变更
└── LICENSE
```

跨技能协作必须使用技能名和安装命令，不得依赖 `../sibling-skill/` 相对链接，因为用户可能只安装一个技能。

## 质量、发布与安全

```bash
python3 scripts/lint_skills.py
```

发布前还必须检查 frontmatter、相对链接、资源完整性、TRACE 阈值、版本清单以及干净环境安装。正式 tag 不得移动；内容变化应发布新版本，并让消费插件通过 tag、peeled SHA 和摘要更新锁文件。

安全边界：不得提交真实密钥、账号、本机绝对路径或私有仓库地址；脚本应默认最小权限，付费、上传、删除和覆盖动作必须保留显式授权门。

## 故障排查

| 现象 | 检查 | 处理 |
|---|---|---|
| 安装后未发现技能 | frontmatter、Agent 发现目录、是否需要刷新 | 用 `skills list --json` 核对实际发现结果 |
| 只安装单个技能后引用缺失 | 是否存在跨技能相对路径 | 把必需资源移入当前技能，或按名称安装依赖技能 |
| 插件完整性检查失败 | tag、peeled SHA、摘要和本地技能清单 | 在源技能仓发布新版本，再由同步 PR 更新插件 |
| 工具或凭据缺失 | `compatibility`、运行时前置条件 | 报告 `UNVERIFIED`，不要猜测成功 |
| 自动化第二次运行仍产生差异 | 生成器非幂等或清单漂移 | 阻止发布并修复生成/排序规则 |
<!-- FULL_STACK_DOC_END -->

## 许可证

Apache License 2.0。
