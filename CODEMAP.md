# CODEMAP — Planet Under Pressure

Doel van dit bestand: snel begrijpen hoe `index.html` in elkaar zit **zonder het hele
bestand te lezen**. Werkwijze die tokens spaart:

1. Lees dit bestand.
2. Zoek met **Grep** op de sectie-banner of functienaam hieronder (bijv. `/* ── PRESSURE RESOLUTIE`).
3. Lees alléén dat stuk met **Read** (offset/limit).

Alles zit in één bestand: `index.html` (CSS + HTML + JS inline, geen build, geen server nodig).
Regelnummers hieronder zijn indicatief en schuiven bij edits — **grep op de banner-tekst** is stabieler.

---

## Secties (banner-comments, in volgorde)

| Banner (grep hierop) | Wat |
|---|---|
| `/* ── TOPBAR/LAYOUT/BOTTOM ── */` | CSS layout |
| `/* ── SVG board ── */` | bord-CSS (citydot, cube-stacks, regionlabel) |
| `/* ── PRESSURE-KAART ONTHULLING` | CSS van de Canva-stijl pressure-kaart |
| `/* ── MODAL ── */` (CSS) en `/* ── MODAL (promise-based` (JS) | modale dialogen |
| `/* ── ROLKAART ── */` / `/* ── STADSDOSSIER ── */` | CSS + JS van storytelling-popups |
| `/* ── SETUP ── */` | CSS van het startscherm |
| `/* ── STATISCHE DATA ── */` | `CITIES`, `CITY`, `CITYLORE`, `NODEC`, `LAND_PATH` |
| `/* ── ICONEN ── */` | `ic(naam,size)` → inline Lucide SVG-strings |
| `/* ── PRESSURE DECK ── */` | `PRESSURE` (24) → `PDEF` lookup |
| `/* ── CRISIS SURGE DECK ── */` | `SURGE` (8) → `SDEF` |
| `/* ── EVENT CARDS ── */` | `EVENTS` (5) → `EDEF` |
| `/* ── IMMEDIATE LEGACY DECK ── */` | `LEGACY` (12) → `LDEF` |
| `/* ── CONSTANTEN ── */` | alle tuningknoppen (zie onder) |
| `/* ── STATE ── */` | `nieuwState()` — vorm van `S` |
| `/* ── HULPFUNCTIES ── */` | `shuffle, cube, regioSteden, totaalCubes, curP, rolVan, …` |
| `/* ── TRACK-WIJZIGINGEN ── */` | `wijzigEcon/Sociaal/Cascade/Warming` |
| `/* ── CUBES & OUTBREAKS ── */` | `plaatsCubes, uitbraak, verwijderCube` |
| `/* ── AUTO-LEGACY / TIPPING POINTS ── */` | regio-overbelasting, Arctisch, Famine |
| `/* ── PRESSURE RESOLUTIE ── */` | `resolvePressure, trekPressure, compoundGemuted` |
| `/* ── ESCALATION ── */` | `resolveEscalation` (Increase→Infect→Intensify→Surge→Legacy) |
| `/* ── WIN / VERLIES ── */` | `winst(), verlies()` |
| `/* ── SETUP ── */` (JS) | `bouwDecks(), startSpel()` |
| `/* ── BEURTVERLOOP ── */` | `eindeActies, handlimiet, volgendeSpeler` |
| `/* ── ACTIES ── */` | alle `actieX()`-functies + `cureCheck/cureKaartEis` |
| `/* ── EVENTS SPELEN ── */` | event-kaarten uitspelen |
| `/* ── STAD AANKLIKKEN ── */` | `klikStad()` (modes + direct klikken) |
| `/* ── RENDER ── */` | `render, renderBoard, renderSidebar, renderBottom, adviseur` |
| `/* ── ACTIE-UITLEG ── */` | `ACTION_INFO` dict + rechtsklik-handlers |
| `/* ── BOARD CLICK + ZOOM/PAN ── */` | klik/rechtsklik op bord, wheel/drag zoom |
| `/* ── REGELS ── */` | `REGELS_HTML` (volledige spelregels-modal) |
| `/* ── SETUP UI ── */` | startscherm-logica, rolkeuze, `btnstart` |

---

## State-object `S` (zie `nieuwState`)

