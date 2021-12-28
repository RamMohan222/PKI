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

#### Convert a JKS (.jks) keystore to a PKCS#12 (.p12 of pfx)
```shell
keytool -importkeystore \
    -srckeystore keystore.jks \
    -destkeystore keystore.p12 \
    -deststoretype PKCS12 \
    -srcalias <jkskeyalias> \
    -deststorepass <password> \
    -destkeypass <password>
    
# With minimal parameters
keytool -importkeystore -srckeystore [MY_KEYSTORE.jks] -destkeystore [MY_FILE.p12] -srcstoretype JKS -deststoretype PKCS12 -deststorepass [PASSWORD_PKCS12]
```
#### To check the contents of the store
```shell
keytool -list -v -keystore [MY_FILE.p12] -storetype PKCS12
# -storetype is optional
```
#### To export certificate from the store
```shell
openssl pkcs12 -in keystore.p12  -nokeys -out certificate.crt
```
#### To export private key from the store
```shell
openssl pkcs12 -in keystore.p12  -nodes -nocerts -out key.pem
```

#### More help
```shell
openssl pkcs12 --help
```
