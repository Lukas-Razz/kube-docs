---
layout: article
title: Certificate
permalink: /docs/certificate.html
key: certificate
aside:
  toc: true
sidebar:
  nav: docs
---

Kubernetes can issue and manage custom trusted certificates using ACME protocol. Certificates are stored as secrets.

This is especially important for enabling TLS when exposing non-web applications, therefore without ingress.

Currently, there are two issuers available:
* **letsencrypt-prod** issuer is web-only and automatically checks the web on provided domain name during issuing process. It is automatically used when exposing application through [ingress](kubectl-expose.html#web-based-applications).
* **letsencrypt-prod-dns** issuer is for other applications. The difference is checking through dns. However, the dns checking is slower, because the dns change must be propagated first.

To issue a kubernetes-managed certificate, the following configuration can be used.
```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: application-dyn-cloud-e-infra-cz-tls
spec:
  secretName: application-dyn-cloud-e-infra-cz-tls
  issuerRef:
    group: cert-manager.io
    kind: ClusterIssuer
    name: <ISSUER-NAME>
  dnsNames:
  - "application.dyn.cloud.e-infra.cz"
  usages:
  - digital signature
  - key encipherment
```
Where ```metadata.name```, ```spec.secretName``` refer to the name of generated certificate. The ```spec.dnsNames``` items are the target dns names of the certificate. Issuer is specified in the ```spec.issuerRef.name```.

The configuration generates an secret specified in ```spec.secretName```, containing certificate and private key pair.