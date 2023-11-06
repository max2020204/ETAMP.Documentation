# Secure Message Encryption

The ETAMP protocol provides a comprehensive security model that includes not only the integrity and authenticity of messages via JWT but also confidentiality through the use of Elliptic Curve Diffie-Hellman (ECDH) encryption. This section outlines the process by which users can secure their message contents using ECDH.

### Overview of ECDH Encryption

ECDH is a public-key encryption technique that allows two parties to establish a shared secret over an insecure channel. This shared secret can then be used to encrypt message contents, ensuring that only the intended recipient can decrypt and read the message.

### ECDH in the ETAMP Protocol

To further enhance privacy, users of the ETAMP protocol have the option to encrypt the payload of their messages with ECDH encryption. This adds an additional layer of security, making the content of the messages accessible only to the sender and the intended recipient.

### Steps for Encrypting Messages with ECDH

1. **Key Exchange:**
   * The sender and recipient must exchange their public keys, which can be done via the ETAMP protocol's KeyExchange update type.
2. **Shared Secret Creation:**
   * Using their own private key and the recipient's public key, the sender generates a shared secret.
3. **Encrypting the Message:**
   * The sender encrypts the message content with the shared secret using a symmetric encryption algorithm, such as AES.
4. **Sending the Encrypted Message:**
   * The encrypted message is then included in the JWT payload and transmitted to the recipient.

### Example of Message Encryption Process

```pseudo
// Pseudo code for message encryption
string senderPrivateKey = "sender_private_key";
string recipientPublicKey = "recipient_public_key";

// Generate the shared secret
string sharedSecret = ECDH_compute_shared_secret(senderPrivateKey, recipientPublicKey);

// Encrypt the message
string messageContent = "The quick brown fox jumps over the lazy dog";
string encryptedMessage = AES_encrypt(messageContent, sharedSecret);

// The JWT payload with the encrypted message
{
  // Standard JWT claims
  "msg": encryptedMessage
  // ... other claims
}
```

### Decryption by the Recipient

Upon receiving the JWT with the encrypte message (`msg`), the recipient uses their private key and the sender's public key to recreate the shared secret and decrypt the message.

### Security Benefits

* **Confidentiality:** Encryption with ECDH ensures that the message content remains confidential during transit.
* **Forward Secrecy:** ECDH provides forward secrecy, meaning that even if private keys are compromised in the future, past communications remain secure.
* **Non-repudiation:** Since the message is still signed as part of the JWT, the integrity and authenticity are preserved alongside confidentiality.

### Conclusion

By incorporating ECDH encryption into the messaging process, the ETAMP protocol ensures that user communications are secure and private. This feature is critical for scenarios where sensitive information needs to be exchanged and protected from any unauthorized access.
