# Token Structure

The ETAMP protocol leverages the versatility of JSON Web Tokens (JWT) for encoding and exchanging information between parties within the network. This documentation outlines the JWT structure and how it's adapted for diverse project needs within the ETAMP protocol.

### Overview of JWT

JWTs provide a compact, URL-safe method for representing claims securely between two parties. Within the ETAMP protocol, these claims form the substance of the messages or transactions communicated.

### JWT Composition

Each JWT consists of three distinct parts:

* **Header**
* **Payload**
* **Signature**

These components are base64-url encoded and joined with periods (`.`) to create the complete token.

### JWT Header

The header declares the token's type and the algorithm used for signing:

```json
{
  "alg": "ES512",
  "typ": "ETAMP"
}
```

### JWT Payload

The payload is the flexible heart of the JWT, containing a set of claims that can be tailored to fit the specific data requirements of any given project within the ETAMP protocol.

{% code overflow="wrap" lineNumbers="true" %}
```json
{
  "iss": "server_identifier",
  "sub": "user_identifier",
  // ... standard JWT claims
  // Project-specific custom claims:
  "customClaim1": "value1",
  "customClaim2": "value2",
  // Additional claims as defined by the project's needs
}
```
{% endcode %}

The payload's structure is designed to be extensible, allowing projects to define and include any relevant information as custom claims.

### JWT Signature

The signature secures the token and confirms the sender's identity. It is calculated by signing the encoded header and payload with the sender's private key using the specified algorithm.

### Full JWT Example

A fully constructed JWT in the ETAMP protocol might look as follows:

{% code overflow="wrap" lineNumbers="true" %}
```
Base64UrlEncode(header) + "." + Base64UrlEncode(payload) + "." + ECDSA(signature)
```
{% endcode %}

### Usage in ETAMP Protocol

In the ETAMP protocol, the `Token` field of a message is populated with a JWT. The payload of this JWT is project-specific, containing the relevant information for the particular operation being performed.

#### Handling Custom Token Fields

* **Validation:** Recipients validate the signature using the sender's public key to ensure the token's authenticity and integrity.
* **Processing:** The payload, including any custom claims, is processed in line with the project's unique requirements.

### Security Considerations

* **Confidentiality:** Encryption should be applied to sensitive data within the token payload to protect it from unauthorized access.
* **Key Management:** Secure management of cryptographic keys is crucial for maintaining the integrity of the token's signature.

### Conclusion

By enabling the inclusion of custom claims, the JWT becomes a powerful tool in the ETAMP protocol, providing a secure and adaptable means of data transfer. This flexibility allows the protocol to cater to a wide array of project-specific needs, ensuring that developers and architects can implement a system that aligns precisely with their operational objectives.
