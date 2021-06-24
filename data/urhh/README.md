# Postup tvorby testovacích certifikátů - URHH

Přepoklad: zdrojové kody tohoto repozitáře jsou stažené na disk C:\URHH-github.

Spustíme cmd.exe
```
cd C:\URHH-github
set PATH="C:\URHH-github\openssl1.1.1\
set OPENSSL_CONF=C:\URHH-github\openssl1.1.1\openssl.cnf
```
## Generování certifikátu CA
Generujeme certifikát CA

* 1 - Vytvoříme privátní klíč pro certifikační autoritu
```
openssl genrsa -out c:\URHH-github\data\urhh\certstest_ca.key 4096
```
* 2 - Vygenerujeme certifikát certifikační autority, jako Common Name zadáme **URHH-TestCA**
```
openssl req -new -x509 -sha512 -key C:\URHH-github\data\urhh\certstest_ca.key -days 7310 -out C:\URHH-github\data\urhh\certstest_ca.pem
```

## Generování přístupového certifikátu
Generujeme přístupový certifikát

Veřejnou část (pem soubor certifikátu a pem soubor CA) URHH zveřejňuje na webových stránkách úřadu.

* 1 - Vytvoříme privátní klíč pro přístupový certifikát
```
openssl genrsa -out c:\URHH-github\data\urhh\certsaccess.key 4096
```
* 2 - Vytvoříme žádost pro přístupový certifikát, jako Common Name bylo zadáno **URHH-TestAccess**
```
openssl req -new  -sha512 -key C:\URHH-github\data\urhh\certsaccess.key -days 7310 -out C:\URHH-github\data\urhh\certsaccess.csr
```
* 3 - Vygenerujeme přístupový certifikát podepsaný autoritou
```
openssl x509 -req -in c:\URHH-github\data\urhh\certsaccess.csr -CA c:\URHH-github\data\urhh\certstest_ca.pem -CAkey c:\URHH-github\data\urhh\certstest_ca.key -CAcreateserial -out c:\URHH-github\data\urhh\certsaccess.pem -days 7310 -sha512 -extensions usr_cert
```
* 4 - Převedeme certifikát do pfx (p12)
```
openssl pkcs12 -export -out c:\URHH-github\data\urhh\certsaccess.pfx -inkey C:\URHH-github\data\urhh\certsaccess.key -in C:\URHH-github\data\urhh\certsaccess.pem -passout pass:heslo123
```

------------------------------------------------------------------------------------------

## Simulace šifrovacího certifikátu

Simulujeme šifrovací certifikát, v běžném provozu bude tento certifikát nahrazen komerčním šifrovacím certifikátem.

Veřejnou část (pem soubor certifikátu) URHH zveřejňuje na webových stránkách úřadu.

* 1 - Vytvoříme privátní klíč pro šifrovací certifikát
```
openssl genrsa -out c:\URHH-github\data\urhh\certsencrypt.key 4096
```
* 2 - Vytvoříme žádost pro šifrovací certifikát, jako Common Name bylo zadáno **URHH-TestEncrypt**
```
openssl req -new  -sha512 -key C:\URHH-github\data\urhh\certsencrypt.key -days 7310 -out C:\URHH-github\data\urhh\certsencrypt.csr
```
* 3 - Vygenerujeme  šifrovací certifikát podepsaný autoritou
```
openssl x509 -req -in c:\URHH-github\data\urhh\certsencrypt.csr -CA c:\URHH-github\data\urhh\certstest_ca.pem -CAkey c:\URHH-github\data\urhh\certstest_ca.key -CAcreateserial -out c:\URHH-github\data\urhh\certsencrypt.pem -days 7310 -sha512 -extensions usr_cert
```

------------------------------------------------------------------------------------------

## Simulace pečeticího certifikátu

Pečetění je založeno na trust listu EU - tento certifikát nelze vytvořit jako self signed. Je nutné získat komerční kvalifikovaný certifikát.
