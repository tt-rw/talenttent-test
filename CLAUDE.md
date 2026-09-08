# CLAUDE.md — The Talent Tent

## Project
Muzikanten-matchingplatform (talenttent.org). Doelgroep 13-25 jaar, Den Haag e.o.
Nederlandstalig. Founder/product owner: Ronald, geen programmeerervaring.

## PPP-principe (verplichte toets bij elke feature-/architectuurbeslissing)
- Presentatie — profiel tonen/bijhouden
- Prestatie — zoekfunctie, iemand vinden om mee te spelen
- Plezier — de app en het spelen leuk vinden, blijven terugkomen
- Voorwaarde 0 — stabiliteit wint altijd van functionaliteit

## Stack
Eén bestand index.html (wordt nu opgesplitst), vanilla JS, Supabase-backend
(client-side, geen server), GitHub Pages. Twee repo's:
- tt-rw/talenttent.org — productie
- tt-rw/talenttent-test — externe bètatest

## Werkregels, verplicht elke sessie
0. Commit en push altijd rechtstreeks naar `main`, geen aparte branch, tenzij
   expliciet gevraagd. Een ticket is pas klaar als `main` is bijgewerkt — niet
   als er een branch of pull request klaarstaat. Ronald uploadt zelf geen
   bestanden meer naar GitHub.
1. Eén ticket per sessie. Nooit een tweede starten zonder expliciet verzoek.
2. Verificatieplicht: elke uitspraak labelen als Geverifieerd / Aanname / Onbekend.
   Nooit "zou moeten werken".
3. str_replace/gerichte edits. Nooit het hele bestand herschrijven, tenzij het
   echt niet anders kan.
4. Regressieprotocol verplicht: nulmeting vóór de wijziging (regelaantal, SHA-256,
   haakjesbalans {}()[], functielijst). Volledige diff-controle ná de wijziging,
   vóór functioneel testen.
5. Playwright-bewijs vóór oplevering, 390×844 (mobiel) en 1440×900 (desktop).
6. Bij een gedeeld element (CSS-klasse, JS-functie, databasekolom): eerst alle
   gebruiksplekken opzoeken, vóór het wijzigen.
7. Nieuwe sessie melden zodra: meer dan één ticket in deze sessie, een eigen
   eerdere uitspraak wordt gecorrigeerd, het bestand meermaals volledig is
   herplaatst, of Ronald twee keer dezelfde fout corrigeert.

## Schrijfstijl (ASD-STE100-achtig, Nederlands)
Eén instructie per zin. Korte zinnen (±20 woorden max). Actieve vorm. Concrete
werkwoorden. Consistente terminologie (bijv. altijd "straal", nooit "radius").

## Huisstijl (samenvatting)
- Kleuren: bg #0d0d0d, accent #f5c518 (goud), accent2 #6ec8d8 (alleen genre/
  gezocht-badges), danger #e5533d, success #4caf50
- Tikdoelen minimaal 44×44px overal
- 4px-afstandsschaal
- Geen emoji, nergens in de UI
- Modals op mobiel (≤560px): volledig scherm (100dvh)
- Modal-breedte altijd via CSS-klasse, nooit inline style
- Destructieve acties: kleur --danger, nooit even zwaar als de primaire actie

## Database
Supabase, client-side only. Claude heeft geen directe databasetoegang — vraagt
om de werkelijke functiedefinitie/data wanneer nodig, bouwt nooit op een
aanname over ongeziene databasecode. SQL voert Ronald zelf uit, in de Supabase
SQL Editor.

## Bestandsstructuur (doel van de huidige opsplitsing)
styles.css, core.js (Supabase-client, globale state, appInit(), showView(),
hash-routing), utils.js, auth.js, postcode.js, wizard.js, search.js,
musicians.js, bands.js, messages.js, modals-shared.js.

KRITIEK: alle bestanden laden als gewone <script src="...">-tags, nooit
type="module". Functies moeten globaal (op window) bereikbaar blijven — de
HTML bevat tientallen onclick="functienaam(...)"-attributen die daarvan
afhankelijk zijn. Laadvolgorde: core.js en utils.js eerst, de rest daarna.

## actielijst.md
Staat in de repo (`actielijst.md`), wordt elke sessie zelf bijgewerkt en
gecommit.
