# C\#

#### Required Libraries:

* `System.IdentityModel.Tokens.Jwt`: For handling JWT tokens.
* `System.Security.Cryptography`: For handling ECDSA signature generation and verification.

{% code overflow="wrap" lineNumbers="true" %}
```bash
dotnet add package System.IdentityModel.Tokens.Jwt
```
{% endcode %}

{% code overflow="wrap" lineNumbers="true" %}
```csharp
using System;
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Security.Cryptography;
using Microsoft.IdentityModel.Tokens;

public class Program
{
    public static void Main()
    {
        // Key generation
        using ECDsa ecdsa = ECDsa.Create(ECCurve.NamedCurves.nistP521);
        var privateKey = new ECDsaSecurityKey(ecdsa);

        // Generate JWT token for a message
        var id = Guid.NewGuid().ToString();
        var token = CreateToken(id, privateKey);

        // ETAMP structure for a message
        var message_etamp = new
        {
            Id = id,
            Token = token,
            SignatureToken = SignMessage(token, ecdsa),
            SignatureMessage = SignMessage(id + token + SignMessage(token, ecdsa), ecdsa)
        };
        Console.WriteLine(message_etamp);
    }

    public static string CreateToken(string id, SecurityKey key)
    {
        var tokenHandler = new JwtSecurityTokenHandler();

        // Generate timestamps for the JWT token
        var currentDateTime = DateTime.UtcNow;
        var expiryDateTime = currentDateTime.AddHours(1);
        var notBeforeDateTime = currentDateTime;
        var senderId = Guid.NewGuid();
        var recipientId = Guid.NewGuid();
        var serverId = Guid.NewGuid();
        var tokenDescriptor = new SecurityTokenDescriptor
        {
            Subject = new ClaimsIdentity(new[]
            {
                new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
                new Claim("messageId", id),
                new Claim("senderUserName", "user1"),
                new Claim("senderId", senderId.ToString()),
                new Claim("recipient", "user2"),
                new Claim("recipientId", recipientId.ToString()),
                new Claim("senderServerName", "WebServer1"),
                new Claim("senderServerId", serverId.ToString()),
                new Claim("recipientServerName", "WebServer1"),
                new Claim("recipientServerId", serverId.ToString()),
                new Claim("iss", $"{serverId}.WebServer1.user1.{senderId}"),
                new Claim("sub", "Message"),
                new Claim("audience", $"{serverId}.WebServer1.user2.{recipientId}"),
                new Claim("message", "ereb5454bwehqwy-3hgerh34ebd="),
                new Claim("timestamp", DateTime.Now.ToUniversalTime().ToString())
            }),
            TokenType = "ETAMP",
            Expires = expiryDateTime,
            NotBefore = notBeforeDateTime,
            SigningCredentials = new SigningCredentials(key, SecurityAlgorithms.EcdsaSha512Signature)
        };

        var token = tokenHandler.CreateToken(tokenDescriptor);
        return tokenHandler.WriteToken(token);
    }

    public static string SignMessage(string message, ECDsa ecdsa)
    {
        var data = Encoding.UTF8.GetBytes(message);
        var signature = ecdsa.SignData(data, HashAlgorithmName.SHA512);
        return Convert.ToBase64String(signature);
    }
}
```
{% endcode %}
