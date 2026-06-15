# Planet Under Pressure — digitale prototype

Een volledig lokale browser-versie van het coöperatieve klimaatcrisis-bordspel
(Pandemic-core), gebouwd op basis van:

- *Planet Under Pressure — Design Bible Pandemic Core V1*
- *Planet Under Pressure — Prototype Build Steps 1–9*
- *Planet Under Pressure — Complete Kaartenset v1* (Pressure / Crisis Surge / Escalation / Events / Immediate Legacy)
- `planet_under_pressure_stadkaarten.html` (visuele stijl: Barlow, donker thema, crème kaarten)

## Starten

Dubbelklik op **`index.html`** — geen server of installatie nodig.
(Optioneel: `python -m http.server` in deze map en open `http://localhost:8000`.)

2–4 spelers, hotseat (om de beurt achter hetzelfde scherm). Kies bij de start
het aantal spelers en de moeilijkheid (4 / 5 / 6 Escalation-kaarten).

## Wat erin zit

- **28 steden, 8 macroregio's** — de 24 kernsteden uit de kaartenset plus de 4 flex-steden uit de Design Bible (Anchorage, Addis Abeba, Bangkok, Manila; zij hebben geen eigen Pressure-kaarten en zijn dus relatief veilige knooppunten en extra stadkaarten voor doorbraken)
- **4 crisistypes**: 🔴 Klimaatimpact · 🟢 Ecologische druk · ⚫ Vervuiling · 🟡 Voedsel & land
- **Volledige beurtstructuur**: 4 acties → 2 spelerskaarten → Pressure-fase volgens escalatietempo
- **Alle 24 Pressure-kaarten** met compound-condities, **8 Crisis Surge-kaarten**, **5 events**, **12 Immediate Legacy-kaarten**
- **5 rollen** (kiesbaar bij de start, of willekeurig): Systems Scientist, Infrastructure Engineer, Transition Economist, Diplomat, en de Crisis Response Coordinator (de "Medic": behandelt alle blokjes van één kleur per actie; na een doorbraak verdwijnen die blokjes gratis waar hij staat of komt)
- **Regionale Tipping Points**: raakt een macroregio overbelast (totaal blokjes ≥ 2× het aantal steden), dan Cascade +1 en plaatsen Pressure-kaarten daar 1 extra blokje zolang de regio boven de drempel zit
- **Pressure-onthulling**: elke getrokken Pressure-kaart verschijnt als vormgegeven kaart (Canva-stijl: stadssilhouet in crisiskleur, effect, compound-waarschuwing) zodat iedereen ziet wat er gebeurt
- **Klik-om-te-verplaatsen**: klik direct op een verbonden stad (gestippelde ring) om te reizen, of op je eigen stad om te behandelen
- **5 gebouwen, elk met een uniek effect**:
  - 🔬 Research Hub — iedereen die er staat mag 1×/beurt de volgende Pressure-kaart bekijken
  - ⚡ Energy Transition Site — rood/zwart behandelen verwijdert er 1 extra blokje
  - 🌿 Ecosystem Recovery Zone — herstelt elke ronde automatisch 1 groen blokje (stad of buurstad)
  - 🛡️ Adaptation Center — vangt de eerste rode uitbraak op; schild herstelt bij elke Escalation
  - 🏛️ Economic & Social Hub — +1 economie per ronde (1×) en gratis sociale campagnes
- **Bewuste keuze: 28 steden, geen 48** — het Pressure-deck (24 kaarten, vaste stad↔type-koppeling) zou bij een Pandemic-formaat bord de helft van de steden nooit raken; 28 houdt elke regio relevant en het potje playtest-baar (~60–90 min)
- **4 doorbraken (cures)** met gebouw-, regio-, economie- en acceptatie-eisen
- **Economische capaciteit** (start 4) en **sociale acceptatie** (start 2, schaal 0–6) als timinglagen

## Meerdere escalatiepaden (bewust ontwerpdoel)

