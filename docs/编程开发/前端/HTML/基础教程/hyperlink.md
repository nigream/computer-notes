# hyperlink

---

## 什么是超链接

- **超链接** 使我们能够将我们的文档链接到 **任何其他文档(或其他资源)** ，也可以链接到 **文档的指定部分** 。

## `<a>`

### 属性

- `href` 用于指定链接到的 url 。
- `title` 用于指定鼠标悬停显示的文字。
  - 注意：由于仅仅使用 **键盘或触摸屏** 的设备无法显示这个文字，所以，如果有重要的文字，不要放在这个属性中。

```html
  <a
    href="https://www.mozilla.org/en-US/"
    title="The best place to find more information about Mozilla's
          mission and how to contribute">the Mozilla homepage</a>
```

### 块级链接

- 几乎任何内容都可以转成一个链接，包括 **块级元素** 。

```html
<a href="https://www.mozilla.org/en-US/">
  <img src="mozilla-image.png" alt="Mozilla homepage" />
</a>
```



### 文档片段

- 使用 `#` 符号 (hash/pound symbol) 可以跳转到文档指定位置

```html
<h2 id="Mailing_address">Mailing address</h2>

<!-- 跳转到其他页面的指定位置 -->
<a href="contacts.html#Mailing_address">mailing address</a>.

<!-- 跳转到当前页面的指定位置 -->
<a href="#Mailing_address">company mailing address</a>
```



## `<a>` 的最佳实践

- 最佳实践有以下几个原则

### 1、使用清晰的链接描述词汇

#### 例子

- 好的链接文本

```html
<p><a href="https://www.mozilla.org/firefox/">Download Firefox</a></p>
```

- 坏的链接文本

```html
<p><a href="https://www.mozilla.org/firefox/">Click here</a>to download Firefox</p>
```

#### 不同用户对链接的偏好

- **Screen readers** : 喜欢从一个链接跳到另一个链接，喜欢脱离上下文去读 **链接文本** 。
- **Search engines** : 使用链接文本去索引文件，所以 **链接文本** 最好包含有效描述链接内容的关键字。
- **Visual readers** : 喜欢大致浏览网页，而不是一个字一个字读，因为链接比较显眼，所以读者很容易看到链接，所以 **链接文本** 很重要。

#### 链接文本Tips

1. 不要使用 **URL** 的作为 **链接文本** 的一部分 —— 因为 **URL** 很丑， **Screen reader** 读起来会很奇怪。
2. 不要在 **链接文本** 中使用 **link** 或者 **links to** 等词 —— 因为这毫无意义， **Screen readers** 能识别链接；因为链接通常有不一样的样式(颜色、下划线等)， **Visual users** 也能识别。
3. **链接文本** 使用尽可能短的描述 —— 因为 **Screen reader** 要读整个文本。
4. 尽可能使用 **不同的文本** 描述 **不同的链接** —— 便于 **Screen reader** 区分，否则如果都是 "click here" 的话，会造成困扰。

### 2、链接到非HTML资源时留下清晰的指示

#### 例子

```html
<p>
  <a href="https://www.example.com/large-report.pdf">
    Download the sales report (PDF, 10MB)
  </a>
</p>

<p>
  <a href="https://www.example.com/video-stream/" target="_blank">
    Watch the video (stream opens in separate tab, HD quality)
  </a>
</p>

<p>
  <a href="https://www.example.com/car-game">
    Play the car game (requires Flash)
  </a>
</p>
```

#### 链接文本Tips

以下几种情况需要添加文本做一些 **额外的描述** ：

- 如果是一个 **下载文件** (如 PDF、Word) 的链接，需要描述下载的 **文件类型** 和 **大小** 等。
- 如果是一个 **流媒体** (如 video、audio) 的链接，如视频，需要描述 **播放的tab** 和 **视频清晰度** 等 。
- 如果是一个 **点击后会有意想不到的动作发生** (如 打开一个窗口、加载一个Flash影片等) 的链接，如 **Flash** ，需要描述需要安装 **Flash** 。

### 3、使用download属性设置下载文件的默认名

- 保存文件时，默认使用 download 属性的值作为文件名。

```html
<a
  href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
  download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a>
```



## `<a>` 邮件链接

### 例子

- `<a>` 标签的 `href` 属性填 `mailto:example@example.com` ，可以形成一个 **邮件链接** 。
- 点击此链接，系统会打开本机的邮件程序，并跳转到给指定邮箱地址 **发邮件的界面** 。

```html
<a href="mailto:nowhere@mozilla.org">Send email to nowhere</a>
```

### 指定邮件详细信息

#### 语法

- 可以指定多个 **收件人** (用 **逗号** 隔开) 。
- 可以用 `query iterm` 的形式 (即Get请求传参的方式)，指定发送邮件的 **详细信息** 。
- 每个字段的值需要进行 **URL编码** ，即 **非打印字符** (如制表符、换行符、分页符等) 和 **空格** 需要用 **百分号转义** 来表示。

```html
<a href="mailto:user1@example.com,user2@example.com...?field1=xxxx&field2=xxx">Send email to nowhere</a>
```

#### 标头字段

- `cc` : **抄送人** (可以指定多个，用 **逗号** 隔开)
- `bcc` : **密件抄送人** (可以指定多个，用 **逗号** 隔开)
- `subject` : **主题** 
- `body` : **主体** (并非真正的标头字段，但允许你指定简短的主体信息)

```html
<a href="mailto:nowhere@mozilla.org,xxx@zwc.com?cc=name2@rapidtables.com,xxx1@zwc.com&bcc=name3@rapidtables.com,xxx2@zwc.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
    Send mail with cc, bcc, subject and body
</a>
```

### `mailto:` 后留空

- 实际上 `mailto:` 后可以留空。
- 点击此链接，系统会打开本机的邮件程序，并跳转到给 **发邮件的界面** (但不指定邮箱地址，用户可以自己手动指定)。

```html
<a href="mailto:">Send email to nowhere</a>
```







---

## 参考

1. https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks