---
layout: post
title: "use openssl to generate p12 file"
description: ""
category: iOS
tags: [ios, object-c]
---

## Use openssl to generate p12 file

### generate key & csr

```
openssl req -new -newkey rsa:2048 -nodes -keyout mykey.key -out csr.certSigningRequest
```

### use cer(download from apple website) generate pem

```
openssl x509 -in {cer.cer} -inform DER -out #{pem.pem}  -outform PEM
```

### use key & pem to generate p12 file

```
openssl pkcs12 -export -inkey mykey.key -in #{pem.pem} -out #{dev_p12}
```

* get the p12 file at the last
