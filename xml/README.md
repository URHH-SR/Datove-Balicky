**Speciální znaky**

Při tvorbě xml je potřeba escapovat znaky < > & " ' - pokud jsou například obsažené v názvu hry.
https://www.htmlhelp.com/reference/html40/entities/special.html

---
**NULL hodnoty**

Jak uvádět NULL hodnotu - Ve vyhlášce se sice píše uvádět NULL ale to není validní NULL hodnota podle xsd.

xsd je následující:

`<xs:element minOccurs="0" name="IDHry" nillable="true" type="string50" />`

Element by tedy neměl být uveden vůbec a nebo by měl být uveden jako 

`<Hra xsi:nil="true" />`

---
**Další popis k xml souborům** 

* **`licencia.xml`**
* * `<IDLicencia>35951486-000384/2020</IDLicencia>` - licence by se měla uvádět ve tvaru, které odpovídá skutečné vydané licenci budete validotivat proti IS na URHH



* **`koncove_zariadenie.xml`**
```
<Koncove_Zariadenie>
   <IDKoncoveZariadenie>12345678-ZARIZENI001</IDKoncoveZariadenie>
   <IDPrevadzka>12345678-KASINO-BRATISLAVA</IDPrevadzka>
   <TypTZ>VHP</TypTZ>
   <VyrobneCisloTZ>ZARIZENI001</VyrobneCisloTZ>
   <UmiestnenieOd>2021-01-01</UmiestnenieOd>
</Koncove_Zariadenie>
```
* * IDKoncoveZariadenie - lze předpokládat validaci proti stávajícímu IS na URHH



* **`hra.xml`**
```
<HHra xmlns="http://urhh.gov.sk/445/2020/hra/v1">
   <Hra xmlns="">
      <IDHraHry>12345678-CBD-1-000000000000001</IDHraHry>
      <IDKoncoveZariadenie>12345678-ZARIZENI001</IDKoncoveZariadenie>
      <IDHry>Pipes &amp; Pints</IDHry
      <DatumTransakcie>2021-04-20T05:24:17+02:00</DatumTransakcie>
      <TypTransakcie>1</TypTransakcie>
      <VyskaTransakcie>5.00</VyskaTransakcie>
      <Kredit>5.00</Kredit>
   </Hra>
</HHra>
```
* * IDHraHry - musí být unikátní v rámci balíčku
