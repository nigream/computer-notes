# Maven基础

---

## Maven配置

**参考：**

```crystal
https://blog.csdn.net/qq_39512532/article/details/117266940

```



### 基本配置

建议在修改配置文件时事先 **备份原文件** ，或者新的文件重命名为 `settings-nigream.xml` 这种带后缀的形式，而不是直接用原名 `settings.xml` 。

#### 修改本地仓库位置

```xml
<localRepository>D:\mvn\repository</localRepository>
```

#### 修改maven默认的JDK版本

```xml
<profile>     
    <id>JDK-1.8</id>       
    <activation>       
        <activeByDefault>true</activeByDefault>       
        <jdk>1.8</jdk>       
    </activation>       
    <properties>       
        <maven.compiler.source>1.8</maven.compiler.source>       
        <maven.compiler.target>1.8</maven.compiler.target>       
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>       
    </properties>
</profile>
```

#### 添加国内镜像源

```xml
<mirrors>
	<!--阿里云公共仓库是central仓和jcenter仓的聚合仓-->
    <mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云公共仓库</name>
		<url>https://maven.aliyun.com/repository/public</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云谷歌仓库</name>
		<url>https://maven.aliyun.com/repository/google</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云阿帕奇仓库</name>
		<url>https://maven.aliyun.com/repository/apache-snapshots</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云spring仓库</name>
		<url>https://maven.aliyun.com/repository/spring</url>
	</mirror>
	<mirror>
		<id>aliyunmaven</id>
		<mirrorOf>*</mirrorOf>
		<name>阿里云spring插件仓库</name>
		<url>https://maven.aliyun.com/repository/spring-plugin</url>
	</mirror>
	<mirror>
		<id>maven</id>
		<mirrorOf>*</mirrorOf>
		<name>maven仓库</name>
		<url>https://mvnrepository.com/</url>
	</mirror>
</mirrors>
```

