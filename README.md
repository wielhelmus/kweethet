# KweetHet

*[English version below](#knowit)*

Een muziekspel voor het scherm van je telefoon of tv: de app speelt een
nummer, jij raadt in welk jaar het uitkwam en legt de kaart op de juiste
plek in je tijdlijn. Wie als eerste het ingestelde aantal kaarten heeft,
wint. Voor 2 tot 6 spelers, op één apparaat.

**Speel het hier:** https://wielhelmus.github.io/kweethet/

## Wat het is

- Werkt offline, ook op een oude telefoon in de tuin zonder bereik
- Nederlands, Engels en Duits (**KweetHet** / **KnowIt** / **WeißIch**)
- Te installeren als app op je beginscherm (Android, iOS, desktop)
- Ruim 20.000 nummers, verdeeld over negen speellijsten — van Top 2000 tot
  Songfestival tot uitsluitend Nederlandstalige nummers
- Zelf instelbaar: aantal spelers, fragmentlengte, hoeveel kaarten je nodig
  hebt om te winnen, en de trekkans per lijst

## Hoe het werkt

Geen bouwstap, geen server. `index.html` is de hele app — HTML, CSS,
JavaScript en de kaartgegevens in één bestand. Een service worker
(`sw.js`) zorgt dat alles ook zonder internet blijft werken; alleen het
afspelen van muziek (via iTunes/Deezer) heeft verbinding nodig.

Wil je 'm lokaal draaien: open `index.html` gewoon in een browser, of zet
er een simpele statische server overheen.

## Licentie

Zie [LICENSE.md](LICENSE.md).

---

## KnowIt

*Nederlandse versie hierboven.*

A music quiz for your phone or TV screen: the app plays a song, you guess
the year it came out and place the card in the right spot on your
timeline. First to reach the target number of cards wins. For 2 to 6
players, one device.

**Play it here:** https://wielhelmus.github.io/kweethet/

### What it is

- Works offline, even on an old phone with no signal
- Dutch, English and German (**KweetHet** / **KnowIt** / **WeißIch**)
- Installable as an app on your home screen (Android, iOS, desktop)
- Over 20,000 songs across nine playlists — from Top 2000 classics to
  Eurovision to Dutch-language-only tracks
- Fully configurable: number of players, clip length, cards needed to
  win, and the draw chance per list

### How it works

No build step, no server. `index.html` is the entire app — HTML, CSS,
JavaScript and the card data in one file. A service worker (`sw.js`)
keeps everything working offline; only playing music (via iTunes/Deezer)
needs a connection.

To run it locally: just open `index.html` in a browser, or serve it with
any simple static file server.

### License

See [LICENSE.md](LICENSE.md).

---

Made by Willem Bakker & Claude.
