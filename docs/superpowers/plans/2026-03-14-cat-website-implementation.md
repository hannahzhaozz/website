# 个人猫咪网站实现计划

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 创建一个梦幻温柔风格的 Hugo 静态网站，展示四只小猫，每只猫有独立页面，包含照片、名字、性格和成长记录。

**Architecture:** 使用 Hugo 静态网站生成器，自定义主题包含样式系统、导航组件、页面模板。网站结构清晰，每个文件职责单一。使用占位图片服务（placehold.co）代替真实图片。

**Tech Stack:** Hugo（静态网站生成）、CSS3（样式）、JavaScript（移动端菜单交互）、Markdown（内容）

---

## File Structure

本项目将创建以下文件：

| 文件 | 职责 |
|------|------|
| `hugo.toml` | Hugo 配置文件，定义站点基本信息 |
| `assets/css/main.css` | 全局样式系统，定义 CSS 变量和响应式设计 |
| `assets/js/menu.js` | 移动端菜单展开/收起交互逻辑 |
| `layouts/_default/base.html` | 基础 HTML 布局，所有页面共享 |
| `layouts/partials/head.html` | HTML `<head>` 部分，包含字体和 meta 标签 |
| `layouts/partials/nav.html` | 导航栏组件，处理页面高亮和响应式 |
| `layouts/index.html` | 首页模板，显示欢迎语和猫咪卡片 |
| `layouts/about.html` | 关于我页面模板 |
| `layouts/cats/single.html` | 猫咪详情页模板，显示完整信息 |
| `content/_index.md` | 首页内容（占位） |
| `content/about.md` | 关于我页面内容（占位） |
| `content/cats/cat1.md` | 猫咪1内容（占位） |
| `content/cats/cat2.md` | 猫咪2内容（占位） |
| `content/cats/cat3.md` | 猫咪3内容（占位） |
| `content/cats/cat4.md` | 猫咪4内容（占位） |

---

## Chunk 1: 基础配置和样式系统

### Task 1: 创建 Hugo 配置文件

**Files:**
- Create: `hugo.toml`

- [ ] **Step 1: 创建 hugo.toml 配置文件**

```toml
baseURL = "/"
languageCode = "zh-CN"
title = "我的小猫咪世界"
theme = ""

[params]
  author = "Hannah"
  description = "欢迎来到我的小猫咪世界"
```

- [ ] **Step 2: 验证配置文件语法**

Run: `hugo config`
Expected: 配置信息正确输出，无错误

- [ ] **Step 3: 提交**

```bash
git add hugo.toml
git commit -m "feat: add hugo configuration"
```

### Task 2: 创建样式系统

**Files:**
- Create: `assets/css/main.css`

- [ ] **Step 1: 创建主样式文件**

