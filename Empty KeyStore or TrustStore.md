## Create Empty KeyStore or TrustStore
---

### 1. Create a store with random keypair
```shell
keytool -genkeypair -alias emptyStore -storepass password -keypass password -keystore emptyStore.jks -dname "CN=Developer, OU=Department, O=Company, L=City, ST=State, C=CA"
```

### 2. Delete private key from the store
```shell
keytool -delete -alias emptyStore -storepass password -keystore emptyStore.jks
```

### 3. Check the Store contents
```shell
keytool -list -keystore emptyStore.jks -storepass password
```
