---
title: "3. 在HTTP/3上升级使用网络套接字"
anchor: "3_WebSockets_Upgrade_over_HTTP_3"
weight: 300
rank: "h1"
---


《[RFC8441]()》定义了在HTTP/2连接一条单独流上执行网络套接字协议（WebSocket Protocol）<sub>《[RFC6455]()》</sub>的机制。
其定义了一个扩展的`CONNECT`方法，通过给`:path`及`:authority`伪头部字段（pseudo-header）指定一个新的`:protocol`伪头部字段。
其同时定义了一个新的通过服务端发送的HTTP/2设置，以告知客户端是否可以使用扩展的`CONNECT`方法。

伪头部字段及设置与HTTP/2在《》中的定义完全一致。
《[HTTP/3]()》的[附录A.3]()要求HTTP/3分别注册设置。
`SETTINGS_ENABLE_CONNECT_PROTOCOL`值是`0x08`（十进制值为8），就像在HTTP/2中那样。

如果服务端圣母支持扩展的`CONNECT`方法，但是却收到了一个不识别或不支持的`:protocol`值，服务端{{< req_level SHOULD >}}给请求回复`501`（未实现）状态码（详见《[HTTP]()》[第15.6.2章]()）。
服务端{{< req_level MAY >}}通过“问题细节（problem details）”回复提供更多信息<sub>《[RFC7807]()》</sub>。

HTTP/3流的关闭也类似于TCP连接在《》中所述的方式关闭。
有序的TCP层级关闭通过流的`FIN`位表示（详见《[HTTP/3]()》[第4.4章]()）。
`RST`异常通过`H3_REQUEST_CANCELLED`（HTTP/3请求取消，详见《[HTTP/3]()》[第8.1章]()）类型的流错误（详见《[HTTP/3]()》[第8章]()）表示。
