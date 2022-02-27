# WebService基础

---

## 什么是WebService

它是一种 **跨编程语言** 和 **跨操作系统平台** 的 **远程调用技术** 。是 **基于网络的 、 分布式的模块化组件** 。

所谓 **跨操作系统平台** ：基于 XML ， XML 本身与平台无关。

所谓 **跨编程语言** ：使用 XSD 作为数据类型系统，不论用什么语言编写，所有数据类型都会被转换为 XSD ，只需要遵守 WebService 的统一标准即可。

> **#疑问** ：普通的HTTP请求不都是 **跨平台跨语言** 的吗？感觉以上说明仍不足以解释：为什么强调 **WeService跨平台跨语言** 。

## WebService的实现

WebService相对普通HTTP请求的



## WebService三要素

webService三要素：soap、wsdl、uddi。

### soap

**SOAP 即 简单对象访问协议(Simple Object Access Protocol)**

- 它是用于交换 XML 编码信息的轻量级协议。
- soap 请求基于 HTTP 的 POST 方法，遵循一种特殊的 xml 消息格式，只要 Content-type 设置为: text/xml，任何数据都可以xml化。SOAP =  HTTP  + XML 数据。
- 它有三个主要方面：
  - XML-envelope 为 **描述信息内容** 和 **如何处理内容** 定义了框架。
  - 将 **程序对象** 编码成为 **XML对象** 的规则。
  - 执行 **远程过程调用(RPC)** 的约定。
- SOAP可以运行在任何其他传输协议上。
- SOAP的组成如下：
  - Envelope – 必须的部分。以XML的根元素出现。
  - Headers – 可选的。
  - Body – 必须的。在body部分，包含要执行的服务器的方法。和发送到服务器的数据。

### wsdl

**WSDL（SebService Definition Language）**

- 通过wsdl说明书，就可以描述webservice服务端对外发布的服务；
- wsdl说明书是一个基于xml文件，通过xml语言描述整个服务（因为是基于XML的，所以WSDL既是机器可阅读的，又是人可阅读的）；
-  在wsdl说明中，描述了：
  - 对外发布的服务名称（类）
  - 接口方法名称（方法）
  - 接口参数（方法参数）
  - 服务返回的数据类型（方法返回值）

### uddi

WebService 提供商又如何将自己开发的 Web 服务公布到因特网上？这就需要使用到 UDDI 了！

**UDDI (Universal Description，Discovery andIntegration) ** : 即 通用的描述，发现以及整合。

- UUDI 是一个跨产业，跨平台的开放性架构，是一套 基于Web的、分布式的、为WebService提供的、信息注册中心的 实现标准规范 。
- 它是一种目录服务，可以帮助企业在互联网上 **发布 和 搜索 WebService** 。
  - 简单来说，UDDI 就是一个目录，只不过在这个目录中存放的是一些关于 Web 服务的信息而已。
- UDDI 的目的是为电子商务建立标准。
