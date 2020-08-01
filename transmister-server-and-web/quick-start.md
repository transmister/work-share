# 快速开始

## 如何开始调试

默认在 80 端口开启调试服务器：

```powershell
node server.js
```

如果 80 端口被占用将会报错并退出，可自定义端口：

```powershell
PORT=[custom-port] node server.js
```

然后打开 `http://localhost:[custom-port]` ，即可开始调试，一旦代码发生更改，页面会自动刷新。

## 如何 `build`

```powershell
npx next build
NODE_ENV=production node server.js # 使用 build 服务
```

## 计划

1. [封装与服务器通讯时端对端加密](./plan/001-封装与服务器通讯时端对端加密.md)
2. [完成登录注册功能](./plan/002-完成登录注册功能.md)
3. [完成聊天组件](./plan/003-完成聊天组件.md)
4. [封装客户端对客户端加密](./plan/004-封装客户端对客户端加密.md)
5. [完成聊天列表](./plan/005-完成聊天列表.md)
