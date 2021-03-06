## Create Trust Store and KeyStore
----
1. KeyStore : It contains private keys and public certificates which are used to authenticate themselves to the connecting party or client.
2. TrustStore: It contains trusted SSL or public certificates.

---
### 1. Create a keypair and keystore

```shell
keytool -genkeypair -alias <name> -keyalg <algorithm> -keysize <size> -dname <subject> -validity <days> -keystore <path to keystore> -storepass <password> -keypass <same password>
```
```txt
-alias <name>
Any value. For example, the host name of the server, or a descriptive name like cassandra node 1.
  
-keyalg <algorithm>
The key algorithm is normally RSA.
  
-keysize <size>
Use a key size of 2048.
  
-dname <subject>
The subject is an X.500 Distinguished Name (DN) with a CN (common name), and optionally O (organization), OU (organizational unit), C (country), and other tokens. An example DN is CN=cassandra node 1, OU=Datacenter 1, OU=QA, O=IBM.
  
-validity <days>
The validity specifies the number of days until the personal certificate expires. For self-signed personal certificates used for internal client/server communications, there is no reason to specify short validity periods, so a ten-year expiration (3650) is acceptable.
  
-keystore <path to keystore>
The path to the keystore file can be anywhere, but it would normally be on the Cassandra conf directory (for example:/etc/cassandra/conf/keystore.jks).
  
-storepass <password>
The keystore password can be any value. It is used to generate a key to encrypt the keystore file.
  
-keypass <same password>
The key password must be the same as the storepass password.
```
### 2. Export a certificate from keystore
```shell
keytool -exportcert -alias <name> -keystore <path to keystore> -file <path to cert file> -storepass <password>
```
```text
-exportcert
The keytool command for exporting a certificate.
-keystore <path to keystore>
The path to the keystore file can be anywhere, but it would normally be on the Cassandra conf directory (for example:/etc/cassandra/conf/keystore.jks).
-alias <name>
Any value. For example, the host name of the server, or a descriptive name like cassandra node 1.
-file <path to the file to which the certificate is exported>
The path to the file that contains the exported certificate (for example: /etc/cassandra/conf/cassandra.cer). It is recommended that the name of the certificate file is different for each Cassandra node to identify them.
-storepass <password>
The keystore password that is used in step 1.
```
### 3. Create a truststore
```shell
keytool -importcert -alias <name> -file <path to cert file>.cer -keystore <path to truststore> -storepass <password>
```
### Example

#### Creating a keystore
```shell
keytool -genkey -alias node1 -keyalg RSA -keystore keystore.jks
# It asks information with interactive terminal
```
#### Creating a keystore with non interactive terminal
```shell
keytool -genkeypair -alias node1 -keyalg RSA -dname "CN=Web Server,OU=Unit,O=Organization,L=City,S=State,C=IN" -keypass changeme -keystore keystore.jks -storepass letmein
# or
keytool -genkeypair -alias node1 -keyalg RSA -dname "CN=node 1, OU=Datacenter 1, OU=QA, O=IBM" -keypass password -keystore keystore.jks -storepass password
```

#### Export a public certificate from the store
```shell
keytool -export -alias node1 -file certificate.cer -keystore keystore.jks
```

#### Importing a certificate into the truststore
```shell
keytool -import -v -trustcacerts -alias node1 -file certificate.cer -keystore truststore.ts
# If There is not truststore.ts then it will create a new truststore.ts
```

#### Importing a certificate into the truststore with non interactive terminal
```shell
keytool -import -trustcacerts -alias node1 -file certificate.cer -keystore truststore.ts -storepass password -noprompt
```

## Enabling node-to-node encryption
1. Create a keystore for the client
```shell
keytool -genkey -alias Client -keyalg RSA -keystore clientKeyStore.p12 -keysize 2048 -validity 3650
```
2. Export the public cert of the client
```shell
keytool -export -keystore clientKeyStore.12 -alias Client -file client.crt
```
3. Create a keystore for the server
```shell
keytool -genkey -alias Server -keyalg RSA -keystore serverKeyStore.p12 -keysize 2048 -validity 3650
```
4. Export the public cert of the server
```shell
keytool -export -keystore serverKeyStore.p12 -alias Server -file server.crt
```
5. Create a truststore for the client
```shell
keytool -genkey -alias ClientTrust -keyalg RSA -keystore clientTrustStore.p12 -keysize 2048
```
6. Create a truststore for the server
```shell
keytool -genkey -alias ServerTrust -keyalg RSA -keystore serverTrustStore.p12 -keysize 2048
```
7. Import the client public certificate into the server truststore
```shell
keytool -import -keystore serverTrustStore.p12 -alias Client -file <path-to-client.crt>
```
8. Import the server public cert into the client truststore
```shell
keytool -import -keystore clientTrustStore.p12 -alias Server -file <path-to-server.crt>
```
9. Delete the existing private key of the server truststore
```shell
keytool -delete -alias serverTrust -keystore serverTrustStore.p12 -storepass <password>
```
10. Delete the existing private key of the client truststore
```shell
keytool -delete -alias clientTrust -keystore clientTrustStore.p12 -storepass <password>
```
## Certificate Signing Request (CSR)
```shell
# Create new CSR
keytool -certreq -alias node -file node.csr -keypass  password -keystore keystore.jks -storepass password

# To convert CSR to PEM
openssl ca -in node.csr -out certificate.pem

# Signing with Root CA
openssl x509 -req -days 3650 -in node.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out signedcert.crt

# import the returned signed certificate:
keytool -import -alias node -file signedcert.crt -keypass password -keystore keystore.jks -storepass password

# import CA certificate
keytool -import -alias ca -file CA.crt -keypass password -keystore keystore.jks -storepass password

# If there is an intermediate certificate and your csr is signed by intermediate certificate then import intermediate certificate
keytool -import -alias interca -file intercert.crt -keypass password -keystore keystore.jks -storepass password

# To confirm signed certificate and exported certificates are same, later with this we can create trust store.
keytool -export -alias node -file node.crt -keystore keystore.jks

#The exported certificate and CA root and/or intermediary certificates must now be imported to the truststore.
keytool -import -trustcacerts -alias node -file node.crt -keystore truststore.ts -storepass password -noprompt
keytool -import -trustcacerts -alias ca -file CA.crt -keystore truststore.ts -storepass password -noprompt
keytool -import -trustcacerts -alias interca -file intercert.pem -keystore truststore.ts -storepass password -noprompt
```
### Self Sign
If you have setup your own certificate authority, you can self-sign the request using openssl
```shell
# Convert the certificate to a plain PEM certificate
openssl x509 -in node.crt -out certificate.pem -outform PEM

# For a self-signed certificate, you must combine the signed certificate with the CA certificate
# For linux
cat certificate.pem [<interca.crt>] CA.crt > certfull.pem
# For windows
type certificate.pem [<interca.crt>] CA.crt > certfull.pem

# This certificate can be imported into your keystore and truststore
# To import your signed certificate into your keystore
keytool -import -alias node -file certfull.pem -keypass password -keystore keystore.jks -storepass password

# Then export the certificate for use in your truststore
keytool -export -alias node -file finalNode.crt -keystore keystore.jks

# The same exported certificate must also be to the truststore
keytool -import -trustcacerts -alias node -file finalNode.crt -keystore truststore.ts -storepass password -noprompt
