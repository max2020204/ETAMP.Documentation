# Signature Token

## The SignatureToken Field in the ETAMP Protocol

The ETAMP protocol incorporates a robust security feature known as the `SignatureToken` field. This field is essential for verifying the authenticity and integrity of the token exchanged between parties in the network.

### Digital Signature Using ECDSA

The `SignatureToken` is a digital signature that is appended to the token. Here's how it is generated:

* **Algorithm:** The signature is created using the Elliptic Curve Digital Signature Algorithm (ECDSA).
* **Curve:** Specifically, the `nistp512` curve is employed, which is known for its strong cryptographic properties.
* **Purpose:** This signature confirms that the token has been issued by a legitimate source and that its contents have not been tampered with during transit.

### Process of Generating the SignatureToken

The generation of a `SignatureToken` involves several steps, which typically include:

1. **Token Preparation:** The original JWT token is prepared, which contains the message, transaction or other details.
2. **Private Key:** The sender's private key is used in the signing process. This key should be kept confidential and secure.
3. **Signing Function:** An ECDSA signing function takes the token and the private key as inputs and produces the signature.

#### Pseudo Code Example:

Below is an example of pseudo code demonstrating the generation of a `SignatureToken`:

```pseudo
string token = "eyJhbGciOi...";  // The JWT token
string privateKey = "-----BEGIN PRIVATE KEY-----\n...";  // The sender's private key in PKCS#8 format

// Generate the signature
string signatureToken = ECDSA_sign(token, privateKey, "nistP512");

// The resulting SignatureToken
SignatureToken: "MEUCIQD...";
```

### Understanding the SignatureToken Output

The `SignatureToken` itself is a string that may look something like "MEUCIQD...". This string is a base64-url encoded representation of the binary signature.

### Importance of the SignatureToken

The `SignatureToken` serves multiple security purposes:

* **Verification:** Recipients use this signature to verify that the token is genuine.
* **Non-Repudiation:** It provides evidence that cannot be reasonably denied by the sender of the token.
* **Integrity:** Any alteration to the token after it has been signed will invalidate the signature, thereby alerting the recipient to potential tampering.

### Conclusion

In summary, the `SignatureToken` field is a vital component of the ETAMP protocol, ensuring that all communications over the network are secure and trustworthy. It is a testament to the protocol's commitment to providing a secure communication framework for its users.
