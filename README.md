# SoundScript 🎧

**听见每一个字** — 把任何 YouTube 视频变成带同步字幕的音频播放器，支持手机锁屏后台播放。

🔗 **在线使用**: [juliensorel2000.github.io/soundscript](https://juliensorel2000.github.io/soundscript)

---

## 功能

- **纯前端，无需后端** — 直接调用 [Piped](https://github.com/TeamPiped/Piped) 公开 API 获取音频流和字幕，零服务器成本
- **真实音频流** — 使用原生 `<audio>` 标签播放，而非 YouTube IFrame
- **手机后台播放** — 关闭屏幕或切换 App 后继续播放
- **锁屏控制** — MediaSession API 在通知栏/锁屏界面显示标题、封面、播放/暂停/快进快退
- **字幕同步** — 实时高亮当前字幕行，二分查找定位，点击跳转播放位置
- **音频可视化** — 播放时频谱动画条
- **多实例容错** — 自动尝试多个 Piped 公开实例，单个宕机不影响使用
- **本地文件** — 支持上传本地音频（mp3/wav/ogg）和 SRT/VTT 字幕文件
- **播放控制** — 进度拖拽、±10s 快进快退、0.5×~2.0× 倍速、音量调节

---

## 架构

```
浏览器 (GitHub Pages)
│
├── 粘贴 YouTube 链接
├── 提取 videoId
│
└── fetch Piped API  →  pipedapi.kavin.rocks/streams/{id}
                         pipedapi.adminforge.de/streams/{id}
                         ...（自动容错）
                    ←   { audioStreams[], subtitles[], title, thumbnail }
│
├── <audio src="最高码率音频直链"> → 手机后台播放 ✓
├── fetch VTT 字幕 → 解析 → 同步高亮
└── MediaSession API → 锁屏控制 ✓
```

---

## 本地运行

```bash
git clone https://github.com/JulienSorel2000/soundscript.git
cd soundscript
open index.html   # 直接用浏览器打开即可，无需本地服务器
```

---

## 部署

静态文件，直接用 GitHub Pages：

Settings → Pages → Source: `main` 分支 → Save

---

## 技术栈

| 技术 | 用途 |
|------|------|
| 原生 HTML/CSS/JS | 单文件，零依赖，零构建 |
| [Piped API](https://github.com/TeamPiped/Piped) | YouTube 音频流 + 字幕提取 |
| `<audio>` + MediaSession API | 后台播放 + 锁屏控制 |
| requestAnimationFrame + 二分查找 | 字幕实时同步 |
| DM Mono · Instrument Serif · Noto Sans SC | 字体 |
| GitHub Pages | 部署 |

---

## License

MIT
