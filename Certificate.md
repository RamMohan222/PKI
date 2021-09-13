req/x509/p7b
-----

#### Generating a Self-Signed Certificate with OpenSSL
The bellow command will generate the private key and certificate
```shell
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt
# Certificate with subject details in the command it self.
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout private.pem -out certificate.crt -subj "/C=IN/ST=AP/L=EN/CN=localhost/emailAddress=localhost@domain.com"

# To convert certificate into PEM/DER [by default certificate into pem]
openssl x509 -in certificatename.cer -outform PEM -out certificatename.pem
openssl x509 -in certificatename.pem -outform DER -out certificatename.der
```

#### Generate a Self-Signed Certificate from an Existing Private Key
1. Generate Private key.
```shell
openssl genrsa -out domain.key 2048

# With password
openssl genrsa -aes128 -out domain.key 2048
openssl genrsa -aes128 -passout pass:password -out private.key 4096
openssl genrsa -aes128 -passout file:password.txt -out private.key 4096
openssl genrsa -aes128 -passout stdin -out private.key 4096
```
##### -aes128|-aes192|-aes256|-aria128|-aria192|-aria256|-camellia128|-camellia192|-camellia256|-des|-des3|-idea
These options encrypt the private key with specified cipher before outputting it. 
If none of these options is specified no encryption is used. If encryption is used a pass phrase is prompted for if it is not supplied via the -passout argument.

2. To convert key into PEM/DER [by default key into pem]
```shell
openssl rsa -outform der -in private.key -out private.der
openssl rsa -outform pem -in private.key -out private.pem
```

3. To extract public key from private key.
```shell
openssl rsa -in privkey.pem -passin pass:password -pubout -out privkey.pub
openssl rsa -in privkey.pem -passin file:password.txt -pubout -out privkey.pub
openssl rsa -in privkey.pem -passin stdin -pubout -out privkey.pub
```

4. Generate Certificate with Private key.
```shell
openssl req -key domain.key -new -x509 -days 365 -out domain.crt
```

5. To convert private key into PKCS8 
```shell
openssl pkcs8 -topk8 -inform PEM -outform PEM -in private.pem -out key-pkcs8.pem
openssl pkcs8 -topk8 -inform PEM -outform DER -in private.der -out key-pkcs8.der
# To convert with out password protection
openssl pkcs8 -topk8 -inform PEM -outform DER -in private.pem -out private.der -nocrypt
```

