# 个人猫咪网站设计文档

**日期：** 2026-03-14
**用户：** 初中生，个人网站需求

---

## 项目概述

创建一个梦幻温柔风格的个人网站，主要展示用户家的四只小猫，每只猫拥有独立的专属页面，记录它们的照片、名字、性格特点和成长历程。

---

## 实现范围

本项目的实现范围包括：

- ✅ 构建 Hugo 网站结构（layouts、配置、样式）
- ✅ 创建所有页面模板（首页、关于我、猫咪详情页）
- ✅ 创建占位内容（用户可后续替换为真实内容）
- ✅ 实现响应式设计和导航功能
- ✅ 处理图片加载错误

**不包含（用户后续自行添加）：**
- 真实的猫咪照片（使用占位图片）
- 具体的猫咪信息（用户根据实际猫咪修改）
- GitHub Pages 部署配置（文档说明如何操作，但不在实现范围内）

---

## 技术方案

**选择：** Hugo + GitHub Pages

- Hugo 用于静态网站生成
- GitHub Pages 用于免费托管
- Markdown 用于内容编写

**选择理由：**
- 完全免费
- 学习曲线适中
- 性能优秀，加载快速
- 便于扩展功能
- 版本控制友好

---

## Hugo 配置

创建 `hugo.toml` 配置文件：

```toml
baseURL = "/"
languageCode = "zh-CN"
title = "我的小猫咪世界"
theme = ""  # 使用自定义主题

[params]
  author = "Hannah"
  description = "欢迎来到我的小猫咪世界"
```

---

## 架构设计

网站由以下独立单元组成，每个单元有清晰边界和接口：

### 单元 1：样式系统 (Styling System)

**职责：** 定义全局视觉风格、颜色变量、字体和响应式断点

**输出：** 可被所有页面引用的 CSS 变量和基础样式

**依赖：** 无

**接口：** 通过 CSS 变量导出颜色、字体、间距等设计令牌

### 单元 2：导航组件 (Navigation Component)

**职责：** 渲染顶部固定导航栏，处理页面高亮和移动端菜单

**输入：**
- Hugo 内置变量 `.RelPermalink` - 当前页面相对路径
- 遍历所有猫咪页面获取链接列表：`{{ range site.RegularPages }}`

**输出：** 导航 HTML 结构，包含：
- 固定顶部导航栏
- 当前页面的高亮样式（通过 `.IsMenuCurrent` 判断）
- 移动端汉堡菜单（通过 CSS 媒体查询控制显示/隐藏）

**依赖：** 样式系统（获取颜色、字体）

**接口实现示例：**
```go-html-template
<nav class="navbar">
  {{ range site.RegularPages }}
    <a href="{{ .RelPermalink }}" class="{{ if .IsMenuCurrent "main" . }}active{{ end }}">
      {{ .Title }}
    </a>
  {{ end }}
</nav>
```

### 单元 3：首页模板 (Home Page Template)

**职责：** 渲染欢迎语和猫咪卡片列表

**输入：**
- 首页 front matter 数据：`{{ .Title }}`, `{{ .Params.welcome_message }}`
- 所有猫咪页面列表：`{{ range where site.RegularPages "Section" "cats" }}`

**输出：** 首页 HTML

**依赖：** 样式系统、导航组件

### 单元 4：猫咪详情模板 (Cat Detail Template)

**职责：** 渲染单个猫咪的完整信息页面

**输入：** 单只猫咪的 front matter 数据
- `{{ .Title }}` - 名字
- `{{ .Params.cover_image }}` - 封面图
- `{{ .Params.personality }}` - 性格
- `{{ .Params.gender }}` - 性别
- `{{ .Params.age }}` - 年龄
- `{{ .Params.growth_records }}` - 生长记录数组
- `{{ .Params.additional_images }}` - 额外图片数组

**输出：** 猫咪详情页 HTML

**依赖：** 样式系统、导航组件

### 单元 5：关于我模板 (About Page Template)

**职责：** 渲染简单的自我介绍页面

**输入：**
- about.md 的 front matter：`{{ .Title }}`
- about.md 的正文内容：`{{ .Content }}`

**输出：** 关于我页面 HTML

**依赖：** 样式系统、导航组件

### 单元 6：布局模板 (Base Layout)

**职责：** 提供 HTML 结构框架，包含 `<head>`、`<body>`、导航栏占位符

**输入：** 页面内容（通过 `{{ block "main" . }}{{ end }}` 机制）

**输出：** 完整 HTML 文档

**依赖：** 样式系统、导航组件

---

## 页面结构

| 页面 | 内容描述 |
|------|----------|
| **首页** | 欢迎语 + 四只小猫的卡片导航，点击进入各自的页面 |
| **关于我** | 简单的自我介绍 |
| **猫咪 1** | 照片 + 名字 + 性格 + 生长记录 |
| **猫咪 2** | 照片 + 名字 + 性格 + 生长记录 |
| **猫咪 3** | 照片 + 名字 + 性格 + 生长记录 |
| **猫咪 4** | 照片 + 名字 + 性格 + 生长记录 |

