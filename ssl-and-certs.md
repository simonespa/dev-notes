# SSL & certificates

- [Create Self-signed Certificates](#create-self-signed-certificates)
- [Update CA for OpenSSL](#update-ca-for-openssl)
- [Generate PEM, CRT and KEY from P12](#generate-pem--crt-and-key-from-p12)
- [CRL](#crl)
- [Java keystore (keytool)](#java-keystore--keytool-)
  - [Create a trust store and import a certificate](#create-a-trust-store-and-import-a-certificate)
  - [Runtime flags](#runtime-flags)
- [Check a PKCS#12 keystore file (.pfx or .p12)](#check-a-pkcs-12-keystore-file--pfx-or-p12-)
- [Check a private key](#check-a-private-key)
- [Check a PEM certificate](#check-a-pem-certificate)
- [Check a keystore file](#check-a-keystore-file)
- [Check an SSL connection](#check-an-ssl-connection)
- [Import/Export keystores](#import-export-keystores)
- [References](#references)
  - [SSL Hardening](#ssl-hardening)
  - [SSL Quality Test](#ssl-quality-test)

## Create Self-signed Certificates

```
openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
openssl rsa -passin pass:x -in server.pass.key -out server.key
rm server.pass.key
openssl req -new -key server.key -out server.csr
openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
```

## Update CA for OpenSSL

```
curl http://curl.haxx.se/ca/cacert.pem >/usr/local/etc/openssl/cert.pem
curl 'https://ca.dev.bbc.co.uk/cloud-ca.pem' >>/usr/local/etc/openssl/cert.pem
```

## Generate PEM, CRT and KEY from P12

```
openssl pkcs12 -in certificate.p12 -out certificate.crt -clcerts -nokeys
openssl pkcs12 -in certificate.p12 -out certificate.key -nocerts -nodes
openssl pkcs12 -in certificate.p12 -out certificate.pem -nodes
```

Ref: https://wiki.openssl.org/index.php/Command_Line_Utilities

## CRL

```
openssl crl -in /etc/crl/d897bec1.r0 -noout -text
```

## Java keystore (keytool)

Reference: https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html

### Create a trust store and import a certificate

```
keytool -genkey -dname "cn=spa.simone@gmail.com" -alias truststorekey -keyalg RSA -keystore ./client-truststore.jks -keypass changeit -storepass changeit
keytool -import -keystore ./client-truststore.jks -file certificate.pem -alias devcert
```

References:

- http://stackoverflow.com/questions/1666052/java-https-client-certificate-authentication
- http://magicmonster.com/kb/prg/java/ssl/pkix_path_building_failed.html

### Runtime flags

```
-Djsse.enableSNIExtension=false -Djavax.net.ssl.trustStore=<location of trust store> -Djavax.net.ssl.trustStorePassword=<trust store password> -Djavax.net.ssl.keyStore=<keystore> -Djavax.net.ssl.keyStorePassword=<keystore password> -Djavax.net.ssl.keyStoreType=PKCS12
```

and

```
PROXY_OPTS=-Dhttps.proxyHost=lalla.cache.com -Dhttp.proxyHost=lalla.cache.com -Dhttps.proxyPort=80 -Dhttp.proxyPort=80
```

## Check a PKCS#12 keystore file (.pfx or .p12)

```
openssl pkcs12 -info -in keyStore.p12
```

## Check a private key

```
openssl rsa -check -in dev.bbc.co.uk.key
```

## Check a PEM certificate

```
openssl x509 -text -noout -in dev.bbc.co.uk.pem
```

## Check a keystore file

```
keytool -list -v -keystore privateKey.store
```

## Check an SSL connection

All the certificates (including Intermediates) should be displayed

```
openssl s_client -connect recommendations-2.test.api.bbci.co.uk:443
```

## Import/Export keystores

```
keytool -importkeystore -srckeystore keystore.p12 -destkeystore keystore.jks -srcstoretype pkcs12
```

## References

- https://www.sslshopper.com/article-most-common-openssl-commands.html
- http://stackoverflow.com/questions/9497719/how-to-extract-a-public-private-key-from-a-pkcs12-file-with-openssl-for-later-us
- http://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file

### SSL Hardening

- http://www.acunetix.com/blog/articles/tls-ssl-cipher-hardening/

### SSL Quality Test

- https://www.ssllabs.com/ssltest/index.html