```css
/* 全局变量 */
:root {
  --color-bg: #fff0f5;
  --color-primary: #87ceeb;
  --color-secondary: #ffb6c1;
  --color-text: #333333;
  --font-family: 'Quicksand', sans-serif;
  --border-radius: 12px;
  --shadow: 0 4px 12px rgba(0,0,0,0.1);
  --transition: all 0.3s ease;
}

/* 基础样式 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: var(--font-family);
  background-color: var(--color-bg);
  color: var(--color-text);
  line-height: 1.6;
}

/* 容器 */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* 导航栏 */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: var(--color-secondary);
  padding: 1rem 2rem;
  box-shadow: var(--shadow);
  z-index: 1000;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-links {
  display: none;
  list-style: none;
}

.nav-links li {
  margin-left: 1rem;
}

.nav-links a {
  text-decoration: none;
  color: var(--color-text);
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: var(--border-radius);
  transition: var(--transition);
}

.nav-links a:hover {
  background-color: rgba(255, 255, 255, 0.3);
  text-decoration: underline;
}

.nav-links a.active {
  background-color: var(--color-primary);
  color: white;
}

/* 菜单按钮 */
.menu-toggle {
  display: block;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0.5rem;
}

/* 移动端菜单展开 */
.nav-links.active {
  display: flex;
  flex-direction: column;
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background-color: var(--color-secondary);
  padding: 1rem;
  box-shadow: var(--shadow);
}

.nav-links.active li {
  margin: 0.5rem 0;
}

/* 主内容区域 */
.main-content {
  margin-top: 80px;
  min-height: calc(100vh - 80px);
}

/* 欢迎区域 */
.welcome {
  text-align: center;
  padding: 3rem 1rem;
}

.welcome h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  color: var(--color-secondary);
}

.welcome p {
  font-size: 1.2rem;
  color: #666;
}

/* 网格容器 */
.grid-container {
  display: grid;
  gap: 2rem;
  padding: 2rem 0;
  grid-template-columns: 1fr;
}

/* 卡片 */
.card {
  background: white;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow);
  overflow: hidden;
  transition: var(--transition);
  cursor: pointer;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.15);
}

.card-image {
  width: 100%;
  height: 250px;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
}

.card-title {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
  color: var(--color-primary);
}

.card-text {
  color: #666;
  line-height: 1.5;
}

/* 猫咪详情页 */
.cat-detail {
  max-width: 800px;
  margin: 2rem auto;
  background: white;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow);
  overflow: hidden;
}

.cat-cover {
  width: 100%;
  height: 400px;
  object-fit: cover;
}

.cat-info {
  padding: 2rem;
}

.cat-header {
  display: flex;
  align-items: center;
  margin-bottom: 2rem;
}

.cat-name {
  font-size: 2.5rem;
  color: var(--color-primary);
  margin-right: 1rem;
}

.cat-tags {
  display: flex;
  gap: 0.5rem;
}

.tag {
  background: var(--color-secondary);
  color: white;
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-size: 0.9rem;
}

.cat-section {
  margin-bottom: 2rem;
}

.cat-section h2 {
  font-size: 1.8rem;
  color: var(--color-secondary);
  margin-bottom: 1rem;
}

/* 生长记录时间线 */
.timeline {
  border-left: 3px solid var(--color-secondary);
  padding-left: 2rem;
  margin-left: 1rem;
}

.timeline-item {
  position: relative;
  margin-bottom: 2rem;
}

.timeline-item::before {
  content: '';
  position: absolute;
  left: -2.6rem;
  top: 0;
  width: 1rem;
  height: 1rem;
  background: var(--color-primary);
  border-radius: 50%;
  border: 3px solid var(--color-secondary);
}

.timeline-date {
  font-weight: 600;
  color: var(--color-primary);
  margin-bottom: 0.5rem;
}

.timeline-content {
  background: var(--color-bg);
  padding: 1rem;
  border-radius: var(--border-radius);
}

.timeline-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: var(--border-radius);
  margin-top: 1rem;
}

/* 额外图片 */
.additional-images {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}

.additional-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: var(--border-radius);
  transition: var(--transition);
}

.additional-image:hover {
  transform: scale(1.05);
}

/* 返回按钮 */
.back-button {
  display: inline-block;
  margin: 1rem 0;
  padding: 0.8rem 1.5rem;
  background: var(--color-secondary);
  color: white;
  text-decoration: none;
  border-radius: var(--border-radius);
  transition: var(--transition);
}

.back-button:hover {
  background: var(--color-primary);
}

/* 关于我页面 */
.about-content {
  max-width: 700px;
  margin: 3rem auto;
  background: white;
  padding: 2rem;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow);
}

/* 响应式 - 桌面端 */
@media (min-width: 769px) {
  .nav-links {
    display: flex;
    flex-direction: row;
  }

  .menu-toggle {
    display: none;
  }

  .grid-container {
    grid-template-columns: repeat(4, 1fr);
  }

  .nav-links.active {
    position: static;
    background: none;
    padding: 0;
    box-shadow: none;
  }

  .nav-links.active li {
    margin: 0 0 0 1rem;
  }
}
```

- [ ] **Step 2: 验证 CSS 文件语法**

Run: 检查文件是否存在且内容正确
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add assets/css/main.css
git commit -m "feat: add styling system with responsive design"
```

### Task 3: 创建移动端菜单脚本

**Files:**
- Create: `assets/js/menu.js`

- [ ] **Step 1: 创建菜单交互脚本**

```javascript
document.addEventListener('DOMContentLoaded', function() {
  const menuToggle = document.querySelector('.menu-toggle');
  const navLinks = document.querySelector('.nav-links');
  const links = document.querySelectorAll('.nav-links a');

  if (menuToggle && navLinks) {
    menuToggle.addEventListener('click', function() {
      navLinks.classList.toggle('active');
    });
  }

  links.forEach(link => {
    link.addEventListener('click', function() {
      if (navLinks) {
        navLinks.classList.remove('active');
      }
    });
  });
});
```

- [ ] **Step 2: 验证 JS 文件语法**

Run: 检查文件是否存在且内容正确
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add assets/js/menu.js
git commit -m "feat: add mobile menu interaction script"
```

---

## Chunk 2: HTML 布局和组件

### Task 4: 创建 HTML head 部分

**Files:**
- Create: `layouts/partials/head.html`

