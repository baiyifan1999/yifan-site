# HANDOFF — yifan-site

Last updated: 2026-09-01  
Repo: github.com/baiyifan1999/yifan-site  
Deploy: Vercel auto-deploys from `main` branch

---

## 当前任务 / Current Task

刚刚切换了 hero 背景图，从原来模糊风景照改成 El Lissitzky 构成主义作品（黑白几何图），用 `invert(1) brightness(0.4)` 滤镜让它适配黑色主题。CSS 已推送，**图片文件还没到位**（见"未完成"）。

---

## 已完成 / Done

| # | 改动 | commit |
|---|------|--------|
| 1 | 建站、替换占位符为真实信息（Yifan Bai、邮箱、GitHub） | `d858813` |
| 2 | 全站中英双语支持（`.zh` CSS class，所有文案加中文） | `d858813` |
| 3 | 文字颜色减淡：`--grey-400` → #c0c0c0，`--grey-200` → #e0e0e0 | `3976f9f` |
| 4 | 中文字颜色统一为 `--grey-400`（与英文副标题一致） | `37977df` |
| 5 | 新增 Translations 和 Notes 两个 section，及 6 个子页面 | `5c64481` |
| 6 | 上传真实笔记内容（EDP/OOSD/Prob 三个大文件） | `3b7df12` |
| 7 | Hero 区加背景图 + 深色蒙版 + 底部渐变淡出 | `f156fd3` |
| 8 | Hero 背景图加模糊滤镜（`blur(4px)`，用 `inset:-12px` 防边缘漏光） | `00db69a` |
| 9 | 所有卡片（Work / Translations / Notes）整块可点击 | `882c442` |
| 10 | 笔记文件排版疏松化（行高、间距、字重） | `94f2b19` |
| 11 | 笔记文件排版与主站对齐（DM Sans/DM Mono、h1-h3字重、分割线颜色） | `04fed8d` |
| 12 | Hero 背景换成 Lissitzky 构成主义图，改用 invert 滤镜适配暗色主题 | `25ffd3f` |

---

## 未完成 / Pending

### 🔴 紧急：Hero 背景图文件缺失

**现状：**
- `index.html` 里引用的是 `/912012absdl.jpg`
- 该文件**不在 git 里**，网站 hero 背景目前是黑色空白

**原因：** GitHub 上传界面不允许在上传时改名，用户还没有提交这个文件。

**解决办法（用户操作）：**
在 GitHub 网页上，进入 `baiyifan1999/yifan-site` 根目录 → "Add file" → "Upload files" → 上传 Lissitzky 图片，**文件名必须保存为 `912012absdl.jpg`**（保持原名），提交到 `main`。

或者：如果用户想用别的文件名，告诉我新名字，我改 `index.html` 里的引用即可，30 秒搞定。

**相关文件乱象（本地 repo 现状）：**
- `hero-bg.jpeg` — 2 字节空文件，无效，历史遗留
- `deleted` — 81KB JPEG，实际上是**原来的风景背景图**（GitHub 重命名操作的产物），可以之后清理掉

### 🟡 Stats 笔记页

`notes/stats.html` 是占位符页面，内容是 "Content coming soon."。  
等用户上传 MAST20005 Statistics 笔记内容再填充。

### 🟡 Translation 子页面

`translations/love-and-poison.html` 和 `translations/as-one.html` 都是占位符，内容 "Content coming soon."。等用户提供译文内容。

---

## 关键决策 / Key Decisions

| 决策 | 原因 |
|------|------|
| Vercel 部署从 `main` branch | 自动部署，推送即上线，无需手动操作 |
| 双语用 `.zh` CSS class | 语义清晰，不破坏英文 DOM 结构，中文作为副标题展示在英文下方 |
| 整块卡片可点击用 `::after` 伪元素 | 不改 HTML 结构，`.project-link::after { position:absolute; inset:0 }` 覆盖整个卡片，SEO 友好 |
| 笔记文件用 Python 替换而非 Edit 工具 | 三个笔记文件 710KB–2.5MB，超出 Read 工具 256KB 限制，必须用 Python 直接操作文件 |
| Hero 背景图用 `::before` 伪元素而非直接设 `background-image` | 这样可以对图片单独加滤镜（blur / invert），不影响其上面的文字内容 |
| Hero `::before` 用 `inset:-12px` + `overflow:hidden` | `blur(4px)` 会让边缘半透明漏光，`inset:-12px` 让图片比容器大一圈，`overflow:hidden` 裁掉多余部分 |
| Lissitzky 图用 `invert(1) brightness(0.4)` | 原图是米白底黑线，invert 后变成黑底白线，适配黑色主题；brightness(0.4) 降亮度防止太刺眼 |

