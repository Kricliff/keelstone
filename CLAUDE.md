# Keelstone — Claude Code Context

## Prosjektoversikt

Kristian Clifford (coach og mental trener) bygger **Keelstone** — mental trening og prestasjonspsykologi for idrettsutøvere, studenter og ledere/fagfolk, **én app, to språk** (norsk + engelsk), ikke en arbeidslivs-/rollebasert app.

**2026-07-22: Keelstone og Mentalt Sterkere er slått sammen til én app.** De startet som to separate prosjekter (se historikk i `Prosjekthistorikk` under), fikk identisk innhold samme dag, og ble deretter besluttet slått sammen: **Keelstone er nå den eneste gjenlevende appen**, og dekker det norske markedet via sitt eksisterende språk-auto-deteksjon (ikke en egen norsk app lenger). `Projects\mentaltsterkere` er lagt ned som eget produkt — repoet står igjen som arkiv/historikk, ikke i aktiv drift. Ikke gjenoppliv det som en egen app med mindre brukeren eksplisitt ber om det.

- **Navn:** Keelstone (App Store-sjekket rent juli 2026; bør reserveres i App Store Connect for endelig bekreftelse)
- **GitHub**: https://github.com/Kricliff/keelstone — **offentlig repo** (endret fra privat 2026-07-22 for å kunne bruke gratis GitHub Pages til personvernsiden). Ikke legg ekte API-nøkler eller hemmeligheter i koden — `REVENUECAT_API_KEY` er fortsatt en plassholder, hold det slik til release-oppsett, bruk da miljøvariabler/Codemagic-secrets i stedet.
- **Bundle ID (planlagt):** com.kricliff.keelstone
- **Support/kontakt:** keelstone@cliffordcoaching.no (alias må opprettes, samme mønster som together@)
- **Coaching-funnel:** Clifford Coaching (https://cliffordcoaching.no)
- **Søsterapp:** «Together» (menns mentale helse, engelsk) i `Projects\sammen` — egen app, egen bundle-ID, ikke del av sammenslåingen over

## Nøkkelbeslutninger (juli 2026, oppdatert 2026-07-22)

- **Tospråklig, én app (norsk + engelsk):** all tekst ligger i en i18n-ordbok (`T[lang]`), aldri hardkodet. Språk auto-detekteres ved første oppstart (norsk enhet → no, ellers en), med manuell bytte i Profil (NO/EN-brytere). Dette er den offisielle løsningen for å dekke både det norske og internasjonale markedet — bevisst valgt fremfor to separate apper (se «Prosjekthistorikk»).
- **20 øvelser, fire kategorier:** Fokus/Focus, Ro/Calm, Selvtillit/Confidence, Restitusjon/Recovery, streak, dagbok (3 spørsmål), Pro-gating (2 gratis øvelser).
- **Ingen fellesskap/UGC i v1:** dette er et personlig treningsverktøy, ikke en community-app. Unngår hele Apple 1.2-moderasjonskravet som Together måtte gjennom.
- **Animert pusteøvelse** (SVG-ring, justerbart tempo) som en kvalitetsdetalj i appen.

## Prosjekthistorikk (for kontekst — ikke reverser disse beslutningene uten eksplisitt beskjed)

1. **16. juli 2026:** Keelstone lanseres som arbeidslivsfokusert, rollebasert app (Leder/Ansatt-spor, rutinebygger, 14-dagers program).
2. **22. juli 2026, tidlig:** Et separat prosjekt, «Mentalt Sterkere» (kun norsk, ingen rollesplitt), startes uavhengig av en annen samtale, uten kjennskap til Keelstone.
3. **22. juli 2026, midt på dagen:** Oppdaget som duplisert arbeid. Besluttet: Keelstone adopterer Mentalt Sterkere sitt innhold (øvelsesbibliotek, ikke rollesplitten), oversatt til engelsk. Begge apper kjøres videre som separate produkter for hvert sitt marked, men med identisk innhold. Felles merkevare hentet fra cliffordcoaching.no (se «Merkevare» under) og full UX-redesign (se `www/index.html`-historikk) gjøres i begge.
4. **22. juli 2026, sent:** Besluttet at «to separate apper med identisk innhold» er unødvendig — Keelstone er allerede tospråklig med auto-deteksjon, så det dekker det norske markedet uten en egen app. **Mentalt Sterkere legges ned som eget produkt.** Keelstone er nå den eneste gjenlevende appen. `Projects\mentaltsterkere`-repoet beholdes som arkiv, ikke slettet.

## Merkevare (delt med cliffordcoaching.no)

Fra 2026-07-22 er fargepaletten hentet direkte fra cliffordcoaching.no for å skape en rød tråd mellom nettside og app: primær `#2F4F4F` (dyp skifergrønn/teal, header og knapper), bakgrunn `#f9f9f9`, tekst nær-svart. Gull `#D9B25C` er en tillagt aksentfarge (siden selv har ingen egen aksent). Skrift: **PT Sans** (samme som siden, lastet via Google Fonts). App-ikon: konsentriske ringer i teal/gull (pusteøvelse-motiv).

## Filstruktur

| Fil/mappe | Beskrivelse |
|---|---|
| `www/index.html` | Appen (produksjon, dette bygges). Vanilla JS + localStorage + i18n |
| `privacy.html` (rot) | Personvernerklæring, NO/EN med språkbryter (samme mønster som appen). Live på https://kricliff.github.io/keelstone/privacy.html via GitHub Pages |
| `www/manifest.webmanifest`, `www/sw.js` | PWA-filer |
| `www/assets/audio/no/`, `www/assets/audio/en/` | Lydfiler per språk, id-er matcher `content/manus.md` (ingen filer lagt inn ennå) |
| `content/manus.md` | Manus til alle 20 øvelser, norsk og engelsk side om side |
| `capacitor.config.json` | App ID `com.kricliff.keelstone`, webDir `www` |
| `serve.ps1` | Lokal preview-server (port 8124, no-cache) |
| `.claude/launch.json` | Preview-konfig (port 8124, unngår kollisjon med Togethers 8123) |
| `codemagic.yaml` | CI/CD-mal (krever ASC-app + signering før den kan bygge) |

## Arkitektur

- Vanilla JS + localStorage, ingen rammeverk (samme filosofi som Together)
- i18n: `T[lang][key]` + `t(key)`; `CATEGORIES[lang]` og `EXERCISES[lang]` er parallelle strukturer med samme id-er og rekkefølge på tvers av språk
- Fire faner: Hjem, Utforsk, Fremgang, Profil (se «UX-redesign» under for detaljer per fane)
- `LS_KEY = 'keelstone_state_v1'`, `FREE_EXERCISE_IDS = new Set(['fokus-1', 'ro-1'])`
- Capacitor 8 (iOS + Android ikke lagt til ennå, kjør `npx cap add ios` / `npx cap add android` når native oppsett skal starte), RevenueCat for abonnement (samme oppsett som Together, API-nøkkel ikke satt inn ennå)
- `npm install --legacy-peer-deps` (RevenueCat v9 + Capacitor v8)

## UX-redesign (2026-07-22)

Fullstendig UX-oppgradering, gjort etter research på ledende meditasjons-/mental trenings-apper (Calm, Headspace, Balance-mønstre):

- **Onboarding ved første oppstart:** velg ett fokusområde (én av de 4 kategoriene) eller hopp over. Lagres i `state.goal`, styrer «Anbefalt for deg» på Hjem og kan endres når som helst i Profil.
- **Hjem:** sirkulær SVG-fremdriftsring (aktive dager siste 7 dager, ikke bare streak-tall), dagens humør-innsjekk (5 emoji, lagres i `state.checkins[dato]`), anbefalt øvelse (goal-basert med fallback til dato-rotasjon, respekterer Pro-lås — dette var en reell bug i den gamle versjonen som ikke sjekket lås på hjem-kortet).
- **Utforsk** (tidligere «Øvelser»): søkefelt (live-filter), kategori-chips + favoritt-chip, varighetsfilter (Under 5 min / 5–7 min / 8+ min), hjerte-ikon per øvelse (`state.favorites`), «Nylig fullført»-seksjon.
- **Fremgang** (slår sammen tidligere «Dagbok» med ny innsikt via en seksjonsbryter Innsikt/Dagbok): ukesdiagram (siste 7 dager, søylediagram), aktivitets-heatmap (siste 10 uker, GitHub-stil), balanse mellom kategorier (horisontal stolpe per kategori). Fargene for kategoriene (`--cat-fokus/ro/selvtillit/restitusjon`) er validert med dataviz-paletteregler (CVD-sjekk) for både lys og mørk modus.
- **Øvelsesspilleren:** tidsbasert fremdriftslinje (fyller seg over oppgitt varighet), favoritt-hjerte i header, fullført-tilstand med egen suksesskort i stedet for bare deaktivert knapp.
- **Profil:** tilpassbart tidspunkt for daglig påminnelse (`state.reminderTime`, `<input type="time">`, erstatter fast kl. 09:00), fokusvelger (samme kategorier som onboarding).
- Alle nye datavisualiseringer fulgte `dataviz`-skillets prosedyre: form først, deretter validert farge (`validate_palette.js`), sekundær koding (ikon+label) der kontrast-WARN krevde det.

## Status (2026-07-22)

Innhold og UX verifisert grundig i nettleser-preview (onboarding, hjem, søk/filter/favoritter, innsikt-diagrammer, spiller, profil-innstillinger, NO/EN-bytte, lys/mørk modus, alle uten konsollfeil).

**iOS-oppsett (2026-07-22):** `ios/` lagt til med `npx cap add ios`, deretter samme SPM→CocoaPods-fiks som Together trengte (fjernet `CapApp-SPM`-referanser fra `project.pbxproj`, lagt til `Podfile` med `IPHONEOS_DEPLOYMENT_TARGET=15.0`-hook), `TARGETED_DEVICE_FAMILY = "1"` (iPhone-only), ekte app-ikon kopiert inn i `AppIcon.appiconset`. `codemagic.yaml` lagt til (kun `ios-workflow`, `android-workflow` ikke satt opp ennå — ingen `android/`-mappe). Alt dette er ikke testet mot en faktisk build ennå (krever macOS/Xcode via Codemagic), kun strukturelt validert (pbxproj brace/parens balansert, YAML syntaks-sjekket).

**Gjenstår, kun gjørbart av Kristian (Apple-ID/RevenueCat-innlogging):**
1. Reserver App ID `com.kricliff.keelstone` i Apple Developer-portalen, opprett appen i App Store Connect
2. Koble Codemagic til `Kricliff/keelstone`-repoet (nå offentlig), bruk eksisterende App Store Connect-integrasjon («Codemagic»-navnet i `codemagic.yaml`)
3. Trigg `ios-workflow` manuelt fra Codemagic-dashbordet — laster opp til TestFlight automatisk ved suksess
4. Opprett RevenueCat-app for `com.kricliff.keelstone`, produkt `com.kricliff.keelstone.pro.monthly`, entitlement `pro`, sett inn API-nøkkelen i `REVENUECAT_API_KEY` i `www/index.html` (søk «SETT_INN_NOKKEL») — ikke blokkerende for første TestFlight-opplasting, men nødvendig før Pro-kjøp fungerer for testere
5. Lydinnspilling (begge språk) — ikke blokkerende for TestFlight-intern testing
6. Personvern-URL for App Store Connect: https://kricliff.github.io/keelstone/privacy.html (allerede live)

## Build-lærdommer (arvet fra Together — ikke gjenta feilene)

- Bruk `npx cap copy ios`, ALDRI `cap sync ios` (overskriver Podfile-hook)
- iOS bruker CocoaPods, ikke SPM (RevenueCat v9 har ingen Package.swift)
- Podfile trenger `post_install`-hook som tvinger `IPHONEOS_DEPLOYMENT_TARGET = 15.0`
- `xcode: latest` i codemagic.yaml (Apple krever iOS 26 SDK for opplasting)
- Alltid `git pull --rebase --autostash` før push
- Ingen em-streker i brukervendt tekst; all app-tekst på norsk OG engelsk (i18n)
