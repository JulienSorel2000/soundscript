# SoundScript 🎧

**听见每一个字** — 把任何 YouTube 视频变成带同步字幕的音频播放器。

🔗 **在线使用**: [juliensorel2000.github.io/soundscript](https://juliensorel2000.github.io/soundscript)

## 功能

- 粘贴 YouTube 链接，即可在线播放
- 自动获取视频字幕，同步滚动高亮
- 📺 视频模式 / 🎧 纯听模式 一键切换
- 支持上传 SRT 字幕文件
- 播放速度调节（0.5x ~ 2.0x）
- 进度拖拽、快进快退、音量控制
- 点击任意字幕行跳转到对应位置

## 使用方式

打开网页 → 粘贴 YouTube 链接 → 点击解析 → 开始听

如果视频没有自动字幕，可以上传 `.srt` 字幕文件。

## 技术栈

- 纯前端，单文件 HTML/CSS/JS
- YouTube IFrame Player API
- 二分查找实现字幕同步
- 部署于 GitHub Pages，零后端

## 本地运行

```bash
git clone https://github.com/JulienSorel2000/soundscript.git
cd soundscript
open index.html
```

## License

MIT
