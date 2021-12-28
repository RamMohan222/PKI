### Create Trust Store and KeyStore
----

#### Creating a keystore
```shell
keytool -genkey -alias myserver -keyalg RSA -keystore keystore.jks
# It asks information with interactive terminal
```
#### Creating a keystore with non interactive terminal
```shell
keytool -genkeypair -alias mystore -keyalg RSA -dname "CN=Web Server,OU=Unit,O=Organization,L=City,S=State,C=IN" -keypass changeme -keystore keystore.jks -storepass letmein
```

#### Export a public certificate from the store
```shell
keytool -export -alias mystore -file certificate.cer -keystore keystore.jks
```

#### Importing a certificate into the truststore
```shell
keytool -import -v -trustcacerts -alias mystore -file certificate.cer -keystore truststore.ts
# If There is not truststore.ts then it will create a new truststore.ts
```

#### Importing a certificate into the truststore
```shell
keytool -import -trustcacerts -alias mystore -file certificate.cer -keystore truststore.ts -storepass password -noprompt
```