```
over          null of verlies/winst-reden (truthy = spel voorbij)
diff          aantal Escalation-kaarten (4/5/6 = makkelijk/standaard/expert)
round         rondenummer
phase         'acties' | 'trekken' | 'pressure'
economie      0..ECON_MAX (start 6)
sociaal       0..SOC_MAX (start 2)
cascade       0..CASCADE_MAX (doodsmeter; vol = verlies)
escN          aantal omgeslagen Escalations (bepaalt pressure-tempo via RATES)
warming       0..WARMING_MAX — Vastgelegde Opwarming (klimaatinertie)
warmingHalt   bool — true na Schone Energie-doorbraak (stopt stijging)
investeringen [{rondes,econ,warming}] — transitie-payoffs die per ronde uitbetalen
_redAmp       bool — is de opwarming-amplificatie deze pressure-fase al toegepast
supply        {ROOD,GROEN,ZWART,GEEL} resterende blokjes (op = verlies)
steden        per stad: {cubes:{4 types}, gebouw:letter|null, protest:int, adaptUsed?}
players       [{naam, rol, stad, hand:[], kleur, usedPeek, usedDiplomat, usedEconShield, usedFreeBuild}]
cur           index huidige speler
actions       resterende acties deze beurt (start 4)
playerDeck/playerDiscard      handkaarten-deck (steden + events + ESCALATION)
pressureDeck/pressureDiscard  Pressure-deck (de tegenstander)
surgeDeck                     Crisis Surge-kaarten
legacyDeck/legacyActive       [{id, regio?, steden?, expires?, used?}]
cures         {energie,eco,sociaal,governance} bool — alle 4 = winst
laatstePressure  laatst getrokken pressure-kaart-id (voor sidebar)
```

`UI = {mode, modeData, peeked, suppressClick, flash}` — `mode` stuurt wat `klikStad` doet
('move','shuttle','charter','ev-*' enz.); `null` = direct klikken (verplaatsen/behandelen).

---

## Tuningknoppen (sectie CONSTANTEN)

```
SUPPLY_START   30   blokjes per kleur (was 24 → kon nog op raken; outbreaks + warming-amp
                    verbruiken vooral rood/zwart snel. Cascade hoort de doodsoorzaak te zijn)
CASCADE_MAX    8    cascade vol = verlies
HAND_LIMIT     7
ECON_MAX 8, SOC_MAX 6
START_STAD     'Amsterdam'
RATES          {4:[2,2,3,3], 5:[2,2,3,3,3], 6:[2,2,3,3,3,4]}  pressure-kaarten/beurt per escN
economie start 6  (in nieuwState; verhoogd van 4 — was te krap)
sociaal start  2
regioDrempel   3 × aantal steden + 2  (regio-overbelasting; functie in AUTO-LEGACY)
               overbelasting legt max 1 extra blokje PER pressure-fase (S._overloadDone),
               en ontwapent automatisch zodra de regio weer onder de drempel zakt

— Vastgelegde Opwarming —
WARMING_MAX        10
WARMING_RISE_AT    8    ≥ zoveel ROOD+ZWART op bord aan rondeafsluiting → +1 opwarming
WARMING_TIER       3    elke 3 punten = +1 extra 🔴 op de 1e rode pressure-kaart/fase

— Transitie-investering (Investeer-actie) —
INVEST_COST 2, INVEST_RONDES 3, INVEST_ECON 1, INVEST_WARMING 1
```

---

## Spelloop (kerncyclus)

`eindeActies()` (BEURTVERLOOP): zet phase→trekken → trek 2 handkaarten (ESCALATION resolvet
direct via `resolveEscalation`) → `handlimiet` → phase→pressure, **`S._redAmp=false`** → trek
`rate` pressure-kaarten, elk via `toonPressureKaart` (onthulling) + `resolvePressure` → `volgendeSpeler`.

`volgendeSpeler()`: reset per-beurt-vlaggen, `S.cur=(cur+1)%n`. Bij wrap naar 0 = **rondestart**:
`round++`, gebouw-effecten (H:+1 econ, N:herstel groen), **investeringen betalen uit**
(per investering +econ/−warming, rondes--, verwijder bij 0), **opwarming stijgt** als
`!warmingHalt && boardRoodZwart()>=WARMING_RISE_AT`, verlopen legacies opruimen. Daarna `actions=4`.

