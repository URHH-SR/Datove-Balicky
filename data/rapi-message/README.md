Swagger dokumentace rapi rozhraní:

https://app.swaggerhub.com/apis/URHH/reporting-api/v1


Ukázka requestu pro metodu rapi/reporting/v1/package/upload.

```
{
   "requestId":"9efa7afd-3f9d-44ec-83bb-603f23b007bb",
   "operatorCode":"12345678",
   "packageVersion":"01",
   "packageKind":"HazardnaHraNaVP",
   "packageTimeFrameStartInclusive":"2020-05-01T05:00:00+02:00",
   "packageTimeFrameEndExclusive":"2020-06-01T05:00:00+02:00",
   "packageUri":"https://sample-operator.sk/packages/9efa7afd-3f9d-44ec-83bb-603f23b007bb.zip.p7e.p7s",
   "packageStamp":"2021-05-19T15:34:52+02:00",
   "packageContent":null
}
```

Pokud se vrátí chyba ErrInvalidRequestId, tak balíček s daným Guid byl již přijat od někoho jiného. V takovém případě se očekává, že provozovatel balíček nahraje s jiným Guid, pod kterým dojde k přijetí.

Stejný Guid se očekává při použití v metodách 'rapi/reporting/v1/package/getStatus' a 'rapi/reporting/v1/package/rollback' - tedy při dotazu na stav zpracování balíčku a při požadavku o jeho rollback.

Rollback je povolen pouze v zákonné lhůtě.

Při dotazu ne 'rapi/reporting/v1/package/getStatus' se vždy vrací kompletní seznam zjištěných nedostatků a informačních zpráv a to i pro stav ještě běžícího procesu validace balíčku. Konečný seznam je tak vrácen teprve, když 'ProcessingState' má jednu z hodnot 'Processed' a nebo 'Failed'.

Způsoby odeslání balíčku:
* a/ packageContent - obsah zapečetěného balíčku v base64 - z důvodu větší velikosti některých balíčků lze využít způsob b/
* b/ packageUri - zapečetěný balíček je možné stáhnout na uvedené url adrese, při přístupu aplikace na straně URHH využije přístupový certifikát URHH
* c/ oba předchozí zároveň - v takovém případě aplikace na straně URHH stahuje balíček ze zadané url a porovnává jej s příchozím packageContent

V případě úspěšného uložení dat na straně aplikace URHH se vrátí odpověď:
```
{
   "requestId":"9efa7afd-3f9d-44ec-83bb-603f23b007bb",
   "statusCode":"Received"
}
```

Nad rámec vyhlášky je na rozhraní rapi přidána extra metoda rapi/reporting/v1/package/list
```
{
  "operatorCode":"12345678"
}
```
Metoda vrací evideovaný seznam balíčků a jejich stavů, v budoucnu pravděpodobně bude návrat z metody omezena pro návrat stavů balíčků jen za poslední tři měsíce
```
{
  "packages": [
    {
      "packageGuid": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "packageVersion": "string",
      "packageKind": "CharitativnaLoteria",
      "packageTimeFrameStartInclusive": "2021-05-19T16:45:40.427Z",
      "packageTimeFrameEndExclusive": "2021-05-19T16:45:40.427Z",
      "processingState": "Planned",
      "packageState": "Processing"
    }
  ]
}
```
