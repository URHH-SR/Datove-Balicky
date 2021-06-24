Swagger dokumentace oapi rozhraní:

https://app.swaggerhub.com/apis/URHH/operator-api/v1

Na oapi rozhraní aplikace URHH zasílá notifikaci o výsledku zpracování balíčku.

Alternativně může provozovatel využít metodu na rapi rozhraní pro zjištění tohoto stavu.

RequestId se váže ke guid balíčku (které provozovatel zaslal na rapi v metodě `rapi/reporting/v1/package/upload`)

Request o úspěšném zpracování vypadá takto:
```
{
   "requestId":"388faf53-148c-4412-b341-2c0b497a5b5c",
   "processingState":"Processed",
   "packageState":"ProcessedSuccessfully",
   "content":"Base64EncodedContent"
}
```

Vytvořený json se na straně aplikace URHH uloží na disk, zašifruje se veřejným certifikátem provozovatele, zapečetí se kvalifikovanými pečeticími prostředky. Obsah se pak vloží jako base64 encoded string do property content. 

Ukázka requestu o neúspěšném zpracování:
```
{
   "requestId":"53360af5-eff3-47b7-a079-1cfaf8c85344",
   "processingState":"Failed",
   "packageState":"InvalidContent",
   "details":[
      {
         "file":"prevadzkovatel.xml",
         "line":1,
         "classificationCode":"Error",
         "errorCode":"EmptyField",
         "detail":"'IDPrevadzkovatel' must not be empty.",
         "result":"Interupted"
      }
   ],
   "content":"Base64EncodedContent"
}
```
