# Id

The `Id` field in the ETAMP protocol is a unique identifier assigned to each message or transaction to ensure traceability and uniqueness. This identifier is a GUID (Globally Unique Identifier), which guarantees that each message or transaction is uniquely identifiable across the entire network, regardless of the server or client generating it.

Here is an example of how the `Id` field might look:

{% code overflow="wrap" lineNumbers="true" %}
```plaintext
GUID Id: 550e8400-e29b-41d4-a716-446655440000
```
{% endcode %}

### ID Synchronization

In the ETAMP protocol, it's crucial to maintain a consistent identification scheme to ensure the accurate tracking and processing of messages and transactions. As such, the `Id` field generated during the creation of an ETAMP structure and the `messageId` field within the JWT must be identical. This synchronization allows for a clear correlation between the ETAMP structure and its corresponding JWT, facilitating reliable message handling and auditing within the network.

#### Example

When creating an ETAMP structure and generating a JWT, ensure that the `Id` and `messageId` fields match, as illustrated below:

{% code overflow="wrap" lineNumbers="true" %}
```json
// ETAMP Structure
{
    "Id": "550e8400-e29b-41d4-a716-446655440000",
    ...
}

// Corresponding JWT Payload
{
    "messageId": "550e8400-e29b-41d4-a716-446655440000",
    ...
}
```
{% endcode %}
