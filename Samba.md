## Samba服务

### 简介

#### 数据共享的方法

- Winsows中最常用的是“网上邻居”。
- Linux 中最常用的是NFS。

Samba只能在局域网内是Windows和Linux进行通信。

|服务名称|使用范围|服务器端|客户端|局限性|
|--|--|--|--|--|
|FTP|内网和公网|Windows、Linux|Windows、Linux|无法直接在服务器端修改数据|
|Samba|内网|Windows、Linux|Windows、Linux|只能在内网使用|
|NFS|内网和公网|Linux|Linux|只能在LInux之间使用|

#### samba的守护进程

- smbd:提供对服务器中文件、打印资源的共享访问139 445
- nmbd:提供给予NetBIOS主机名称的解析 137 138
