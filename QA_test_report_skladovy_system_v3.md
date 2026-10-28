# Skladový systém — QA protokol v3

**Datum testu:** 28. 10. 2026  
**Artefakt:** `Skladovy_system_FEFO_QR_v3.html`  
**Rozsah:** lokální referenční simulace; nejde o produkční napojení na Google Sheets / Apps Script.

## Výsledek

- **18 / 18** interních invariantů referenční skladové logiky prošlo.
- **10 / 10** skriptovaných interakčních kontrol prošlo: přepnutí režimů, FEFO doporučení, přijímání a odmítání payloadů, blokace nesprávného výdeje, dohledatelnost spotřeby, přesun boxu, generování QR SVG a osm tiskových slotů. Skutečná kamera nebyla fyzicky testována.
- Generovaný QR kód byl vyrenderován a dekódován. Výsledek dekódování: přesně `PACK-26-00001`.
- Statická kontrola HTML: bez duplicitních ID prvků.

## Funkce v3

1. **Dashboard:** skladové KPI, prioritní FEFO kandidát, vyhledávání, filtry a detail vybraného balení.
2. **Scan QR:** samostatná stránka se dvěma funkčními režimy — kamera a ruční ID / klávesnicová čtečka. Kamera se spouští až po akci uživatele.
3. **FEFO brána:** kontrola nejbližší expirace, příjmu a ID. Při naskenování neprioritního balení se spotřeba zablokuje a zobrazí se tlačítko pro načtení doporučeného kandidáta.
4. **QR štítky:** offline generovaný SVG QR; datový obsah je pouze prostý PACK ID. Bez URL, JSON a skladových metadat. Hromadný tisk A4: 2 × 4 = 8 štítků na stránku.
5. **Pohyby a výroba:** simulace spotřeby, pohybu, opravného pohybu a vazby na výrobní dávku.
6. **QA testy:** 18 kontrol základních invariants a QR formátu.

## Ověřené invarianty

- FEFO: nejbližší expirace → nejstarší příjem → vzestupné ID.
- Prošlé, prázdné, bez expirace, mimo sklad a s neaktivním umístěním nejsou běžnými kandidáty.
- Přímé umístění PACK na LOC není podporováno; umístění je odvozováno přes BOX.
- Přesun boxu změní odvozenou pozici jeho balení a nemění ID balení.
- Neaktivní pozice se zamítne.
- QR validace přijme statické PACK ID a odmítne URL / JSON.
- Spotřeba propojuje PACK, MOV a BATCH.
- Opakovaný idempotency klíč nezaloží druhý záznam v referenční testovací funkci.
- Oprava přidává navazující záznam, originál zůstává zachován.
- Historická instance boxu a nová instance se stejným štítkem mají odlišné interní UID.
- Hromadný tisk vytvoří přesně osm slotů na tiskovou stránku; nedokončená stránka se doplní prázdnými místy.

## Otevřená rizika před produkcí

1. **Integrace Apps Script není součástí této simulace.** Produkční server musí znovu ověřit stav balení, oprávnění, FEFO pořadí a aktivní vazby pod `LockService` bezprostředně před zápisem.
2. **Google Sheets není plnohodnotná transakční databáze.** Je potřeba implementovat idempotentní operace, řízení souběhu, zotavení po částečném zápisu a chránit logy před běžnou editací.
3. Kamerové skenování závisí na oprávnění kamery, zabezpečeném kontextu (HTTPS / localhost) a podpoře `BarcodeDetector`. Ruční zadání a klávesnicová čtečka jsou fallback.
4. Tisk je nutné ověřit na konkrétním archu A4, tiskárně, rozměrech štítku a reálné čitelnosti po tisku.
5. Simulovaná spotřeba neodečítá zůstatkovou hmotnost balení; průběžné vážení je mimo rozsah PRD.

## Spuštění

Otevři `Skladovy_system_FEFO_QR_v3.html` v aktuálním prohlížeči. Pro kameru použij HTTPS nebo localhost a povol přístup ke kameře. Pokud prohlížeč dekódování kamery nepodporuje, vyber **Ruční ID / čtečka**.

Všechna data jsou testovací a zůstávají v lokální simulaci. Soubor neprovádí zápis do skutečného Google Sheets.
