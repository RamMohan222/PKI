#### i Certificate Extensions
1. PEM
2. DER

#### ii Certificate store and Archive Extensions
1. PKCS#7
2. PKCS#8
3. PKCS#10
4. PKCS#11
5. PKCS#12
---

## Certificate Extensions
* **PEM** - The PEM format is the most common format used for certificates. The PEM certificates saved with .cer,.crt or .pem extensions. These files are Base64 encoded ASCII files.
* **DER** - The DER format is the binary form of the certificate. DER formatted certificates do not contain the "BEGIN CERTIFICATE/END CERTIFICATE" statements. The most common extension for these certificates is .der.

## Certificate store and Archive Extensions
1. **PKCS#7**
The PKCS#7 or P7B format is stored in Base64 ASCII format and has a file extension of .p7b or .p7c.
A P7B file only contains certificates and chain certificates (Intermediate CAs), not the private key. The most common platforms that support P7B files are Microsoft Windows and Java Tomcat.
```shell
# Certificates are should be in .pem (basic64) format.
# To convert CER to P7B
openssl crl2pkcs7 -nocrl -certfile node.cer -out certificatename.p7b -certfile IntermediateCert.cer -certfile CACert.cer
# To convert P7B to CER
openssl pkcs7 -print_certs -in certificatename.p7b -out certificatename.cer
```

2. **PKCS#8**
In cryptography, PKCS #8 is a standard syntax for storing private key information. The PKCS #8 private key may be encrypted with a passphrase.
```shell
# Certificate should be in .pem (basic64) format. 
# pkcs8 with out password encryption.
openSSL pkcs8 -topk8 -nocrypt -in certificatename.pem -out certificatename.pk8

# pkcs8 with password encryption.
openSSL pkcs8 -topk8 -in certificatename.pem -out certificatename.pk8
```
If the private key is encrypted with the PKCS#5 v2.0 algorithm. To decrypt with pkcs8 utility we need to specifying PKCS#5 v1.5 or PKCS#12 algorithms with -v1 flag.
```shell
openssl pkcs8 -topk8 -in <PKCS#5v2.0_key_file> -out <new_key_file> -v1 PBE-SHA1-3DES
```

3. **PKCS#10**
In public key infrastructure (PKI) systems, the PKCS#10 is standerd for encoded certificate signing request (.csr) certificates. These .csr files are by default basic64 encoded files to use with X.509. The following command is used to see .csr encoded file contents.
```shell
openssl asn1parse -i -in <certificate>.csr
```

4. **PKCS#11**
 The PKCS#11 is one of the Public-Key Cryptography Standards to generate and update the security tokens.
 
5. **PKCS#12**
It creates keystore or truststore and .pfx or p12 or pkcs12 are the extension.
```shell
openssl pkcs12 -export -in <signed_cert_filename> -inkey <private_key_filename> -name 'tomcat' -out keystore.p12
```
If you have a chain of certificates, combine the certificates into a single file and use it for the input file, as shown below. The order of certificates must be from server certificate to the CA root certificate.
```shell
# In linux
cat <signed_cert_filename> <intermediate.cert> [<intermediate2.cert>] <root ca.cert> > cert-chain.txt
# In Windows 
type <signed_cert_filename> <intermediate.cert> [<intermediate2.cert>] <root ca.cert> > cert-chain.txt

openssl pkcs12 -export -in cert-chain.txt -inkey <private_key_filename> -name 'tomcat' -out keystore.p12

# To convert PFX to PEM
openssl pkcs12 -in certificatename.pfx -nocerts -nodes -out certificatename.pem
```
