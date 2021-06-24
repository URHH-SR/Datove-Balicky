# Referenční klient komunikačního rozhraní dle vyhlášky č. 445/2020 - Nenahrazuje QSCD

Vyhláška 445/2020

https://www.zakonypreludi.sk/zz/2020-445

Swagger dokumentace rapi rozhraní:

https://app.swaggerhub.com/apis/URHH/reporting-api/v1

Swagger dokumentace oapi rozhraní:

https://app.swaggerhub.com/apis/URHH/operator-api/v1

Obsah repozitáře:
* `/crypto_cli` - Java knihovna a nástroje pro spuštění jednotlivých funkcí z příkazové řádky
    * vytváření uznávané elektronické pečeti balíčku
    * ověřování uznávané a kvalifikované elektronické pečeti balíčku
    * šifrování balíčku
    * dešifrování balíčku
* `/data`
    * `empty_packages` - ukázkové prázdné balíčky
    * `oapi-message` - ukázkový json request pro oapi metodu oapi/reporting/v1/notifier
    * `operator` - testovací certifikáty provozovatele
    * `rapi-message` - ukázkový json request pro rapi metodu rapi/reporting/v1/package/upload
    * `urhh` - testovací certifikáty URHH
* `/doc`
    * `cbData_ReportingSK_IntegracniDokument_TS_v3.docx` - dokumentace s dalším popisem pro integraci nového klienta
    * `ValidationList.xlsx` - pracovní verze přehledu možných chyb ze syntax validace
* `/openssl1.1.1` - kompilace openssl pro windows
* `/swagger` - json soubory s popisem interface oapi a rapi
* `/xml` - další popis k obsahu xml souborů
* `/xsd` - xsd definice s nástinem jak by měly vypadat xml soubory. Aplikace URHH nepracuje s xsd a xsd jsou tak pouze informativní.

Seznam dalších README.md souborů s popisy a postupy:

* `/data/empty-packages/README.md` - popis prázdných balíčků
* `/data/oapi-message/README.md` - ukázka zpráv z oapi rozhraní
* `/data/operator/README.md` - popis generování přístupového a dalších testovacích certifikátů provozovatele
* `/data/rapi-message/README.md` - ukázka zpráv z rapi rozhraní
* `/data/urhh/README.md` - popis generování testovacích certifikátů URHH
* `/xml/README.md` - další popis k xml souborům

Zdrojové kódy jsou umístěné zde:
https://github.com/Ministerstvo-financi/hazard-komunikacni-rozhrani

# Obecný popis
Provozovatel tvoří balíček, který zašifruje a zapečetí a odešle na rapi rozhraní URHH.

Aplikace URHH po validaci balíček buď přijme nebo odmítne.

O výsledku notifikuje oapi rozhraní provozovatele.

Stejný výsledek (stav zpracování balíčku) provozovatel zjistí i voláním příslušné metody na rapi rozhraní.

Další volání jsou určená pro vyvolání opravy v zákonné lhůtě a nebo zjištění stavu zpracování dalších balíčků v aplikaci URHH (metoda byla přidaná nad rámec vyhlášky).


# Licence
Referenční implementace tam, kde je vytvářen nový kód, je licencována pod Apache License v2.0.
Referenční implementace obsahuje nebo využívá další komponenty, které jsou licencovány odpovídajícími 
licencemi:
* OpenSSL - [vlastní licence](https://www.openssl.org/source/license.html)
* Apache HTTPD - [Apache License v2.0](https://www.apache.org/licenses/LICENSE-2.0)
* DSS - [LGPL](https://github.com/esig/dss/blob/master/LICENSE) 
* .NET Core - [MIT](https://github.com/dotnet/core/blob/master/LICENSE.TXT)
* OpenJDK - [GPL](https://openjdk.java.net/legal/OCTLA-JDK9+.pdf)
