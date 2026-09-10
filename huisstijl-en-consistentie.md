# The Talent Tent — Huisstijl en consistentie

**Aangemaakt:** 22-08-2026, op verzoek van Ronald: "consistentie is belangrijk."
**Status:** vastlegging van bestaande én nieuwe regels, geen losse ideeënlijst.
**Meesturen bij elke sessie die het uiterlijk van de app raakt** (net als
`zoekfunctienaslagwerk.md` en `niveaubepaling-naslagwerk.md` voor hun eigen
onderwerp).

**Waarom dit document er nu komt:** tot vandaag stonden huisstijlregels
verspreid — een deel in de projectinstructies, een deel alleen in de code
zelf, een deel alleen in Ronalds hoofd. Bij de gele-lijn-vraag (22-08-2026)
bleek dat drie vergelijkbare schermen (muzikantprofiel, eigen profiel,
bandprofiel) toevallig, niet met opzet, hetzelfde patroon deelden. Dit
document maakt "hetzelfde patroon" een regel, geen toeval.

**Werkwijze voortaan:** vóór het bouwen van een nieuw scherm of onderdeel,
dit document naast een vergelijkbaar bestaand scherm leggen. Wijkt het af,
dan is dat een bewuste keuze die hier ook bij komt — niet een nieuw eigen
patroontje.

---

## 1. Kleuren

| Naam | Waarde | Gebruik |
|---|---|---|
| `--bg` | `#0d0d0d` | Paginaondergrond |
| `--surface` | `#161616` | Kaarten, panelen, modals |
| `--surface2` | `#1e1e1e` | Een laag boven `--surface` (bijv. invoervelden, hover) |
| `--border` | `#2a2a2a` | Randen, scheidingslijnen |
| `--accent` | `#f5c518` (goud) | Merkkleur — knoppen, actieve status, nadruk |
| `--accent2` | `#6ec8d8` (cyaan) | **Alleen** genre/gezocht-badges — nooit voor gewone nadruk |
| `--danger` | `#e5533d` | Onomkeerbare/destructieve acties (verwijderen) |
| `--success` | `#4caf50` | Geslaagd/actief (bijv. "Deze week bijgewerkt") |
| `--text` | `#f0f0f0` | Gewone tekst |
| `--muted` | `#888` | Bijschriften, minder belangrijke tekst |

**Regel:** een individueel profiel (muzikant of band) mag een eigen
`profile_color` hebben — die kleur vervangt dan `--accent` binnen dát
profiel (balk, badges, avatarrand). Overal daarbuiten (knoppen, navigatie,
systeemmeldingen) blijft `--accent` het merkgoud, nooit de profielkleur van
wie er toevallig is ingelogd.

---

## 2. Typografie

- **Tekst:** Roboto (Regular/Bold/Italic).
- **Display/woordmerk:** `Alfa Slab One` — **uitsluitend** voor `.logo`.
  Tijdelijk, in afwachting van het eigen letterontwerp (huisstijlblad
  "PROTO2 – PERFORMANCE"). Zodra dat er is: alleen de CSS-variabele
  `--font-display` wijzigt, `.logo` blijft de enige gebruiker.
- **Paneeltitels** (`.landing-title`, `.panel-title`, `.filter-title`,
  `.band-name`): Roboto Bold, niet Alfa Slab One — die bleek te
  zwaar/onleesbaar op kleinere schaal (Ronald, 06-08-2026).
- **Basisformaten:** `--fs-sm` 12px · `--fs-md` 15px (invoervelden altijd
  16px, zie §7) · `--fs-lg` 28px · `--fs-display` clamp(40px, 7vw, 72px)
  (mobiel vast op 28px).

---

## 3. Afstand — 4px-schaal (TT-114, vastgelegd 19-08-2026, nog niet auditmatig doorgevoerd)

