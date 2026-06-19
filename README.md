# GlowScan (werknaam) — Professionele AI-gezichtsanalyse

Instant, in de browser, **gemeten in echte millimeters** (geen LLM-giswerk) + verbeter-tips per kenmerk.
Positionering: het gat dat Qoves openlaat — zij $150/jr, 28 dagen, 6 foto's, klinisch; **wij seconden, €29, 1 selfie, viraal.**

## Draaien (web, geen build)
```bash
cd ~/glowscan && python3 -m http.server 8000
# open http://localhost:8000
```
> Moet via http/localhost (niet file://): MediaPipe laadt wasm + model van CDN.

---

## Wat je écht kunt meten (de eerlijke engineering)

**De doorbraak: de iris als liniaal.** Een menselijke iris is ~**11,7 mm** breed bij vrijwel iedereen (heel lage variantie). MediaPipe geeft iris-punten → we meten de iris in pixels → **mm per pixel** → alle afstanden worden **echte millimeters** (pupilafstand, gezichtsbreedte, mondbreedte…). Dit is wat "speelgoed" van "instrument" scheidt.

**Betrouwbaar vanaf een frontale foto:**
- Symmetrie (afwijking t.o.v. middellijn) · gezichtsderden · midface-ratio · fWHR
- Canthal tilt (ooghoek) · oogafstand · neusbreedte · kaak-taper · lipverhouding
- Absolute mm: pupilafstand (PD), gezichtsbreedte/-hoogte, mond-/neusbreedte

**Alleen vanaf profielfoto's (daarom multi-upload):**
- Kaakhoek (gonial) · nasolabiale hoek — *gemarkeerd als `beta`/indicatief*

**Eerlijke beperkingen (staan ook in de app):**
- MediaPipe is getraind op ~frontale gezichten → **profiel = minder nauwkeurig**. Daarom labelen we profielmetrics als indicatief. (Dit is precies waarom Qoves menselijke review inzet.)
- 2D-foto schat 3D-hoeken; fotohoek/lens vertekenen. Goed genoeg voor glow-up-richting, niet voor chirurgische planning.
- Ideaal-ranges (`iLo`/`iHi`) zijn benaderingen uit aesthetic-literatuur → kalibreren op echte data.

---

## Mag dit (juridisch/privacy)? — Ja, mits je deze rails houdt

- **Geen "bijzondere" biometrie.** Onder de AVG is gezichtsdata alleen *special category* als je 'm gebruikt om iemand **uniek te identificeren**. Esthetisch meten ≠ identificeren → lichter regime. Het blijft wél persoonsgegeven.
- **Je grootste schild: on-device.** De foto wordt **lokaal** verwerkt en **nergens geüpload/opgeslagen**. Geen server = nauwelijks AVG-blootstelling. Behoud dit.
- **Bij LLM-tips later:** stuur alleen de **cijfers** naar de backend, **niet de foto**. Cijfers zijn niet identificerend → foto blijft op het toestel.
- **Geen medische claims.** Geen diagnose/behandeling → geen medical-device-regulering (EU MDR). Frame cosmetisch/educatief + disclaimer (staat in de app).
- **18+ en consent.** Leeftijdsvinkje + privacy-melding vóór analyse (ingebouwd).
- **Geen totaalcijfer.** Alleen losse metrics → minder body-image/app-store-risico.
- **Vermijd:** foto's serverside bewaren · marketing naar minderjarigen · medische claims · een "hotness"-totaalscore. Dát zijn de echte risico's.

**Verdict:** shipbaar in NL/EU met deze opzet. De architectuur (alles client-side) is je beste juridische én marketingargument tegelijk.

---

## Instructies (voor de gebruiker — staat in de app)
1. Vink 18+/consent → **Start**
2. Upload **frontaal** (verplicht); optioneel **links/rechts profiel** voor kaak-/neushoeken
3. Foto-tips: neutrale blik, mond dicht, haar weg, gelijk licht, camera op ooghoogte, recht/90°
4. **Analyseer** → rapport met overlay (je foto met meetlijnen) + mm-panel + metrics
5. Laagste score = gratis "#1 kans"; rest achter €29 (code → €5 korting + €5 creator-payout)

## Hoe uitbreiden (volgorde van impact)
1. **LLM-tips** — backend stuurt *alleen de metrics + geslacht* → Claude → gepersonaliseerde tekst per kenmerk.
2. **Betaling** — Stripe Checkout / RevenueCat; test €29 vs €39.
3. **Creator-codes** — `code → creator → €5 payout` + dashboard met herhaalverkoop (= wie "blijft plakken").
4. **Meer profielmetrics** — nasofrontale hoek, kin-/neusprojectie, cervicomentale hoek (alle profiel).
5. **Voortgang/recurring** — her-scan over tijd, before/after van je eigen metrics (terugkerende omzet).
6. **App store** — zelfde web → Expo/Capacitor naar iOS/Android ná web-tractie.
7. **Kalibratie** — verzamel (geanonimiseerd, met consent) metric-data om ideaal-ranges per etniciteit/geslacht te verfijnen.

## Architectuur
```
[browser] selfie + profielen → MediaPipe (478 pts + iris) → metrics in mm → overlay + paywall
                                                  └─(later)→ [backend] → Claude → tips (alleen cijfers, geen foto)
```

## Eerste echte test (vóór je verder bouwt)
DM 10 micro-creators (skincare/grooming/glow-up, volwassener publiek): gratis levenslange toegang + eigen code (volgers €5 korting, jij €5/verkoop voor altijd). Bijten er 2-3 → motor draait → doorbouwen.
