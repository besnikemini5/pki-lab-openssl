# Certificate used for the wrong purpose

**Symptom**
```
error 26 at 0 depth lookup: unsuitable certificate purpose
```

**Cause**
The chain is valid, but the certificate's Extended Key Usage does not allow this use. The user certificate has only `clientAuth` (TLS Web Client Authentication) and was checked as a server certificate (`serverAuth`).

**How to diagnose**
`openssl x509 -in certs/user.crt -noout -ext extendedKeyUsage`

**Fix**
Reissue the certificate with the correct Extended Key Usage. In a real CA, this means using the correct certificate template.