- [ ] **Step 1: 创建 head 模板**

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="{{ site.Params.description }}">
  <title>{{ if .Title }}{{ .Title }} | {{ end }}{{ site.Title }}</title>

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;500;600&display=swap" rel="stylesheet">

  <!-- CSS -->
  {{ $css := resources.Get "css/main.css" }}
  {{ with $css }}
    <link rel="stylesheet" href="{{ .RelPermalink }}">
  {{ end }}

  <!-- JavaScript -->
  {{ $js := resources.Get "js/menu.js" }}
  {{ with $js }}
    <script src="{{ .RelPermalink }}"></script>
  {{ end }}
</head>
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add layouts/partials/head.html
git commit -m "feat: add head partial with fonts and assets"
```

### Task 5: 创建导航组件

**Files:**
- Create: `layouts/partials/nav.html`

- [ ] **Step 1: 创建导航栏组件**

```html
<nav class="navbar">
  <button class="menu-toggle" aria-label="Toggle menu">☰</button>
  <ul class="nav-links">
    <li><a href="/" class="{{ if eq .RelPermalink "/" }}active{{ end }}">首页</a></li>
    <li><a href="/about/" class="{{ if eq .RelPermalink "/about/" }}active{{ end }}">关于我</a></li>
    {{ range where site.RegularPages "Section" "cats" }}
      <li><a href="{{ .RelPermalink }}" class="{{ if eq .RelPermalink $.RelPermalink }}active{{ end }}">{{ .Title }}</a></li>
    {{ end }}
  </ul>
</nav>
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add layouts/partials/nav.html
git commit -m "feat: add navigation component with active state"
```

### Task 6: 创建基础布局模板

**Files:**
- Create: `layouts/_default/base.html`

- [ ] **Step 1: 创建基础布局**

```html
<!DOCTYPE html>
<html lang="zh-CN">
  {{ partial "head.html" . }}
  <body>
    {{ partial "nav.html" . }}

    <main class="main-content">
      <div class="container">
        {{ block "main" . }}{{ end }}
      </div>
    </main>
  </body>
</html>
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add layouts/_default/base.html
git commit -m "feat: add base layout template"
```

---

## Chunk 3: 页面模板

### Task 7: 创建首页模板

**Files:**
- Create: `layouts/index.html`

- [ ] **Step 1: 创建首页模板**

```html
{{ define "main" }}
<div class="welcome">
  <h1>{{ .Title }}</h1>
  <p>{{ .Params.welcome_message }}</p>
</div>

<div class="grid-container">
  {{ range where site.RegularPages "Section" "cats" }}
    <div class="card">
      {{ with .Params.cover_image }}
        <img src="{{ . }}" alt="{{ .Title }}" class="card-image">
      {{ else }}
        <img src="https://placehold.co/400x300/ffb6c1/333?text=照片" alt="{{ .Title }}" class="card-image">
      {{ end }}
      <div class="card-content">
        <h3 class="card-title">{{ .Title }}</h3>
        <p class="card-text">{{ with .Params.personality }}{{ . }}{{ else }}一只可爱的小猫咪{{ end }}</p>
      </div>
    </div>
  {{ end }}
</div>
{{ end }}
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add layouts/index.html
git commit -m "feat: add home page template"
```

### Task 8: 创建关于我页面模板

**Files:**
- Create: `layouts/about.html`

- [ ] **Step 1: 创建关于我页面模板**

```html
{{ define "main" }}
<div class="welcome">
  <h1>{{ .Title }}</h1>
</div>

<div class="about-content">
  {{ .Content }}
</div>

<div class="container">
  <a href="/" class="back-button">返回首页</a>
