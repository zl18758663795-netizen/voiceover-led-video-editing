# 旁白驱动的视频剪辑

把已有配音、字幕、实拍素材或剪辑工程，整理为可播放、可修改、可核对的短视频。

本项目是一套 Agent Skill：教代理如何确定剪辑范围、安排画面、同步字幕、处理返修并检查交付。实际渲染使用运行环境已有的 FFmpeg、剪辑软件或图形渲染能力。版本：**0.1.0**。

## 来自哪些经验

- 配音已经准备好时，按实际声音安排画面，不重新配音或强套文稿的预计时长。
- 实拍参考、已确认的字幕和局部修改要求，优先于工具的默认比例、模板和音乐。
- 整片变速需要同步声音、字幕、关键帧、转场与音效；原来的慢放镜头也要正确处理。
- 烧录字幕、软字幕和播放器字幕需要区分，避免一段话显示两遍。
- 工程保存、导出成功、技术检查、视觉检查和实际听音分别报告。

## 安装

把本仓库内 `skills/voiceover-led-video-editing/` 整个目录复制到所用 Agent 的技能目录。Codex 的首次安装例子：

```bash
git clone https://github.com/zl18758663795-netizen/voiceover-led-video-editing.git
cd voiceover-led-video-editing
mkdir -p "$HOME/.codex/skills"
cp -R "skills/voiceover-led-video-editing" "$HOME/.codex/skills/"
```

以上适用于首次安装。升级已有安装时先比较并备份本地修改，再替换整个对应目录，避免误覆盖定制或残留旧文件。其他支持 `SKILL.md` 的运行环境，使用各自文档指定的技能目录。安装后按运行环境要求刷新技能或开始新对话。

本 Skill 本身不要求额外 Python 包，也不自动安装软件或读取凭据。实际剪辑和检查需要相应后端已可用；具体 API 与版本按安装环境核实。

## 使用例子

```text
使用 $voiceover-led-video-editing。配音和 SRT 已经准备好，
你只负责画面，保留旁白、字幕文字和时间，按参考片做成可播放审核片。
```

```text
使用 $voiceover-led-video-editing。把这条片整体提速为1.06倍，
原版保留，所有声音、字幕和画面同步，并检查有没有重复字幕。
```

```text
使用 $voiceover-led-video-editing。仅替换22–28秒的背景素材，
保持配音、字幕、总时长和其余镜头不变，导出新的审核版。
```

## 目录与范围

- [技能入口](skills/voiceover-led-video-editing/SKILL.md)
- [时间线与字幕](skills/voiceover-led-video-editing/references/timeline-and-subtitles.md)
- [画面选择与工具接入](skills/voiceover-led-video-editing/references/visuals-and-tools.md)
- [验收交付与基础检查命令](skills/voiceover-led-video-editing/references/quality-and-handoff.md)
- [工作示例](skills/voiceover-led-video-editing/references/worked-examples.md)
- [来源与依赖边界](SOURCES.md)
- [本版验证记录](docs/validation.md)

支持解说、新闻混剪、B-roll 编排、字幕修复和局部返修。复杂多机位、精细调色、专业混音，以及具体剪辑软件的完整驱动接口需要相应专用能力。

本项目不附带私人素材、声音样本、第三方 Skill 源码或商业服务账户。MIT 许可覆盖本仓库原创文件；外部软件与素材适用各自许可。
