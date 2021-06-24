**Know how k využití QSCD HW modulu**

**ICA**
V rámci realizace vzdáleného pečetění pro provozovatele - zkušenosti s ICA a jejich službou RemoteSeal.

Po aktivaci získáte aktivační json soubor a sadu kodů v java / C#.

Realizováno jako C#.NET FW 4.7.1 konzolová aplikace - dodaných 6 řádků kódu obaleno lepším provoláním a sestaveno jako command line utilita, která bere tyto vstupy:

- vstupní soubor
- výstupní soubor
- aktivační soubor

Pečetění proběhne tak, že se spočítá hash ze vstupního souboru a ten se odešle k zapečetění na vzdálený server ICA. Zpátky se vrátí signatura a vytvoří se kontejner. Kod od ICA je natolik banální a jednoduchý, že jeho integrace trvala hodinu. Pro ytvoření čas. razítka musí být pčeticí certifikát registrován i pro něj. Ze strany provozovatele ale časové razítko není nutné.

Zapečetěný soubor lze ověřit provoláním funkce -validateFile z původních jar souborů =  pečeť **je kompatibilní** se řešením postaveným na jar souborech cz MF.

**Disig**
Lze u nich využít komponentu, která vrátí samotný podpis. Jako parametr bere přístupový certifikát a kod certifikátu, který se má použít pro vytvoření pečetě.
Dodaný kód vrací samotnou signaturu. CADES-B-x kontejner je pak nutné vytvořit ručně - v realizaci. Pravděpodobně by šlo využít po úpravách původní jar knihovny.
