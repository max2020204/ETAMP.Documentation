# Python

#### Description:

In Python, the `jwt` and `ecdsa` libraries are used to generate JWT tokens and to sign messages respectively. The `uuid` library is used to generate a unique identifier for the `Id` field.

#### Required Libraries:

1. `PyJWT`: A Python library to encode and decode JWT tokens.
2. `ecdsa`: A Python library for signing and verifying messages using ECDSA.
3. `uuid`: A Python library to generate unique identifiers.
4. `cryptography` : A Python library provides cryptographic recipes and primitives.



{% code overflow="wrap" lineNumbers="true" %}
```bash
pip install PyJWT ecdsa cryptography
```
{% endcode %}

{% code overflow="wrap" lineNumbers="true" %}
```python
import jwt
import ecdsa
from cryptography.hazmat.primitives.asymmetric import ec
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.backends import default_backend
import uuid
import datetime
import base64

# Key generation for JWT
private_key_jwt = ecdsa.SigningKey.generate(curve=ecdsa.NIST521p)
public_key_jwt = private_key_jwt.verifying_key

# Key generation for ECDH
private_key_ecdh = ec.generate_private_key(ec.SECP256R1(), default_backend())
peer_public_key = ec.generate_private_key(ec.SECP256R1(), default_backend()).public_key()

def private_key_to_pem(sk):
    # Convert the SigningKey to a PEM formatted string
    return sk.to_pem().decode()

def sign_message(message, private_key):
    signature = private_key.sign(message.encode())
    return base64.b64encode(signature).decode()

def encrypt_message_ecdh(message):
    shared_key = private_key_ecdh.exchange(ec.ECDH(), peer_public_key)
    return shared_key[:len(message)].hex()  # Return as hex for simplicity

def generate_etamp():
    header = {
        "alg": "ES512",
        "typ": "ETAMP"
    }
    
    current_time = datetime.datetime.now(datetime.timezone.utc)
    expiration_time = current_time + datetime.timedelta(seconds=3600)  # 1 hour later
    not_before_time = current_time

    senderServerId = str(uuid.uuid4())
    senderId = str(uuid.uuid4())
    recipientId = str(uuid.uuid4())
    message_id = str(uuid.uuid4())

    payload = {
        "jti": str(uuid.uuid4()),
        "exp": int(expiration_time.timestamp()),
        "nbf": int(not_before_time.timestamp()),
        "messageId": message_id,
        "senderUserName": "user1",
        "senderId": senderId,
        "recipient": "user2",
        "recipientId": recipientId,
        "senderServerName": "WebServer1",
        "senderServerId": senderServerId,
        "recipientServerName": "WebServer1",
        "recipientServerId": senderServerId,
        "iss": f"{senderServerId}.WebServer1.user1.{senderId}",
        "sub": "Message",
        "audience": f"{senderServerId}.WebServer1.user2.{recipientId}",
        "message": encrypt_message_ecdh("This is a secret message"),
        "timestamp": current_time.strftime('%Y-%m-%dT%H:%M:%SZ')
    }
    
    pem_private_key = private_key_to_pem(private_key_jwt)
    token = jwt.encode(payload, pem_private_key, algorithm="ES512", headers=header)

    # ETAMP structure for a message
    message_etamp = {
        "Id": message_id,
        "Token": token,
        "SignatureToken": sign_message(token, private_key_jwt),
        "SignatureMessage": sign_message(message_id + token + sign_message(token, private_key_jwt), private_key_jwt)
    }

    return message_etamp

if __name__ == "__main__":
    print(generate_etamp())
```
{% endcode %}
