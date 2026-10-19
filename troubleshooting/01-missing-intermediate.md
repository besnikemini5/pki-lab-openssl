# Missing intermediate certificate

**Symptom**
```
error 20 at 0 depth lookup: unable to get local issuer certificate
```

**Cause**
The certificate's issuer (the Issuing CA) was not provided, so the chain could not be built from the server certificate up to the trusted root. In practice, this happens when a server sends only its own certificate and not the intermediate.

**How to diagnose**
- Check who issued the certificate: `openssl x509 -in certs/server.crt -noout -issuer`
- On a live server, count the certificates it sends: `openssl s_client -connect host:443 -showcerts`

**Fix**
Provide the intermediate: `-untrusted intermediate/int.crt` when verifying, or configure the server to send the full chain (its certificate + intermediate).
