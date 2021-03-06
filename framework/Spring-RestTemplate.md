# 概述

## 优秀HttpClient的重要性

- 虽然服务间的调用越来越多的誓言RPC和消息队列，但是HTTP依然有适合的场景。
- RPC的优势在于高效的网络传输模型（常用NIO实现），以及针对服务调用场景专门设计协议和高效的序列化技术。而HTTP优势在于其成熟稳定，使用实现简单，被广泛支持，兼容性良好，防火墙友好，消息的可读性高。在开放API、跨平台的服务间调用、对性能要求不苛刻的场景中有着广泛的使用。因为HTTP存在着很广泛的应用场景，所以选择一个优秀的HTTP Client便十分重要。
- 优秀HTTP Client需要具备的特性：
  - 连接池
    - HTTP 1.1不支持多路复用，只有HTTP Pipeline着用半复用的模型支持，所以，在需要频繁发送消息的场景中，连接池是必须支持的，以减少频繁建立连接多带来的不必要的性能损耗。
  - 超时时间设置（连接超时，读取超时）
    - 当对端出现问题的时候，长时间的，甚至无限的超时等待是绝对不能接受的，所以必须设置超时时间。
  - 是否支持异步
    - HTTP 相关技术（服务器端和客户端）通常被人认为是性能低下的一个重要原因在于，在很长一段时间里，HTTP 的相关实现缺乏对异步的支持。这不仅指非阻塞 IO，也包括异步的编程模型。缺乏异步编程模型的后果就是，即便 HTTP 协议栈是基于非阻塞 IO 实现的，调用客户端的或者在服务端处理消息的线程有大量时间被浪费在了等待 IO 上面。所以，异步是非常重要的特性。
  - 请求和响应的编解码
    - 通常，开发人员希望面向对象使用各种服务（这里面自然也包括基于 HTTP 协议的服务），而不是直接面对原始的消息和响应开发。所以，透明地将 HTTP 请求和响应进行编解码是十分有必要，因为这可以很大程度地降低开发人员的工作量。
  - 可扩展性
    - 不论一个框架设计的多好，总有一些特殊场景是它们无法原生支持的。这时可扩展性的好坏便体现出来了。

## RestTemplate

- 身基于成熟的 HTTP Client 实现（Apache HttpClient、OkHttp 等），并可以灵活地在这些实现中切换，而且具有良好的扩展性。
- 最重要的是提供了前面几个 HTTP Client 不具备的消息编解码能力。
- ​