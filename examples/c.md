# C\#

The ETAMP (Encrypted Token and Message Protocol) is a sophisticated .NET library designed for the secure encryption and validation of messages and tokens. Leveraging advanced cryptographic methods such as ECC, AES, and ECDH, ETAMP offers high security and flexibility for digital communication needs. This guide provides a step-by-step example of creating an ETAMP token, encrypting it, and utilizing ETAMP's cryptographic components.

**Installation**

To start using ETAMP in your project, first, ensure that the ETAMP package is installed via the NuGet Package Manager:

```shell
Install-Package ETAMP
```

**Creating and Encrypting an ETAMP Token**

To create and encrypt an ETAMP token, follow these steps:

1.  **Define a Payload Class**

    Define a class for the payload you intend to encrypt. For example, an `Order` class:

    ```csharp
    public class Order : BasePayload {
        public string ItemName { get; set; }
        public decimal Price { get; set; }
    }
    ```
2.  **Initialize Encryption Components**

    Initialize the necessary components for encryption, including the key wrapper and encryption service factory:

    ```csharp
    var ecdhKeyWrapper = new EcdhKeyWrapper();
    var encryptionServiceFactory = new EncryptionServiceFactory();
    encryptionServiceFactory.RegisterEncryptionService("AES", () => new AesEncryptionService());

    var eciesEncryptionService = new EciesEncryptionService(ecdhKeyWrapper, encryptionServiceFactory, "AES");
    var etampEncryption = new ETAMPEncryption(eciesEncryptionService);
    ```
3.  **Create and Encrypt the Payload**

    Instantiate your payload and encrypt it using ETAMP:

    ```csharp
    var order = new Order {
        ItemName = "Laptop",
        Price = 999.99M
    };

    ETAMPEncrypted encryptedETAMP = etampEncryption.CreateEncryptETAMP("order", order, true, 1.0);
    ```

    The `encryptedETAMP` object now contains the encrypted token and its cryptographic details.

**Using Cryptographic Components**

The ETAMP library integrates several cryptographic components, such as `EcdhKeyWrapper` and `EncryptionServiceFactory`, to provide enhanced security through Elliptic Curve Cryptography (ECC), Advanced Encryption Standard (AES), and Elliptic-curve Diffie–Hellman (ECDH). These components are crucial for the encryption process, ensuring that the token content is secured from creation without handling an unencrypted token at any stage.

**Conclusion**

This guide outlines how to securely create and encrypt ETAMP tokens in a .NET application using the ETAMP library. The library's design, based on SOLID principles, ensures it is adaptable to specific security requirements with customizable curves, keys, and algorithms, offering high maintainability and scalability for secure digital communication.
