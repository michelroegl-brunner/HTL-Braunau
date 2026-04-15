- [OSI-Modell und Layer 1](#osi-modell-und-layer-1)
  - [Grundlagen des OSI-Modells](#grundlagen-des-osi-modells)
  - [Kabeltypen und Verbindungstypen auf Layer
    1](#kabeltypen-und-verbindungstypen-auf-layer-1)
    - [Kupfer (Twisted Pair)](#kupfer-twisted-pair)
    - [Glasfaser (LWL)](#glasfaser-lwl)
    - [Funk (WLAN)](#funk-wlan)
  - [Vergleich Kupfer vs. Glasfaser
    (LWL)](#vergleich-kupfer-vs.-glasfaser-lwl)
    - [Wann nimmt man was?](#wann-nimmt-man-was)
  - [Zusammenfassung](#zusammenfassung)
  - [Quellen (Auswahl)](#quellen-auswahl)

# OSI-Modell und Layer 1

## Grundlagen des OSI-Modells

Das OSI-Modell beschreibt die Kommunikation in Netzwerken in sieben
Schichten. Jede Schicht hat klar definierte Aufgaben und bietet der
darueberliegenden Schicht einen Dienst.

| **Layer** | **Name**     | **Kernaufgabe**                                                    |
|:---------:|:-------------|:-------------------------------------------------------------------|
|     7     | Application  | Schnittstelle zu Anwendungen (z. B. HTTP, DNS, SMTP).              |
|     6     | Presentation | Datenformat, Kodierung, Kompression, Verschluesselung.             |
|     5     | Session      | Aufbau, Steuerung und Abbau von Sitzungen.                         |
|     4     | Transport    | Ende-zu-Ende-Transport, Zuverlaessigkeit, Ports (TCP/UDP).         |
|     3     | Network      | Logische Adressierung und Routing (IP).                            |
|     2     | Data Link    | Frames, MAC-Adressen, Fehlererkennung im lokalen Netz.             |
|     1     | Physical     | Uebertragung von Bits als elektrische, optische oder Funk-Signale. |

Diese Unterlage fokussiert auf **Layer 1**. Typische Themen dort sind:

- Uebertragungsmedien (Kupfer, Glasfaser, Funk)

- Steckertypen und Pinbelegungen

- Signalpegel, Daempfung, Stoerungen

- Maximale Reichweiten und physikalische Grenzwerte

## Kabeltypen und Verbindungstypen auf Layer 1

### Kupfer (Twisted Pair)

Twisted-Pair-Kabel bestehen aus verdrillten Adernpaaren. Die Verdrillung
reduziert elektromagnetische Einstreuungen und Nebensprechen
(*Crosstalk*), weil Stoersignale auf beide Leiter eines Paares aehnlich
einwirken und sich differenziell besser unterdruecken lassen.

<figure>

<figcaption>Beispiel eines UTP-Kabels (Quelle: Wikimedia Commons, Datei:
UTP_cable.jpg)</figcaption>
</figure>

#### UTP, STP, FTP, S/FTP

Die Bezeichnungen geben an, ob ein Gesamtschirm und/oder ein Paarschirm
vorhanden ist:

- **U/UTP (UTP):** Kein Gesamtschirm, kein Paarschirm. Guenstig,
  flexibel, in Buero-/Schulumgebungen haeufig.

- **F/UTP (oft FTP genannt):** Folien-Gesamtschirm um alle Paare.

- **S/UTP (oft STP genannt):** Geflecht-Gesamtschirm um alle Paare.

- **S/FTP:** Geflecht-Gesamtschirm plus Folien-Schirm je Adernpaar. Sehr
  gute EMV-Eigenschaften.

Grundregel: Je staerker die Stoerquelle (z. B. Maschinen, Motoren, lange
parallele Stromfuehrung), desto wichtiger ist eine gute Schirmung und
saubere Erdung.

#### Cat-Standards bei Kupferkabeln

Die Kategorie (*Category*, Cat) beschreibt die
Uebertragungseigenschaften der Kupferverkabelung.

| **Kategorie** | **Bandbreite** | **Typische Datenraten**                    | **Typische Verwendung**                  |
|:--------------|:---------------|:-------------------------------------------|:-----------------------------------------|
| Cat5e         | 100 MHz        | 1 Gbit/s bis 100 m                         | Standard-LAN, Schul-/Buero-Netz          |
| Cat6          | 250 MHz        | 1 Gbit/s bis 100 m, 10 Gbit/s bis ca. 55 m | Neuinstallationen mit Reserve            |
| Cat6A         | 500 MHz        | 10 Gbit/s bis 100 m                        | Professionelle strukturierte Verkabelung |
| Cat8          | 2000 MHz       | 25/40 Gbit/s bis 30 m                      | Rechenzentrum, kurze High-Speed-Links    |

Hinweis: Cat7/7A existieren als ISO/IEC-Klassen, sind aber im TIA-Umfeld
nicht als klassische Cat-TIA-Stufen wie Cat6A/Cat8 verankert.

#### Farbcode T568A und T568B

Fuer RJ45 (8P8C) sind zwei Belegungsvarianten ueblich: T568A und T568B.
Technisch sind beide gleichwertig, wichtig ist die **konsistente
Nutzung** innerhalb einer Installation.

| **Pin** | **T568A**    | **T568B**    |
|:-------:|:-------------|:-------------|
|    1    | Weiss/Gruen  | Weiss/Orange |
|    2    | Gruen        | Orange       |
|    3    | Weiss/Orange | Weiss/Gruen  |
|    4    | Blau         | Blau         |
|    5    | Weiss/Blau   | Weiss/Blau   |
|    6    | Orange       | Gruen        |
|    7    | Weiss/Braun  | Weiss/Braun  |
|    8    | Braun        | Braun        |

In der Praxis wird in vielen europaeischen Installationen haeufig T568B
verwendet. Normativ sind beide Varianten zulaessig, solange nicht
gemischt wird.

#### Was ist ein Crossover-Kabel?

Ein Crossover-Kabel ist an einem Ende nach T568A und am anderen Ende
nach T568B aufgelegt. Dadurch werden Sende- und Empfangsadern gekreuzt.
Frueher war das fuer direkte Verbindungen gleichartiger Geraete relevant
(z. B. Switch zu Switch). Heute erkennen viele Geraete den Leitungstyp
automatisch (*Auto-MDI/X*).

### Glasfaser (LWL)

LWL (Lichtwellenleiter) uebertraegt Daten als Lichtimpulse statt als
elektrische Signale. Dadurch ist die Uebertragung unempfindlich gegen
elektromagnetische Stoerungen.

#### Singlemode vs. Multimode

| **Merkmal**       | **Singlemode (OS1/OS2)**    | **Multimode (OM1-OM5)**                 |
|:------------------|:----------------------------|:----------------------------------------|
| Kerndurchmesser   | ca. 9 um                    | 50 oder 62.5 um                         |
| Lichtquelle       | Laser (typ. 1310/1550 nm)   | LED/VCSEL (typ. 850/1300 nm)            |
| Reichweite        | Sehr hoch (km-Bereich)      | Eher kurz bis mittel (typ. Gebaeude/DC) |
| Typischer Einsatz | Backbone, Campus, FTTH, WAN | Rechenzentrum, Inhouse-Verkabelung      |

<figure>

<figcaption>Singlemode vs. Multimode (Quelle: Wikimedia Commons, Datei:
Multimode_vs_Single_Mode_Fiber.png)</figcaption>
</figure>

### Funk (WLAN)

WLAN basiert auf IEEE 802.11 und nutzt auf Layer 1 Funkwellen statt
Kabel.

- **2.4 GHz:** Gute Reichweite und Durchdringung, aber stoeranfaelliger
  und meist geringere nutzbare Datenrate.

- **5 GHz:** Hoehere Datenraten, mehr Kanaele, geringere Reichweite als
  2.4 GHz.

- **6 GHz (Wi-Fi 6E/7):** Sehr viele freie Kanaele, hohe Datenraten,
  kurze Reichweite und hohe Daempfung.

Bei WLAN gilt: Hoehere Frequenz bringt oft mehr Geschwindigkeit, aber
meist weniger Reichweite pro Access Point.

## Vergleich Kupfer vs. Glasfaser (LWL)

| **Kriterium**               | **Kupfer (Twisted Pair)**                        | **Glasfaser (LWL)**                                          |
|:----------------------------|:-------------------------------------------------|:-------------------------------------------------------------|
| EMI-Stoeranfaelligkeit      | Hoeher, je nach Schirmung besser beherrschbar    | Sehr gering, da optische Uebertragung                        |
| Max. Reichweite pro Link    | Typisch 100 m (LAN-Standard)                     | Von hunderten Metern bis viele Kilometer                     |
| Bandbreiten-Reserve         | Hoch, aber physikalisch frueher limitiert        | Sehr hoch, besonders fuer Backbone/Skalierung                |
| Kosten der Aktivkomponenten | Oft guenstiger (Kupferports)                     | Optiken/Module oft teurer                                    |
| Installation                | Einfach, robust im Alltag, bekannte RJ45-Technik | Sorgfalt bei Reinigung, Biegeradius und Spleiss/Stecktechnik |
| Galvanische Trennung        | Nicht gegeben                                    | Gegeben, dadurch Vorteil bei Potenzialunterschieden          |

### Wann nimmt man was?

- **Kupfer (Twisted Pair):** Arbeitsplatzverkabelung, kurze bis mittlere
  Strecken in Gebaeuden, gutes Preis-Leistungs-Verhaeltnis.

- **LWL:** Backbone zwischen Verteilern/Gebaeuden, lange Strecken, hohe
  Bandbreite und Umgebungen mit starker EMV-Belastung.

## Zusammenfassung

Layer 1 bildet die technische Basis jeder Netzkommunikation. Die Wahl
des Mediums entscheidet direkt ueber Reichweite, Stabilitaet, Datenrate
und Kosten:

- Kupfer ist fuer viele Standard-LAN-Szenarien wirtschaftlich und
  praxisnah.

- LWL ist bei grossen Distanzen, hohen Bandbreiten und EMV-Anforderungen
  klar im Vorteil.

- WLAN erweitert Layer 1 um Mobilitaet, benoetigt aber sorgfaeltige
  Funkplanung.

## Quellen (Auswahl)

- Fluke Networks:
  `https://www.flukenetworks.com/knowledge-base/application-or-standards-articles-copper/differences-between-wiring-codes-t568a-vs`

- ANSI/TIA-568 Uebersicht: `https://en.wikipedia.org/wiki/ANSI/TIA-568`

- Eaton/Tripp Lite Ethernet Cables:
  `https://tripplite.eaton.com/products/ethernet-cable-types`

- Tektronix 802.11 PHY Primer:
  `https://www.tek.com/en/documents/primer/wi-fi-overview-80211-physical-layer-and-transmitter-measurements`

- Twisted Pair Grundlagen: `https://en.wikipedia.org/wiki/Twisted_pair`

- LWL Singlemode vs Multimode:
  `https://commons.wikimedia.org/wiki/File:Multimode_vs_Single_Mode_Fiber.png`