---

## 重要文件 / Key Files

```
yifan-site/
├── index.html              # 主页，全部 section 都在这里
├── 912012absdl.jpg         # ⚠️ 还不存在！Hero 背景图（需用户上传）
├── hero-bg.jpeg            # ⚠️ 空文件（2 bytes），无用，可删
├── deleted                 # ⚠️ 旧风景背景图，误命名，可删
├── notes/
│   ├── edp.html            # COMP20008 笔记，~2.5MB，真实内容
│   ├── prob.html           # MAST20006 笔记，~915KB，含嵌入式 KaTeX 字体（base64）
│   ├── oosd.html           # SWEN20003 笔记，~710KB，真实内容
│   └── stats.html          # MAST20005 笔记，4KB，占位符
└── translations/
    ├── love-and-poison.html # 占位符
    └── as-one.html          # 占位符
```

---

## 已知问题 / Known Issues

1. **Hero 背景图不显示**（见"未完成"，最高优先级）

2. **hero-bg.jpeg 是空文件（2 bytes）**  
   GitHub 上"Rename hero-bg.jpeg to deleted"这个操作把真实图片内容放进了名叫 `deleted` 的文件，`hero-bg.jpeg` 变成了空文件。本地 git 状态反映了这个混乱。可以在适当时候清理。

3. **prob.html 里 KaTeX 字体是 base64 嵌入的**  
   任何 grep/Read 操作都会触发超长输出（整个文件都会被返回）。以后修改 prob.html 必须用 Python 脚本，过滤掉含 `base64` 的行再搜索。

---

## 踩过的坑 / Pitfalls to Avoid

### 1. 推送到错误分支
**坑：** 第一次推送到了 `claude/bilingual-portfolio-update-isetmy` 而不是 `main`，Vercel 不部署非 main 分支，网站没有更新。  
**避免：** 始终确认 `git branch` 当前是 `main`，push 命令明确写 `origin main`。

### 2. 图片文件名
**坑：** 第一次 hero 图用了 `url('/hero-bg.jpg')`，但实际文件是 `.jpeg`，404。  
**避免：** 图片文件名和 CSS 引用必须完全一致，包括扩展名大小写。用 `ls` 确认实际文件名再写 CSS。

### 3. 大文件不能用 Read/Edit 工具
**坑：** notes/edp.html（2.5MB）、prob.html（915KB）、oosd.html（710KB）超出 Read 工具 256KB 限制，直接 Read 会截断，Edit 工具找不到字符串。  
**方案：** 用 `head -n N` 读 CSS 头部，用 Python `open().read() / .replace() / write()` 做替换。

### 4. prob.html grep 会返回整个文件
**坑：** prob.html 里嵌入了大量 base64 字体数据，grep 某个 CSS 关键词时会把整个文件输出（base64 行包含所有字母组合）。  
**方案：** Python 里用 `[line for line in content.split('\n') if 'base64' not in line]` 过滤后再搜索。

### 5. 多行 CSS 替换的空白符要精确匹配
**坑：** 替换 `section.module>h2` 和 `.topic>h3` 时，Python 脚本用了猜测的换行格式，实际文件用的是不同缩进，导致两次 NOT FOUND。  
**方案：** 先用 `content.find('目标选择器')` 定位，然后 `repr(content[idx:idx+200])` 看真实字符（包括 `\n`、`\t`、空格），再写替换字符串。

### 6. 远端有新提交时 push 会被拒绝
**坑：** 用户在 GitHub 网页上传文件或重命名后，本地没有这些提交就直接 push，会报 `rejected (fetch first)`。  
**方案：** 先 `git pull origin main --rebase`，再 push。

### 7. GitHub 网页不能在上传时改名
**坑：** 用户在 GitHub 上传图片时，界面不允许改文件名。  
**方案：** 直接改 CSS 里的引用文件名，适配用户能上传的文件名，不要求用户改文件名。

---

## 接下来要做 / Next Steps

1. **用户上传 `912012absdl.jpg`** 到 GitHub repo 根目录（当前最高优先级，不上传 hero 是黑的）
2. 确认上线后检查 hero 显示效果（invert + brightness 是否合适）
3. 可选：清理 `hero-bg.jpeg`（空文件）和 `deleted`（旧图）
4. 等用户提供 Stats 笔记内容 → 填充 `notes/stats.html`
5. 等用户提供翻译内容 → 填充 `translations/` 两个子页面
