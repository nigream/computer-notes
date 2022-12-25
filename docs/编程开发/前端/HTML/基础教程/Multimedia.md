# Multimedia

---

## `<img>`

### 概述

- 这是一个 `void element/empty element` (没有子元素，没有结束标签)

- 使用：

  ```html
  <img src="dinosaur.jpg" alt="Dinosaur" />
  <img src="images/dinosaur.jpg" alt="Dinosaur" />
  ```

#### 关于SEO

- 为了SEO，文件名称要有 **可读性** 。比如 `dinosaur.jpg` 好于 `img835.jpg` 。

#### 关于 hotlinking

- 未经授权不要使用别人网站的图片地址 (这被称为 **hotlinking** )。
  - 这样做会导致别人需要承担图片访问的流量，而且你也不能保证图片不会被删除或被替换。

#### 关于版权

- 你使用的图片最好满足以下条件之一：
  1. 你 **拥有** 这张图片。
  2. 你有来自图片拥有者明确的、书面的 **许可** 。
  3. 你有充分的证据证明这张图片是在 **公共领域** 的 (即可随意使用) 。

#### 关于绝对路径

像如下这种 **绝对路径** 最好不要出现，因为：

- 如果使用简单的部署，你需要将你的 **图片** 和 你的 **网站** 部署到同一台服务器。
- 如果使用高级的部署，你需要使用 [CDN (Content Delivery Network)](https://developer.mozilla.org/en-US/docs/Glossary/CDN) 去分发你的 **图片** 。

```html
<img src="https://www.example.com/images/dinosaur.jpg" alt="Dinosaur" />
```

### alt

#### alt 使用

- `alt` 属性用于 **图片无法展示** 或 **由于网速过慢导致渲染时间比较长** 时的文本展示。

  ```html
  <img
    src="images/dinosaur.jpg"
    alt="The head and torso of a dinosaur skeleton;
            it has a large head with long sharp teeth" />
  ```

- 你可以通过错误拼写图片名称来查看效果。

#### alt 什么时候起作用

1. `screen reader` 能够听到这个内容；同时这个描述性的文字对其他用户也有用。
2. 路径拼写错误。
3. 浏览器不支持图片类型，比如 文本浏览器 [Lynx](https://en.wikipedia.org/wiki/Lynx_(web_browser)) 。
4. 搜索引擎会匹配 alt 中的内容。
5. 用户关掉了图片显示，以减少数据传输量或减少干扰，特别是手机用户或网络带宽限制或昂贵的地方。

#### alt 应该写什么

1. 



## Replaced elements

- 像 `<img>` 和 `<video>` 等元素，由于它的 **内容** 和 **大小** 由外部的资源决定 (即图片或视频文件) ，所以也被称为 [Replaced elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element) .

---

## 参考

1. MDN Guide: [Images in HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML)

