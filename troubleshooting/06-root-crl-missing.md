# Root CA CRL missing (full chain check)

**Symptom**
```
error 3 at 1 depth lookup: unable to get certificate CRL
```

**Cause**
With `-crl_check_all`, every certificate in the chain is checked for revocation, including the Issuing CA certificate itself (depth 1). That requires a CRL published by the Root CA, which was not provided.

**How to diagnose**
The depth number shows which level failed: depth 0 is the end certificate, depth 1 is the Issuing CA.

**Fix**
The Root CA must also publish a CRL. Offline root CAs publish theirs on a long schedule (e.g. every few months), and it must be renewed before it expires.
