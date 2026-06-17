# Měřič místnosti (AR) — PWA

WebXR měřič prostoru kamerou. Body klepnutím, počítá **vzdálenost**, **plochu** a **výšku**,
umí **ověřit přesnost** kartou/A4, hlídá podporu zařízení a měření **exportuje do reportu**,
který si pak v klidu projdeš.

## Soubory
```
index.html              — celá aplikace (UI + WebXR logika)
manifest.webmanifest    — PWA manifest
sw.js                   — service worker (instalace + offline shell)
icons/                  — ikony PWA
```

## Nasazení na Vercel
WebXR vyžaduje **HTTPS** — lokální `file://` ani `http://` nestačí. Nejjednodušší cesta:

1. `npm i -g vercel` (pokud ještě nemáš)
2. ve složce projektu spusť `vercel` a potvrď. Hotovo, dostaneš HTTPS URL.

(Stejně dobře funguje jakýkoli statický hosting s HTTPS — GitHub Pages, Cloudflare Pages, Netlify…)
Žádný backend není potřeba, vše běží v telefonu.

## Použití
1. Otevři URL v **Chrome na Androidu** (S23 Ultra ✓, Fold7 si appka sama otestuje).
   Můžeš ji nainstalovat na plochu (Přidat na plochu).
2. **Spustit AR měření** → namiř na podlahu, dokud nenaskočí amber kroužek.
3. Vyber režim, klepni **● Umístit bod** (nebo klepni do scény).
   - Vzdálenost / Výška = 2 body. Plocha = obejdi rohy (3+ bodů).
4. **✓** uloží měření — pojmenuj ho (Podlaha, Sloup, Okno, Tloušťka zdi…).
5. Na úvodní obrazovce **Exportovat report** stáhne samostatný HTML soubor
   se všemi měřeními, půdorysem podlahy a snímky — otevřeš ho kdykoli později.

## 3D kostra
Tlačítko **3D kostra (náhled)** na úvodu (a stejná sekce v reportu) složí z naměřených
bodů 3D drátěný model: černé pozadí, světlá průhledná krychle místnosti (podlaha
vytažená do výšky stropu) a oranžové **čárkované kóty** s naměřenými čísly. Sloupy,
okna i tloušťky zdí se vykreslí na svých skutečných místech v prostoru (všechna měření
z jedné AR session sdílí stejný souřadný systém). Táhnutím otáčíš, dvěma prsty zoom.
3D náhled potřebuje internet (načítá 3D knihovnu z CDN).

## Ověření přesnosti
Na úvodu vyber **Karta** nebo **A4**, spusť ověření a změř hranu předmětu.
Appka ukáže odchylku oproti etalonu (karta/občanka = ID-1 85,6 × 54 mm, A4 = 297 × 210 mm).
Klidně použij běžnou platební/věrnostní kartu — stejný formát, bez osobních údajů.

## Upřímně k „fotkám"
Webové AR (na rozdíl od nativní appky) **nemá zaručený přístup k obrazu z kamery** —
záleží na podpoře modulu `camera-access`. Appka se snímek z kamery pokusí získat
(„📷 Snímek" / automaticky při uložení), a když to zařízení nedovolí, řekne to a doporučí
**snímek obrazovky telefonu** (Power + Vol-), který spolehlivě zachytí i vykreslené čáry.
Spolehlivý digitální záznam je tak hlavně **report**: u každého prvku název + rozměr,
plus auto-generovaný **půdorys** z naměřené podlahy.

## Poznámky / limity
- Funguje na **Androidu (Chrome) s ARCore**. iOS Safari WebXR-AR neumí — tam by to vyžadovalo obezličku.
- Přesnost: typicky jednotky procent; horší při slabém světle, na lesklých/holých plochách a na dlouhých vzdálenostech.
- Výška: strop se přes hit-test najde jen když ho ARCore detekuje jako plochu; jinak miř na horní hranu zdi.
- Uložená měření se drží i v `localStorage`, ale snímky jen po dobu session — proto je důležitý export reportu.