`resolvePressure(id)`: compound vóór primair → plaats blokje(s). Extra blokjes door:
Famine Spiral (geel), regio-overbelasting, en **Vastgelegde Opwarming** (1e rode kaart/fase
krijgt +`warmingTier()`). `plaatsCubes` → bij 4e blokje `uitbraak` (Cascade+1, spreidt naar buren).

`resolveEscalation()`: ① Increase (tempo; Esc II+ geeft −1 sociaal) ② Infect (onderste kaart,
3 blokjes) ③ Intensify (discard terug bovenop) ④ Crisis Surge ⑤ Immediate Legacy (Esc III+).
Adaptation Center-schilden herstellen hier. (Opwarming-amplificatie geldt NIET voor Infect/Surge.)

---

## Klimaat-mechaniek (korte vs. lange termijn)  ← nieuw

- **Vastgelegde Opwarming** = inertie. Stijgt bij verwaarlozing (veel rood/zwart op bord).
  **Niet** wegbehandelbaar. Per tier (3 punten) legt de 1e rode pressure-kaart/fase 1 extra 🔴.
  Geen aparte verliesconditie — het is een amplificator (maakt een verwaarloosd bord organisch
  zwaarder), bewust géén nieuwe doodsmeter (game was al aan de zware kant).
- **Investeer-actie** (`actieInvesteren`): bij een gebouw, kost INVEST_COST econ nu; pusht een
  payoff die INVEST_RONDES ronden lang +econ/−warming geeft. Co-benefit: klimaatactie verdient terug.
- **Schone Energie-doorbraak** (cureId `energie` in `actieDoorbraak`): zet `warmingHalt=true`,
  −3 warming direct, +2 econ (transitie-boom).
- UI: 5e track "Opwarming" in `renderSidebar` (tracksBox) + `.invest-row` met lopende payoffs.
  Knop `investeer` in `renderBottom`; uitleg in `ACTION_INFO.investeer`. Tip in `adviseur`.

---

## Rollen, gebouwen, cures, decks (waar te vinden)

- **ROLLEN** (5): scientist/engineer/economist/diplomat/responder. Velden: `naam, kleur, titel,
  desc` (vaardigheid), `bio` + `flavor` (achtergrondverhaal → `toonRolKaart`).
- **GEBOUWEN** (5): R Research Hub · E Energy Transition Site · N Ecosystem Recovery Zone ·
  A Adaptation Center · H Economic & Social Hub. Elk uniek effect; `MAX_GEBOUWEN=6`.
- **CURES** (4): energie/eco/sociaal/governance. Eisen `cureCheck`, kaartaantal `cureKaartEis`
  (4, of 3 voor de bijpassende specialist), validatie `cureKaartenGeldig`.
- **CITYLORE**: 28 documentaire-toon stadsprofielen (naast `CITIES`). Gebruikt in `toonStadDossier`
  (rechtsklik op stad → profiel + live situatie uit bordstatus).

---

## Quirks / valkuilen

- **`preview_screenshot` time-out** in deze omgeving: verifieer met `preview_eval` (DOM-queries),
  niet met screenshots.
- **Modal-wachtrij**: `showModal` is promise-based met queue (`_modalQueue`,`_pumpModal`) tegen
  race conditions tijdens escalatie-kettingen. Niet vervangen door inline promises.
- **Pressure-animatie**: 250 ms/kaart (langzamer → eval-tests time-outen bij multi-kaart).
- **Grep & speciale tekens**: het bestand bevat `</b>` letterlijk; bij Edit exact kopiëren.
- **Pacific-routes** (Vancouver/Anchorage ↔ Tokio) worden apart als rand-lijnen getekend in `renderBoard`.

---

## Deploy

Static single-file → **GitHub Pages**. Repo `FabianB88/planet-under-pressure`, branch `master`,
Pages = branch master / root. Live: https://fabianb88.github.io/planet-under-pressure/
Updaten: `git add index.html && git commit -m "…" && git push` (Pages herdeployt vanzelf ~1 min).
Lokaal testen: `.claude/launch.json` start `python -m http.server 8765` (preview-naam `planet-under-pressure`).
