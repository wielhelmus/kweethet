# KweetHet — projectafspraken

Lees dit voordat je iets wijzigt. Deze afspraken zijn ontstaan uit fouten die
al een keer gemaakt zijn; ze overslaan levert die fouten opnieuw op.

Communiceer met Willem in het **Nederlands**.

---

## Wat dit is

Een muziekspel: de app speelt een fragment, de speler raadt het releasejaar en
legt de kaart op de juiste plek in zijn tijdlijn. Eerste met het ingestelde
aantal kaarten wint. 2 tot 6 spelers, één apparaat.

Het draait als **losse HTML-app** — één bestand met alles erin, zodat het
offline werkt en op GitHub Pages gehost kan worden.

## Bestanden

| Bestand | Wat |
|---|---|
| `index.html` | De hele app: HTML, CSS, JavaScript en de kaartgegevens. ~1,4 MB |
| `sw.js` | Service worker; zorgt dat de app offline werkt |
| `manifest.webmanifest` | Naam, kleuren en icoon voor "zet op beginscherm" |
| `icon-192.png`, `icon-512.png`, `icon-maskable.png` | Iconen |
| `LICENSE.md` | Gebruiksvoorwaarden |
| `LEES-MIJ.md` | Uitleg voor Willem over publiceren op GitHub Pages |
| `.nojekyll` | Laat GitHub Pages de bestanden ongewijzigd doorgeven |

Er is **geen bouwstap**. `index.html` is wat de browser krijgt. Bewerk het
bestand rechtstreeks.

---

## Vaste regels

### 1. Versienummer bij elke wijziging

Willem wil elke versie genummerd zien als **`KweetHet.<dd-mm-jj>`**, dus dag-
maand-jaar met tweecijferig jaartal. Bij een wijziging op 12 september 2026
wordt dat `KweetHet.12-09-26`.

Pas twee plekken samen aan, anders zien telefoons de nieuwe versie niet:

```
index.html   const APP_VERSION='KweetHet.12-09-26';
sw.js        const CACHE = 'kweethet-12-09-26';
```

De cachenaam móet meebewegen: blijft die gelijk, dan serveert de service worker
de oude versie uit de cache en lijkt het alsof je wijziging niets deed.

### 2. Reservekopie vóór je in de kaartgegevens snijdt

`ALL_CARDS` is de enige plek waar de nummers staan. Verwijder je er iets, dan
is het weg. Schrijf de huidige inhoud eerst weg naar een JSON-bestand buiten
`index.html`. Dit is een keer misgegaan en kostte een reconstructie uit oude
zip-bestanden.

### 3. Test voordat je oplevert

Minimaal: syntaxcontrole van het JavaScript, en een volledig potje uitspelen.
Zie "Testen" hieronder. Lever niets op met de mededeling dat het "zou moeten
werken".

### 4. Drie talen, altijd samen

Nieuwe tekst betekent drie vertalingen: `nl`, `en`, `de`. Een sleutel die in
één taal ontbreekt valt stilzwijgend terug op Nederlands, dus je ziet het niet
meteen.

### 5. Wees eerlijk over wat je niet gecontroleerd hebt

Zeg het als je iets niet hebt kunnen testen. Niet gladstrijken.

---

## Hoe de app in elkaar zit

### Kaartgegevens

```js
const ALL_CARDS = [[jaar, artiest, titel, spotify_id, lijst], ...]
```

- `lijst` is 0 t/m 7 en komt overeen met "Lijst 1" t/m "Lijst 8" in het scherm
- `spotify_id` mag leeg zijn; audio wordt op artiest+titel gezocht, niet op id
- ongeveer 19.000 kaarten

De lijsten: 0 = Top 2000, 1 = Alternatief, 2 = Breed, 3 = Extra,
4 = Top 40 #1-hits, 5 = Rock aller tijden, 6 = Songfestival,
7 = Top Aller Tijden.

### Stapel opbouwen (`newGame`)

Gebeurt in deze volgorde, en die volgorde is bewust:

1. alleen de aangevinkte lijsten
2. bij Engels of Duits: Nederlandstalige nummers eruit (`isDutchCard`)
3. ontdubbelen op `songKey` (artiest+titel, kleinvergelijkend) — hetzelfde
   nummer staat soms in twee lijsten; de laagste gekozen lijst wint
4. pas dan kaartobjecten maken

Elke kaart krijgt `ix`: zijn plek in `ALL_CARDS`. Die wordt gebruikt om een
potje compact te bewaren.

### Trekkans

`GROUP_W` bepaalt hoe vaak elke lijst aan de beurt komt; alleen de **verhouding**
tussen de aangevinkte lijsten telt. Willem stelt dit in met schuifjes; de waarde
wordt bewaard in `localStorage`. `GROUP_W_DEF` is de standaard.

Binnen een lijst heeft elke **artiest** evenveel kans, ongeacht hoeveel nummers
hij heeft — anders krijg je zes keer achter elkaar Queen.

### Audio

