# 项目结构 {#directory-structure}

Slidev 对项目结构进行了一些约定，以尽量减少配置项，使功能扩展更加灵活直观。

基本结构如下所示：

```bash
your-slidev/
  ├── components/       # 自定义组件
  ├── layouts/          # 自定义布局
  ├── public/           # 静态资源
  ├── setup/            # 自定义 setup / hooks
  ├── snippets/         # 代码片段
  ├── styles/           # 自定义样式
  ├── index.html        # 注入的 index.html
  ├── slides.md         # 幻灯片主入口
  └── vite.config.ts    # 扩展 vite 配置
```

以上所有均为可选。

## 组件 {#components}

约定：`./components/*.{vue,js,ts,jsx,tsx,md}`

<LinkCard link="guide/component" />

## 布局 {#layouts}

约定: `./layouts/*.{vue,js,ts,jsx,tsx}`

<LinkCard link="guide/layout" />

## 静态资源 {#public}

约定: `./public/*`

开发过程中，此目录中的资源文件将在 `/` 下提供，并会按原样复制到 dist 目录的根目录中。欲了解更多，请参阅 [处理静态资源](../guide/faq#assets-handling)。

## 样式 {#style}

约定: `./style.css` | `./styles/index.{css,js,ts}`

遵循上述约定的文件将被注入到 App 的根目录中。如果你需要引入多个 css 入口，你可以按如下方式创建结构并自行管理引入顺序。


```bash
your-slidev/
  ├── ...
  └── styles/
      ├── index.ts
      ├── base.css
      ├── code.css
      └── layouts.css
```

```ts
// styles/index.ts

import './base.css'
import './code.css'
import './layouts.css'
```

样式得益于 [UnoCSS](https://unocss.dev/) 和 [PostCSS](https://postcss.org/)，你拥有开箱即用的 css 嵌套和 [at-directives](https://unocss.dev/transformers/directives#apply)。示例：


<!-- eslint-skip -->

```less
.slidev-layout {
  --uno: px-14 py-10 text-[1.1rem];

  h1, h2, h3, h4, p, div {
    --uno: select-none;
  }

  pre, code {
    --uno: select-text;
  }

  a {
    color: theme('colors.primary');
  }
}
```

[了解更多关于此语法的信息](https://unocss.dev/transformers/directives#apply)。

## `index.html` {#index-html}

约定: `index.html`

`index.html` 提供了向主 `index.html` 中注入 meta 标签以及 scripts 标签的能力。

例如，对于以下自定义 `index.html` 来说：

```html
<!-- ./index.html -->
<head>
  <link rel="preconnect" href="https://fonts.gstatic.com">
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Nunito+Sans:wght@200;400;600&display=swap" rel="stylesheet">
</head>

<body>
  <script src="./your-scripts"></script>
</body>
```

最终部署的 `index.html` 效果如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="icon" type="image/png" href="https://cdn.jsdelivr.net/gh/slidevjs/slidev/assets/favicon.png">
  <!-- injected head -->
  <link rel="preconnect" href="https://fonts.gstatic.com">
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Nunito+Sans:wght@200;400;600&display=swap" rel="stylesheet">
</head>
<body>
  <div id="app"></div>
  <script type="module" src="__ENTRY__"></script>
  <!-- 注入的 body -->
  <script src="./your-scripts"></script>
</body>
</html>
```

## 全局图层 {#global-layers}

约定: `global-top.vue` | `global-bottom.vue` | `custom-nav-controls.vue` | `layouts/slide-top.vue` | `layouts/slide-bottom.vue`

<LinkCard link="features/global-layers" />
