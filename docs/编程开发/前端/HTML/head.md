# head

---

## `<title>`

**作用**

- 标签页名称
- 收藏的默认名称
- 搜索结果

## `<meta>`

### charset

- 指定 **字符编码** 
- 虽然有些浏览器可能会自动修复编码问题，但最好还是指定编码为 `utf-8` 。

```html
<meta charset="utf-8">
```

### name 与 content

- 有许多 `<meta>` 标签包含 `name` 字段 和 `content` 字段。

#### author 类型

- 指定作者。

```html
<meta name="author" content="Chris Mills" />
```

#### description 类型

- 指定网站描述。
- 这样利于SEO([Search Engine Optimization](https://developer.mozilla.org/en-US/docs/Glossary/SEO))

```html
<meta
  name="description"
  content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications." />
```

#### keywords 类型 (已弃用)

- 此类型的 `meta` 可以用于匹配搜索的关键字。
- 由于该配置被人滥用（填写很多不相关的关键字），所以已被弃用。

```html
<meta name="keywords" content="fill, in, your, keywords, here">
```

#### 某些网站专用的类型

如：

- Facebook 的 `The Open Graph protocol` : https://ogp.me/
- Twitter 的 `Twitter Cards` : https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards

其作用就是，当用户在分享链接时，会根据他们专用的 meta 标签中的内容生成 **卡片** 。

## `<link>`

### favicon

#### 如何自定义图标

1. 将 **图标图片** 保存在 **网站首页** 所在目录下 (最好保存成 `.ico` 格式, `.gif` 和 `.png` 格式也被大多数浏览器支持)

2. 在 `<head>` 标签中新增 `<link>` 标签

   ```html
   <link rel="icon" href="favicon.ico" type="image/x-icon" />
   ```

#### 多种图标类型

```html
<!-- third-generation iPad with high-resolution Retina display: -->
<link
  rel="apple-touch-icon-precomposed"
  sizes="144x144"
  href="https://developer.mozilla.org/static/img/favicon144.png" />
<!-- iPhone with high-resolution Retina display: -->
<link
  rel="apple-touch-icon-precomposed"
  sizes="114x114"
  href="https://developer.mozilla.org/static/img/favicon114.png" />
<!-- first- and second-generation iPad: -->
<link
  rel="apple-touch-icon-precomposed"
  sizes="72x72"
  href="https://developer.mozilla.org/static/img/favicon72.png" />
<!-- non-Retina iPhone, iPod Touch, and Android 2.1+ devices: -->
<link
  rel="apple-touch-icon-precomposed"
  href="https://developer.mozilla.org/static/img/favicon57.png" />
<!-- basic favicon -->
<link
  rel="icon"
  href="https://developer.mozilla.org/static/img/favicon32.png" />
```

### css

- 引入 css

```html
<link rel="stylesheet" href="my-css-file.css" />
```



## `<script>`

### js

- 引入js

```html
<script src="my-js-file.js" defer></script>
```

- `defer` : 用于表示在解析完整个html页面后，再运行js。
- `<script>` 并非一个 `void element` ，它其中是可以额外加js代码的。





---

## 参考

1. MDN: https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML