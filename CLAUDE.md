# Verbouwplan begane grond

Interactieve plattegrond waarin de bewoner zelf verbouwvarianten uitprobeert (badkamer verplaatsen, muren slopen, sanitair schuiven). Live via GitHub Pages: https://joris-git.github.io/Verbouwplan/ (publiceert automatisch vanaf `main`, map `/`).

## Opbouw

- Alles staat in **één bestand: `index.html`** (HTML, CSS en vanilla JS, geen build). Houd dat zo.
- Extern geladen: Google Fonts (IBM Plex Sans Condensed, IBM Plex Mono) en **Three.js 0.160.0** van jsdelivr (via de importmap in `<head>`). Three.js wordt pas geladen als de gebruiker op 3D tikt; de 2D-tekening werkt zonder. Voeg geen andere libraries toe zonder overleg.
- `README.md` is de gebruikersinstructie. Werk die bij als de bediening verandert.

## Datamodel

- Coördinaten in **millimeters**. `x` loopt van links naar rechts vanaf de binnenkant van de linker zijgevel. `y` loopt van de **tuin (boven) naar de straat (onder)**, met `y = 0` op de binnenkant van de achtergevel. De aanbouw met douche en portaal heeft negatieve `y`.
- Elk element is een object in `items` met `kind`:
  - `wall`: kent de vlaggen `bear` (dragend), `demo` (slopen) en `nieuw`.
  - `door` / `window`: `rot` in stappen van 90°, `flip` en `slide`. Een `door` met `open` is een doorgang zonder deur: wel een opening, geen deurblad.
  - `room`: bepaalt het oppervlak en de label. Ruimtes met **dezelfde naam** tellen als één ruimte (L-vorm): het oppervlak wordt opgeteld en de naam staat alleen op het grootste deel (`roomParts`, `roomArea`, `roomMain`).
  - `fix`: sanitair en inrichting. `type` verwijst naar de bibliotheek `T`. `moved` geeft aan dat de gebruiker het element heeft verplaatst.
  - `marker`: leidingpunt, met `m` als `afvoer`, `water` of `gas`.
- `x,y,w,h` is altijd de omhullende rechthoek. `rot` bepaalt alleen hoe het symbool daarbinnen staat. De voorkant van een symbool is lokaal `+y`.
- `BASE` is de bestaande situatie, overgenomen van bouwtekening P36N uit 1997. Correcties van de bewoner gaan voor de tekening. Zo heeft de hal alleen een deur naar de woonkamer, kom je vanuit de gang via één deur direct in de L-vormige slaapkamer 1 (er is geen tweede gangetje), heeft slaapkamer 2 rechtsachter een tuindeur die naar buiten draait en staat daar geen koelkast. Van de gang naar de keuken is een open doorgang zonder deur. Tussen slaapkamer 2 en de keuken zit een dichte muur. Er is geen deur van de keuken naar het portaal. In de badkamer (douche) staat geen toilet. **Verander je `BASE`, verhoog dan `BASE_VER`.** Anders blijft de oude bestaande situatie uit `localStorage` staan. Eigen varianten worden niet aangepast.
- `T[type].cl` is de benodigde vrije ruimte vóór het element. `T[type].rs` geeft aan dat het element van maat mag wisselen (dan verschijnen er grijpblokjes).

## Gedrag dat bewust zo is

