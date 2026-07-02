# 一个是`**通信管道**`，一个是`**接口协议**`，关系如下：

## 1. /var/run/docker.sock

- 类型：**Unix 域套接字文件**（本地进程通信载体）
- 作用：管道、通道
  相当于：一根网线

dockerd 监听这个文件，所有请求都要通过它才能发给后台服务。

## 2. REST API

- 类型：**一套接口规范 / 指令集合**
- 作用：规定能发什么命令、数据格式
- 相当于：网线里传输的通讯话术（HTTP 请求）

比如 GET /containers/json、POST /containers/create 这些操作，就是 REST API。

## 两者关系

1. 你执行 docker ps
2. docker cli 组装一条符合 REST API 规范的 HTTP GET 请求
3. 通过 `/var/run/docker.sock` 这条管道发给 dockerd
4. dockerd 解析 REST API 请求，执行操作，再原路返回结果

## 一句话区分

- docker.sock = 路（传输通道）
- REST API = 路上跑的指令、消息（交互标准）

没有 sock，API 传不进去；没有 API，管道里不知道该发什么操作。