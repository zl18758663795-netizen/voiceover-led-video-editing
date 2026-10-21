# 验收与交付

## 检查顺序与边界

1. **技术检查**：真实路径、文件非空、画幅、帧率、时长、视频/音频轨、完整解码。检查是否意外留下额外音轨、字幕轨或审核界面。
2. **内容与时间**：开头、末句、每个修改区间及前后切点；语句与画面任务对应；字幕时间、数字、专名、来源标签可读，音效与动作同步。
3. **视觉检查**：按切点和内容抽取关键帧，尤其长字幕、转场、图表、竖屏裁切和片尾。检查重复字幕、黑帧、冻结、伪文字、人物/物体连续性。黑场或静帧有时是设计，检测结果要回到时间点判断。
4. **声音与播放**：完整播放成片，并实际试听声音；检查爆音、漏音、音量突变、音乐遮盖、音调、变速接点、末字与停顿。只执行浏览器 `play()`、读 `ended` 或解码日志，不能称为已听完整片。

每项记录 `pass / fail / not_checked / not_applicable` 以及证据。缺少实时听音或完整视听能力时明确标记，继续交付可供用户检查的审核片；不能伪填 `pass`。不把“技术通过”自动升级为“最终观感合格”。

检查绑定本次成片的路径、哈希和版本。改字幕、音频、图层、速度或编码设置后，按受影响范围重新检查，新导出至少重新确认技术参数和改动接点。只改文件名、且字节不变时，可沿用该文件的内容检查证据。

## 可运行的基础检查

先确认本机 `ffmpeg` 与 `ffprobe` 可用；运行工作目录为当前导出目录。下面使用的 `review.mp4` 是本次实际导出文件名，不是声明它已经存在。命令只覆盖技术检查和取帧，不能自动进行主观验收。

```bash
ffprobe -v error -show_format -show_streams -of json "review.mp4"
```

```bash
ffmpeg -hide_banner -nostdin -v error -xerror -i "review.mp4" -map 0:v:0 -map '0:a?' -f null -
```

```bash
ffmpeg -hide_banner -nostdin -v error -n -ss 0 -i "review.mp4" -frames:v 1 "check-start.png"
```

```bash
ffmpeg -hide_banner -nostdin -v error -n -sseof -1 -i "review.mp4" -frames:v 1 "check-tail.png"
```

按真实切点另外取帧；首尾两帧不足以检查整片。`-n` 防止覆盖已有图片，再次检查时采用本轮文件名。

## 交付状态

| 状态 | 可以报告什么 |
|---|---|
| 工程准备完成 | 已整理素材、时间线或可编辑工程；MP4尚未导出 |
| 部分预览 | 可播放，但含占位、缺素材或未完成段落，指出具体范围 |
| 审核片 | 已完成本轮剪辑并导出，逐项报告已查与未查项目 |
| 最终母版 | 约定的技术、内容、视觉和声音检查已完成且无阻塞问题；若用户要求最终确认，确认也已取得 |

用户是否验收是独立字段。最终母版也不代表已归档、上传或发布；这些动作按实际执行结果报告。

精简检查记录示例，仅表达记录格式：

```json
{
  "artifact": "exports/review-v2.mp4",
  "stage": "review",
  "technical": {"status": "not_checked", "evidence": []},
  "subtitles": {"status": "not_checked", "evidence": []},
  "visual": {"status": "not_checked", "evidence": []},
  "full_playback": {"status": "not_checked", "evidence": []},
  "audio_listening": {"status": "not_checked", "evidence": []},
  "user_acceptance": "pending",
  "known_issues": []
}
```

实际使用时添加本次文件哈希、检查时间和真实证据；示例中的路径和状态不是一次验收结果。

## 交付内容

先给可直接播放的 MP4，再给需要的工程、SRT、简短改动清单和剩余检查。保留原版与当前有效版本，指出被替代的旧版，避免把失败版本作为当前成品一起交付。

需要 ZIP 时检查 CRC、文件清单与哈希。只包含本次交付所需文件；公开 Skill 包与私人视频工程分开，不携带私人音色、凭据、浏览器数据、绝对机器路径或未经允许公开的媒体。
