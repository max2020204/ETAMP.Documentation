# Signature Message

The ETAMP protocol ensures the security of communications by including a `SignatureMessage` field, which encompasses the digital signature of the entire message payload. This field is critical for maintaining the integrity and non-repudiation of messages.

### Comprehensive Message Signing

The `SignatureMessage` provides an additional layer of security by signing not just the token, but the entire message. This includes the following components:

* **Id:** A unique identifier for the message.
* **Token:** The JWT that carries the message content.
* **SignatureToken:** The digital signature of the token.

By signing all parts together, the protocol ensures that the message, in its entirety, is authenticated.

### Digital Signature Process

Like the `SignatureToken`, the `SignatureMessage` is also generated using the ECDSA digital signature algorithm. The steps are as follows:

1. **Message Concatenation:** The unique Id, Token, and SignatureToken are concatenated to form a single string.
2. **Private Key Usage:** The sender's private key, which should be securely stored, is used to sign the concatenated string.
3. **Signature Generation:** The ECDSA algorithm is applied using the `nistp512` curve to create a robust signature.

#### Pseudo Code for Signature Generation:

Here's a pseudo code example illustrating how the `SignatureMessage` might be generated:

```pseudo
string message = "550e8400-e29b-41d4-a716-446655440000eyJhbGciOi...MEUCIQD...";  // Concatenation of Id, Token, and SignatureToken
string privateKey = "-----BEGIN PRIVATE KEY-----\n...";  // The sender's private key in PKCS#8 format

// Generate the signature
string signatureMessage = ECDSA_sign(message, privateKey, "nistP512");

// The resulting SignatureMessage
SignatureMessage: "MEYCIQD...";
```

### The Role of SignatureMessage

The `SignatureMessage` serves critical functions within the ETAMP protocol:

* **Integrity Check:** It verifies that the message has not been altered since it was signed.
* **Authentication:** It authenticates the sender, as the signature can only be generated with the sender's private key.
* **Non-Repudiation:** It prevents the sender from denying the sending of the message.

### Security Implications

Using the `nistp512` curve for ECDSA ensures that the `SignatureMessage` is secured with one of the most robust cryptographic algorithms, making it computationally infeasible to forge or tamper with.

### Conclusion

The `SignatureMessage` field is an indispensable security mechanism within the ETAMP protocol. It encapsulates the trustworthiness of the entire message, ensuring that each communication carried out over the network can be trusted and verified.
