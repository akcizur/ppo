# Skladový systém — FEFO & Barcode Detector v4

**Výchozí stránka:** `index.html`  
**Samostatný soubor:** `Skladovy_system_FEFO_Barcode_Detector_v4.html`

## Spuštění

1. Otevři aplikaci přes GitHub Pages/HTTPS nebo lokálně přes localhost.
2. Přejdi na **Scan QR**.
3. Zvol režim **QR + alfanumerické čárové kódy** nebo **Pouze QR Code**.
4. Stiskni **Spustit kameru** a povol přístup ke kameře.
5. Pokud prohlížeč nenabízí `BarcodeDetector`, použij ruční ID nebo USB/Bluetooth čtečku, která funguje jako klávesnice.

## Zásady

- QR štítek obsahuje výhradně prostý text ID balení, např. `PACK-26-00001`; neobsahuje URL ani JSON.
- Rozšířený detektor použije jen formáty, které aktuální prohlížeč hlásí jako podporované a které lze využít pro alfanumerické PACK ID.
- EAN/UPC se pro ID balení nepoužívají, protože očekávaný identifikátor obsahuje písmena.
- FEFO v demo rozhraní je referenční logika. Produkční kontrola výdeje musí proběhnout v Apps Scriptu před zápisem.
- Data jsou testovací; aplikace zatím není propojena se skutečným Google Sheets/Apps Script backendem.

Detaily ověření: [QA_test_report_Barcode_Detector_v4.md](QA_test_report_Barcode_Detector_v4.md).
