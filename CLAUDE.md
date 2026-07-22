# Keelstone — Claude Code Context

## Prosjektoversikt

Kristian Clifford (coach og mental trener) bygger **Keelstone**, den internasjonale (tospråklige) søsterappen til **Mentalt Sterkere** (`C:\Users\KristianClifford\Projects\mentaltsterkere`, kun norsk). Fra 2026-07-22 deler de to appene samme konsept og innhold: mental trening og prestasjonspsykologi for idrettsutøvere, studenter og ledere/fagfolk, ikke en arbeidslivs-/rollebasert app. **Hold dem synkronisert** — nytt innhold i den ene bør speiles i den andre, med samme øvelse-id-er.

- **Navn:** Keelstone (App Store-sjekket rent juli 2026; bør reserveres i App Store Connect for endelig bekreftelse)
- **Bundle ID (planlagt):** com.kricliff.keelstone
- **Support/kontakt:** keelstone@cliffordcoaching.no (alias må opprettes, samme mønster som together@ og mentaltsterkere@)
- **Coaching-funnel:** Clifford Coaching (https://cliffordcoaching.no)
- **Søsterapper:** «Together» (menns mentale helse, engelsk) i `Projects\sammen`, «Mentalt Sterkere» (norsk, samme innhold som Keelstone) i `Projects\mentaltsterkere`

## Nøkkelbeslutninger (juli 2026, oppdatert 2026-07-22)

- **Tospråklig fra start (norsk + engelsk):** all tekst ligger i en i18n-ordbok (`T[lang]`), aldri hardkodet. Språk auto-detekteres ved første oppstart (norsk enhet → no, ellers en), med manuell bytte i Profil (NO/EN-brytere).
- **Samme konsept og innhold som Mentalt Sterkere:** 20 øvelser jevnt fordelt på fire kategorier (Fokus/Focus, Ro/Calm, Selvtillit/Confidence, Restitusjon/Recovery), streak, dagbok (3 spørsmål), Pro-gating (2 gratis øvelser). Den tidligere Leder/Ansatt-rollesplitten, rutinebyggeren og 14-dagers-programmet er fjernet, erstattet med dette biblioteket, oversatt til engelsk (norsk er identisk med Mentalt Sterkere).
- **Ingen fellesskap/UGC i v1:** dette er et personlig treningsverktøy, ikke en community-app. Unngår hele Apple 1.2-moderasjonskravet som Together måtte gjennom.
- **Beholdt fra tidligere versjon:** den animerte pusteøvelsen (SVG-ring, justerbart tempo) er penere enn Mentalt Sterkeres enklere variant og er beholdt i Keelstone som en kvalitetsdifferensiering for det internasjonale markedet, samme boksepust-konsept.

## Merkevare (delt med Mentalt Sterkere og cliffordcoaching.no)

Fra 2026-07-22 er fargepaletten hentet direkte fra cliffordcoaching.no for å skape en rød tråd mellom nettside og app: primær `#2F4F4F` (dyp skifergrønn/teal, header og knapper), bakgrunn `#f9f9f9`, tekst nær-svart. Gull `#D9B25C` er en tillagt aksentfarge (siden selv har ingen egen aksent). Skrift: **PT Sans** (samme som siden, lastet via Google Fonts). App-ikon: konsentriske ringer i teal/gull (pusteøvelse-motiv), samme design som Mentalt Sterkere. Samme palett gjelder Mentalt Sterkere identisk.

## Filstruktur

| Fil/mappe | Beskrivelse |
|---|---|
| `www/index.html` | Appen (produksjon, dette bygges). Vanilla JS + localStorage + i18n, samme arkitektur som Mentalt Sterkere |
| `www/manifest.webmanifest`, `www/sw.js` | PWA-filer, samme mønster som Mentalt Sterkere |
| `www/assets/audio/no/`, `www/assets/audio/en/` | Lydfiler per språk, id-er matcher `content/manus.md` (ingen filer lagt inn ennå) |
| `content/manus.md` | Manus til alle 20 øvelser, norsk og engelsk side om side |
| `capacitor.config.json` | App ID `com.kricliff.keelstone`, webDir `www` |
| `serve.ps1` | Lokal preview-server (port 8124, no-cache) |
| `.claude/launch.json` | Preview-konfig (port 8124, unngår kollisjon med Togethers 8123) |
| `codemagic.yaml` | CI/CD-mal (krever ASC-app + signering før den kan bygge) |

## Arkitektur

- Vanilla JS + localStorage, ingen rammeverk (samme filosofi som Together og Mentalt Sterkere)
- i18n: `T[lang][key]` + `t(key)`; `CATEGORIES[lang]` og `EXERCISES[lang]` er parallelle strukturer med samme id-er og rekkefølge på tvers av språk
- Fire faner: Hjem, Øvelser, Dagbok, Profil (samme som Mentalt Sterkere, ikke lenger fem faner med rollevalg)
- `LS_KEY = 'keelstone_state_v1'`, `FREE_EXERCISE_IDS = new Set(['fokus-1', 'ro-1'])`
- Capacitor 8 (iOS + Android ikke lagt til ennå, kjør `npx cap add ios` / `npx cap add android` når native oppsett skal starte), RevenueCat for abonnement (samme oppsett som Together, API-nøkkel ikke satt inn ennå)
- `npm install --legacy-peer-deps` (RevenueCat v9 + Capacitor v8)

## Status (2026-07-22)

Innhold og struktur er nå identisk med Mentalt Sterkere (bortsett fra språk), verifisert i nettleser-preview (navigasjon, øvelsesvisning, dagbok, språkbytte NO/EN, pusteanimasjon, alle uten konsollfeil). Gjenstår: ekte app-ikoner (PNG, `www/assets/` er tom), lydinnspilling (begge språk), `npx cap add ios/android`, App Store Connect-app, signering, RevenueCat-produkt (`com.kricliff.keelstone.pro.monthly`), push til GitHub (`github.com/Kricliff/keelstone`, ikke bekreftet at push har skjedd).

## Build-lærdommer (arvet fra Together, ikke gjenta feilene)

- Bruk `npx cap copy ios`, ALDRI `cap sync ios` (overskriver Podfile-hook)
- iOS bruker CocoaPods, ikke SPM (RevenueCat v9 har ingen Package.swift)
- Podfile trenger `post_install`-hook som tvinger `IPHONEOS_DEPLOYMENT_TARGET = 15.0`
- `xcode: latest` i codemagic.yaml (Apple krever nyeste iOS SDK for opplasting)
- Alltid `git pull --rebase --autostash` før push
- Ingen em-streker i brukervendt tekst; all app-tekst på norsk OG engelsk (i18n)

## Build-lærdommer (arvet fra Together — ikke gjenta feilene)

- Bruk `npx cap copy ios`, ALDRI `cap sync ios` (overskriver Podfile-hook)
- iOS bruker CocoaPods, ikke SPM (RevenueCat v9 har ingen Package.swift)
- Podfile trenger `post_install`-hook som tvinger `IPHONEOS_DEPLOYMENT_TARGET = 15.0`
- `xcode: latest` i codemagic.yaml (Apple krever iOS 26 SDK for opplasting)
- Alltid `git pull --rebase --autostash` før push
- Ingen em-streker i brukervendt tekst; all app-tekst på norsk OG engelsk (i18n)