Elke marge, opvulling en afstand is een veelvoud van **4px** (4, 8, 12, 16,
20, 24, 32...). Dit is al grotendeels de praktijk in de bestaande code; deze
regel maakt het uitgangspunt expliciet voor nieuw werk. Een volledige audit
van bestaande afwijkingen is nog niet gedaan — dat is een aparte, eigen
opschoningsronde, geen onderdeel van losse feature-sessies.

**Vaste marges op mobiel (≤560px), zodat de kop en de pagina-inhoud
uitlijnen (22-08-2026):**
- Linker/rechter marge van de koprij én van elke pagina-inhoud: **16px**.
- Geen enkel scherm krijgt een eigen, afwijkende zijmarge zonder reden.

**Formuliergroep-afstand (TT-192, 04-09-2026) — eerste auditmatig
doorgevoerde toepassing van de TT-114-schaal, nog niet app-breed:**
- Tussen twee `.field-group`-blokken (een label + veld, of een label +
  tag-groep) is de afstand altijd **20px**. Vastgelegd in de klasse zelf
  (`.field-group { margin-bottom: 20px }`), **nooit via een inline
  `style="margin-top/bottom:...px"`** op één specifiek blok.
- Label → veld blijft **8px** (`.field { gap: 8px }`), ongewijzigd.
- Aanleiding: zes plekken in de aanmeldwizard en het profiel-bewerken-
  duplicaat weken af (16px/24px door elkaar, geen patroon) — gevonden bij
  een audit op verzoek van Ronald, hersteld naar de bestaande 20px-regel.

**Knoppenrij-tussenruimte (TT-228, 09-09-2026):** `.action-row` ging van
10px naar **8px**, gelijk aan `.btn-row`. 10px was geen veelvoud van 4.

**Inspringing van tekst in een formulier (`--field-inset`, TT-233,
10-09-2026):** elke tekstregel in een formulier begint op **8px** vanaf de
linkerrand van het veld — gelijk aan `--radius-field`, dus precies waar de
ronding overgaat in de rechte rand. Dat geldt voor het label, de hulptekst
eronder én de tekst in het veld zelf. Elk veldtype had hiervoor zijn eigen
maat (invoerveld 16px, keuzeveld 14px, label 0px), waardoor geen enkele regel
uitlijnde. Gebruik altijd `var(--field-inset)`, nooit een los getal.

**Open punt, nog niet vastgelegd:** `.panel-title` is standaard 6px
margin-bottom. Volgt er geen `.panel-sub`, dan gebruiken "Over ons" en
"Instellingen" een inline 20px — maar "Privacyverklaring",
"Gebruiksvoorwaarden" en "Gedragscode" (ook zonder `.panel-sub`) hebben een
overbodige inline 6px. Geen vastgelegde regel voor deze uitzondering; keuze
ligt bij Ronald.

---

## 4. Ronde hoeken

Geen volledig uniforme waarde (nog niet geaudit), maar wel een consistent
gebruikspatroon:

| Element | Waarde |
|---|---|
| Kaarten/panelen (`--radius`) | 12px |
| **Velden (`--radius-field`)** | **8px** |
| Knoppen (`.btn`) | 8px |
| Modals (`.modal-box`) | 16px (0 op mobiel, volledig scherm) |
| Badges/chips/pillen | 20px of `999px` (volledig rond) |
| Ronde iconen/avatars | `50%` |

**`--radius-field` geldt voor élk veld** (TT-233, 10-09-2026): invoerveld,
keuzeveld, wielveld en lijstoptie in een bladwijzer. Eén variabele, zodat twee
velden naast elkaar nooit een andere ronding krijgen. Nooit een losse waarde
in een regel zetten.

---

## 5. Knoppen

- **Primair** (`.btn-primary`): gouden achtergrond, zwarte tekst — één per
  scherm de duidelijk belangrijkste actie.
