# Forklaring av iCalendar formatering

Her er en kort forklaring av hvordan kalender formatering fungerer hvis du ønsker å ligge til hendelser. Hvis du ønsker å lese mer kan du sjekke ut https://en.wikipedia.org/wiki/ICalendar eller https://icalendar.org

## Hvordan er en event satt opp?

Her kan du finne en template du kan bruke når du skal ligge til nye events i kalender filene.

```
BEGIN:VEVENT
UID:
DTSTAMP:
SUMMARY:
DTSTART;TZID=Europe/Oslo:
DTEND;TZID=Europe/Oslo:
LOCATION:
DESCRIPTION:
END:VEVENT
```

<details>
<summary>Eksempel på en event</summary>

```bash
BEGIN:VEVENT
UID:54e95b8e-abd8-472d-ad2e-64a56cf098d7
DTSTAMP:20260212T133700Z
SUMMARY:🤓 Kodeonsdag - Bergen
DTSTART;TZID=Europe/Oslo:20260603T190000
DTEND;TZID=Europe/Oslo:20260603T210000
LOCATION:Veiten 3, 5012 Bergen
DESCRIPTION:Tema: TBA\nInfo: Husk innesko!
END:VEVENT
```

</details>

</br>

> [!TIP]
> Her er en forklaring av nødvendige felter og deres verdier, for mer utfyllende forklaring kan du lese specs [her](https://www.rfc-editor.org/rfc/rfc5545#section-3.6.1)

- `BEGIN:VEVENT` og `END:VEVENT` definere start og slutt på en event, alt innhold må være mellom disse.
- `UID` er en unik nøkkel for hendelsen og må være unik for at oppdateringer skal fungere. Anbefaler [UUID4](#hvordan-generere-uuid4-lett)
- `DTSTAMP` Datetime for når eventen ble laget i [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) formatet, trenger ikke å være samme dato som eventen.
- `SUMMARY`: Tittel for hendelse, bør være så kort som mulig mens den beholder mening
- `DTSTART;TZID=Europe/Oslo`:Startpunkt for hendelsen, bruk [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) formatet
- `DTEND;TZID=Europe/Oslo`:Sluttpunkt for hendelsen, bruk [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) formatet
- `LOCATION` Lokasjon for hendelsen, her kan du bruke addresse eller annen informasjon.
- `DESCRIPTION` Mer utfyllende beskrivelsen av hendelsen, her kan du bruke `\n` for ny linje

## Hvordan ligger jeg til en event manuelt?

Etter du har lagt inn detaljene i template over må du ligge den til i riktig kalendar ([Bergen](hubbel_bergen.ics) / [Os](hubbel_os.ics)), du gjør det ved å lime inn innholdet mellom `END:VTIMEZONE` (linje 22) og `END:VCALENDAR` (siste linje). Pass på at du ikke overskriver eksisterende events med mindre du vill endre eller fjerne dem.

```
BEGIN:VCALENDAR
PRODID:-//Tidsbanken//Kodeonsdag//NO
VERSION:2.0
CALSCALE:GREGORIAN
METHOD:PUBLISH
BEGIN:VTIMEZONE
TZID:Europe/Oslo
BEGIN:STANDARD
DTSTART:19701025T030000
TZOFFSETFROM:+0200
TZOFFSETTO:+0100
TZNAME:CET
RRULE:FREQ=YEARLY;BYMONTH=10;BYDAY=-1SU
END:STANDARD
BEGIN:DAYLIGHT
DTSTART:19700329T020000
TZOFFSETFROM:+0100
TZOFFSETTO:+0200
TZNAME:CEST
RRULE:FREQ=YEARLY;BYMONTH=3;BYDAY=-1SU
END:DAYLIGHT
END:VTIMEZONE
...
<Lim inn eventen din her>
...
END:VCALENDAR
```

## Hvordan kan jeg teste eller feilsøke mine endringer?

Etter du har modifisert en fil kan du verifisere og feilsøke den ved å laste den opp / lime inn på https://icalendar.org/validator.html

## Tips og tricks

### Hvordan generere UUID4 lett

hvis du har bash kan du bruk uuidgen fra [libuuid(3)](https://linux.die.net/man/3/libuuid)

```bash
uuidgen
```

Du kan bruke node fra terminalen hvis du har det installert

```bash
node -p "require('node:crypto').randomUUID()"
```