Zoekt eerst op iTunes, dan op Deezer, met de volledige artiest en titel. Levert
dat niets op, dan volgt een **tweede poging** met opgeschoonde tekst
(`cleanAr` / `cleanTi`): "Måneskin, Iggy Pop — I WANNA BE YOUR SLAVE (with Iggy
Pop)" wordt "Måneskin — I WANNA BE YOUR SLAVE".

Wijzig je die opschoonregels, dan verandert welke nummers speelbaar zijn.

### Meertaligheid

- `TR` = woordenboek met `nl`, `en`, `de`
- `t('sleutel', {n: 3})` haalt tekst op en vult `{...}` in
- `applyI18n()` vervangt alles met `data-i18n="sleutel"`
- `setLang(l)` schakelt om en bouwt de schermen opnieuw op

De merknaam is per taal anders: **KweetHet** / **KnowIt** / **WeißIch**.

### Potje bewaren

De stand wordt na elke schermupdate bewaard (`saveGame`), zodat verversen niets
kost. Er wordt opgeslagen op kaartindex, niet op kaartinhoud — daarom staat
`deck: ALL_CARDS.length` in het bestand: wijkt dat af bij het laden, dan is de
stapel gewijzigd, kloppen de indexen niet meer en wordt het oude potje
weggegooid. **Laat die controle staan.**

### Opslagsleutels

`kweethet.lang.v1`, `kweethet.win.v1`, `kweethet.cliplen.v1`, `kweethet.vol.v1`,
`kweethet.qr.v1`, `kweethet.kermis.v1`, `kweethet.names.v1`,
`kweethet.played.v1`, `kweethet.hist.v1`, `kweethet.weights.v1`,
`kweethet.game.v1`

---

## Valkuilen die al een keer toesloegen

**`${t('...')}` breekt in strings met enkele aanhalingstekens.** De
aanhalingstekens rond de sleutel sluiten de string voortijdig. Gebruik
samenvoegen: `'... '+t('sleutel')+' ...'`.

**De app heeft CSS-regels die de jouwe overschrijven.** `.field input` en
`.listpick input` zijn breed geformuleerd en gaven een schuifregelaar ooit een
hoogte van 52 px met een kader. Maak je selector specifieker
(`.listpick .lslide input[type=range]`) — maar hang hem **niet** aan `.field`,
want datzelfde blok wordt ook buiten het instelscherm gebruikt.

**Een rij met een schuifregelaar heeft `flex-wrap: wrap` nodig,** anders wordt
de rest van de regel platgedrukt.

**Slepen op een schuifregelaar binnen een `<label>` vinkt het vakje aan of
uit.** Vang dat af op de rij zelf.

**`applyI18n` vervangt de inhoud van elementen met `data-i18n`.** Zet daar geen
iconen of pijltjes in als kindelement — die verdwijnen bij de eerste
taalwissel. Gebruik CSS (`::before` / `::after`).

**Nieuwe tekst die dynamisch wordt gezet, moet ook bij `setLang` opnieuw
gezet worden.** De "al gespeeld"-tekst schakelde ooit niet mee omdat
`refreshHist()` alleen bij het opstarten werd aangeroepen.

**De kopbalk sloeg op smalle telefoons zijwaarts om** in plaats van naar een
tweede rij, waardoor knoppen onbereikbaar leken. Test wijzigingen aan de
kopbalk op 320 px breed.

---

## Testen

### Syntaxcontrole

```bash
python3 -c "
import re; html=open('index.html',encoding='utf-8').read()
sc=re.findall(r'<script>(.*?)</script>',html,re.S)
open('/tmp/chk.js','w').write('var x,window={addEventListener(){}},document={},localStorage={getItem(){return null},setItem(){}},navigator={};'+';'.join(sc))
" && node --check /tmp/chk.js
```

### Spelen zonder geluid

In een browsertest is er geen netwerk naar de muziekdiensten. Vervang het
afspelen door een lege belofte:

```js
const a = getAudio(); a.play = () => Promise.resolve();
```

Daarna kun je een volledig potje afspelen door in een lus `chooseSlot`,
`reveal` en `endTurn` aan te roepen tot `G.phase === 'win'`. Reken op 200 tot
250 beurten; loopt het tegen de 400 aan, dan is dat niet meteen een fout.

### Waar je op moet letten

- geen fouten in de console
- alle drie de talen omschakelen en terug
- 320 px breed: geen horizontaal scrollen
- aantikbare elementen minstens 44 px hoog
- een potje bewaren, de pagina verversen, en kijken of de stand identiek terugkomt

---

## Publiceren

De app staat op GitHub Pages. Na een wijziging: `index.html` en `sw.js`
committen en pushen. Binnen een minuut of twee staat het live.

Willem opent de site op zijn iPhone via Safari en heeft hem op het beginscherm
gezet. Ziet hij de oude versie, dan is de cachenaam in `sw.js` niet meegewijzigd
(zie regel 1), of moet de app één keer uit de app-switcher geveegd worden.

---

## Wat Willem waardeert

- Eerst meten, dan wijzigen — geen aannames over wat er stuk is
- Kleine, controleerbare stappen
- Ontwerpvoorstellen eerst laten zien voordat je ze inbouwt
- Openheid over afwegingen en over wat niet getest is