- **Ghost** (`.btn-ghost`): transparant met rand — secundaire acties.
- **Destructief** (verwijderen e.d.): `.btn-ghost` met `color`/`border-color`
  op `--danger`. **Nooit** even zwaar in het beeld als de primaire actie op
  hetzelfde scherm — bij twijfel achter een menu (zie §8).
  - **Uitzondering, vastgelegd 06-09-2026 (TT-191):** de tegel "Account
    verwijderen" in Instellingen toont de titel in de gewone tekstkleur
    (`--text`), niet in `--danger` — op verzoek van Ronald, die dit scherm
    niet alarmerend wil laten aanvoelen. Dit is een beperkte, op zichzelf
    staande uitzondering voor déze ene plek, geen nieuwe algemene regel.
    Bij elk volgend voorstel om een destructieve actie niet in `--danger`
    te tonen: eerst navragen bij Ronald, nooit automatisch toepassen op
    basis van dit precedent.
- Minimale hoogte **44px**, ongeacht knoptype (zie §6).

### Volgorde en formaat in een knoppenrij (TT-228, 09-09-2026)

- Staan twee knoppen **naast elkaar**, dan staat de **secundaire links** en
  de **primaire rechts**. Zonder uitzondering, ook bij een destructieve
  primaire actie ("Terug" links, "Account verwijderen" rechts).
- Beide knoppen in zo'n rij zijn **exact even breed**.
- Gebruik daarvoor `.btn-row` (of `.action-row`, met een extra marge
  erboven). **Nooit** een eigen inline `style="display:flex;gap:..."` met
  `flex:1` op de knoppen.
- **Waarom geen `flex:1`, geverifieerd met een meting:** `.btn-ghost` heeft
  een rand van 1px, `.btn-primary` niet. `flex-basis: 0` kan niet kleiner
  worden dan rand plus opvulling, dus beide knoppen beginnen 2px uit elkaar
  en die 2px blijft staan bij het verdelen van de resterende ruimte —
  gemeten 175px tegen 173px op een scherm van 390px. TT-207 (04-09-2026)
  probeerde dit met `min-width: 0` en nam het verschil níét weg. Beide
  klassen gebruiken nu `display: grid` met `grid-auto-flow: column` en
  `grid-auto-columns: 1fr`; een `1fr`-kolom is altijd exact even breed,
  ongeacht rand of tekstlengte.