1. **Escalation-kaarten** — Increase → Infect → Intensify → Surge → Legacy
2. **Crisis Surge-deck** — verrassingskaart bij elke Escalation, hard als de stad al belast is
3. **Immediate Legacy** — blijvende wereldveranderingen vanaf Escalation III (one-shot legacy, geen stickers)
4. **Tipping points uit bordstatus** — Arctisch kantelpunt (10+ rood) en Drought & Famine Spiral (4+ geel in Zuid-Azië/Sub-Sahara)
5. **Compound-effecten** op Pressure-kaarten (druk versterkt druk)
6. **Uitbraak-cascades** — 4e blokje laat een stad uitbarsten naar alle buren

## Win / verlies

**Winst**: alle 4 doorbraken. **Verlies**: Cascade Track vol (8) · een blokjesvoorraad
op · Player Deck op · economie 0 op het moment van een Escalation.
Uitbraken zijn — zoals in Pandemic — geen game-over maar verspreiding: er ontstaat
méér om op te lossen, en pas een volle Cascade Track betekent verlies.

## Bewuste afwijkingen t.o.v. de papieren documenten

| Afwijking | Reden |
|---|---|
| Doorbraken kosten **4 stadkaarten** (3 met passende rol) i.p.v. 5 | Met slechts 24 stadkaarten in het hele spel is 4×5 wiskundig vrijwel onhaalbaar; het designdoc noemt "1 cure met 4 kaarten" zelf als tuningknop |
| ⚫ Zwart krijgt treat-all via 🌍 Mondiale Samenwerking | Mechanische symmetrie: elk cube-type heeft één doorbraak die hem "geneest" |
| Arctisch tipping point bij **10+** rood (doc: 4+) | 4+ triggert al bij setup; 10+ geeft mid-game spanning |
| Shuttle werkt tussen **alle** gebouwen | Eén duidelijke regel i.p.v. per-gebouw-uitzonderingen in de digitale UI |
| Alle 5 gebouwtypes direct beschikbaar (prototype-doc: eerst 3) | De cures voor Ecologisch Herstel en Mondiale Samenwerking vereisen de 2 extra gebouwtypes |
| Blokjesvoorraad 24 per kleur (Pandemic-verhouding) | Playtest: 15–18 liep te snel leeg — je verloor al rond de eerste uitbraak op voorraad i.p.v. op de Cascade Track |

Alle overige waarden (escalatietempo 2-2-3-3-3, startinfectie 3/2/1×3, handlimiet 7,
startkaarten 4/3/2, economiestart 4, acceptatiestart 2, max 6 gebouwen) volgen de documenten.

## Bediening & UI

- **Startscherm** legt doel, beurtstructuur, blokjes/uitbraken en alle indicatoren uit (groen ▲ = goed, rood ▲ = slecht)
- **Rechtsklik** op een actieknop of handkaart → uitleg van die actie/kaart
- **Scroll** op de kaart = zoomen, **slepen** = verschuiven, knoppen rechtsboven = zoom/reset
- Crisisblokjes liggen als **fysieke stapels** op de steden; een stapel van 3 krijgt een witte rand (kritiek — volgende blokje = uitbraak); hover voor uitleg; legenda linksonder op het bord
- Pressure-inslagen en uitbraken krijgen een **flits-effect** op de kaart
- **Adviseur** in het spelerspaneel geeft elke beurt één contextuele tip (kritieke steden, haalbare doorbraken, wanneer campagne/diplomatie zinvol is)
- **De-escaleren op afstand**: protest verwijderen kan ook in elke andere stad zolang je zelf in een stad met gebouw staat

## Bronnen / licenties

- Wereldkaart: Natural Earth 110m land (publiek domein), equirectangulair geprojecteerd; steden staan op hun echte coördinaten
- UI-iconen: [Lucide](https://lucide.dev) (MIT), inline embedded — geen netwerk nodig
- Fonts: Barlow / Barlow Condensed via Google Fonts (enige online resource; spel werkt ook zonder, met fallback-font)
