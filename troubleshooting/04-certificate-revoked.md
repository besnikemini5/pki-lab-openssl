# Certificate revoked

**Symptom**
```
error 23 at 0 depth lookup: certificate revoked
```

**Cause**
The certificate is within its validity dates and its chain is valid, but the CA has revoked it and listed its serial number in the CRL (here with reason Key Compromise, e.g. a lost smart card).

**How to diagnose**
- Get the certificate's serial: `openssl x509 -in certs/user.crt -noout -serial`
- Look for that serial in the CRL: `openssl crl -in intermediate/int.crl -noout -text`
- On the CA, revoked entries start with `R` in `intermediate/index.txt`.

**Fix**
Revocation is intended and cannot be undone for this reason. Issue a new certificate (e.g. enroll a new smart card) for the user.
