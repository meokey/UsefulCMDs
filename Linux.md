## This note is for all Linux-like systems
##
Find Out Which Process Listening on a Particular Port
```
$ sudo ss -tulpn | grep 80
$ sudo lsof -i :80
$ sudo fuser 80/tcp
$ sudo netstat -ltnp | grep -w ':80' 
```
Find out ports that listens other than local connections
```
$ sudo ss -tulpn | grep -v "127.0.0."
```

# check remote port reachable
check single port
```
$ nc -zv 192.168.1.15 22
```
port range
```
$ nc -zv 192.168.1.15 20-80
```
# Certificates and openssl
## Create self-signed certificate
Create a private key file
```
$ openssl genrsa -out my_private_key.key 4096
```
(optional) Extract publicy key file from the private key file if needed
```
$ openssl rsa -in my_private_key.key -pubout -out my_public_key.key
```
Generate certificate from the private key
```
$ openssl req -x509 -new -key my_private_key.key -days 365 -out mycert.pem
$ openssl req -x509 -outform der -new -newkey rsa:4096 -keyout my_private_key.pem -days 3650 -out mycert.crt
```
## Create signed certificate
Create Certificate Signing Request (CSR) file and associated private key file
```
$ openssl req -newkey rsa:4096 -keyout my_private_key.pem -out my_csr.csr
```
or with pre-defined configuration file
```
$ openssl req -new -config req.conf -keyout my_private_key.pem -out my_csr.csr
```
where req.conf:
```
[req]
prompt=no
default_md=sha256
default_bits=4096
distinguished_name=dn
req_extensions=req_ext
[dn]
C=CA
ST=State
L=Locality
O=Organisation name
OU=Organization unit
CN=example.com
[req_ext]
subjectAltName=@alt_names
[alt_names]
DNS.1=example.com
DNS.2=www.example.com
DNS.3=ftp.example.com
```
Create CSR from existing private key
```
$ openssl req -new -key my_private_key.pem -out my_csr.csr
```
Generate CSR from existing private key and certificate
```
$ openssl x509 -x509toreq -in cert.pem -out example.csr -signkey example.key
```
Send the CSR file to CA (e.g., DigiCert) to sign and you will get your certificate file back
## Validate certificates
Check CSR
```
$ openssl req -text -noout -verify -in server.csr
```
Check/View the Status of a Certificate file
```
$ openssl x509 -text -in mycert.pem -noout
```
Find the expiration date of a .pem type TLS/SSL certificate
```
$ openssl x509 -enddate -noout -in mycert.pem
```
Check the certificate of a website
```
$ openssl s_client -connect google.com:443 | openssl x509 -noout -text
```
Check the certificate of a website with Server Name Indication (SNI)
```
$ openssl s_client -connect google.com:443 -servername ibm.com | openssl x509 -noout -text
```
Check the key file
```
$ openssl rsa -in my_private_key.pem -check
```
## Convert certificate formats
### PEM to PKCS#12
```
$ openssl pkcs12 -export -name "yourdomain-(expiration date)" \
-out yourdomain.pfx -inkey yourdomain.key -in yourdomain.crt
```
### PKCS#12 to PEM
Use the following command to extract the private key from a PKCS#12 (.pfx) file and convert it into a PEM encoded private key:
```
$ openssl pkcs12 -in yourdomain.pfx -nocerts -out yourdomain.key -nodes
```
Use the following command to extract the certificate from a PKCS#12 (.pfx) file and convert it into a PEM encoded certificate:
```
$ openssl pkcs12 -in yourdomain.pfx -nokeys -clcerts -out yourdomain.crt
```
### PEM to DER
Use the following command to convert a PEM encoded certificate into a DER encoded certificate:
```
$ openssl x509 -inform PEM -in yourdomain.crt -outform DER -out yourdomain.der
```
Use the following command to convert a PEM encoded private key into a DER encoded private key:
```
$ openssl rsa -inform PEM -in yourdomain.key -outform DER -out yourdomain_key.der
```
### DER to PEM
Use the following command to convert a DER encoded certificate into a PEM encoded certificate:
```
$ openssl x509 -inform DER -in yourdomain.der -outform PEM -out yourdomain.crt
```
Use the following command to convert a DER encoded private key into a PEM encoded private key:
```
$ openssl rsa -inform DER -in yourdomain_key.der -outform PEM -out yourdomain.key
```
