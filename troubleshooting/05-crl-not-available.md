# CRL required but not available

**Symptom**
```
error 3 at 0 depth lookup: unable to get certificate CRL
```

**Cause**
Revocation checking was required (`-crl_check`), but no CRL from the issuing CA was available. The verifier refuses instead of assuming the certificate is not revoked. In real environments this happens when the CRL location is unreachable or the CRL has passed its Next Update date.

**How to diagnose**
- Check the CRL exists and when it expires: `openssl crl -in intermediate/int.crl -noout -lastupdate -nextupdate`
- In a real environment, check that the CRL distribution point is reachable from the machine doing the check.

**Fix**
Publish a current CRL (`openssl ca -config intermediate/ca.cnf -gencrl -out intermediate/int.crl`) and make it available to the verifier (`-CRLfile` here).
