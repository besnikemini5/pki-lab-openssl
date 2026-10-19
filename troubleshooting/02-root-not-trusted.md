# Root CA not in the trust store

**Symptom**
```
error 2 at 1 depth lookup: unable to get issuer certificate
```

**Cause**
The intermediate was found, but its issuer (the Root CA) was not available as a trust anchor. By default OpenSSL requires the chain to end at a self-signed root. With `-partial_chain` it accepts the intermediate as the anchor and verification passes.

**How to diagnose**
- Compare issuer and subject at each level: `openssl x509 -in intermediate/int.crt -noout -issuer -subject`
- Match the intermediate's Authority Key Identifier with the root's Subject Key Identifier.

**Fix**
Add the Root CA certificate to the trust store (`-CAfile root/root.crt` here, or the system / Windows trusted root store in a real environment).
