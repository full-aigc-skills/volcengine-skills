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

## 许可证

Apache License 2.0。
