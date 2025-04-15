# Network Interview

### OSI 七层模型 和 TCP/IP 四层模型: 

OSI`Open Systems Interconnection`七层模型：

1. _应用层_`Application Layer`：用户`user`与应用`network services`之间的接口`Interfac`（如**HTTP、FTP、SMTP**）。
1. _表示层_`Presentation Layer`：数据格式转换、加密、压缩。
1. _会话层_`Session Layer`：建立、管理、终止会话。
1. 传输层`Transport Layer`：端到端的连接（如**TCP、UDP**）。
1. 网络层`Network Layer`：数据的路由`Routing`（如**IP协议**）。
1. _数据链路层_`Data Link Layer`：数据帧`frame`的传输、错误检测与纠正。
1. _物理层_`Physical Layer`：负责传输原始比特流`raw bits`（如电缆`cable`、光纤`fiber`）。

TCP/IP四层模型：

1. 应用层（对应OSI的应用、表示、会话层）
1. 传输层`Transport Layer `（对应OSI的传输层）
1. 网络层`Internet Layer`（对应OSI的网络层）
1. 网络接口层`Network Access Layer`（对应OSI的物理层和数据链路层）

<img width="300" alt="Screenshot 2025-04-15 at 3 00 56 AM" src="https://github.com/user-attachments/assets/11424a1b-3e99-4149-ac6b-ade748a3ffd7" />


### 什么是路由和交换？

路由器`Router`： 工作在**网络层**，根据**IP地址**转发数据包，负责**不同子网**间的通信。

交换机`Switch`： 工作在**数据链路层**，根据**MAC地址**转发**数据帧**，通常用于**局域网内部**的通信。



