---
layout: post
title: Integrate cert-manager to k3s ingress with Google Domains
description: Integrate cert-manager to k3s ingress with Google Domains
keywords: 
---

### Concepts

#### Issuer
- Inssuers, ClusterIssuers are k8s resources that represent CA that are able to generate signed certificates.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Issuer
metadata:
  name: ca-issuer
  namespace: mesh-system
spec:
  ca:
    secretName: ca-key-pair
```

This is a simple Issuer that will sign certificates based on a private key. The certificate  stored in the secret `ca-key-pair` can be used to trust newly signed certificates by this Issuer in Public Key Infrastructure (PKI) system.

##### Namespaces
An Issuer is namespaced resource.
If you want to create a single Issuer that can be consumed in multiple namespaces, you should consider ClusterIssuer

#### Certificate
Defines a desired x509 certificate which will be renewed and kept up to date
When a Certificate is created, a corresponding CertificateRequest resource is created by cert-manager containing the encoded x509 certificate request.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: acme-crt
spec:
  secretName: acme-crt-secret
  dnsNames:
  - foo.example.com
  - bar.example.com
  issuerRef:
    name: letencrypt-prod
    kind: Issuer
    group: cert-manager.io
```

This Certificate will tell cert-manager to attempt to use the Issuer named letencrypt-prod to obtain a certificate key pair for the `foo.example.com` and `bar.example.com` domains.
If successful, the resulting key and certificate will be stored in a secret named `acme-crt-secret` with keys `tls.key` and `tls.crt` respectively.

