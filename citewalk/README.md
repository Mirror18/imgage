# citewalk 图库目录

项目隔离层。仓库根目录原有的时间戳图片和 `note/` 不属于本目录，本次没有移动、改名或覆盖。

## 目录规则

```
citewalk/cities/<city>/<category-or-cover>/<原有相对路径>
```

- 城市和分类沿用 `cityWalk-front` 分支 `h5/main` 里 `assets/` 已有的目录，不从文件名新编地点名。
- 广州保持原分类：`art`、`districts`、`overview`、`map`，以及它们下面已有的 `icons`、`miniatures`。
- 上海原图是 `assets/shanghai/<文件名>`，没有地点子目录。这些文件放进 `cities/shanghai/cover/`。`cover` 只表示「没有地点语义」，不是一个新地点。
- `assets/art`、`assets/bg`、`assets/icons` 没有城市目录，放进 `cities/_shared/`。`_shared` 不是城市名。
- 不补磁盘上不存在的照片。`data.js` 里写到的 `assets/` 图片路径都能在源目录找到。
- 广州 `art` 里有两张图只在磁盘上、`data.js` 没有写成字符串：`hero-huadu-hongxiuquan-home.jpg`、`hero-huadu-lake.jpg`。它们仍留在 `cities/guangzhou/art/`，不单独立地点。

源提交：`Mirror18/cityWalk-front` `h5/main` `2389ca669b7e0a236d2015b0e7e8b6f45b9dee34`。

## 给后端的 base

```
CITYWALK_STATIC_BASE_URL=https://raw.githubusercontent.com/Mirror18/imgage/<本目录所在提交的完整 SHA>/citewalk
```

提交 SHA 以 PR 头为准，不要用 `main` 或 `blob` 页面。前端或后端把以 `/cities/` 开头的路径接在这个 base 后面。

只有广州可以做这一次前缀替换后直接拼接：

| 原路径 | 图库 publicPath | 能否只换 base |
| --- | --- | --- |
| `assets/guangzhou/<其余路径>` | `/cities/guangzhou/<其余路径>` | 可以。`STATIC_BASE + publicPath` 就是文件 |
| `assets/shanghai/<文件名>` | `/cities/shanghai/cover/<文件名>` | 不可以。多了一层 `cover` |
| `assets/art\|bg\|icons/<其余路径>` | `/cities/_shared/art\|bg\|icons/<其余路径>` | 不可以。城市段是 `_shared`，不是把 `assets` 换成 `cities` |

`main` 分支压缩兜底图用的是 `static/cities/guangzhou/maps/`。本图库以 `h5/main` 原图为准，对应目录名是 `map`，不是 `maps`。

## 机器清单

`manifest.json` 每个文件一条：

- `source`：`h5/main` 的 `assets/...`
- `galleryPath`：本仓库相对路径
- `publicPath`：接在 `CITYWALK_STATIC_BASE_URL` 后面的路径
- `baseAligned`：`true` 表示 `assets/guangzhou/...` 与 `/cities/guangzhou/...` 对齐；`false` 表示必须用这条 `publicPath`，不能只替换 base
