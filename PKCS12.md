PKCS12
-----

#### Convert cert.pem and private key key.pem into a single cert.p12 file, key in the key-store-password manually for the .p12 file.
```shell
openssl pkcs12 -export -out cert.p12 -in cert.cer -inkey key.pem
```

#### With root CA
```shell
openssl pkcs12 -export -out cert.p12 -in server.crt -inkey server.key -chain -CAfile ca.crt
```

#### No password for cert.p12
```shell
openssl pkcs12 -export -out cert.p12 -in cert.cer -inkey key.pem -passout pass: -nokeys
```

#### Password Welcome@123 for cert.p12
```shell
openssl pkcs12 -export -out cert.p12 -in cert.cer -inkey key.pem -passout pass: Welcome@123
```

#### More help
```shell
openssl pkcs12 --help
```
