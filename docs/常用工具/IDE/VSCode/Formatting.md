# Formatting

---

## HTML

### VSCode的默认格式化工具

#### 说明

- 官方说明：https://code.visualstudio.com/docs/languages/html#_formatting
- 基于 [js-beautify](https://www.npmjs.com/package/js-beautify) ，提供了一些配置项

#### 使用

- `Shift+Alt+F` : 格式化文件
- `Ctrl+K Ctrl+F` : 格式化选中的内容

### beautify

- 插件地址：https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify
- 同样基于 [js-beautify](https://www.npmjs.com/package/js-beautify) ，但比VSCode内置的beautify多了些个性化配置项。



### Prettier

- 插件地址：https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode
- 可配置内容较少，且格式化后很丑，点名批评 **文本和标签同行时，会自动换行，导致多出空格** 。



### 格式化配置

```json
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true
```

