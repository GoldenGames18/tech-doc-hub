# 🪪 Identity Framework

## Management of available retry attempts.

```C#
if (userManager.SupportsUserLockout && userManager.IsLockedOut(userId))
    return;

var user = userManager.FindById(userId);
if (userManager.CheckPassword(user, password))
{
    if (userManager.SupportsUserLockout && userManager.GetAccessFailedCount(userId) > 0)
    {
        userManager.ResetAccessFailedCount(userId);
    }

    // Authenticate user
}
else
{
    if (userManager.SupportsUserLockout && userManager.GetLockoutEnabled(userId))
    {
        userManager.AccessFailed(userId);
    }
}
```

## Implement a password algorithm

[Isopoh.Cryptography.Argon2](https://mheyman.github.io/Isopoh.Cryptography.Argon2/index.html)

**Argon2**  is currently one of the best password hashing algorithms and remains backward compatible if you change the lane size.

```C#
public class Argon2PasswordHasher<TUser> : IPasswordHasher<TUser> where TUser : class
{
    private byte[] GenerateSalt()
    {
        var salt = new byte[32];
        RandomNumberGenerator.Fill(salt);
        return salt;
    }

    public string HashPassword(TUser user, string password)
    {
        var config = new Argon2Config
        {
            Type = Argon2Type.HybridAddressing,
            Version = Argon2Version.Nineteen,
            Password = System.Text.Encoding.UTF8.GetBytes(password),
            Salt = GenerateSalt(),
            MemoryCost = 15360,
            TimeCost = 2,
            Lanes = 1
        };
        using var cypher = new Argon2(config);
        using var hash = cypher.Hash();
        return config.EncodeString(hash.Buffer);
    }

    public PasswordVerificationResult VerifyHashedPassword(TUser user, string hashedPassword, string providedPassword)
    {
        return Argon2.Verify(hashedPassword, providedPassword)
            ? PasswordVerificationResult.Success
            : PasswordVerificationResult.Failed;
    }
}
```

