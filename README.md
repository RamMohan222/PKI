## PKI Architecture 
---
1. Root CA Certificate
2. Intemediate CA Certificate signed by Root CA
3. Node1 Ceritificate signed by Intemediate CA
4. Node2 Certificate Signed by Intemediate CA

## PKI infrastructure with openssl
---
1. Generate Root CA key
2. Create Root CA certificate with key
3. Generate Intermediate CA Key
4. Create Intermediate CA CSR
5. Sign and create signed Intermediate CA certificate with Root CA key and Root CA certificate using intermediate CSR.
6. Generate Node CA key 
7. Create Node CSR
8. Sign and create signed Node certificate with Intemediate CA key and Intermediate CA certificate using Node CSR.
9. Create PKCS#12 file with Node key, Node Certificate, Intermediate CA certificate and Root CA certificate.

## PKI infrastructure with keytool
---
1. Create Root CA keypair and root keystore
2. Extracting the CA certificate.
3. Create Intermediate CA keypair and intermediate keystore
4. Create CSR for Intermediate CA certificate
5. Sign the CSR and Create signed Intermediate CA certificate with Root CA keypair
6. Create node keypair and node keystore
7. Create CSR for node certificate
8. Sign the node CSR and Create signed node certificate with Intermediate CA keypair
9. Install signed node certificate, root CA certificate and Intermediate signed CA certificate into node keystore
10. Create trust store and install root CA certificate, Intermediate signed CA certificate and signed node certificate for client.