- Knoppen die **ónder elkaar** op volle breedte staan (bijv. "Stuur een
  bericht" boven "Deel dit profiel") vallen buiten deze regel. Volle
  breedte is op een telefoon prettiger dan twee halve knoppen — zie
  `app-first-toetslijst.md` punt 1.

---

## 6. Tikdoelen en toegankelijkheid

- Elk klikbaar element is minimaal **44×44px** (Apple) / **48dp** (Google) —
  ook het onzichtbare tikvlak rond een klein icoon (bijv. `::after` met
  negatieve `inset`).
- Toelichtings-/infoknoppen (`.niveau-info-btn`) zijn **standaard al** in de
  accentkleur, niet pas bij hover — op een telefoon bestaat geen hover
  (22-08-2026, punt 7 uit de vorige review).
- **Sluit-/terugknop (`.modal-close`/`.modal-back`), vastgelegd 07-09-2026
  (TT-223):** de **zichtbare** cirkel is 33×33px (25% kleiner dan het
  tikdoel), niet 44×44px. Het **tikdoel** blijft wél gewoon 44×44px, via een
  onzichtbaar `::after`-vlak van 5,5px rondom de zichtbare cirkel — zelfde
  patroon als `.niveau-info-btn::after`. Icoon-lettergrootte 14px (was
  18px, evenredig meegeschaald). **Regel voor nieuwe kleine cirkelknoppen:**
  een kleinere zichtbare knop mag altijd, zolang het tikdoel via dit
  `::after`-patroon op 44×44px blijft — nooit de zichtbare knop zelf naar
  onder de 44px optrekken zonder die onzichtbare marge.
- **Bekend, nog openstaand:** de items in een uitklapmenu (`.nav-menu-item`)
  zijn 39-40px hoog, net onder de norm. Meerdere keren gesignaleerd, nog niet
  gerepareerd — klein genoeg voor een aparte sessie.

---

## 7. Formuliervelden

- Minimaal **16px** lettergrootte — kleiner laat iOS Safari inzoomen bij
  focus. Dit geldt specifiek voor invoervelden, `--fs-md` (15px) zelf blijft
  voor overige tekst.
- Juiste `autocomplete`-attributen overal (`email`, `new-password`,
  `current-password`, `given-name`, `family-name`) — nodig voor
  wachtwoordmanagers en browser-autofill.
- Een bekend/eerder gebruikt gegeven (bijv. laatst gebruikt e-mailadres) mag
  het veld alvast vullen — voorkomt dat de eigen autofill-balk van de
  browser het hele toetsenbordgebied bedekt (TT-122).

### 7.1 Wiel-keuze — het draaiwiel (vastgelegd 10-09-2026, TT-233)

Subsectie onder §7, geen nieuwe hoofdsectie: §8 t/m §14 worden elders bij
nummer aangehaald, hernummeren breekt die verwijzingen.

Het draaiwiel is een **vaste component van de huisstijl**, naast knop, chip,
tikveld en pil.

#### Wanneer wel, wanneer niet

| | Component |
|---|---|
| Getal of niveau uit een **vaste, geordende reeks** (straal, leeftijd, niveau, frequentie) | **Wiel-keuze** |
| Keuze uit een **ongeordende lijst** waar je op zoekt (instrument, genre, plaats) | Tikveld + keuzelijst met zoekveld |
| Korte, ongeordende lijst zonder zoekveld (Sorteren op, Weergave) | **Lijst in dezelfde bladwijzer** |
| Aan/uit, of maximaal drie standen | Chip of pil, direct op het scherm |
| Vrij getal zonder zinnige stappen | Bestaat niet in deze app — kies stappen |

Een wiel staat **nooit vast open in een formulier**. Het is een keuzelaag die
opkomt en weer weggaat. Het formulier toont alleen het tikveld met de gekozen
stand in woorden.

#### De beweging — één duim, drie tellen

De norm waaraan het wiel wordt afgemeten. De gebruiker houdt de telefoon in
één hand en doet alles met zijn duim:

1. **Duim tikt het veld.** De bladwijzer komt binnen 200 ms van onderen op.
   Het midden van het wiel ligt in de onderste helft van het scherm, nooit
   erboven.
2. **Duim veegt.** Het wiel loopt mee, blijft doorlopen na loslaten, en klikt
   vast op de dichtstbijzijnde regel.
3. **Duim tikt de waarde.** De waarde wordt gekozen en de bladwijzer gaat weg.
   Geen bevestigingsknop ertussen.

**Sluiten hoort bij kiezen.** Een wiel met **één kolom** sluit op de tik die
de waarde kiest. Een wiel met **twee kolommen** (een bereik) sluit daar niet
op — de keuze is pas af als beide kolommen staan. Dat wiel sluit bij een tik
buiten de bladwijzer, een veeg omlaag, of "Gereed". Elke wijziging is op dat
moment al verwerkt; "Gereed" bevestigt niets, het sluit alleen.

Een bladwijzer sluit verder altijd bij: een tik op de verduistering, een veeg
omlaag over de greep, Escape, en de terugknop van het toestel — dat laatste
conform de geschiedenisregel (de terugknop sluit eerst een open laag, dan de
vorige view).

#### Maten en opmaak

| Onderdeel | Waarde |
|---|---|
| Regelhoogte | **44px** — gelijk aan het tikdoel uit §6, geen `::after`-truc nodig |
| Zichtbare regels | **5** (twee boven, gekozen, twee onder) |
| Paneelhoogte | **220px** (5 × 44) |
| Paneel | `--surface2`, rand `--line` in `--border`, hoeken `--radius` (12px) |
| Markeringsbalk | goud op 10% dekking, lijn boven en onder op 35%, hoeken 8px, over de volle breedte van het paneel |
| Vervaging | verloop naar `--surface2` van 0-34% en 66-100% |
| Lege regels boven/onder | 2 × 44px, zodat ook de eerste en laatste waarde in het midden kan staan |
| Kolombreedte | vast: getal 72px, koppelwoord 44px, eenheid 52px |
| Plaatsing kolommen | **gecentreerd** in het paneel, als één blok |
| Bladwijzer | `--surface`, hoeken 16px alleen boven, greep 36×4px in `--border`, 16px opvulling |
| Verduistering | zwart op 55% |

**Kolommen horen in één paneel.** Een bereik is één keuze: één paneel, twee
kolommen, één markeringsbalk erover — nooit twee losse panelen naast elkaar.
De eenheid ("km", "jaar") en het koppelwoord ("t/m") zijn **vaste kolommen in
het paneel**, geen losse woorden ernaast.

**De kolommen staan gecentreerd.** Ze krijgen een vaste breedte en vormen
samen één blok in het midden van het paneel; ze rekken niet uit over de volle
breedte. Reden: de meeste gebruikers bedienen de app met hun rechterduim, en
kolommen tegen de linkerrand dwingen tot het verleggen van de telefoon. De
markeringsbalk loopt wél over de volle breedte — die markeert de regel, niet
de kolom.

**Een browser-keuzelijst (`<select>`) wordt nooit zichtbaar gebruikt.** De
uitklaplijst tekent het besturingssysteem, niet de pagina: hij is niet af te
ronden en niet te animeren, en hij breekt daarmee het beeld van elk scherm
waar hij op staat. Gebruik in plaats daarvan een lijst in de bladwijzer, met
`--radius-field` per rij en de gekozen rij in `--accent` met een `✓`. Het
`<select>` blijft verborgen in de HTML staan als bron van waarheid, zodat
bestaande code die `.value` leest of zet ongewijzigd blijft werken.

**Het wiel staat nooit naast een ander wiel.** Een bladwijzer toont één wiel
op volle schermbreedte. Twee wielen naast elkaar passen niet: op een scherm
van 360px is er per vak 158px, en een leeftijdswiel met vier kolommen heeft
190px nodig. De vélden mogen wel naast elkaar (§7.2).

#### Teksten

- De **uit-stand** heet **"Geen"**, bovenaan het wiel én in het gesloten veld.
- Het **tikveld** toont de stand in woorden: "Geen", "25 km", "3 t/m 5".
  Nooit een kaal cijfer zonder eenheid.
- Onder het wiel staat een **terugleesregel** die het bereik in woorden
  herhaalt en, waar dat kan, het aantal treffers noemt: "18 t/m 30 jaar ·
  41 muzikanten". Vaste hoogte, zodat de knoppen eronder niet verspringen.
- Een tikveld met een actief filter krijgt een **gouden rand**.

#### Uitzondering op §9

§9 zegt: op mobiel is elke modal volledig scherm. **Een wiel-bladwijzer is
dat niet.** Een keuzelaag van circa 300px is geen scherm; volledig maken
haalt de vloeiendheid weg en verbergt de context waar de keuze bij hoort.
Bewuste afwijking, alleen voor de wiel-keuze.

#### Techniek — vast, niet vrij te kiezen

- Vastklikken met `scroll-snap-type: y mandatory` en `scroll-snap-align:
  center`. Geen eigen berekening van de eindpositie: het toestel doet het
  doorrollen beter.
- De regelhoogte staat op **twee plekken** — `styles.css` en `WHEEL_ITEM_H`
  in `utils.js`. Die twee moeten gelijk blijven, anders leest het wiel de
  verkeerde waarde af.
- Een wiel wordt **pas opgebouwd of bijgesteld als het zichtbaar is.** Een
  verborgen element heeft geen hoogte, dus een gezette scrollpositie komt
  niet aan. Vermoedelijke oorzaak van de straal-fout van 09-09-2026 (wiel
  startte op 500 km in plaats van 5).
- Tijdens het scrollen wordt niet elke tussenwaarde verwerkt: pas als het
  wiel circa 140 ms stilstaat, telt de waarde.
- Toegankelijkheid: `role="listbox"` op het wiel, `role="option"` en
  `aria-selected` op de regels, `aria-label` per kolom, bediening met
  pijltoetsen, PageUp/PageDown, Home en End.
- `prefers-reduced-motion`: de bladwijzer verschijnt zonder schuifbeweging,
  het wiel springt zonder animatie naar de waarde.
- **Aanname, nog niet getest:** een korte tril per gepasseerde regel
  (`navigator.vibrate(8)`) versterkt het gevoel van vastklikken op Android.
  iOS Safari ondersteunt `navigator.vibrate` niet. Alleen inbouwen als het op
  een echt toestel merkbaar beter voelt.

### 7.2 Indeling van de filterrij (10-09-2026, TT-233)

- **Straal staat naast Plaats**, op dezelfde regel. Het straalveld is **96px
  vast** — genoeg voor "25 km" gecentreerd, met ruimte voor het pijltje
  rechts. Het groeit niet mee. Plaats krijgt de rest van de regel, want
  plaatsnamen verschillen sterk in lengte.
- **Leeftijd en Niveau staan naast elkaar**, twee gelijke kolommen met 12px
  ertussen (`1fr 1fr`, zelfde principe als een knoppenrij, §5).
- Onder 360px schermbreedte vallen Leeftijd en Niveau **onder elkaar**. De
  velden zelf blijven gelijk.
- **De waarde in een wielveld staat gecentreerd.** Het pijltje staat los
  rechts, buiten de opvulling, zodat de tekst in het midden van het véld
  staat en niet in het midden van de restruimte. Velden waar je in typt
  (Gebruikersnaam, Plaats) blijven **links uitgelijnd** — gecentreerde
  invoertekst springt tijdens het typen.
- **Geen toelichtingsregel onder de rij.** De betekenis van een niveau staat
  op de terugleesregel in het wiel en volledig achter de i-knop. Een derde
  plek met dezelfde uitleg maakt het scherm alleen langer.
- De balk **Muzikant · Band · Setlist** blijft ongewijzigd bovenaan het
  zoekscherm, boven de filtertitel.
- Een **i-knop in een label** staat rechts in de labelregel, op één lijn met
  de rechterrand van de tekst in het veld eronder — niet direct achter het
  woord. Patroon: `.label-with-info` met `justify-content: space-between` en
  `padding-right: var(--field-inset)`.
- Tussen twee blokken in het filterpaneel staat **20px**, de standaardmaat uit
  §3. Nooit een inline `style="margin-bottom:..."` op een blok apart.

---

## 8. Menu's — wanneer een actie achter een ⋯-knop hoort

**Regel:** een scherm toont **één** primaire actie direct. Alles wat minder
frequent is, of onomkeerbaar/gevoelig, staat achter een klein ⋯-menuknopje
ernaast — nooit als evenwaardige knop op dezelfde rij.

**Verwijderen is nooit een kaal kruisje (✕) los op een kaart of chip.**
Dat voelt willekeurig en onveilig — "alsof je ieder moment kan worden
gecancelled" (Ronald, 22-08-2026, over het kruisje bij bandleden). Een
verwijderactie hoort in een scherm waar die bewust wordt opgezocht (een
beheer-/bewerkscherm), met een volwaardige, duidelijk benoemde knop
("Verwijderen"), niet als terloops icoontje in een overzicht.

**Een bevestiging in twee stappen is altijd te annuleren (TT-226,
09-09-2026).** Wisselt een knop na de eerste klik van functie (bijv. ✕ →
"Zeker?", of "Terug" → "Terug zonder opslaan?"), dan zet een klik ergens
anders in de app hem terug naar de oorspronkelijke staat. Nooit een
gewapende knop die alleen te annuleren is door het scherm te verlaten. Vast
patroon: registreer de `document`-klikluisteraar pas ná de huidige klik
(`setTimeout(..., 0)`) en laat hem vanzelf verdwijnen (`{ once: true }`) —
zie `handleCancelClick()` en `jstArmOutsideCancel()`.

**Toegepast op (22-08-2026):**
- Hamburgermenu naast het woordmerk (Over ons, juridische pagina's, in-/uitloggen)
- Mijn Profiel, naast de naam (Profiel wijzigen, band-uitnodigingen aan/uit, Account verwijderen)
- Mijn Bands, elke bandkaart naast de naam — alleen zichtbaar voor de
  beheerder: **Bandprofiel bewerken · Bandleden wijzigen · Bandbeheer**
  (definitieve labels, Ronald 22-08-2026). Bij meerdere bands in de lijst
  heeft elke kaart zijn eigen knop/menu — gebruik dan geen vast `id`, maar
  `event.currentTarget`/`closest()` (zie `toggleBandMoreMenu()` als
  voorbeeld).

Vast patroon (gebruikt bij het hamburgermenu én het profielmenu):
```html
<div class="profile-actions-menu-wrap" style="position:relative;">
  <button class="nav-menu-btn" onclick="...">⋯-icoon</button>
  <div class="inline-menu-dropdown">
    <button class="nav-menu-item">Gewone actie</button>
    <button class="nav-menu-item" style="color:var(--danger);">Destructieve actie</button>
  </div>
</div>
```
Sluit bij een klik buiten het menu en bij Escape (zie `toggleNavMenu()` /
`toggleProfileMoreMenu()` / `toggleBandMoreMenu()` als voorbeeld).

---

## 9. Modals

- Op mobiel (≤560px) is elke modal **volledig scherm** (`100dvh`, geen
  ronde hoeken, geen kruisje-in-een-doosje) — een modal is op een telefoon
  een eigen scherm, geen dialoogvenstertje (TT-U25).
  - **Uitzondering, vastgelegd 10-09-2026 (TT-233):** de wiel-bladwijzer.
    Zie §7.1.
- **Verplicht:** de breedte van een modal wordt altijd via een **klasse**
  ingesteld (`.modal-box-sm` 400px / `.modal-box-md` 440px /
  `.modal-box-slg` 520px / `.modal-box-lg` 480px / `.modal-box-xl` 760px),
  **nooit** via een inline `style="max-width:...px"` op het element zelf.
  Een inline stijl wint altijd van de mobiele volledig-scherm-regel, ongeacht
  hoe die regel is geschreven — dat veroorzaakte TT-118 (zeven modals tegelijk
  te breed op een telefoon, 22-08-2026). Nieuwe breedte nodig? Voeg een
  nieuwe klasse toe, gebruik nooit een losse inline maat.
- De sluitknop staat rechtsboven, consistent door de hele app. Zichtbare
  maat en tikdoel: zie §6 (TT-223, 07-09-2026).

---

## 10. Profiel-/detailweergaves — de regel uit dit gesprek

**Een muzikantprofiel en een bandprofiel zijn hetzelfde soort scherm en
voelen hetzelfde**, ongeacht of het je eigen profiel is of dat van iemand
anders, en ongeacht of het in een modal staat of op een eigen pagina.

Vast patroon, boven aan elke profiel-/detailweergave:
1. **Kleurbalk** (`.profile-header-band`, 8px hoog) in de profielkleur
   (`profile_color`, terugval op `--accent`). Directe eerste regel van de
   inhoud — geen ruimte ervoor.
2. Direct daaronder: **foto/avatar + naam + kernmeta** op één regel
   (leeftijd/plaats voor een muzikant, plaats/genres voor een band).
3. Statusregel (bijv. "Deze week bijgewerkt", of de bandstatus-badge).
4. Badges (instrumenten/genres, of band-"wij zoeken").

**Belangrijke technische kanttekening (les uit de gele-lijn-fout,
22-08-2026):** de kleurbalk "bleedt" soms naar de rand van zijn eigen
container met een negatieve marge (bijv. `-32px`, exact gelijk aan de
opvulling van een `.modal-box`). Die marge **moet exact overeenkomen met de
opvulling van de container waarin hij op dat moment staat** — een modal
(32px opvulling) en een eigen pagina zoals `.my-profile-wrap` (andere
opvulling) hebben **niet dezelfde marge nodig**. Kopieer deze stijl dus
nooit letterlijk naar een nieuwe context zonder de opvulling van die
context na te meten. `buildMusicianDetailHTML()` regelt dit per context via
de parameter `inModal`; volg dat patroon bij nieuwe profielweergaves.

**Bewust nog niet gelijkgetrokken:** "Mijn Bands" (de lijst met eigen
bands) is geen profiel-/detailweergave maar een lijst met kaarten — die
krijgt bewust geen kleurbalk per kaart. Een lijst met meerdere volle
kleurbalken oogt rommelig, niet consistenter.

---

## 11. Navigatie

**Sinds TT-224 (07-09-2026) heeft de webapp dezelfde schil als de
telefoon-app op elke breedte.** De onderbalk staat er altijd, de bovenbalk
met tabs (`.app-nav`) is overal verborgen, en modals vullen het canvas.
Boven 560px staat de hele app in een canvas van 25% lucht — 50% app — 25%
lucht, gecentreerd (`#appRoot { width: 50% }`).

- **Onderbalk** (`app-bottom-nav`), vier vaste knoppen: Zoeken · Berichten ·
  Bands · Profiel. Altijd zichtbaar, duimbereik.
- **Kop:** woordmerk + hamburgermenu op één rij, verticaal gecentreerd,
  16px marge links/rechts (zie §3). De kop is **sticky** — blijft zichtbaar
  bij scrollen (bewust besluit 22-08-2026, keert TT-15 van 06-08-2026 terug).
- Hamburgermenu: minder-frequente links (Over ons, juridische pagina's,
  Inloggen/Uitloggen). Icoon 26px, knop 44×44px.
- **Kop, inhoud en onderbalk zijn altijd exact even breed** (TT-225,
  08-09-2026). Een paginawrapper (`main`, `.landing-wrap`, `.auth-wrap`,
  `.search-wrap`, `.my-profile-wrap`) heeft daarom géén eigen `max-width`,
  wel `width: 100%` — zonder die breedte krimpt hij naar zijn eigen inhoud
  binnen de flex-kolom van `.app-view.active`.

---

## 12. Iconen en illustraties

- **Geen emoji**, nergens in de UI — tekst-only of lijn-SVG-iconen.
- Lijn-iconen (`stroke="currentColor"`, geen vulling) voor navigatie en
  acties, consistent gewicht (`stroke-width="2"`).

---

## 13. Meldingen/banners (TT-115, vastgelegd 19-08-2026, nog niet gebouwd)

Nog geen gestandaardiseerde bannercomponent — op dit moment heeft elke
melding (band-uitnodiging, foutmelding, "nog niet geladen") zijn eigen,
losse opmaak. Vastgelegd als aandachtspunt voor de grote update: één
herbruikbare bannerstijl (kleur naar aanleiding van soort melding: neutraal/
succes/waarschuwing/fout), in plaats van steeds opnieuw uitgevonden opmaak.

---

## 14. Wat hier bewust buiten valt

Dit document gaat over **uiterlijk en interactiepatronen**, niet over
databasestructuur, matchinglogica of teksttoon (zie
`zoekfunctienaslagwerk.md`, `niveaubepaling-naslagwerk.md` en de
werkinstructies voor die onderwerpen).
