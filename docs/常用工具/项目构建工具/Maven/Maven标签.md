# Maven标签

---

## `<optional>`

参考：https://www.jianshu.com/p/562001b86aa9

```xml
<optional>true</optional>
```

- 用例：

  ```xml
  <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-core</artifactId>
      <optional>true</optional>
  </dependency>
  ```

  

- 用于表明这个依赖不会被传递，即当A依赖B、B依赖C时，A中不会导入C（默认排除了C）。

- 当你开发的组件时，有一个功能，有多种实现方式的 jar 可以提供，但是实际只会使用一种的情况下。可以把实现的 jar 包设置成 `<optional>true</optional>` 。表示：你依赖我并用到这个功能时，可以自行导入别的可替代的 jar 包。