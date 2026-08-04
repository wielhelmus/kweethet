# KweetHet op je iPhone zetten (GitHub Pages)

Deze map bevat de complete app. Alles werkt met relatieve paden, dus je kunt
hem in de hoofdmap van een repository zetten — geen instellingen nodig.

## Stap 1 — Repository maken
1. Ga naar https://github.com/new (gratis account volstaat).
2. Kies een naam, bijvoorbeeld `kweethet`.
3. Zet 'm op **Public** (bij een gratis account werkt Pages alleen zo).
4. Klik **Create repository**.

## Stap 2 — Bestanden uploaden
1. Klik in de nieuwe repo op **uploading an existing file**.
2. Sleep **alle bestanden uit deze map** erin (dus de losse bestanden,
   niet de map zelf).
3. Klik onderaan op **Commit changes**.

Let op: `.nojekyll` is een verborgen bestand. Zie je 'm niet in je verkenner,
zet dan verborgen bestanden aan — zonder dit bestand werkt het meestal ook,
maar het voorkomt problemen.

## Stap 3 — Pages aanzetten
1. Ga in de repo naar **Settings** → **Pages** (linkermenu).
2. Bij *Source*: kies **Deploy from a branch**.
3. Branch: **main**, map: **/ (root)** → **Save**.
4. Wacht 1 à 2 minuten. Bovenaan verschijnt je adres:
   `https://JOUWNAAM.github.io/kweethet/`

## Stap 4 — Op je beginscherm zetten
1. Open dat adres op je iPhone **in Safari** (dit werkt niet in Chrome).
2. Tik op de deelknop (vierkantje met pijltje omhoog).
3. Kies **Zet op beginscherm** → **Voeg toe**.

Je hebt nu een icoon zoals een gewone app: schermvullend, zonder adresbalk.

## Goed om te weten
- Het spel werkt **offline**, behalve de muziekfragmenten — die hebben altijd
  internet nodig.
- Het scorebord en je taalkeuze blijven bewaard op de telefoon.
- **Versienummering:** elke versie heet `KweetHet.<datum>` — deze is
  **KweetHet.04-08-26**. Je ziet 'm onderaan het instelscherm.
- **App bijwerken:** upload de nieuwe `index.html` en `sw.js` naar de repo. In
  `sw.js` staat bovenaan dezelfde datum (`kweethet-04-08-26`); die verandert
  automatisch mee, waardoor je telefoon de nieuwe versie ophaalt in plaats van
  de oude uit de cache.
- Iedereen met de link kan het spel spelen; je kunt 'm dus delen.

## Bestanden
- `index.html` — de app zelf
- `manifest.webmanifest` — naam, kleuren en icoon voor het beginscherm
- `sw.js` — zorgt dat de app offline werkt
- `icon-192.png`, `icon-512.png`, `icon-maskable.png` — de iconen
- `.nojekyll` — laat GitHub de bestanden ongewijzigd doorgeven
