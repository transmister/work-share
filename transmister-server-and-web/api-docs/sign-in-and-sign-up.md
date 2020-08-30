# Sign In and Sign Up

Sign in and sign up are the very beginning of the transmission. But before this, you need to make sure [the encrypted connection to the server](./message-structure.md) is initialized.

## Sign In

You need to request user to enter the username and password. After that, you should send the username and the SHA512 password.

```js
encryptedSocket.emit("e", { // send these data with `encryptedSocket`
    event: "signIn", // event name
    data: {
        username: username, // username doesn't need to SHA512
        passwordSHA512: SHA512(password) // SHA512 the password on your client to make sure the server won't know the user's password
    },
});
```

And you need to receive the message from the server to check if the username and the password matches.

If matches, the server will save your sign in to database and send a message like this:

```json
{
    "event": "success",
    "data": {
        "successId": "signIn.success"
    }
}
```

If not matches:

```json
{
    "event": "error",
    "data": {
        "errId": "signIn.incorrectUsernameOrPassword"
    }
}
```

If this, you should request user to enter again.
