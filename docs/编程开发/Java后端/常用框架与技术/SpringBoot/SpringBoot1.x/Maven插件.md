

# Maven 插件

---

## 打包插件

```xml
<build>
    <plugins>
        <!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

- 使用该插件，打的包可以直接用 `java -jar` 命令来执行， `jar` 包结构如下：

  ```sh
  example.jar
      |
      +-META-INF
      | +-MANIFEST.MF
      +-org
      | +-springframework
      | 	+-boot
      | 		+-loader
      | 			+-<spring boot loader classes>
      +-BOOT-INF
          +-classes
          |	+-com
          | 		+-mycompany
          | 			+-project
          | 				+-YourClasses.class
          +-lib
              +-dependency1.jar
              +-dependency2.jar
  ```

  

  ![image-20220309020555796](maven插件/image-20220309020555796.png)

- 若不用该插件，打的包不能直接用 `java -jar` 命令来执行，会提示 `xxx.jar中没有主清单属性` 。

  ![image-20220309020630235](maven插件/image-20220309020630235.png)

- 使用插件打包会比不使用插件打包，多 `lib` 和 `org.springframework.boot.loader` 目录，前者包含项目依赖，后者包含项目启动的相关类。

