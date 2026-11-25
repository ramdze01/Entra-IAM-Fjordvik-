# Modul 6 - IGA: tilgangspakker og tilgangsgjennomganger

**Behov:** I Fjordvik ble ekstra tilgang gitt manuelt og aldri fjernet. Ingen kontrollerte jevnlig hvem som hadde tilgang til hva.

**Løsning:** Tilgang til lønnsrapporter styres med en tilgangspakke og en kvartalsvis tilgangsgjennomgang:

1. **Tilgangspakke:** Ansatte ber selv om tilgang, HR-leder godkjenner, og tilgangen utløper automatisk etter 90 dager.
2. **Tilgangsgjennomgang:** HR-leder bekrefter hvert kvartal hvem som fortsatt skal ha tilgang. Den som avslås, mister tilgangen automatisk.

Scenario: Ola Hav i Salg trenger lesetilgang til lønnsrapporter for et prosjekt. Kari Nilsen (HR-leder) eier rapportene.

## Oppsett

### 1. Gruppe og katalog

Opprettet gruppen `GRP-Lonnsrapporter-Les` med statisk medlemskap og ingen medlemmer. Alle medlemmer skal komme gjennom tilgangspakken. Gruppen er lagt i katalogen *Fjordvik HR*, som samler ressursene HR eier.

![Gruppen i katalogen Fjordvik HR](01-katalog-fjordvik-hr.png)

### 2. Tilgangspakke

| Innstilling | Verdi |
|---|---|
| Navn | Lønnsrapporter - Lesetilgang |
| Gir | Medlemskap i GRP-Lonnsrapporter-Les |
| Hvem kan be om tilgang | Alle ansatte (ikke gjester) |
| Godkjenner | Kari Nilsen (HR-leder) |
| Begrunnelse | Kreves fra både den som ber om tilgang og godkjenner |
| Utløper | Etter 90 dager |

![Tilgangspakken, del 1](02-tilgangspakke-1.png)

![Tilgangspakken, del 2](02-tilgangspakke-2.png)

### 3. Tilgangsgjennomgang

| Innstilling | Verdi |
|---|---|
| Gjelder | Alle medlemmer av GRP-Lonnsrapporter-Les |
| Kontrollør | Kari Nilsen |
| Hyppighet | Kvartalsvis |
| Resultat | Brukes automatisk på gruppen |
| Hvis kontrolløren ikke svarer | Tilgangen fjernes |
| Beslutningshjelp | Anbefaler å avslå brukere uten innlogging de siste 30 dagene |

![Innstillingene for tilgangsgjennomgangen](06-tilgangsgjennomgang.png)

## Test: hele livsløpet til en tilgang

| # | Steg | Utført av | Resultat |
|---|---|---|---|
| 1 | Ola ber om tilgang med begrunnelse | Ola (selvbetjening i My Access) | Forespørsel sendt til Kari |
| 2 | Kari godkjenner med begrunnelse | Kari | Ola lagt til i gruppen |
| 3 | Kari avslår Ola i tilgangsgjennomgangen | Kari | Ola fjernet fra gruppen |

**Steg 1:** Ola ber om tilgangspakken i My Access og oppgir hvorfor han trenger den.

![Ola ber om tilgang](03-ola-ber-om-tilgang.png)

**Steg 2:** Kari godkjenner forespørselen. Ola blir lagt til i gruppen automatisk.

![Kari godkjenner](04-kari-godkjenner.png)

![Ola er medlem av gruppen](05-ola-i-gruppen.png)

**Steg 3:** Prosjektet er avsluttet. I tilgangsgjennomgangen avslår Kari Ola. Beslutningshjelpen anbefalte også å avslå.

![Kari avslår Ola](07-kari-avslar-ola.png)

![Resultatet av gjennomgangen](08-gjennomgang-resultat.png)

Gjennomgangen ble stoppet manuelt for å bruke resultatet med en gang, og Ola ble fjernet fra gruppen.

![Ola er fjernet fra gruppen](09-ola-fjernet.png)

## Sporbarhet

Audit logs for gruppen viser at ingen administrator endret medlemskapet manuelt. Ola ble lagt til av Identity Governance (tilgangspakken) etter Karis godkjenning, og fjernet av tilgangsgjennomgangen etter Karis avslag.

![Audit logs for GRP-Lonnsrapporter-Les](10-audit-log.png)

## Funn

**Fjerningen tok rundt to timer.** Etter at gjennomgangen ble stoppet, sto den på *Applying* i omtrent to timer før Ola ble fjernet. Resultatet av en tilgangsgjennomgang er altså ikke umiddelbart. Hvis tilgang må fjernes med en gang, for eksempel ved en sikkerhetshendelse, må det gjøres manuelt.

**Beslutningshjelpen tok feil.** Den anbefalte å avslå Ola fordi han ikke hadde logget inn de siste 30 dagene, selv om han logget inn samme dag. Aktivitetsdata oppdateres med forsinkelse. Anbefalingen er bare et forslag, og kontrolløren må ta beslutningen selv.

## Begrensninger

- Gjennomgangen er satt opp på gruppen, mens Ola fikk tilgang gjennom tilgangspakken. Tildelingen i tilgangspakken kan fortsatt gjelde selv om gruppemedlemskapet er fjernet. Microsoft anbefaler å gjennomgå selve tilgangspakken når tilgang gis gjennom pakker.
- Rollekonflikter (Segregation of Duties) er ikke satt opp. En naturlig utvidelse er å hindre at samme person har både "betale fakturaer" og "godkjenne fakturaer".
- Gruppen representerer tilgang til lønnsrapporter, men er ikke koblet til et ekte system i labben.
