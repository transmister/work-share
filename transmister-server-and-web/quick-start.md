# Quick Start

## How to start debugging

Before starting the server, you need to make sure that MongoDB service is active. The server will automatically create a new database called 'transmister'.

Then you can start the server starts at port 80 defaultly:

```powershell
npm run dev
```

If port 80 is using by other tasks, the server will automatically throw out an error and stop, you can customize the port:

```powershell
PORT=[custom-port] node server.js
```

Now, you can visit `http://localhost:[custom-port]` to debug.

## Build

```powershell
npm run build
npm start
```