</div>
{{ end }}
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add layouts/about.html
git commit -m "feat: add about page template"
```

### Task 9: 创建猫咪详情页模板

**Files:**
- Create: `layouts/cats/single.html`

- [ ] **Step 1: 创建猫咪详情页模板**

```html
{{ define "main" }}
<div class="cat-detail">
  {{ with .Params.cover_image }}
    <img src="{{ . }}" alt="{{ .Title }}" class="cat-cover">
  {{ else }}
    <img src="https://placehold.co/400x300/ffb6c1/333?text=照片" alt="{{ .Title }}" class="cat-cover">
  {{ end }}

  <div class="cat-info">
    <div class="cat-header">
      <h1 class="cat-name">{{ .Title }}</h1>
      <div class="cat-tags">
        {{ with .Params.gender }}
          <span class="tag">{{ . }}</span>
        {{ end }}
        {{ with .Params.age }}
          <span class="tag">{{ . }}</span>
        {{ end }}
      </div>
    </div>

    {{ with .Params.personality }}
      <div class="cat-section">
        <h2>性格</h2>
        <p>{{ . }}</p>
      </div>
    {{ end }}

    {{ with .Params.growth_records }}
      {{ if (len .) }}
        <div class="cat-section">
          <h2>成长记录</h2>
          <div class="timeline">
            {{ range . }}
              <div class="timeline-item">
                <div class="timeline-date">{{ .date }}</div>
                <div class="timeline-content">
                  <p>{{ .event }}</p>
                  {{ with .image }}
                    <img src="{{ . }}" alt="{{ .event }}" class="timeline-image">
                  {{ end }}
                </div>
              </div>
            {{ end }}
          </div>
        </div>
      {{ else }}
        <div class="cat-section">
          <h2>成长记录</h2>
          <p>暂时还没有成长记录哦~</p>
        </div>
      {{ end }}
    {{ else }}
      <div class="cat-section">
        <h2>成长记录</h2>
        <p>暂时还没有成长记录哦~</p>
      </div>
    {{ end }}

    {{ with .Params.additional_images }}
      {{ if (len .) }}
        <div class="cat-section">
          <h2>更多照片</h2>
          <div class="additional-images">
            {{ range . }}
              <img src="{{ . }}" alt="{{ .Title }}" class="additional-image">
            {{ end }}
          </div>
        </div>
      {{ end }}
    {{ end }}

    {{ with .Content }}
      <div class="cat-section">
        <h2>关于 {{ $.Title }}</h2>
        {{ . }}
      </div>
    {{ end }}
  </div>
</div>

<div class="container">
  <a href="/" class="back-button">返回首页</a>
</div>
{{ end }}
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add layouts/cats/single.html
git commit -m "feat: add cat detail page template with error handling"
```

---

## Chunk 4: 内容文件

### Task 10: 创建首页内容

**Files:**
- Create: `content/_index.md`

- [ ] **Step 1: 创建首页内容**

```markdown
---
title: "欢迎来到我的小猫咪世界"
welcome_message: "这里是四个毛孩子的家，每一只都是我的心头好"
---

欢迎来到我的网站！在这里，你可以认识我家四只可爱的小猫。
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add content/_index.md
git commit -m "feat: add home page content"
```

### Task 11: 创建关于我页面内容

**Files:**
- Create: `content/about.md`

- [ ] **Step 1: 创建关于我页面内容**

```markdown
---
title: "关于我"
---

你好！我是一个初中生，喜欢画画、听音乐，当然还有——撸猫！

这四只小猫是我们家的一员，它们给我带来了很多快乐。
希望你能喜欢我的网站！
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add content/about.md
git commit -m "feat: add about page content"
```

### Task 12: 创建猫咪 1 内容

**Files:**
- Create: `content/cats/cat1.md`

- [ ] **Step 1: 创建猫咪1内容**

```markdown
---
title: "毛毛"
cover_image: "https://placehold.co/400x300/ffb6c1/333?text=毛毛"
personality: "活泼好动，喜欢追逐激光笔，是个小调皮鬼"
gender: "公"
age: "2岁"
birthday: "2022-03-15"
growth_records:
  - date: "2022-03-15"
    event: "来到我们家，小小的，好可爱！"
    image: "https://placehold.co/400x300/87ceeb/333?text=到达"
  - date: "2022-06-01"
    event: "第一次打疫苗，很勇敢哦"
    image: "https://placehold.co/400x300/ffb6c1/333?text=疫苗"
  - date: "2023-01-20"
    event: "一岁生日啦！给它准备了小鱼干蛋糕"
    image: "https://placehold.co/400x300/87ceeb/333?text=生日"
additional_images:
  - "https://placehold.co/400x300/ffb6c1/333?text=照片1"
  - "https://placehold.co/400x300/87ceeb/333?text=照片2"
  - "https://placehold.co/400x300/ffb6c1/333?text=照片3"
---

毛毛是个特别活泼的小家伙，每天都能看到它在客厅里跑来跑去。
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add content/cats/cat1.md
git commit -m "feat: add cat1 content"
```

### Task 13: 创建猫咪 2 内容

**Files:**
- Create: `content/cats/cat2.md`

- [ ] **Step 1: 创建猫咪2内容**

```markdown
---
title: "喵喵"
cover_image: "https://placehold.co/400x300/87ceeb/333?text=喵喵"
personality: "安静温柔，喜欢趴在窗台上晒太阳"
gender: "母"
age: "3岁"
birthday: "2021-05-20"
growth_records:
  - date: "2021-05-20"
    event: "来到我们家第一天"
    image: "https://placehold.co/400x300/ffb6c1/333?text=到达"
  - date: "2022-05-20"
    event: "一岁生日啦"
    image: "https://placehold.co/400x300/87ceeb/333?text=生日"
