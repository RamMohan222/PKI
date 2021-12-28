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
