# 绝区零 · 影画图鉴 / ZZZ Shadow Painting

一个纯静态的《绝区零》角色影画（Shadow Painting）3D 视差滚动图鉴。

鼠标滚轮 / 方向键切换角色，光标移动时三层影画产生景深视差与 3D 倾斜效果。

## 目录结构

```
.
├── index.html          # 单文件页面（内联 CSS + JS，零依赖）
├── files.json          # 图片索引（GitHub Pages 无目录列表，靠它发现图片）
├── 404.html            # 自定义 404（Pages 自动使用根目录的 404.html）
├── og-cover.jpg        # 社交分享封面 1200x630（og:image / twitter:image）
├── favicon.svg         # 矢量图标
├── favicon.ico         # 16/32/48 多尺寸图标
├── robots.txt
├── sitemap.xml
├── .nojekyll           # 关闭 Jekyll 处理
├── 可琳/ 叶瞬光/ 照/ 爱丽丝/ 琉音/ 艾莲/ 诺姆/
│   └── <角色>-影画1.webp    # 视差底层（暗色剪影）
│       <角色>-影画2.webp    # 视差中层（单色调）
│       <角色>-影画3.webp    # 视差顶层（全彩）
```

> 构建脚本不在本仓库内（PNG 原图约 230 MB），单独放在
> `D:\Desktop\ZZZ-原图备份\_tools\`：`resize.py`（重压 + 生成索引）、
> `make_og.py`（生成分享封面与图标）、`compress.py`（初版全量转 WebP）。

## 图片发现机制

`index.html` 按以下优先级自动发现图片，任一路径命中即可工作：

1. `list.txt` —— 手写清单，一行一个文件夹名或图片相对路径
2. 本地文件夹句柄（`showDirectoryPicker` 授权后存 IndexedDB，刷新自动重扫）
3. `files.json` —— **静态托管环境（GitHub Pages / 对象存储）的实际生效路径**
4. 目录列表 —— 仅在本机静态服务器等支持列目录的环境生效，且**只在第 3 步没命中时才试**

> 在 GitHub Pages 这类无目录列表的托管环境上，**`files.json` 是唯一起作用的兜底**，
> 新增角色后需要重新生成，否则新角色不会出现。

## 分层懒加载

页面不是一次性把 21 张图全拉下来的，而是按「层」给不同预载窗口：

| 图层 | 预载范围 | 原因 |
|---|---|---|
| 影画1（base） | 聚焦卡前后各 1.7 个卡片位 | 滑动时视线所及，不能空着 |
| 影画2 / 影画3 | **只有聚焦卡** | CSS 里非聚焦卡的这两层恒为 `opacity: 0`，根本看不见 |

实测首屏只请求 5 张图 / 约 389 KB（原先 21 张 / 10.31 MB）。
图片就位后淡入，加载中显示占位骨架，取不到则在那一层显示「素材缺失」。

## 图片规格

页面里卡片的最大渲染宽度是 638 CSS px（`Math.min(vw*0.52, 560)` × 聚焦时 `scaleFor` 的 1.14），
DPR=2 屏幕上需要 1276 px，所以基准档取 **1280 px**——再大就是纯浪费。

| 素材 | 档位 | 是否被页面加载 |
|---|---|---|
| 影画1/2/3 | 1280 px，WebP q86 | ✅ 是 |
| 立绘、立绘1 | 1600 px，WebP q84 | ❌ 否（`index.html` 零引用，仅留作扩展） |
| `<角色>.webp` 等全身图 | 1200 px，WebP q84 | ❌ 否 |

原始 PNG 素材未入库（体积约 230 MB，单独备份在 `D:\Desktop\ZZZ-原图备份`）。

## 重新压缩 / 新增角色

```bash
# 1) 把新角色的 PNG 丢进 D:\Desktop\ZZZ-原图备份\<角色>\
# 2) 重压 + 重建 files.json（档位规则写在脚本里）
D:\app\anaconda3\python.exe D:\Desktop\ZZZ-原图备份\_tools\resize.py
```

> `index.html` 里的 `INLINE_IMAGES` 数组是给「双击 file:// 直接打开」用的兜底清单，
> 新增角色时**这一处也要同步加**。

## 本地预览

```bash
python -m http.server 8000
# 浏览器打开 http://localhost:8000
```

## 部署

推送到 `main` 分支后由 GitHub Pages 自动发布。

> 注意：GitHub 网页端改自定义域名设置时会自己往 `main` 提交
> `Create CNAME` / `Delete CNAME`，本地仓库容易因此落后。
> push 前先 `git pull --rebase`。

## 说明

素材版权归 miHoYo / 上海米哈游影铁科技有限公司所有，本项目仅供个人学习与展示使用。
