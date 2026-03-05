# SoundScript 🎧

**听见每一个字** — 把任何 YouTube 视频变成带同步字幕的音频播放器，支持手机锁屏后台播放。

🔗 **在线使用**: [juliensorel2000.github.io/soundscript](https://juliensorel2000.github.io/soundscript)

---

## 功能

- **纯前端，无需后端** — 单文件 `index.html`，部署在 GitHub Pages，零服务器成本
- **多源容错** — 依次尝试 Cobalt → Piped → Invidious，第一个成功即用，自动跳过失败来源
- **真实音频流** — 使用原生 `<audio>` 标签播放，而非 YouTube IFrame
- **手机后台播放** — 关闭屏幕或切换 App 后继续播放
- **锁屏控制** — MediaSession API 在通知栏/锁屏界面显示标题、封面、播放/暂停/快进快退
- **字幕同步** — 实时高亮当前字幕行，二分查找定位，点击跳转播放位置
- **音频可视化** — 播放时频谱动画条
- **本地文件** — 支持上传本地音频（mp3/wav/ogg）和 SRT/VTT 字幕文件
- **播放控制** — 进度拖拽、±10s 快进快退、0.5×~2.0× 倍速、音量调节

---

## 多源容错架构

第三方 YouTube API 不稳定是开源社区的普遍问题。SoundScript 同时支持三个独立来源，按优先级依次尝试，任何一个成功即停止：

### 音频来源

| 优先级 | 来源 | 说明 |
|--------|------|------|
| 1 | **[Cobalt](https://cobalt.tools)** | 活跃维护的媒体下载服务，提供免费公开 API |
| 2 | **[Piped](https://github.com/TeamPiped/Piped)** | 开源 YouTube 前端，轮询多个公开实例 |
| 3 | **[Invidious](https://invidious.io)** | 开源 YouTube 代理，轮询多个公开实例 |

### 字幕来源

| 优先级 | 来源 |
|--------|------|
| 1 | Piped API（subtitles 数组，VTT 格式） |
| 2 | Invidious API（captions 数组） |
| 3 | 用户手动上传 SRT/VTT 文件 |

### 架构图

```
浏览器 (GitHub Pages)
│
├── 粘贴 YouTube 链接 → 提取 videoId
│
└── resolveAudio(videoId)
    │
    ├─ 尝试 Cobalt API ──────────────→ api.cobalt.tools
    │   成功 → 直接使用音频 URL
    │   失败 ↓
    │
    ├─ 尝试 Piped（轮询实例）────────→ pipedapi.kavin.rocks
    │   成功 → audioStreams 最高码率    pipedapi.adminforge.de
    │   失败 ↓                          pipedapi.leptons.xyz
    │                                   piped-api.privacy.com.de
    └─ 尝试 Invidious（轮询实例）────→ inv.nadeko.net
        成功 → adaptiveFormats 最高码率 yewtu.be
                                        invidious.nerdvpn.de

<audio src="音频直链"> → 手机后台播放 ✓
fetch VTT 字幕 → 解析 → 二分查找同步高亮
MediaSession API → 锁屏控制 ✓
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
| [Cobalt API](https://cobalt.tools) | YouTube 音频提取（首选） |
| [Piped API](https://github.com/TeamPiped/Piped) | YouTube 音频流 + 字幕（备选） |
| [Invidious API](https://invidious.io) | YouTube 音频流 + 字幕（备选） |
| `<audio>` + MediaSession API | 后台播放 + 锁屏控制 |
| requestAnimationFrame + 二分查找 | 字幕实时同步 |
| DM Mono · Instrument Serif · Noto Sans SC | 字体 |
| GitHub Pages | 部署 |

---

## License

MIT
