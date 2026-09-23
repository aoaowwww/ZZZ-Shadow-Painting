# 绝区零 · 影画图鉴 / ZZZ Shadow Painting

一个纯静态的《绝区零》角色影画（Shadow Painting）3D 视差滚动图鉴。

鼠标滚轮 / 方向键切换角色，光标移动时三层影画产生景深视差与 3D 倾斜效果。

## 目录结构

```
.
├── index.html          # 单文件页面（内联 CSS + JS，零依赖）
├── files.json          # 图片索引（GitHub Pages 无目录列表，靠它发现图片）
├── .nojekyll           # 关闭 Jekyll 处理
├── 可琳/ 叶瞬光/ 照/ 爱丽丝/ 琉音/ 艾莲/ 诺姆/
│   └── <角色>-影画1.webp    # 视差底层
│       <角色>-影画2.webp    # 视差中层
│       <角色>-影画3.webp    # 视差顶层
└── 派派/               # 占位目录（暂未收录素材）
```

## 图片发现机制

`index.html` 按以下优先级自动发现图片，任一路径命中即可工作：

1. `list.txt` —— 手写清单，一行一个文件夹名或图片相对路径
2. 本地文件夹句柄（`showDirectoryPicker` 授权后存 IndexedDB，刷新自动重扫）
3. 目录列表 —— 仅在本机静态服务器等支持列目录的环境生效
4. `files.json` —— **静态托管环境（GitHub Pages / 对象存储）的实际生效路径**

> 在 GitHub Pages 这类无目录列表的托管环境上，**`files.json` 是唯一起作用的兜底**，
> 新增角色后需要重新生成，否则新角色不会出现。

## 本地预览

```bash
python -m http.server 8000
# 浏览器打开 http://localhost:8000
```

## 部署

推送到 `main` 分支后由 GitHub Pages 自动发布。

## 图片规格

- 全分辨率 WebP（quality 88，保留 alpha），单张约 0.5–2MB
- 原始 PNG 素材未入库（体积约 230MB，单独备份）

## 说明

素材版权归 miHoYo / 上海米哈游影铁科技有限公司所有，本项目仅供个人学习与展示使用。
