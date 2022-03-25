# Maven插件

---



## spring-boot-maven-plugin 插件爆红

问题描述：新建 SpringBoot 项目时，pom 文件中 spring-boot-maven-plugin 插件爆红

解决方法：添加版本号。

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.2.1.RELEASE</version>
        </plugin>
    </plugins>
</build>
```

