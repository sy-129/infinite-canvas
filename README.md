# 无限画布

零依赖、单文件（11 KB）的无限画布。打开 `index.html` 就能用，不需要构建、不需要服务器。

## 功能

- **平移**：拖空白处；移动端单指拖动
- **缩放**：滚轮 / 双指捏合，15% – 400%，缩放围绕指针位置
- **放东西**：图片（本地文件或 URL）、便签、文字
- **编辑**：双击便签或文字直接改
- **移动 / 删除**：拖动元素，选中后点右上角 ✕，或按 Delete
- **适应视图**：一键把所有内容收进屏幕
- **保存 / 载入**：存 localStorage，刷新自动恢复

## 部署

### GitHub Pages（推荐，两步）

1. 仓库 → **Settings → Pages**
2. Source 选 **Deploy from a branch**，分支 `main`、目录 `/ (root)`，Save

一分钟后就上线：`https://<你的用户名>.github.io/infinite-canvas/`

### 任意静态托管

把 `index.html` 丢到服务器根目录即可。Nginx 一行：

```nginx
location / { root /var/www/canvas; index index.html; }
```

### 本地

```bash
python3 -m http.server 8080
# 打开 http://127.0.0.1:8080
```

## 数据存在哪

只在浏览器 `localStorage` 的 `infcanvas_v1` 里。清缓存会丢，重要版面请定期用「图片」按钮把内容截图另存。

要持久化到服务端，接一个 POST 接口即可，`items` 数组就是全部状态：

```js
fetch('/api/board', { method: 'POST', body: JSON.stringify({ S, items }) })
```

## 为什么是手写的

试过三个现成项目，都没能直接部署：

| 项目 | 星标 | 卡在哪 |
|---|---|---|
| tldraw/tldraw | 51k | Vite 构建依赖 esbuild 原生二进制 |
| dgmjs/dgmjs | 1.7k | 同上 |
| mbrlabs/Lorien | 6.9k | Godot 桌面程序，不是网页 |
| excalidraw/excalidraw | — | 官方构建产物 2.5 MB，拉了三次都超时 |

所以按同样交互写了一个单文件版本，行为对齐常见无限画布：pan / zoom / 图元 / 保存。
