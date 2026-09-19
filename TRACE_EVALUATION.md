# TRACE 评测报告

评测日期：2026-09-20。评测方法：固定版本确定性 TRACE 基分 + 人工语义复核。语义复核确认付费任务授权、凭据脱敏、终态失败和任务恢复契约均与技能领域一致，本轮未做无证据的分数校准。

| Skill | T | R | A | C | E | Overall |
|---|---:|---:|---:|---:|---:|---:|
| volcengine-asr-transcribe | 5.0 | 4.6 | 4.5 | 4.7 | 4.5 | 4.64 |
| volcengine-audio-spec | 5.0 | 4.6 | 4.5 | 4.7 | 4.5 | 4.64 |
| volcengine-design-use | 5.0 | 4.6 | 4.5 | 4.7 | 4.5 | 4.64 |
| volcengine-image-generation | 5.0 | 4.6 | 4.5 | 4.8 | 4.5 | 4.65 |
| volcengine-tts-narration | 5.0 | 4.6 | 4.5 | 4.7 | 4.5 | 4.64 |
| volcengine-video-generation | 5.0 | 4.6 | 4.5 | 4.8 | 4.5 | 4.65 |

结论：6 个技能全部达到 4.5 发布门禁，平均 4.643；下一阶段应以受控 provider canary 增强真实运行证据，不能把静态 TRACE 误写成在线服务验证。

发布前复跑：

```bash
python3 scripts/trace_gate.py --evaluator <trace_evaluate.py> --threshold 4.5
```
