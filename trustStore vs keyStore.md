### Trust Store vs KeyStore
----

#### KeyStore Creation
```shell
C:/> keytool -genkeypair -alias myserver -keyalg RSA -dname "CN=Web Server,OU=Unit,O=Organization,L=City,S=State,C=IN" -keypass changeme -keystore server.jks -storepass letmein
```