- **Bestaande situatie is heilig.** `snapshot()` maakt bij de eerste wijziging automatisch een nieuwe variant aan.
- **Waarschuwingen** (`issues()`) gelden alleen voor sanitair met `nieuw` of `moved`. De bestaande situatie geeft geen rode meldingen.
- **Muren eerst aantikken, dan slepen.** Een vergrendeling is er niet meer. Sleep je aan een muur die niet gekozen is, dan schuift de tekening en volgt een melding. Een muur schuift alleen dwars op zijn richting. `wallLinks()` zoekt wat meegaat: dwarsmuren waarvan een uiteinde tegen de muur zit (die worden langer of korter), deuren en ramen in de muur (die schuiven mee) en ruimtes waarvan de rand voor minstens 80% langs de muur loopt (die worden groter of kleiner). `clampLinks()` houdt elk meegaand stuk minstens 5 cm.
- **Paneel:** per soort element alleen de nodige knoppen. Dragend en Draai 90° voor muren staan onder "Meer". Sanitair heeft geen Slopen, alleen Verwijder.
- **Opslag:** varianten staan in `localStorage` onder de sleutel `verbouwplan-v1`. Uitleg gezien: `verbouwplan-intro`. Delen gaat via "Kopieer variant als tekst" (JSON).
- **Maten:** de gebruiker ziet **centimeters**; intern wordt in mm gerekend. Alleen de velden onder "exacte maten" tonen mm.
- **3D-maquette** (knop 2D / 3D in de werkbalk) wordt elke keer opnieuw opgebouwd uit `items` (`build3d()`, aangeroepen via `v3.dirty` aan het eind van `render()`). Verschuiven gebeurt alleen in 2D; in 3D kun je elementen wel aantikken om ze te kiezen. Hoogtes staan in `H3` (plafond 2600, deur 2100, borstwering 900). Deuren en ramen snijden een opening in de muur waarmee ze overlappen. Een `window` met "deur" of "pui" in de naam loopt door tot de vloer. Kleuren in 3D komen uit de `--v3-*`-variabelen op `:root`. Standaard staan de muren op lage stand (`v3.low`). Bij **Rondkijken** zijn ze vol hoog, met plafond, en krijgen nieuwe muren de gewone muurkleur.
- **Chat** (knop "Chat"): de browser roept de Claude API rechtstreeks aan (`fetch`, header `anthropic-dangerous-direct-browser-access`). Model `claude-opus-5-5`, `fallbacks: "default"` met beta `server-side-fallback-2026-07-01`, effort `medium`, automatische prompt-caching. De gebruiker vult een eigen API-sleutel in; die staat in `localStorage` onder `verbouwplan-sleutel` en komt nooit in de repo. Claude krijgt bij elke vraag de tekening als tekst (`planText()`) en antwoordt met de tools in `CHAT_TOOLS` (strict). `runTool()` voert ze uit; muren gaan via `wallLinks()`. Eén chatopdracht = één `snapshot()`, dus ↶ draait de hele opdracht terug. Het gesprek is append-only: antwoorden, met thinking-blokken, gaan ongewijzigd terug. Bij een fout wordt alleen de mislukte laatste beurt verwijderd. Test de chat met een nagebootste API (`context.route` op `https://api.anthropic.com/v1/messages`).
- **Uitleg** staat in `STEPS` (6 stappen) en opent bij het eerste bezoek en via de knop `?`.

## Taal en stijl

- Alle tekst in de interface en alle commits in het **Nederlands**: kort, gewone woorden, gericht op een bewoner zonder bouwkennis.
- Kleuren alleen via de CSS-variabelen op `:root`; licht en donker thema zijn allebei gedefinieerd. Rood (`--accent`) betekent nieuw/slopen/probleem, blauw (`--water`) is sanitair.
- Werkt ook op de telefoon (onder 860px: tekening vol, onderpaneel inklapbaar). Test daar altijd mee.

## Testen

Headless met Playwright en Chrome (`/usr/bin/google-chrome`): maak screenshots op 1400×860 en 390×780 (met touch), doorloop de uitleg, versleep een toilet, voeg een bad toe. Controleer of er geen JS-fouten zijn (`pageerror`). Test ook 3D: klik op 3D, probeer elke kijkrichting, Muren laag en Rondkijken. Is jsdelivr niet bereikbaar, haal Three.js dan op met `npm pack three@0.160.0` en stuur de jsdelivr-verzoeken met `context.route` naar die lokale kopie. Start Chromium headless met `--use-gl=angle --use-angle=swiftshader` zodat WebGL werkt.

## Let op

- Geen adres of andere herleidbare gegevens van het huis in de repository; deze is openbaar.
- Maten van onderdelen zijn van een foto van de bouwtekening afgelezen en kunnen enkele centimeters afwijken.
