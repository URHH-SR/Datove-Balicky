# Vytvoření a odeslání balíčku provozovatele na rapi URHH

Přepoklad: zdrojové kody tohoto repozitáře jsou stažené na disk C:\URHH-github a na disku je ve složce C:\jdk-11.0.8 dostupná java v této verzi.

Jsou-li soubory v adresáři lib umístěné na jiném umístění na disku, je potřeba upravit soubor `/lib/keystore.properties` 

Spustíme cmd.exe
```
cd C:\URHH-github
```

Šifrujeme veřejnými certifikáty provozovatele a URHH. Každý držitel příslušeného privátního klíče je pak schopen obsah dešifrovat.
```
c:\jdk-11.0.8\bin\java.exe 
   -jar c:\URHH-github\crypto_cli\lib\crypto_cli-1.0-jar-with-dependencies.jar 
   -f encryptFile 
   -i c:\URHH-github\data\empty-packages\Bingo_2021-05-01T05_1.zip 
   -o c:\URHH-github\data\empty-packages\Bingo_2021-05-01T05_1.zip.p7e 
   -cert c:\URHH-github\data\urhh\certs\encrypt.pem 
   -cert c:\URHH-github\data\operator\certs\encrypt.pem
```
![image](https://user-images.githubusercontent.com/29448695/118829552-7f103e00-b8be-11eb-848b-5f0873673535.png)


Soubor se pečetí privátním klíčem provozovatele. URHH je schopno pečeť na základě veřejné části od provozovatele ověřit.

**POZOR: Nenahrazuje QSCD**

```
c:\jdk-11.0.8\bin\java.exe 
   -jar c:\URHH-github\crypto_cli\lib\crypto_cli-1.0-jar-with-dependencies.jar 
   -f signFile 
   -i c:\URHH-github\data\empty-packages\Bingo_2021-05-01T05_1.zip.p7e 
   -o c:\URHH-github\data\empty-packages\Bingo_2021-05-01T05_1.zip.p7e.p7s 
   -spks c:/URHH-github/data/operator/certs/sign.pfx 
   -pass heslo123
```
Pozor ! - cesta u spks parametru musí být s dopřednými lomítky i ve windows prostředí !

![image](https://user-images.githubusercontent.com/29448695/118830627-648a9480-b8bf-11eb-8cf6-8819f88b2e29.png)

Odeslání balíčku na rapi URHH - je nutné využít privátní část přístupového certifikátu
Obsah lze předat buď jako obsah request zprávy, tak odkazem.
URHH si odkaz stáhne - při přístupu využije svůj přístupový certifikát, jehož veřejnou část zveřejňuje na stránkách úřadu.
Podrobnější popis requestu zde: `/data/rapi-message/README.md`
```
curl -X POST "https://localhost:5799/rapi/reporting/v1/package/upload" \
     -H  "accept: application/json" \
     -H  "Content-Type: application/json" \
     --key c:\URHH-github\data\operator\certs\access.key \
     --cert c:\URHH-github\data\operator\certs\access.pem \ 
     -d "{\"requestId\":\"8f430525-c9c5-4caf-81ef-728b20e861c2\",\"operatorCode\":\"12345678\",\"packageVersion\":\"01\",\"packageKind\":\"Bingo\",\"packageTimeFrameStartInclusive\":\"2021-05-01T05:00:00+02:00\",\"packageTimeFrameEndExclusive\":\"2021-05-02T05:00:00+02:00\",\"packageContent\":\"\",\"packageUri\":\"https://example.com/8f430525-c9c5-4caf-81ef-728b20e861c2/Bingo_2021-05-01T05_1.zip.p7e.p7s\",\"packageStamp\":\"2021-05-18T13:05:19.1044853+02:00\"}"
```

------------------------------------------------------------------------------------------

# Validace balíčku a jeho dešifrování na straně URHH

Na straně aplikace URHH se validuje zapečetěný soubor pomocí veřejné části pečeticího certifikátu provozovatele
```
c:\jdk-11.0.8\bin\java.exe 
   -jar c:\URHH-github\crypto_cli\lib\crypto_cli-1.0-jar-with-dependencies.jar 
   -f validateFile 
   -i c:\URHH-github\data\empty-packages\Bingo_2021-05-01T05_1.zip.p7e.p7s
   -o c:\URHH-github\data\empty-packages\2-Bingo_2021-05-01T05_1.zip.p7e
   -cert c:\URHH-github\data\operator\certs\sign.pem 
   -cert c:\URHH-github\data\operator\certs\sign.pem
```
#### Poznámka - pro pečetění a validaci pečetě nelze využít self signed certifikáty, v případě použití komerčního pečeticího certifikátu je ale postup v pořádku.

Na straně aplikace URHH se dešifruje privátním klíčem z šifrovacího certfikátu URHH
```
c:\jdk-11.0.8\bin\java.exe 
   -jar c:\URHH-github\crypto_cli\lib\crypto_cli-1.0-jar-with-dependencies.jar 
   -f decryptFile 
   -i c:\URHH-github\data\empty-packages\2-Bingo_2021-05-01T05_1.zip.p7e 
   -o c:\URHH-github\data\empty-packages\2-Bingo_2021-05-01T05_1.zip 
   -k c:\URHH-github\data\urhh\certs\encrypt.key
   -cert c:\URHH-github\data\urhh\certs\encrypt.pem
```
