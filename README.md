# PKI Lab with OpenSSL

A two-tier PKI (offline-style root CA + issuing CA) built with OpenSSL, issuing a TLS server certificate and a client-authentication (user) certificate.

## Certificate chain

```
Besnik Lab Root CA
 └── Besnik Lab Issuing CA
      ├── server  (TLS Web Server Authentication, DNS:app.lab.local)
      └── user    (TLS Web Client Authentication, email:anna@lab.local)
```

## Troubleshooting notes

Errors reproduced on purpose in this lab, with cause, diagnosis and fix:

- [Missing intermediate certificate](troubleshooting/01-missing-intermediate.md)
- [Root CA not in the trust store](troubleshooting/02-root-not-trusted.md)
- [Certificate used for the wrong purpose](troubleshooting/03-wrong-certificate-purpose.md)

## Private keys

Private keys (`*.key`) are excluded with `.gitignore`. Only public certificates, requests and documentation are published. A leaked CA private key would let anyone issue certificates the PKI trusts.

## Part 2: Revocation

The Issuing CA was set up with an OpenSSL CA database (`intermediate/ca.cnf`, `index.txt`, `crlnumber`). The user certificate was revoked with reason Key Compromise (as if a smart card were lost), and a CRL was published (`intermediate/int.crl`, valid 30 days).

- User certificate checked against the CRL: `certificate revoked`
- Server certificate checked against the CRL: `OK`

More troubleshooting notes:

- [Certificate revoked](troubleshooting/04-certificate-revoked.md)
- [CRL required but not available](troubleshooting/05-crl-not-available.md)
- [Root CA CRL missing (full chain check)](troubleshooting/06-root-crl-missing.md)
