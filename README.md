# SoundScript 🎧

**听见每一个字** — 把任何 YouTube 视频变成带同步字幕的真实音频播放器，支持手机锁屏后台播放。

🔗 **在线使用**: [juliensorel2000.github.io/soundscript](https://juliensorel2000.github.io/soundscript)

---

## 功能

- **真实音频流**：使用 `<audio>` 标签直接播放提取的音频直链，而非 YouTube IFrame
- **手机后台播放**：关闭屏幕或切换 App 后继续播放
- **锁屏控制**：通过 MediaSession API 在锁屏界面显示标题、封面、播放/暂停/快进快退
- **字幕同步滚动**：实时高亮当前字幕，二分查找定位，点击跳转
- **音频可视化**：Web Audio API 驱动的频谱动画条
- **本地文件支持**：上传本地音频 + SRT/VTT 字幕文件离线使用
- **播放控制**：进度拖拽、±10s 快进快退、0.5×~2.0× 倍速、音量调节

---

## 架构

```
┌─────────────────────────┐     HTTPS      ┌─────────────────────────┐
│   前端 (GitHub Pages)   │ ─────────────► │   后端 (Render.com)     │
│                         │                │                         │
│   index.html            │ POST /extract  │   app.py (Flask)        │
│   - <audio> 播放        │ POST /subtitles│   - yt-dlp 提取音频URL  │
│   - MediaSession API    │                │   - 解析字幕 json3/vtt  │
│   - Web Audio 可视化    │ ◄────────────  │   - gunicorn 部署       │
└─────────────────────────┘  audio_url +   └─────────────────────────┘
                              subtitles
```

---

## 本地开发

### 前端

```bash
git clone https://github.com/JulienSorel2000/soundscript.git
cd soundscript
# 直接用浏览器打开（需要后端配合）
open index.html
```

### 后端

```bash
git clone https://github.com/JulienSorel2000/soundscript-api.git
cd soundscript-api
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python app.py
# 后端运行在 http://localhost:5000
```

本地调试时，将 `index.html` 中的 `API_BASE` 改为 `http://localhost:5000`。

---

## 部署

### 后端 (Render.com)

1. 访问 [render.com](https://render.com)，连接 `soundscript-api` GitHub 仓库
2. Render 会自动识别 `render.yaml` 配置
3. 部署完成后获得形如 `https://soundscript-api.onrender.com` 的 URL
4. 将前端 `index.html` 中的 `API_BASE` 替换为此 URL

### 前端 (GitHub Pages)

Settings → Pages → Source 选 `main` 分支，自动部署。

---

## 技术栈

| 层 | 技术 |
|---|---|
| 前端 | 纯 HTML/CSS/JS，单文件，零依赖 |
| 音频播放 | `<audio>` + MediaSession API |
| 可视化 | Web Audio API (AnalyserNode) |
| 字幕同步 | 二分查找，requestAnimationFrame |
| 后端 | Python · Flask · yt-dlp |
| 部署 | GitHub Pages (前端) · Render.com (后端) |
| 字体 | DM Mono · Instrument Serif · Noto Sans SC |

---

## License

MIT
