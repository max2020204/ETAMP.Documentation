# Protocol Architecture

The ETAMP protocol defines a unique structure for messages and transactions. The structure is as follows:

{% code overflow="wrap" lineNumbers="true" %}
```plaintext
GUID Id
double Version
string Token
string UpdateType
string SignatureToken
string SignatureMessage
```
{% endcode %}
