# Creating SSL certificate for development

[Source](https://zeropointdevelopment.com/how-to-get-https-working-in-windows-10-localhost-dev-environment/)

## Create private key

- Run below command and enter the phrase
- For windows proceed the commands with winpty

```cmd
winpty openssl genrsa -des3 -out rootSSL.key 2048
```

## Create certificate file

```cmd
winpty openssl req -x509 -new -nodes -key rootSSL.key -sha256 -days 1024 -out rootSSL.pem
```

## Add to Windows

- Add the certificate (.pem) file to the _Third party root certificate authorities_

## Create private key for new domain

```cmd
winpty openssl req -new -sha256 -nodes -out sdxvm.csr -newkey rsa:2048 -keyout sdxvm.key -subj "//C=US\ST=TX\L=Houston\O=sdxvm\OU=Dev\CN=sdxvm\emailAddress=schalla@sdxvm"
```

## Create new certificate using root SSL certificate

```cmd
winpty openssl x509 -req -in sdxvm.csr -CA rootSSL.pem -CAkey rootSSL.key -CAcreateserial -out sdxvm.crt -days 500 -sha256 -extensions "authorityKeyIdentifier=keyid,issuer\n basicConstraints=CA:FALSE\n keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment\n  subjectAltName=DNS:sdxvm"
```

## Convert crt file to pfx

```cmd
winpty openssl pkcs12 -export -out sdxvm.pfx -inkey sdxvm.key -in sdxvm.crt
```

## Localhost SSL

```
New-SelfSignedCertificate -Subject "localhost" -TextExtension @("2.5.29.17={text}DNS=localhost&IPAddress=127.0.0.1&IPAddress=::1")
```

## Install the certificate on IIS
