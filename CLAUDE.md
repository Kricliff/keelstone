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
- `LS_KEY = 'keelstone_state_v1'`, `FREE_EXERCISE_IDS = new Set(['fokus-1', 'ro-1', 'selvtillit-1', 'restitusjon-1'])` (én gratis øvelse per kategori, 2026-07-23)
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

## Runde 2-funksjoner (2026-07-23)

- **Onboarding-fiks:** `#app.onboarding main` manglet `env(safe-area-inset-top)` siden header (som normalt bærer safe-area-paddingen) skjules under onboarding — overskriften ble klippet av notch/statuslinje på ekte enheter. Fikset med egen CSS-regel.
- **Gratis-nivå utvidet:** fra 2 faste øvelser til én øvelse per kategori (`fokus-1, ro-1, selvtillit-1, restitusjon-1`), pluss oppdatert `freeDesc`-tekst.
- **«Godt å se deg igjen»-prompt:** `state.lastOpenDate` spores; `checkComeback()` kjøres ved init og viser en modal hvis det er ≥3 dager siden forrige økt (samme modal-mønster som paywall). Merk: verken Together eller Keelstone hadde dette fra før — det er en ny, felles UX-idé, ikke portert fra en eksisterende Together-funksjon.
- **Coach-kontaktskjema (kun norsk):** nytt kort i Profil, synlig kun når `state.lang === 'no'`. Åpner et skjema (Navn/Telefon/E-post/Grunn) som bygger en `mailto:`-lenke til `keelstone@cliffordcoaching.no` med prefylt emne/brødtekst og navigerer dit via `window.location.href` — ingen backend, konsistent med appens no-backend-filosofi. Bruker lover 24-timers svar i UI-teksten.
- **Yoga-øktspiller:** ny seksjon i Utforsk, alltid Pro-låst. `POSE_PATHS` (9 håndtegnede strekfigur-positurer, delt SVG viewBox `0 0 120 150`) + `YOGA_ROUTINES_DEF` (3 ruter: Morgenflyt, Kontorpause, Kveldsro, 4-5 positurer hver). Egen fullskjerm-overlay (`#yogaOverlay`) strukturelt lik `#breathOverlay` men enklere (ingen partikkelanimasjon), med nedtellingstimer per positur, autofremgang, pause/neste/avslutt. Fullførte økter logges i `state.completedIds` med syntetisk id `yoga-<routineId>:<dato>` — teller mot streak/aktivitetsdiagrammer, men ikke mot kategori-balanse (siden yoga ikke er en av de 4 hovedkategoriene, med hensikt).
- **3-dagers gratis prøveperiode:** kun UI-tekst lagt til i paywall-modalen (`trialBadge`/`trialNote`, CTA endret til «Start 3 dager gratis»). Selve prøveperiode-mekanikken må settes opp som en Introductory Offer på `com.kricliff.keelstone.pro.monthly` i App Store Connect + verifiseres i RevenueCat — kan ikke gjøres fra kode, krever Kristians innlogging.
- **Pre-prestasjon-ritual (2026-07-23):** differensieringsfunksjon foreslått av Claude og godkjent av Kristian. Ett (ikke flere) brukerdefinert ritual lagres i `state.ritual = {mantra, focusNote}`, satt opp via kort på Hjem → modal. Spilles av i en egen 3-stegs fullskjerm-overlay (`#ritualOverlay`): mantra → 3 runder boksepusting (gjenbruker samme animasjonsmatematikk som `runBreathTick`, men egne element-id-er `ritOrb/ritAura1/ritAura2/ritRing` for å unngå å røre den frittstående pusteøvelsen) → fokuspunkt. Teller bevisst IKKE mot streak/`completedIds`, siden det er et pre-event-verktøy, ikke en treningsøkt. **How to apply:** hvis flere ritualer per bruker ønskes senere, bytt `state.ritual` til en array og legg til en enkel liste-UI — spilleren (`renderRitualStageHtml`/`advanceRitualStage`) trenger ikke endres, den tar allerede imot ett ritual-objekt om gangen.

## Status (2026-07-22)

Innhold og UX verifisert grundig i nettleser-preview (onboarding, hjem, søk/filter/favoritter, innsikt-diagrammer, spiller, profil-innstillinger, NO/EN-bytte, lys/mørk modus, alle uten konsollfeil).

**iOS-oppsett (2026-07-22):** `ios/` lagt til med `npx cap add ios`, deretter samme SPM→CocoaPods-fiks som Together trengte (fjernet `CapApp-SPM`-referanser fra `project.pbxproj`, lagt til `Podfile` med `IPHONEOS_DEPLOYMENT_TARGET=15.0`-hook), `TARGETED_DEVICE_FAMILY = "1"` (iPhone-only), ekte app-ikon kopiert inn i `AppIcon.appiconset`.

