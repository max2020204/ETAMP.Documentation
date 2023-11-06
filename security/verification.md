# Verification

The ETAMP protocol ensures the security and integrity of communications through rigorous verification and authentication processes. This page details how signatures are verified and how authenticity is confirmed within the system.

### Signature Verification

Every message transmitted within the ETAMP network is signed using the sender's private key. To ensure the message has not been tampered with, the recipient must verify the signature using the sender's public key.

#### Process of Signature Verification

1. **Obtaining the Signature:**
   * Extract the `SignatureMessage` or `SignatureToken` from the received message.
2. **Extracting the Public Key:**
   * Retrieve the sender's public key, which may be distributed beforehand or sent as part of the message in a secure manner.
3. **Verifying the Signature:**
   * Use the corresponding signature verification function with the sender's public key to check the signature against the message or token payload.

#### Pseudo Code for Signature Verification

```pseudo
// Pseudo code for verifying the signature
string receivedMessage = "The JWT message";
string receivedSignature = "The signature to verify";
string senderPublicKey = "sender_public_key";

bool isSignatureValid = ECDSA_verify(receivedMessage, receivedSignature, senderPublicKey);

if (isSignatureValid) {
  // Proceed with processing the message
} else {
  // Reject the message and potentially log the verification failure
}
```

### Authentication of Message Origin

Authentication confirms that a message was indeed sent by the claimed sender and helps prevent impersonation and man-in-the-middle attacks.

#### Steps for Authentication

1. **Validating the Token:**
   * Check the JWT's `SignatureToken` to ensure the token was signed by the sender.
2. **Confirming Identity:**
   * Cross-reference the sender's identifier in the JWT payload (`sub` claim) with the public key used for signature verification.
3. **Ensuring Non-Repudiation:**
   * The combination of signature verification and identity confirmation ensures that the sender cannot deny having sent the message.

### Security Benefits

* **Integrity:** Signature verification ensures that the message received is exactly as the sender sent it and has not been altered.
* **Authenticity:** Authentication proves that the message truly came from the sender and not an imposter.
* **Trust:** These processes build trust in the communication channel, as each participant can be confident in the origin and integrity of the messages.

### Best Practices for Enhanced Security

* **Key Management:** Ensure that public keys are exchanged and stored securely to prevent unauthorized access or manipulation.
* **Certificate Authorities:** Use certificate authorities (CAs) or a public key infrastructure (PKI) to validate public keys when possible.
* **Regular Key Rotation:** Periodically rotate keys to minimize the risk of key compromise affecting long-term message security.

### Conclusion

Verification and authentication are critical components of the ETAMP protocol's security model. By diligently following these processes, participants can ensure the reliability and confidentiality of their communications within the network.