---

## 数据结构 (Front Matter Schema)

> **注意：** 以下为占位内容示例，用户后续根据实际情况修改

### 首页 (content/_index.md)

```yaml
---
title: "欢迎来到我的小猫咪世界"
welcome_message: "这里是四个毛孩子的家，每一只都是我的心头好"
---

欢迎来到我的网站！在这里，你可以认识我家四只可爱的小猫。
```

### 关于我 (content/about.md)

```yaml
---
title: "关于我"
---

你好！我是一个初中生，喜欢画画、听音乐，当然还有——撸猫！

这四只小猫是我们家的一员，它们给我带来了很多快乐。
希望你能喜欢我的网站！
```

### 猫咪页面 (content/cats/cat1/index.md)

```yaml
---
title: "毛毛"
cover_image: "/images/cats/cat1/cover.jpg"
personality: "活泼好动，喜欢追逐激光笔，是个小调皮鬼"
gender: "公"
age: "2岁"
birthday: "2022-03-15"
growth_records:
  - date: "2022-03-15"
    event: "来到我们家，小小的，好可爱！"
    image: "/images/cats/cat1/arrival.jpg"
  - date: "2022-06-01"
    event: "第一次打疫苗，很勇敢哦"
    image: "/images/cats/cat1/vaccine.jpg"
  - date: "2023-01-20"
    event: "一岁生日啦！给它准备了小鱼干蛋糕"
    image: "/images/cats/cat1/birthday.jpg"
additional_images:
  - "/images/cats/cat1/photo1.jpg"
  - "/images/cats/cat1/photo2.jpg"
---

毛毛是个特别活泼的小家伙，每天都能看到它在客厅里跑来跑去。
```

### 猫咪页面 2-4 (content/cats/cat2/index.md, cat3/index.md, cat4/index.md)

使用相同的结构，替换为对应的猫咪信息。初始创建时可复制 cat1 的内容作为占位。

---

## 占位图片方案

由于用户需要自行提供真实照片，实现时将使用以下占位方案：

1. **纯色占位图**：使用 SVG 生成带"猫咪照片"文字的占位图
2. **占位图片服务**：使用 `https://placehold.co/400x300/ffb6c1/333?text=猫+照片` 生成占位图

实现模板中图片加载逻辑：
```go-html-template
{{ $img := .Resources.GetMatch "cover.jpg" }}
{{ if $img }}
  <img src="{{ $img.RelPermalink }}" alt="{{ .Title }}">
{{ else }}
  <img src="https://placehold.co/400x300/ffb6c1/333?text=猫+照片" alt="{{ .Title }}">
{{ end }}
```

用户后续将真实图片放入对应目录后，Hugo 会自动使用真实图片。

---

## 内容组织方式

使用 Hugo 的 **页面包（Page Bundles）** 来组织：

```
hannah-website/
├── content/
│   ├── _index.md                    # 首页
│   ├── about.md                     # 关于我
│   └── cats/
│       ├── _index.md                # 猫咪列表页（可选，暂不创建）
│       ├── cat1/
│       │   ├── index.md             # 猫咪 1 内容
│       │   ├── cover.jpg            # 猫咪 1 封面图（用户后续添加）
│       │   └── photos/              # 猫咪 1 其他照片（用户后续添加）
│       ├── cat2/
│       │   └── index.md             # 猫咪 2 内容
│       ├── cat3/
│       │   └── index.md             # 猫咪 3 内容
│       └── cat4/
│           └── index.md             # 猫咪 4 内容
├── layouts/
│   ├── _default/
│   │   └── base.html                # 基础布局
│   ├── partials/
│   │   ├── head.html                # HTML head 部分
│   │   ├── nav.html                 # 导航栏组件
│   │   └── footer.html              # 页脚组件（可选）
│   ├── index.html                   # 首页模板
│   ├── about.html                   # 关于我模板
│   └── cats/
│       └── single.html              # 猫咪详情页模板
└── assets/
    └── css/
        └── main.css                 # 主样式文件
```

**图片引用方式：**
- 封面图：在 cat1/ 目录下命名为 cover.jpg，模板通过 `{{ .Resources.GetMatch "cover.*" }}` 获取
- 生长记录图片：使用占位图片服务，用户后续可在页面包内添加真实图片
- 额外图片：使用占位图片服务，用户后续可在页面包的 photos/ 目录添加

---

## 导航栏行为

### 桌面端（宽度 > 768px）
- 固定在页面顶部
- 显示所有页面链接：首页 | 关于我 | 毛毛 | 喵喵 | 咪咪 | 小白
- 当前页面链接高亮显示（背景色变化）
- 无下拉菜单

### 移动端（宽度 ≤ 768px）
- 固定在页面顶部
- 显示"菜单"图标（使用 ☰ 符号）
- 点击图标展开/收起导航链接列表（通过简单的 JavaScript toggle class）
- 当前页面链接高亮显示
- 点击任意链接后自动收起菜单

