
# Mi újság az F1 Race Engineer 2026-06-os frissítésében?

**Kiadás dátuma:** 2026. június 5.
**Érintett verzió:** v4.10+

Ez a dokumentum a 2026-06-03-i F1 25 — 2026 Season Pack DLC megjelenéséhez
készült funkciókat és változásokat foglalja össze. A "klasszikus" Setup Guide
PDF nem tartalmazza még ezeket, ezért külön fájlba gyűjtöttük őket.

---

## 🆕 F1 25 — 2026 Season Pack DLC támogatás

### Automatikus UDP-formátum felismerés

A program **mindkét formátumot támogatja egyszerre**, nincs külön mód-választás:

- **F1 25 alap (UDP Format: 2025)** — 22 autó, megszokott működés
- **2026 DLC (UDP Format: 2026)** — 24 autó (Audi + Cadillac), új MOM/Aero adatok

A program a játék által küldött csomag fejlécéből (`m_packetFormat`) automatikusan
felismeri, és a megfelelő paramétereket használja. **Nincs teendőd**:
csak állítsd be az UDP formátumot a játékban, és minden megy.

### Új pálya: Madring

A 2026-os Madridban épülő utcai pálya támogatva (track_id=42). A becsült 100%-os
köridő 85 másodperc — élesben a saját mért időddel finomodik. A pályatérkép
a Térkép funkción belül első körön tanul, vagy a backend mentett cache-ből
betöltődik (ha van).

---

## ⚡ Overtake / Boost (MOM) — új vizualizáció a Dashboard-on

A 2026-os szabályok szerint a DRS helyét az **Overtake Mode (MOM)** vette át.
Új vizuális elemek segítenek a versenyzés közbeni döntésekben.

### Felső lebegő detekciós-vonal sáv

Új lebegő sáv jelenik meg a Dashboard tetején — **csak akkor**, amikor van
értelme nézni:

- **Mikor jelenik meg?** Ha a következő detekciós vonal 800 méteren belül van,
  ÉS legalább egy autó 2 másodpercen belül van tőled (előtted vagy mögötted)
- **Hogyan néz ki?** Két oldali töltődő sáv (bal: mögötted, jobb: előtted),
  középen lila "DETEKCIÓS PONT" pillérrel. Ahogy közeledsz a vonalhoz, a két
  sáv kifelé tölt és összeér a közepén (= a detekciós vonalon vagyunk)
- **Színek:**
  - **Halvány narancs**: az adott oldali autó 1-2 mp-en belül
  - **Sötét piros pulzál** (bal): mögöttem < 1 mp → ha most átérünk, ő kapja az Overtake-et
  - **Kék pulzál** (jobb): előttem < 1 mp → ha most átérünk, **én kapom**

### Sárga ⚡ villám — akut MOM-támadás jelzése

Egy nagy sárga villám pulzál a bal oldalon **akkor és csak akkor**, amikor a
mögötted lévő autó **valódi MOM-támadást** indít. Négy feltétel egyszerre:

1. A mögötted lévő autó < 1 másodpercen belül van
2. A vonalon megnyerte az Overtake-jogot (jogosult a kör végéig)
3. Épp **nyomja** az MOM-gombot
4. Boost ERS-módban hajt (`m_ersDeployMode == 3`)

A négyes szűrés azért fontos, mert az AI gyakran nyomja az ERS-t, de az
**igazi MOM-támadás ritka** — csak akkor villog a villám, amikor tényleg
veszély van.

### Jobb / bal oldali ívek a versenyzés közben

Ugyanazok az ívek, mint a 2025-ös DRS-hez voltak — csak a 2026-os módban
a jobb oldali színe **zöld helyett kék** (Overtake szín-konvenció szerint):

- **Bal piros ív**: mögöttem < 1 mp (klasszikus "vigyázz, közel jön")
- **Jobb kék ív**: előttem < 1 mp (klasszikus "támadási lehetőség")
- **Vastagabb pulzáló** változat: 0.5 mp-en belül (kritikus)

---

## 🎙️ Magyar TTS pilóta-név kiejtés

A hangos mérnök **magyaros kiejtéssel** mondja a pilóta-neveket. Néhány példa:

| Eredeti | Magyaros kiejtés |
|---|---|
| Norris | **Norrisz** |
| Verstappen | **Versztappen** |
| Hamilton | **Hemilton** |
| Leclerc | **Lökler** |
| Sainz | **Szánc** |
| Russell | **Rasszel** |
| Piastri | **Piasztri** |
| Alonso | **Alonzó** |
| Tsunoda | **Cunoda** |
| Magnussen | **Magnüsszen** |
| Bearman | **Bermen** |
| Doohan | **Doán** |
| Lawson | **Lóvszon** |
| Hadjar | **Hadzsár** |

