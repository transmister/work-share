# 封装与服务器通讯时端对端加密

封装已完成：

transmister-server-and-web/socket/encryption.js 3:5

```ts
encryptedSocket.emit(event: string, data: any)
encryptedSocket.on(event: string, listener: Function)
```

---

目前我们使用 JSON 作为数据库，但当数据量大时，读写速度会变慢，所以最好换成 [MongoDB](https://www.mongodb.com/) ，或者：

在存储数据时，只追加到数据库文件末尾，读取时从末尾向开头扫描。

```
"oJji06jrjKpHwvtUAAAA": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBAIiOpaUOc9VnSiTtjtgqXRhFbzh3giszzQQZbbEMjJX7UtaxM/YL9ct8\n71yWaFUE2lhzDJuvIa9QDZNfTIF6QcbmTQcjreer+/ddz53SfgdFRxco7pDrkkFJ\naFrTVDL7HBlEmDvSK10rvMH5HtC60Cy7QNGBUY2oGOIqNJ1EC6ofAgMBAAE=\n-----END RSA PUBLIC KEY-----"
```

然后追加数据：

```
"oJji06jrjKpHwvtUAAAA": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBAIiOpaUOc9VnSiTtjtgqXRhFbzh3giszzQQZbbEMjJX7UtaxM/YL9ct8\n71yWaFUE2lhzDJuvIa9QDZNfTIF6QcbmTQcjreer+/ddz53SfgdFRxco7pDrkkFJ\naFrTVDL7HBlEmDvSK10rvMH5HtC60Cy7QNGBUY2oGOIqNJ1EC6ofAgMBAAE=\n-----END RSA PUBLIC KEY-----"
"R0DFSUWzH4b9RN5QAAAB": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBANkfMH5VrAi6RDLe9hMtrbxzPKR4ghK2iydoDfqjv9ZJxWMKyBjPt7En\nycL8oGdxnjNphDeyHwntCOUFMainZaelO03vq2LpmPaKO56zwJPWnXJJRIwNI6Zi\n6RPDz8AONGuhDN+pJxfyQ4x1bD3V05nbICfyaIKOjwTcQXIBTaSvAgMBAAE=\n-----END RSA PUBLIC KEY-----"
```

读取 `key: "oJji06jrjKpHwvtUAAAA"` 时：

```
"oJji06jrjKpHwvtUAAAA": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBAIiOpaUOc9VnSiTtjtgqXRhFbzh3giszzQQZbbEMjJX7UtaxM/YL9ct8\n71yWaFUE2lhzDJuvIa9QDZNfTIF6QcbmTQcjreer+/ddz53SfgdFRxco7pDrkkFJ\naFrTVDL7HBlEmDvSK10rvMH5HtC60Cy7QNGBUY2oGOIqNJ1EC6ofAgMBAAE=\n-----END RSA PUBLIC KEY-----"
>   "R0DFSUWzH4b9RN5QAAAB": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBANkfMH5VrAi6RDLe9hMtrbxzPKR4ghK2iydoDfqjv9ZJxWMKyBjPt7En\nycL8oGdxnjNphDeyHwntCOUFMainZaelO03vq2LpmPaKO56zwJPWnXJJRIwNI6Zi\n6RPDz8AONGuhDN+pJxfyQ4x1bD3V05nbICfyaIKOjwTcQXIBTaSvAgMBAAE=\n-----END RSA PUBLIC KEY-----" // wrong key
```

然后：

```
>   "oJji06jrjKpHwvtUAAAA": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBAIiOpaUOc9VnSiTtjtgqXRhFbzh3giszzQQZbbEMjJX7UtaxM/YL9ct8\n71yWaFUE2lhzDJuvIa9QDZNfTIF6QcbmTQcjreer+/ddz53SfgdFRxco7pDrkkFJ\naFrTVDL7HBlEmDvSK10rvMH5HtC60Cy7QNGBUY2oGOIqNJ1EC6ofAgMBAAE=\n-----END RSA PUBLIC KEY-----" // right key
"R0DFSUWzH4b9RN5QAAAB": "-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBANkfMH5VrAi6RDLe9hMtrbxzPKR4ghK2iydoDfqjv9ZJxWMKyBjPt7En\nycL8oGdxnjNphDeyHwntCOUFMainZaelO03vq2LpmPaKO56zwJPWnXJJRIwNI6Zi\n6RPDz8AONGuhDN+pJxfyQ4x1bD3V05nbICfyaIKOjwTcQXIBTaSvAgMBAAE=\n-----END RSA PUBLIC KEY-----"
```

最后返回：`"-----BEGIN RSA PUBLIC KEY-----\nMIGJAoGBAIiOpaUOc9VnSiTtjtgqXRhFbzh3giszzQQZbbEMjJX7UtaxM/YL9ct8\n71yWaFUE2lhzDJuvIa9QDZNfTIF6QcbmTQcjreer+/ddz53SfgdFRxco7pDrkkFJ\naFrTVDL7HBlEmDvSK10rvMH5HtC60Cy7QNGBUY2oGOIqNJ1EC6ofAgMBAAE=\n-----END RSA PUBLIC KEY-----"`

（极其麻烦）

---

实现端对端加密时，把 `socket.id` 和客户端公钥对应起来，然后在之后的数据传输时，就使用这个公钥。同时，服务端生成密钥对，将公钥发送客户端，这时候：

服务端：

```js
var keysDb = {
  "oJji06jrjKpHwvtUAAAA": {
    client: {
      public: "the public key from client"
    },
    server: {
      public: "the public key initialized by the server",
      private: "the private key initialized by the server"
    }
  }
}
```

客户端：

```js
var keys = {
  client: {
    public: "the public key initialized by the client",
    private: "the private key initialized by the client"
  },
  server: {
    public: "the public key from server"
  }
}
```

然后封装，使它可以这样调用：

```js
// Encrypt
socket.emit('e', encryptToServer(data))

// Decrypt
socket.on('e', (data) => {
    ((data) => {
        ...
    })(decryptToServer(data))
})
```