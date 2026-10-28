# QA report — Skladový systém FEFO & Barcode Detector v4

Datum sestavení: 28. 10. 2026

## Výsledek ověření

- **20/20 kontrol referenční logiky PASS** v lokálním JavaScript test harnessu.
- **JavaScript syntaxe PASS** v artefaktu v4.
- Výběr formátů BarcodeDetectoru ověřen: režim QR předává pouze `qr_code`; rozšířený režim vybírá jen formáty hlášené prohlížečem a vhodné pro alfanumerické ID balení.
- Validace payloadu ověřena: přesný text `PACK-26-00001` projde; URL, JSON a čistě číselný kód neprojdou.
- QR štítek nadále kóduje pouze prostý text ID balení.

## Co přidává verze 4

- Nativní API `BarcodeDetector` pro QR a další podporované symbologie.
- Režimy `Pouze QR Code` a `QR + alfanumerické čárové kódy`.
- Dynamické načtení `BarcodeDetector.getSupportedFormats()` a zobrazení podporovaných formátů.
- U výsledku skenu se zobrazuje zdroj a rozpoznaná symbologie.
- Každý výsledek je validován proti formátu `PACK-RR-00000` (číselná řada může být delší). URL ani JSON nejsou přijímány.
- Pokud browser API není dostupné, aplikace nabídne ruční zadání nebo klávesnicovou USB/Bluetooth čtečku.

## Proč nejsou zapnuté EAN/UPC

ID balení je alfanumerické, například `PACK-26-00001`. EAN/UPC jsou číselné formáty, a proto nejsou vhodnou symbologií pro toto ID. Rozšířený režim povoluje QR, Code 128, Code 39, Code 93, Data Matrix, PDF417 a Aztec — vždy pouze pokud je prohlížeč hlásí jako podporované.

## Omezení testu

Kameru a fyzické čtení štítku nelze automaticky potvrdit bez cílového zařízení. Před nasazením ověř skutečný telefon/počítač, vytištěný štítek a kamerová oprávnění. `BarcodeDetector` není dostupný ve všech prohlížečích; vyžaduje zabezpečený kontext (HTTPS nebo localhost) v prohlížečích, které jej podporují.

## Produkční poznámka

Toto je lokální demo s testovacími daty. Není připojeno ke Google Sheets ani Apps Scriptu. Produkční aplikace musí potvrdit existenci balení a aktuální FEFO pořadí na serveru v okamžiku skladové operace.