### 交互细节
- 导航栏背景色：使用主色调（浅粉 `#ffb6c1`）
- 链接悬停效果：文字颜色加深 + 轻微下划线
- 平滑滚动：点击锚点链接时平滑滚动到目标位置

### 当前页面高亮实现
```go-html-template
{{ $currentPage := . }}
<nav>
  {{ range site.RegularPages }}
    <a href="{{ .RelPermalink }}" class="{{ if eq $currentPage.RelPermalink .RelPermalink }}active{{ end }}">
      {{ .Title }}
    </a>
  {{ end }}
</nav>
```

---

## 视觉风格

| 元素 | 设计 |
|------|------|
| **背景色** | 柔和粉色 `#fff0f5`（淡薰衣草粉） |
| **主色调** | 天空蓝 `#87ceeb` + 浅粉 `#ffb6c1` |
| **字体** | 圆润可爱的字体（如 Quicksand 或 Nunito） |
| **圆角** | 卡片和按钮使用圆角（border-radius: 12px） |
| **阴影** | 轻微阴影增加层次感（box-shadow: 0 4px 12px rgba(0,0,0,0.1)） |
| **动画** | 鼠标悬停时卡片轻微上浮（transform: translateY(-5px)） |
| **间距** | 充足的留白，营造轻盈感 |

### CSS 变量定义

```css
:root {
  --color-bg: #fff0f5;
  --color-primary: #87ceeb;
  --color-secondary: #ffb6c1;
  --color-text: #333333;
  --font-family: 'Quicksand', 'Nunito', sans-serif;
  --border-radius: 12px;
  --shadow: 0 4px 12px rgba(0,0,0,0.1);
  --transition: all 0.3s ease;
}
```

---

## 响应式设计

| 设备 | 屏幕宽度 | 卡片布局 | 导航 |
|------|----------|----------|------|
| **桌面端** | > 1024px | 4列网格 | 水平展开 |
| **平板端** | 769px - 1024px | 2列网格 | 水平展开 |
| **手机端** | ≤ 768px | 单列布局 | 汉堡菜单 |

### 断点定义

```css
/* 手机端默认 */
.grid-container { grid-template-columns: 1fr; }

/* 平板端 */
@media (min-width: 769px) {
  .grid-container { grid-template-columns: repeat(2, 1fr); }
}

/* 桌面端 */
@media (min-width: 1025px) {
  .grid-container { grid-template-columns: repeat(4, 1fr); }
}
```

---

## 错误处理

### 缺失图片
- **行为**：显示占位图片（带"猫+照片"文字）
- **技术**：使用 `{{ with .Resources.GetMatch }}` 检查图片存在性，不存在时使用 placehold.co

### 空生长记录
- **行为**：显示"暂时还没有成长记录哦~"
- **技术**：检查 `growth_records` 数组长度，为 0 时显示提示

### 空额外图片
- **行为**：不显示额外图片区域
- **技术**：检查 `additional_images` 数组长度，为 0 时不渲染该区域

### 缺失字段
- **行为**：显示默认值或跳过该部分
- **技术**：使用 `{{ with .Params.field }}` 条件渲染

### 空的页面内容
- **行为**：显示"内容更新中..."提示
- **技术**：检查页面内容长度

---

## 用户体验设计

1. **首页**：打开网站后，首先看到四只可爱的猫咪卡片，吸引眼球
2. **导航**：顶部的导航栏让用户可以随时切换页面
3. **猫咪页面**：点击卡片进入猫咪页面，展示详细信息
4. **生长记录**：按时间线展示猫咪的成长故事，有照片有文字
5. **返回**：每个页面都有"返回首页"按钮

---

## 项目完成标准

项目完成时应满足以下标准：

1. ✅ 可以通过 `hugo serve` 本地预览网站
2. ✅ 所有页面（首页、关于我、4个猫咪页）都能正常访问
3. ✅ 导航栏在桌面端和移动端都能正常工作
4. ✅ 页面样式符合设计的视觉风格
5. ✅ 使用占位图片，且占位图片显示正常
6. ✅ 响应式设计在不同屏幕尺寸下正常显示

---

## 后续步骤（用户操作）

实现完成后，用户需要：

1. **安装 Hugo**：按照 [Hugo 官方文档](https://gohugo.io/installation/) 安装
2. **添加真实图片**：将猫咪照片放入对应的 content/cats/catX/ 目录
3. **修改猫咪信息**：根据真实猫咪修改 cat1/cat2/cat3/cat4/index.md 中的内容
4. **修改个人信息**：修改 about.md 中的自我介绍
5. **部署到 GitHub Pages**：
   - 创建 GitHub 仓库
   - 推送代码
   - 在仓库设置中启用 GitHub Pages

---

## 后续可扩展功能

（暂不实现，未来可考虑）

- 猫咪视频展示
- 评论区
- 猫咪日记时间线
- 分享功能
