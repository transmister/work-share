# Realize Client-to-Client Encrytion

We provided a text field and a button on the title bar, user can input the target user's username in the text field and click the button to initialize end-to-end encryption to the target user, then,  the user will be able to select the target user in the menu and send message to his/her.

[Remember not to save the data that should not save in the server](../DO_NOT_SAVE.md)

## `initializeEncryptionToAnotherClient`

Defined in [transmister/transmister-server-and-web master /socket/encryption.js:50](https://github.com/transmister/transmister-server-and-web/blob/master/socket/encryption.js#L50)

But it's an empty function, the value is given in [transmister/transmister-server-and-web master /socket/encryption.js:114](https://github.com/transmister/transmister-server-and-web/blob/master/socket/encryption.js#L114)

Refered in [transmister/transmister-server-and-web master /components/titleBar/searchBar.js:21](https://github.com/transmister/transmister-server-and-web/blob/master/components/titleBar/searchBar.js#L21) to initialize encryption to the target user when "+" button clicked.

## `encryptedSocket.specific.emit`

**This Method is not tested.** - _Aug 31, 2020_

Defined in [transmister/transmister-server-and-web master /socket/encryption.js:37](https://github.com/transmister/transmister-server-and-web/blob/master/socket/encryption.js#L37).

Value given in [transmister/transmister-server-and-web master /socket/encryption.js:91](https://github.com/transmister/transmister-server-and-web/blob/master/socket/encryption.js#L91).

## `encryptedSocket.specific.on`

**This Method is not tested.** - _Aug 31, 2020_

Defined in [transmister/transmister-server-and-web master /socket/encryption.js:4](https://github.com/transmister/transmister-server-and-web/blob/master/socket/encryption.js#L44).

Value given in [transmister/transmister-server-and-web master /socket/encryption.js:106](https://github.com/transmister/transmister-server-and-web/blob/master/socket/encryption.js#L106).