#### ACME Orders and Challenges
cert-manager supports requesting certificates from ACME Server including [Let's Encrypt](https://letsencrypt.org/).

To successfully request a certificate, cert-manager must solve ACME Challenges which are completed in order to prove that the client owns the DNS addresses that are being requested.

##### Orders

Orders resources are used by the ACME issuer to manage the lifecycle of of an ACME 'order' for a signed TLS certificate

### Installation

[Kubernetes](https://cert-manager.io/docs/installation/kubernetes/)

```bash
$ kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.16.1/cert-manager.yaml
```
#### Verification
```bash
$ kubectl get pods --namespace cert-manager

NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-5c6866597-zw7kh               1/1     Running   0          2m
cert-manager-cainjector-577f6d9fd7-tr77l   1/1     Running   0          2m
cert-manager-webhook-787858fcdb-nlzsq      1/1     Running   0          2m
```

Create a test Issuer

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: cert-manager-test
---
apiVersion: cert-manager.io/v1alpha2
kind: Issuer
metadata:
  name: test-selfsigned
  namespace: cert-manager-test
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: selfsigned-cert
  namespace: cert-manager-test
spec:
  dnsNames:
    - example.com
  secretName: selfsigned-cert-tls
  issuerRef:
    name: test-selfsigned
```

Create the test resources

```bash
$ kubectl apply -f cert-manager-test.yaml
```

Check the status of Certificate

```bash
$ kubectl describe certificate -n cert-manager-test
Status:
  Conditions:
    Last Transition Time:  2020-08-11T14:34:08Z
    Message:               Certificate is up to date and has not expired
    Reason:                Ready
    Status:                True
    Type:                  Ready
  Not After:               2020-11-09T14:34:07Z
  Not Before:              2020-08-11T14:34:07Z
  Renewal Time:            2020-10-10T14:34:07Z
  Revision:                1
Events:
  Type    Reason     Age   From          Message
  ----    ------     ----  ----          -------
  Normal  Issuing    42s   cert-manager  Issuing certificate as Secret does not exist
  Normal  Generated  42s   cert-manager  Stored new private key in temporary Secret resource "selfsigned-cert-6pkq2"
  Normal  Requested  42s   cert-manager  Created new CertificateRequest resource "selfsigned-cert-nbsqw"
  Normal  Issuing    42s   cert-manager  The certificate has been successfully issued
```

Clean up

```bash
$ k delete -f cert-manager-test.yaml
```

### Configuration
In this post, we are gonna generate Let's Encrypt certificate which is ACME server

[ACME](https://cert-manager.io/docs/configuration/acme/)

##### Setup Service Account for Google CloudDNS
```bash
$ export PROJECT_ID=$(gcloud config get-value project)
$ gcloud iam service-accounts create dns01-solver --display-name "dns01-solver"
$ gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member serviceAccount:dns01-solver@$PROJECT_ID.iam.gserviceaccount.com \
  --role roles/dns.admin
```
##### Create account secret
In this step, we are going to generate `key.json` from google service account and save it into k8s secret

```bash
$ gcloud iam service-accounts keys create key.json \
  --iam-account dns01-solver@$PROJECT_ID.iam.gserviceaccount.com
$ kubectl -n cert-manager create secret generic clouddns-dns01-solver-svc-acct \
  --from-file=key.json
```

> Note: If you have already added the Secret but get an error: `...due to error processing: error getting clouddns service account: secret "XXX" not found`, the Secret may be in the wrong namespace. If you’re configuring a ClusterIssuer, move the Secret to the Cluster Resource Namespace which is cert-manager by default. If you’re configuring an Issuer, the Secret should be stored in the same namespace as the Issuer resource.

So we should create `clouddns-dns01-solver-svc-acct` in the `cert-manager` namespace

`key.json` file contains credentials data, so, after generated secret from this file, we should clear it from local

```bash
$ rm key.json
```

##### Add your domain to CloudDNS

Access [https://console.cloud.google.com/net-services/dns/zones](https://console.cloud.google.com/net-services/dns/zones) to add your domain to a public zone

With A record point to our server IP address

![https://user-images.githubusercontent.com/25602820/89972264-95ca9a00-dc87-11ea-8d10-fa42d64f68de.jpg](https://user-images.githubusercontent.com/25602820/89972264-95ca9a00-dc87-11ea-8d10-fa42d64f68de.jpg)

##### Create a basic Issuer

For test, staging environment we will use `https://acme-staging-v02.api.letsencrypt.org/directory` as ACME server, for production environment, please use
`https://acme-v02.api.letsencrypt.org/directory`

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letencrypt-staging
spec:
  acme:
    email: xuannam2620@gmail.com # this email will be used by let's encrypt to inform us about certificate expiring
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-issuer-account-key
    solvers:
    - dns01:
        clouddns:
          project: tf-test-xxxx
          serviceAccountSecretRef
            name: clouddns-dns01-solver-svc-acct
            key: key.json
```

About DNS01 Challenge for Google CloudDNS please refer: [https://cert-manager.io/docs/configuration/acme/dns01/google/](https://cert-manager.io/docs/configuration/acme/dns01/google/)

Create the ClusterIssuer

```bash
$ kubectl create -f letsencrypt-staging-issuer.yaml
```

Confirm the result by

```bash
$ kubectl get clusterissuer
NAME                         READY   AGE
letsencrypt-staging-issuer   True    38m
```

##### Create a Let's Encrypt Certificate

```yaml
apiVersion: cert-manager.io/v1alpha2
metadata:
  name: namtx-dev-letsencrypt-cert
spec:
  secretName: namtx-dev-letsencrypt-tls
  issuerRef:
    name: letsencrypt-staging-issuer
    kind: ClusterIssuer
  dnsNames:
  - namtx.dev
```

Apply it by

```bash
$ kubectl create -f letsencrypt-staging-certificate.yaml
```

Confirm by

```bash
kubectl get cert
NAME                         READY   SECRET                      AGE
namtx-dev-letsencrypt-cert   True    namtx-dev-letsencrypt-tls   37m
```

So, our Certificate is ready to use, let's add it to our Ingress

### Add certificate to our Ingress

After previous step, a certificate will be issued by Let's Encrypt and saved into secret `namtx-dev-letsencrypt-tls` (we specified the name by `secretName` value in certificate yaml file)

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: backend-ingress
spec:
  rules:
  - host: namtx.dev
    http:
      paths:
      - path: /
  backend:
    serviceName: backend-service
    servicePort: 80
  tls:
  - hosts:
    - namtx.dev
    secretName: namtx-dev-letsencrypt-tls
```

### References

- [https://cert-manager.io/docs/](https://cert-manager.io/docs/)