**Første TestFlight-build: SUKSESS (2026-07-23).** `Kricliff/keelstone` koblet til Codemagic, `ios-workflow` trigget manuelt, build #6 fullførte grønt (3m49s) og lastet opp til TestFlight. Underveis måtte to signeringsproblemer fikses (se «Build-lærdommer» under for detaljer): (1) `environment.ios_signing`-blokken fjernet fra `codemagic.yaml` til fordel for et `app-store-connect fetch-signing-files --create`-script, (2) en helt ny `CERTIFICATE_PRIVATE_KEY` generert og lagt inn som secret i Codemagic (gruppe `ios_signing`) siden Codemagic ikke kan gjenbruke en eksisterende Apple-sertifikat-privatnøkkel den ikke selv genererte. App ID `com.kricliff.keelstone` og en «Keelstone App Store»-provisioning-profil er nå registrert i Apple Developer-portalen.

**Gjenstår, kun gjørbart av Kristian (Apple-ID/RevenueCat-innlogging):**
1. Sjekk TestFlight i App Store Connect for at build #6 er prosessert og installer/test appen på en fysisk enhet
2. Opprett RevenueCat-app for `com.kricliff.keelstone`, produkt `com.kricliff.keelstone.pro.monthly`, entitlement `pro`, sett inn API-nøkkelen i `REVENUECAT_API_KEY` i `www/index.html` (søk «SETT_INN_NOKKEL») — ikke blokkerende for TestFlight-testing av UI, men nødvendig før Pro-kjøp fungerer for testere
3. Lydinnspilling (begge språk) — ikke blokkerende for TestFlight-intern testing
4. Personvern-URL for App Store Connect: https://kricliff.github.io/keelstone/privacy.html (allerede live)

## Build-lærdommer (arvet fra Together — ikke gjenta feilene)

- Bruk `npx cap copy ios`, ALDRI `cap sync ios` (overskriver Podfile-hook)
- iOS bruker CocoaPods, ikke SPM (RevenueCat v9 har ingen Package.swift)
- Podfile trenger `post_install`-hook som tvinger `IPHONEOS_DEPLOYMENT_TARGET = 15.0`
- `xcode: latest` i codemagic.yaml (Apple krever iOS 26 SDK for opplasting)
- Alltid `git pull --rebase --autostash` før push
- Ingen em-streker i brukervendt tekst; all app-tekst på norsk OG engelsk (i18n)
- **IKKE bruk `environment: ios_signing: distribution_type/bundle_identifier` i codemagic.yaml sammen med API-basert automatisk signering.** Dette feltet trigger Codemagics EGEN pre-flight-sjekk mot dens interne (tomme) signeringsfil-lager, FØR noen scripts kjører i det hele tatt — den gjør IKKE et live kall mot App Store Connect API, selv om `integrations: app_store_connect: Codemagic` er satt opp korrekt. Symptom: build feiler umiddelbart («No matching profiles found for bundle identifier ... and distribution type "app_store"») uten noen logger overhodet, selv om appen er registrert og et gyldig profil finnes i Apple Developer-portalen. Riktig mønster: sett `BUNDLE_ID` som en vanlig `environment.vars`, og bruk scripts-stegene `keychain initialize` → `app-store-connect fetch-signing-files $BUNDLE_ID --type IOS_APP_STORE --create` → `keychain add-certificates` → `xcode-project use-profiles` (i den rekkefølgen) — dette gjør det faktiske live API-kallet og oppretter profil/sertifikat automatisk om de mangler. Løst for Keelstone 2026-07-23 (commit f8efe4f). Sjekk om samme feil finnes i Togethers `codemagic.yaml` hvis den også bruker `ios_signing`-blokken.

**Andre signeringsfelle: «Cannot save Signing Certificates without certificate private key».** Selv med `fetch-signing-files --create` feiler builden hvis Apple-kontoen allerede har et distribution-sertifikat som IKKE ble generert av Codemagic (f.eks. laget lokalt i Xcode) — Codemagic finner sertifikatet via API-et, men Apple returnerer aldri private nøkler, så Codemagic kan ikke bruke det og feiler i stedet for å lage et nytt. Fiks: generer en ny 2048-bit RSA-nøkkel lokalt (`openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 | openssl rsa -traditional`), legg den inn som secret-variabelen `CERTIFICATE_PRIVATE_KEY` i Codemagic (app → Environment variables, egen variabelgruppe, f.eks. `ios_signing`), og referer gruppen i `codemagic.yaml` via `environment.groups: [ios_signing]`. Da genererer Codemagic et helt nytt Apple-distribusjonssertifikat den selv har nøkkelen til. **Viktig:** Claude skal ALDRI lime inn private nøkler/secrets i nettskjemaer selv (blokkert av sikkerhetsregler) — nøkkelen vises til Kristian i chatten, og han limer den inn selv. Løst for Keelstone 2026-07-23 (commit 83e367e).
