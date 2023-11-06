# JavaScript (Node.js)

#### Required Libraries:

* `jsonwebtoken`: For handling JWT tokens.
* `elliptic`: For handling ECDSA signature generation and verification.
* `uuid`: For generating unique identifiers.

{% code overflow="wrap" lineNumbers="true" %}
```bash
npm install jsonwebtoken elliptic uuid
```
{% endcode %}

{% code overflow="wrap" lineNumbers="true" %}
```javascript
const jwt = require('jsonwebtoken');
const ECDSA = require('elliptic').ec;
const ec = new ECDSA('p521');
const uuid = require('uuid');
const { generateKeyPairSync } = require('crypto');

// Key generation
//const keyPair = ec.genKeyPair();
const { publicKey, privateKey } = generateKeyPairSync('ec', {
    namedCurve: 'secp521r1', // This is equivalent to the p521 curve you were using
    publicKeyEncoding: {
        type: 'spki',
        format: 'pem'
    },
    privateKeyEncoding: {
        type: 'pkcs8',
        format: 'pem'
    }
});

function signMessage(message, privateKey) {
    const signature = ec.sign(message, privateKey, 'hex');
    return Buffer.from(signature.toDER()).toString('base64');
}
// JWT header with custom 'typ' value
const jwtHeader = {
    alg: 'ES512',
    typ: 'ETAMP'
};
// Generate JWT token for a message
const id = uuid.v4();
const jti = uuid.v4();
const messageId = uuid.v4();
const senderId = uuid.v4();
const recipientId = uuid.v4();
const senderServerId = uuid.v4();
const payload = {
    jti: jti,
    exp: 1679992314,
    nbf: 1679988714,
    messageId: messageId,
    senderUserName: "user1",
    senderId: senderId,
    recipient: "user2",
    recipientId: recipientId,
    senderServerName: "WebServer1",
    senderServerId: senderServerId,
    recipientServerName: "WebServer1",
    recipientServerId: senderServerId, // Также используем senderServerId для recipientServerId
    iss: `${senderServerId}.WebServer1.user1.${senderId}`,
    sub: "Message",
    audience: `${senderServerId}.WebServer1.user2.${recipientId}`,
    message: "ereb5454bwehqwy-3hgerh34ebd=",
    timestamp: new Date().toISOString()
};
const token = jwt.sign(payload, privateKey, {
    algorithm: 'ES512',
    header: jwtHeader
});

// ETAMP structure for a message
const message_etamp = {
    Id: id,
    Token: token,
    SignatureToken: signMessage(token, privateKey),
    SignatureMessage: signMessage(id + token + signMessage(token, privateKey), privateKey)
};

console.log(JSON.stringify(message_etamp, null, 4));
```
{% endcode %}