Összesen 37 pilóta van a listában (mostani mezőny + klasszikus pilóták a
Career módhoz). Az ismeretlen neveket (pl. ligás Steam-felhasználó) a TTS
az eredeti formában mondja.

**Csak magyar nyelvben aktív** — angolban a TTS természetesen az angol
kiejtést használja.

---

## 🏆 Verseny vége — végösszegzés bemondás

A célon átérés után a mérnök bemondja a végeredményt + a dobogósokat:

> *"Vége a versenynek. 7. lettél. A versenyt Norrisz nyerte, második Versztappen,
> harmadik Hemilton."*

Ez **csak versenyen** szól (sprint/race), nem időfutamon vagy időmérőn. A
trigger a F1 25 `m_resultStatus = 3` (finished) — vagyis amikor a saját
autóra fizikailag véget ér a verseny.

---

## ⏱️ Időfutam tisztítás

Az időfutamon (Time Trial) a mérnök korábban hibásan **versenyhez kapcsolódó**
üzeneteket adott (pl. "Utolsó kör, hozd haza", "Gumik túlmelegedtek, hűteni
kell", "Automata stratégia mód"). Ezek mind eltűntek.

Időfutamon mostantól **csak az intro szól**:

> *"Rádió tiszta. Üdv az időfutamon. Pálya: Madring."*

Más mérnöki üzenet nincs — a sofőr nyugodtan dolgozhat a hot-lap-okon.

---

## 🔧 Wheel-spin coaching finomítás

A "Túl agresszív a kigyorsítás, kipörög a hátulja!" típusú üzenet:

- **Új küszöb**: 5.0 (volt 2.5) — a standing-rajt és a normál kanyar-kijárati
  kigyorsítás már nem triggereli
- **60 másodperces cooldown** — max percenként egyszer szólhat
- **Csak versenyen** — időfutamon a sofőr tudatosan keresi a határt, nem
  mérnöki tanácsra vár

Filozófiai változás: ez most már **összesített** figyelmeztetés ("mostanában
gyakran kipörögtél"), nem valós idejű riasztás. A pillanatnyi kipörgést a
sofőr érzi a kormányon — a mérnöki tanács pedig finomabban koccol be.

---

## 🪟 Electron főablak maximalizálva indul

A program nyitásakor a fő ablak **automatikusan maximalizált** (a Windows
taskbar-ig kitölt). A meglévő minimize/maximize/close gombok továbbra is
működnek a custom titlebar-on.

---

## ☕ "Buy Me a Coffee" gomb a főmenüben

A támogatói gomb most a **főmenü fejlécében is** elérhető, a "📖 Manual"
link és az óra között. Ugyanaz a sárga gomb, ugyanaz a link:
**buymeacoffee.com/zeropointrace**

A három helyen (főmenü + dashboard + licenc oldal) most már konzisztens
a gomb.

---

## 🧹 Eltávolított, nem működő gombok

A Dashboard fejlécéből **két nem működő gomb eltávolítva**:
- "Rádió Teszt"
- "Időjárás Kérés"

Maradt: "← FŐMENÜ", "Logolás: KI", "🗺️ Térkép", "Segítek a fejlesztésben",
"☕ Buy Me a Coffee".

---

## 🔍 Diagnosztika — új pályák gyors azonosítása

Ha egy ismeretlen `track_id` érkezik a játékból (pl. F1 25 jövőbeli DLC új
pálya), a program **session-enként egyszer** WARNING-ot ír a log-fájlba:

```
WARNING - Ismeretlen track_id=33 (packet_format=2026).
Bővítendő a config.F1_TRACKS mapping.
```

Erről a számról könnyen jelezhetjük az új pályát, és gyorsan beépíthető a
következő frissítésbe.

---

## 🙏 Köszönet

Köszönjük a beépített funkciókat és a folyamatos visszajelzéseket!
Ha találsz bármi furát az új funkciókban, mondd, és javítjuk.

- **Visszajelzés**: a Dashboardon a "Segítek a fejlesztésben" gomb
- **Email**: f1zeropointracing@gmail.com
- **Support**: [buymeacoffee.com/zeropointrace](https://buymeacoffee.com/zeropointrace)

---

**F1 Race Engineer © 2026 ZeroPoint Racing**
