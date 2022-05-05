# HTML基础

---

## Case-Insensitive

- `html` 标签大小写不敏感，但是建议全小写。

## Elements

### 结构

- 结构（图片来源：https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started/grumpy-cat-small.png）

  ![grumpy-cat-small](HTML基础/grumpy-cat-small.png)



### Block-level elements

https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements

如： `<p></p>` 、 `<div></div>`

- **块级元素** 在页面上形成可见的块。
- **块级元素** 会在它之前的任何内容之后另起一行显示，而它之后的任何内容也会另起一行显示。
- **块级元素** 通常是结构化的元素，如 headings, paragraphs, lists, navigation menus, or footers 。
- **块级元素** 可以被嵌套在块级元素中，而不可以被嵌套在行内元素中。

### Inline elements

https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements

如： `<a></a>` 、 `<em></em>` 、 `<strong></strong>`

- **行内元素** 被包含在 **块级元素** 中，并且它通常仅包含网页的一小部分（即不包含整个段落或大量内容）。
- **行内元素** 之前的内容如果也是 **行内元素** ，则它不会另起一行显示。

### Empty elements / Void elements

- 只有 `Opening tag ` 而没有 `Content` 和 `Closing tag` 的标签。
- `html` 没有规定 `Empty Elements` 必须加上 `/` ，但是加上也是合法的，这样也有利于将 `html` 转成合法的 `xml` ，如 `<img src="images/cat.jpg" alt="cat" />`

## Attributes

### 结构

图片来源：https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started/grumpy-cat-attribute-small.png

![grumpy-cat-attribute-small](HTML基础/grumpy-cat-attribute-small.png)

### 单引号还是双引号

- 属性值一定要用 `""` 或者 `''` 包裹起来，否则如果属性值中包含空格，可能会出问题。
- `""` 或者 `''` 是等价的，在一种引号中使用另一种引号是合法的，但是在一个引号中使用同一种引号，则需要使用转义字符。
- 如：这样是错的： `<a href='https://www.example.com' title='Isn't this fun?'>A link to my example.</a>` 而这样是对的： `<a href='https://www.example.com' title='Isn&apos;t this fun?'>A link to my example.</a>`

### Boolean attributes

- 只有一个值的属性（或者不填值）
- 如：`<input type="text" disabled="disabled">` 等价于 `<input type="text" disabled>`

## HTML document

### 结构

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

### `<!DOCTYPE html>`

- 在史前时代，该标签用于指向一些用于检测 HTML 错误的 URL，如：

  ```html
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
  ```

- 如今，它的作用仅仅是使网页可以正常显示，简化为：

  ```html
  <!DOCTYPE html>
  ```

### `<html></html>`

- `root element` 包含了页面所有的内容

### `<head></head>`

- 这个元素中的内容将不会在页面上呈现，它包含了浏览器可以搜索的关键词、CSS、字符集等。

### `<body></body>`

- 页面上的所有内容都必须被包含在这个标签中。

## Whitespace

- 在两个字符之间不管有按多少次空格键和换行键，都会被渲染为 `一个空格` 。
- 元素缩进时，我们一般使用 `两个空格` 。

## 转义字符(Entity references)

常用转义字符如下：

| Literal character | Character reference equivalent | Description               |
| :---------------- | :----------------------------- | ------------------------- |
| `<`               | `&lt;`                         | less than                 |
| `>`               | `&gt;`                         | greater than              |
| `"`               | `&quot;`                       | quotation                 |
| `'`               | `&apos;`                       | apostrophe(省略符号/撇号) |
| `&`               | `&amp;`                        | ampersand                 |

转义字符大全：https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references

## HTML Comments

- `<!-- 这是注释 -->` 