additional_images:
  - "https://placehold.co/400x300/ffb6c1/333?text=照片1"
  - "https://placehold.co/400x300/87ceeb/333?text=照片2"
---

喵喵是个文静的小姐姐，总是安静地陪伴在我身边。
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add content/cats/cat2.md
git commit -m "feat: add cat2 content"
```

### Task 14: 创建猫咪 3 内容

**Files:**
- Create: `content/cats/cat3.md`

- [ ] **Step 1: 创建猫咪3内容**

```markdown
---
title: "咪咪"
cover_image: "https://placehold.co/400x300/ffb6c1/333?text=咪咪"
personality: "超级粘人，喜欢被人抱着"
gender: "母"
age: "1岁"
birthday: "2023-08-10"
growth_records:
  - date: "2023-08-10"
    event: "刚满月就来到我们家"
    image: "https://placehold.co/400x300/87ceeb/333?text=到达"
additional_images:
  - "https://placehold.co/400x300/ffb6c1/333?text=照片1"
  - "https://placehold.co/400x300/87ceeb/333?text=照片2"
  - "https://placehold.co/400x300/ffb6c1/333?text=照片3"
---

咪咪是个小粘人精，走到哪里都要跟着，超级可爱！
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add content/cats/cat3.md
git commit -m "feat: add cat3 content"
```

### Task 15: 创建猫咪 4 内容

**Files:**
- Create: `content/cats/cat4.md`

- [ ] **Step 1: 创建猫咪4内容**

```markdown
---
title: "小白"
cover_image: "https://placehold.co/400x300/87ceeb/333?text=小白"
personality: "贪吃鬼，听到开罐头的声音就冲过来"
gender: "公"
age: "2岁"
birthday: "2022-11-25"
growth_records:
  - date: "2022-11-25"
    event: "冬天捡回来的小可怜"
    image: "https://placehold.co/400x300/ffb6c1/333?text=到达"
  - date: "2023-11-25"
    event: "一岁生日，吃得很开心"
    image: "https://placehold.co/400x300/87ceeb/333?text=生日"
additional_images:
  - "https://placehold.co/400x300/ffb6c1/333?text=照片1"
  - "https://placehold.co/400x300/87ceeb/333?text=照片2"
---

小白是个贪吃鬼，但是特别亲人，是个乖孩子。
```

- [ ] **Step 2: 验证文件创建**

Run: 检查文件是否存在
Expected: 文件创建成功

- [ ] **Step 3: 提交**

```bash
git add content/cats/cat4.md
git commit -m "feat: add cat4 content"
```

---

## Chunk 5: 测试和验证

### Task 16: 验证网站构建

**Files:**
- Test: 本地 Hugo 构建

- [ ] **Step 1: 构建 Hugo 网站**

Run: `hugo`
Expected: 构建成功，输出到 public 目录

- [ ] **Step 2: 启动本地服务器**

Run: `hugo serve -D`
Expected: 服务器启动成功，显示访问 URL（通常是 http://localhost:1313）

- [ ] **Step 3: 手动验证所有页面**

1. 访问首页 http://localhost:1313/ - 检查四只猫的卡片是否显示
2. 访问关于我页面 http://localhost:1313/about/ - 检查内容是否显示
3. 访问猫咪1页面 http://localhost:1313/cats/cat1/ - 检查详情是否完整
4. 访问猫咪2-4页面 - 检查详情是否完整
5. 检查导航栏是否高亮当前页面
6. 在手机模式下（浏览器开发者工具）检查菜单按钮是否显示和正常工作
7. 点击菜单链接后检查菜单是否自动收起
8. 检查占位图片是否正常显示

- [ ] **Step 4: 停止服务器**

Run: `Ctrl+C` 停止 Hugo serve

- [ ] **Step 5: 提交最终代码**

```bash
git add -A
git commit -m "feat: complete cat website implementation"
```

---

## 完成标准检查清单

项目完成时，确保以下标准全部满足：

- [ ] 可以通过 `hugo serve` 本地预览网站
- [ ] 所有页面（首页、关于我、4个猫咪页）都能正常访问
- [ ] 导航栏在桌面端显示所有链接，当前页面高亮
- [ ] 导航栏在移动端显示汉堡菜单，点击可展开/收起
- [ ] 点击导航链接后，移动端菜单自动收起
- [ ] 页面样式符合设计的视觉风格（粉色/蓝色、圆角、阴影）
- [ ] 占位图片正常显示
- [ ] 响应式设计在不同屏幕尺寸下正常显示
