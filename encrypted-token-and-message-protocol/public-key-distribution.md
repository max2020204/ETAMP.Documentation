# Public Key Distribution

In the ETAMP protocol, ensuring secure communication is paramount. One of the foundational steps towards this goal is the distribution of public keys among servers and users. These public keys are shared in a standardized format known as PEM (Privacy Enhanced Mail), which encodes the key data in Base64, wrapped between a header and a footer.

### Advantages of Using PEM Format

1. **Human-Readable Format**:
   * PEM is text-based, making it easier to handle and inspect, which is beneficial during debugging or manual validation processes.
   * The clear header and footer lines encapsulating the encoded data make it easily identifiable and delineate the key data clearly.
2. **Standardized**:
   * The PEM format is widely accepted and used across various cryptographic systems and libraries, making it a standardized and reliable choice for key distribution within the ETAMP protocol.
3. **Ease of Sharing**:
   * Being a text format, PEM-encoded keys can be easily shared across different mediums, whether it's via email, text files, or through configuration settings.
4. **Support for Various Cryptographic Algorithms**:
   * PEM format is versatile and supports various cryptographic algorithms, making it a flexible choice for different cryptographic operations within the ETAMP protocol.

### Example of a Public Key in PEM Format

A public key in PEM format is represented as a Base64 encoded string, wrapped between a "-----BEGIN PUBLIC KEY-----" header and a "-----END PUBLIC KEY-----" footer. Here's a simplified example of what a public key might look like in PEM format:

{% code overflow="wrap" lineNumbers="true" %}
```
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE0IisxZvukTysleNoTD4LcIq9SBjN
0U2g1y3v8haoyUDoiZvHOE1aIzG3uRBIqlQ1Q+eziHSjV0M8uiNNk3FAJQ==
-----END PUBLIC KEY-----
```
{% endcode %}

In this example, the actual public key data is encoded in Base64 between the header and footer lines. The ETAMP protocol utilizes this format to distribute and verify public keys efficiently, ensuring the authenticity and integrity of the tokens and signatures within the network.
