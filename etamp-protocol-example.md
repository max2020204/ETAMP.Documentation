# ETAMP Protocol Example

In the ETAMP protocol, each message is wrapped in a structured format to ensure security and proper communication. Here is an example based on the given structure for sending a message from `fromUser` to `toUser`.

### 1. Key Generation:

*   **Private Key**:

    {% code overflow="wrap" lineNumbers="true" %}
    ```plaintext
    MHcCAQEEIOsakR2Im5ao5a6yhbv1j0SrebD0Krz0vFJbY1zQ+oL6oAoGCCqGSM49AwEHoUQDQgAEK6kLAhRPYjV3xPOIbDkTyzbL8Bo3d8R/Zvf3W1RXHKAz4rmc8fH6BJCSLyd8FZL492VvvWmp/4ilc1a1+swvdxg==
    ```
    {% endcode %}
*   **Public Key**:

    {% code overflow="wrap" lineNumbers="true" %}
    ```plaintext
    BErqQsCFE9iNdfE84hsORPLOsvwGjdzxH9m9/dYVccoDPiuZzx8foEkJIpvJ3wVkvj3ZW+9aan/iKVzVrX6zC93G
    ```
    {% endcode %}

### 2. Creating a JWT Token for a Message:

*   **Header**:

    {% code overflow="wrap" lineNumbers="true" %}
    ```json
    {
       "alg":"ES512",
       "typ":"JWT"
    }
    ```
    {% endcode %}
*   **Payload**:

    {% code overflow="wrap" lineNumbers="true" %}
    ```json
    {
       "jti":"6F923C36-9A16-460A-85C1-F10F43A61411",
       "exp":1679992314,
       "nbf":1679988714,
       "messageId":"8D85BEAF-9468-49A8-A165-75AB42B01BEB",
       "senderUserName":"user1",
       "senderId":"524B496E-BDE9-473B-A143-55A7F590C373",
       "recipient":"user2",
       "recipientId":"2480756A-DCBB-498B-A92D-AF1093BA26D6",
       "senderServerName":"WebServer1",
       "senderServerId":"B2F7714F-891F-4A4F-93F0-D472309DE102",
       "recipientServerName":"WebServer1",
       "recipientServerId":"B2F7714F-891F-4A4F-93F0-D472309DE102",
       "iss":"B2F7714F-891F-4A4F-93F0-D472309DE102.WebServer1.user1.524B496E-BDE9-473B-A143-55A7F590C373",
       "sub":"Message",
       "audience":"B2F7714F-891F-4A4F-93F0-D472309DE102.WebServer1.user2.2480756A-DCBB-498B-A92D-AF1093BA26D6",
       "message":"ereb5454bwehqwy-3hgerh34ebd=",
       "timestamp":"2023-10-10T10:10:10Z"
    }
    ```
    {% endcode %}
*   **Signature**:

    {% code overflow="wrap" lineNumbers="true" %}
    ```plaintext
    MEUCIQDz3Cz8mYph5xTC1DR0fUvD2wP12eN0P1vHzoWxAil7yAIgTzbsBIhsyP0oGhjs7hhJ6v3E396zZm60EhpzG4F55X0=
    ```
    {% endcode %}

### 3. Creating the ETAMP Structure for a Message:

{% code overflow="wrap" lineNumbers="true" %}
```json
{
  "Id": "550e8400-e29b-41d4-a716-446655440000",
  "Version": "1.0",
  "UpdateType":"Message",
  "Token": "eyJhbGciOiAiRVMyNTYiLCAidHlwIjogIkpXVCJ9.eyJqdGkiOiAiNTUwZTg0MDAtZTI5Yi00MWQ0LWE3MTYtNDQ2NjU1NDQwMDAwIiwgbWVzc2FnZUlkOiAiMTIzNDU2Nzg5MCIsIGZyb206ICJBbGljZSIsIHVzZXJOYW1lOiAiQm9iIiwgbWVzc2FnZTogIkhlbGxvLCBCb2IhIn0.MEUCIQDz3Cz8mYph5xTC1DR0fUvD2wP12eN0P1vHzoWxAil7yAIgTzbsBIhsyP0oGhjs7hhJ6v3E396zZm60EhpzG4F55X0=",
  "SignatureToken": "MEUCIQDz3Cz8mYph5xTC1DR0fUvD2wP12eN0P1vHzoWxAil7yAIgTzbsBIhsyP0oGhjs7hhJ6v3E396zZm60EhpzG4F55X0=",
  "SignatureMessage": "MEYCIQDz3Cz8mYph5xTC1DR0fUvD2wP12eN0P1vHzoWxAil7yAIgTzbsBIhsyP0oGhjs7hhJ6v3E396zZm60EhpzG4F55X0="
}
```
{% endcode %}
