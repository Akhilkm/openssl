## OpenSSL create your own CA 

### Generate a ca.key with 2048bit:

openssl genrsa -out ca.key 2048

### According to the ca.key generate a ca.crt (use -days to set the certificate effective time)

openssl req -x509 -new -nodes -key ca.key -subj "/CN=${MASTER_IP}" -days 10000 -out ca.crt

### Generate a server.key with 2048bit

openssl genrsa -out server.key 2048

### Create a config file for generating a Certificate Signing Request (CSR)

csr.conf

### Generate the certificate signing request based on the config file

openssl req -new -key server.key -out server.csr -config csr.conf

### Generate the server certificate using the ca.key, ca.crt and server.csr

openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key \
-CAcreateserial -out server.crt -days 10000 \
-extensions v3_ext -extfile csr.conf

### View the certificate

openssl x509  -noout -text -in ./server.crt
