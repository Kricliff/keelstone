# Keelstone — Claude Code Context

## Prosjektoversikt

Kristian Clifford (coach og mental trener) bygger sin andre app: **Keelstone** — en prestasjonspsykologi- og mental helse-app for arbeidslivet, med to separate rollebaserte opplevelser (**Leder** og **Ansatt**), valgt ved onboarding. B2C først; B2B (aggregert, anonymisert team-innsikt) senere, eventuelt egen app.

- **Arbeidstittel/navn:** Keelstone (App Store-sjekket rent juli 2026; bør reserveres i App Store Connect for endelig bekreftelse)
- **Bundle ID (planlagt):** com.kricliff.keelstone
- **Coaching-funnel:** Clifford Coaching (https://cliffordcoaching.no)
- **Søsterapp:** «Together» (menns mentale helse) i `C:\Users\KristianClifford\Projects\sammen` — mye arkitektur er gjenbrukt derfra

## Nøkkelbeslutninger (juli 2026)

- **Tospråklig fra start (norsk + engelsk):** all tekst ligger i en i18n-ordbok (`T`), aldri hardkodet. Språk auto-detekteres ved første oppstart (norsk enhet → no, ellers en), med manuell bytte i Profil. Én app, lokalisert App Store-oppføring — ikke to apper.
- **Ingen fellesskap/UGC i v1:** dette er et personlig treningsverktøy, ikke en community-app. Unngår hele Apple 1.2-moderasjonskravet som Together måtte gjennom, og gir raskere godkjenning.
- **Lyd senere:** v1 er tekst/visuelt. Stemmeinnhold (Kristians stemme, begge språk) kommer i senere versjon.
- To roller (Ansatt/Leder) er to spor i samme app, styrt av `state.role`.

## Filstruktur

| Fil/mappe | Beskrivelse |
|---|---|
| `www/index.html` | Appen (produksjon — dette bygges). Vanilla JS + localStorage + i18n |
| `capacitor.config.json` | App ID `com.kricliff.keelstone`, webDir `www` |
| `serve.ps1` | Lokal preview-server (port 8124, no-cache) |
| `.claude/launch.json` | Preview-konfig (port 8124 — unngår kollisjon med Togethers 8123) |
| `codemagic.yaml` | CI/CD-mal (krever ASC-app + signering før den kan bygge) |

## Arkitektur

- Vanilla JS + localStorage, ingen rammeverk (samme filosofi som Together)
- i18n: `T[lang][key]` + `t(key)`; rolleavhengig innhold via `T[lang].ansatt` / `T[lang].leder`
- Fem faner: Hjem, Trening, Prestasjon, Lære, Profil (+ rollevalg ved onboarding)
- Capacitor 8 (iOS + Android), RevenueCat for abonnement (samme oppsett som Together)
- `npm install --legacy-peer-deps` (RevenueCat v9 + Capacitor v8)

## Innhold

Detaljert innholdsplan (øvelser, programmer, Prestasjon-modul, Lære, utbrenthets-selvsjekk):
`C:\Users\KristianClifford\OneDrive - Great People\Skrivebord\momentum-innholdsplan-v1.md`
(filnavnet sier «momentum» fra en tidligere arbeidstittel — innholdet gjelder Keelstone). PRD: `prd-prestasjonspsykologi-app-ledere-ansatte.md` samme mappe.

## Status

Skjelett satt opp (i18n + roller + fem faner + innsjekk-lagring). Gjenstår: fylle inn fullt innhold fra planen, native oppsett (iOS/Android), App Store Connect-app, signering, RevenueCat-produkter.

## Build-lærdommer (arvet fra Together — ikke gjenta feilene)

- Bruk `npx cap copy ios`, ALDRI `cap sync ios` (overskriver Podfile-hook)
- iOS bruker CocoaPods, ikke SPM (RevenueCat v9 har ingen Package.swift)
- Podfile trenger `post_install`-hook som tvinger `IPHONEOS_DEPLOYMENT_TARGET = 15.0`
- `xcode: latest` i codemagic.yaml (Apple krever iOS 26 SDK for opplasting)
- Alltid `git pull --rebase --autostash` før push
- Ingen em-streker i brukervendt tekst; all app-tekst på norsk OG engelsk (i18n)
