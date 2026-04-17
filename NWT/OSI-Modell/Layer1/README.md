- [OSI-Modell und Layer 1](#osi-modell-und-layer-1)
  - [Grundlagen des OSI-Modells](#grundlagen-des-osi-modells)
  - [Kabeltypen und Verbindungstypen auf Layer
    1](#kabeltypen-und-verbindungstypen-auf-layer-1)
    - [Kupferkabel (Twisted Pair)](#kupferkabel-twisted-pair)
    - [Glasfaser (LWL)](#glasfaser-lwl)
    - [Funk (WLAN)](#funk-wlan)
  - [Vergleich Kupfer vs. Glasfaser (LWL) vs.
    Funk](#vergleich-kupfer-vs.-glasfaser-lwl-vs.-funk)
    - [Wann nimmt man was?](#wann-nimmt-man-was)
  - [Zusammenfassung](#zusammenfassung)

# OSI-Modell und Layer 1

## Grundlagen des OSI-Modells

Das OSI-Modell (*Open Systems Interconnection Model*) ist ein
Referenzmodell, das die Netzwerkkommunikation in sieben klar getrennte
Schichten unterteilt. Es wurde von der ISO standardisiert, damit Geräte
und Systeme unterschiedlicher Hersteller trotz verschiedener technischer
Umsetzungen miteinander kommunizieren können. Die zentrale Idee ist,
dass jede Schicht eine definierte Aufgabe übernimmt und der
nächsthöheren Schicht einen Dienst bereitstellt.

Durch diese Aufteilung lassen sich Netzwerke besser planen, vergleichen
und Fehler systematisch eingrenzen. Wenn zum Beispiel eine Verbindung
physisch vorhanden ist, aber keine Daten ausgetauscht werden, kann
gezielt geprüft werden, auf welchem Layer das Problem liegt. Das Modell
ist daher nicht nur theoretisch wichtig, sondern auch im praktischen
Betrieb und bei der Fehlersuche ein zentrales Denkwerkzeug.

#### Kurzer geschichtlicher Überblick

Die Entwicklung des OSI-Modells begann in einer Zeit, in der viele
Hersteller eigene, nicht kompatible Netzwerklösungen einsetzten. Anfang
der 1970er Jahre entstanden zahlreiche proprietäre Systeme, was den
Austausch zwischen unterschiedlichen Plattformen stark erschwerte. Um
dieses Problem zu lösen, starteten internationale
Standardisierungsgremien Ende der 1970er Jahre die Arbeit an einem
einheitlichen Referenzrahmen.

Ein wichtiger Meilenstein war **1978**, als die grundlegende
Schichtenidee in der Standardisierungsarbeit konkretisiert wurde.
**1983** veröffentlichte die ISO die erste vollständige Beschreibung des
Referenzmodells. **1984** wurde das Modell dann als internationaler
Standard *ISO 7498* breit etabliert. Später folgten Erweiterungen und
Präzisierungen, unter anderem in den 1990er Jahren, um das Modell besser
mit realen Protokollwelten und Management-Aspekten zu verbinden.

Auch wenn sich im praktischen Internetbetrieb vor allem das
TCP/IP-Modell durchgesetzt hat, blieb das OSI-Modell bis heute zentral
für Ausbildung, Planung und Fehlersuche. Der Grund ist didaktisch und
technisch zugleich: Die saubere Trennung in sieben Layer macht komplexe
Kommunikationsabläufe verständlich und hilft, Probleme systematisch
einer Schicht zuzuordnen.

<figure>

<figcaption>OSI-7-Schichtenmodell</figcaption>
</figure>

In dieser Unterlage liegt der Schwerpunkt auf **Layer 1**. Zur
Einordnung ist wichtig, dass je nach Layer unterschiedliche
Dateneinheiten betrachtet werden: Auf Layer 4 spricht man von
*Segments*, auf Layer 3 von *Packets*, auf Layer 2 von *Frames* und auf
Layer 1 von *Bits*. Damit ist Layer 1 die physikalische Basis aller
darüberliegenden Kommunikationsprozesse.

Typische Themen auf Layer 1 sind:

- Übertragungsmedien (Kupfer, Glasfaser, Funk)

- Steckertypen und Pinbelegungen

- Signalpegel, Dämpfung, Störungen

- Maximale Reichweiten und physikalische Grenzwerte

Für das Verständnis ist ausserdem wichtig, dass das OSI-Modell ein
Referenzmodell und kein konkretes Protokoll ist. Es gibt also nicht “das
eine OSI-Protokoll”, sondern viele reale Technologien, die sich in
dieses Modell einordnen lassen. Ethernet, Glasfaserstandards oder
WLAN-Standards setzen die Aufgaben verschiedener Layer praktisch um.
Gerade im Unterricht hilft das Modell dabei, komplexe Zusammenhänge zu
strukturieren: Man lernt nicht nur einzelne Technologien auswendig,
sondern versteht, warum sie an einer bestimmten Stelle im Gesamtsystem
gebraucht werden.

Ein weiterer Vorteil der Schichtung ist die Austauschbarkeit von
Technologien. Wenn auf Layer 1 beispielsweise statt Kupfer plötzlich
Glasfaser verwendet wird, können die darüberliegenden Layer in vielen
Fällen unverändert bleiben. Das macht Netzwerke flexibel und
erweiterbar. Genau deshalb ist Layer 1 trotz seiner scheinbar einfachen
Aufgabe so entscheidend: Ohne stabile physikalische Übertragung
funktionieren alle höheren Protokolle nicht zuverlässig.

## Kabeltypen und Verbindungstypen auf Layer 1

### Kupferkabel (Twisted Pair)

Der Physical Layer ist die unterste Schicht des OSI-Modells und
zuständig für die Übertragung eines Bitstroms über ein physisches
Medium. Bei Kupferkabeln erfolgt diese Übertragung als elektrische
Spannungssignale. Layer 1 definiert dabei unter anderem Steckertypen,
Kabelmaterial, Übertragungsraten, Signaleigenschaften und zulässige
Leitungslanegen. Wichtig: Eine inhaltliche Interpretation der Daten
findet auf dieser Schicht nicht statt, es werden ausschliesslich Bits
übertragen.

Twisted-Pair-Kabel bestehen aus acht Kupferadern, die zu vier Paaren
zusammengefasst und paarweise verdrillt sind. Diese Verdrillung hat
einen klaren physikalischen Zweck: Sie reduziert elektromagnetische
Einstreuungen und Nebensprechen (*Crosstalk*), weil Störsignale auf
beide Leiter eines Paares ähnlich einwirken und sich differenziell
besser unterdrücken lassen. Damit steigt die Signalqualität und stabile
Datenübertragung wird über die vorgesehene Strecke möglich.

Der grundsätzliche Kabelaufbau besteht aus den Kupferadern selbst, einer
Isolierung um jede einzelne Ader, optionalen Schirmungsstrukturen und
einem robusten Aussenmantel. In hochwertigen Kabeln werden zusätzlich
Trennelemente eingesetzt, damit die Paare mechanisch stabil bleiben und
sich bei Biegung weniger beeinflussen. Das verbessert vor allem bei
höheren Frequenzen die Übertragungsqualität. In professionellen
Installationen spielt daher nicht nur die Kategorie (z. B. Cat6A),
sondern auch die Verlegequalität eine grosse Rolle.

<figure>

<figcaption>Beispiel eines UTP-Kabels (Quelle: Wikimedia Commons, Datei:
UTP_cable.jpg)</figcaption>
</figure>

#### UTP, STP, FTP, S/FTP

Die Bezeichnungen geben an, ob ein Gesamtschirm und/oder ein Paarschirm
vorhanden ist:

- **U/UTP (UTP):** Kein Gesamtschirm, kein Paarschirm. Günstig,
  flexibel, in Büro-/Schulumgebungen häufig.

- **F/UTP (oft FTP genannt):** Folien-Gesamtschirm um alle Paare.

- **S/UTP (oft STP genannt):** Geflecht-Gesamtschirm um alle Paare.

- **S/FTP:** Geflecht-Gesamtschirm plus Folien-Schirm je Adernpaar. Sehr
  gute EMV-Eigenschaften.

Abschirmung schützt Kabel vor StörQuellen wie Stromleitungen, Maschinen,
FunkQuellen oder benachbarten Datenkabeln. Grundregel: Je stärker die
Umgebung stört (z. B. Industriehallen, viele leistungsstarke Geräte,
lange parallele Leitungsführung), desto wichtiger sind hochwertige
Schirmung und saubere Erdung.

In der Praxis entscheidet die Umgebung über die passende Schirmungsart.
In ruhigen Büro- oder Klassenraumumgebungen reicht oft UTP aus. In
technischen Räumen mit Schaltschranken, Motoren oder vielen parallel
geführten Leitungen ist S/FTP meist die sicherere Wahl. Wichtig ist
dabei: Eine gute Schirmung wirkt nur dann optimal, wenn die gesamte
Installation fachgerecht ausgeführt ist, also inklusive geeigneter
Anschlusskomponenten und korrekter Potentialführung.

#### Cat-Standards bei Kupferkabeln

Die Kategorie (*Category*, Cat) beschreibt die Übertragungseigenschaften
der Kupferverkabelung. Sie gibt insbesondere Auskunft über die maximal
nutzbare Frequenz, die mögliche Datenrate und damit indirekt über die
Qualitätsanforderungen an Kabel und Installation.

| **Kategorie** | **Bandbreite** | **Typische Datenraten**                    | **Typische Verwendung**                  |
|:--------------|:---------------|:-------------------------------------------|:-----------------------------------------|
| Cat5e         | 100 MHz        | 1 Gbit/s bis 100 m                         | Standard-LAN, Schul-/Büro-Netz           |
| Cat6          | 250 MHz        | 1 Gbit/s bis 100 m, 10 Gbit/s bis ca. 55 m | Neuinstallationen mit Reserve            |
| Cat6A         | 500 MHz        | 10 Gbit/s bis 100 m                        | Professionelle strukturierte Verkabelung |
| Cat8          | 2000 MHz       | 25/40 Gbit/s bis 30 m                      | Rechenzentrum, kurze High-Speed-Links    |

In klassischen LAN-Installationen gilt für Kupferverbindungen
typischerweise eine maximale Linklänge von 100 m. In der Praxis werden
heute häufig Cat6A oder Cat7-basierte Verkabelungen eingesetzt, da sie
ausreichende Reserven für 10-Gigabit-Anwendungen bieten.

Diese 100 m setzen sich typischerweise aus bis zu 90 m Verlegekabel und
insgesamt bis zu 10 m Patchkabeln zusammen. Wird die Strecke länger,
steigt die Dämpfung und die Fehlerrate kann zunehmen. Das zeigt, dass
Layer 1 nicht nur von theoretischen Datenraten lebt, sondern stark von
physikalischen Effekten wie Widerstand, Frequenzverhalten und
Signal-Rausch-Abstand abhängt.

#### Farbcode T568A und T568B

Der am weitesten verbreitete Netzwerkstecker im Kupferbereich ist RJ45
(8P8C) mit acht Pins für acht Adern. Für die Belegung sind zwei
Varianten normiert: T568A und T568B. Technisch sind beide gleichwertig,
entscheidend ist die **konsistente Nutzung** innerhalb einer
Installation.

<figure>

<figcaption>T568A- und T568B-Pinbelegung</figcaption>
</figure>

In vielen europäischen Installationen wird T568B bevorzugt. Die Pins 1
und 2 werden klassisch als Sendeleitungen, die Pins 3 und 6 als
Empfangsleitungen betrachtet (historische Sicht auf MDI-Portbelegung).

Neben RJ45 existieren im strukturierten Verkabelungsbereich auch
alternative Stecksysteme wie GG45 oder TERA, die in speziellen
Umgebungen eingesetzt werden. Im Alltag dominiert jedoch RJ45, weil es
sehr weit verbreitet, robust und mit Ethernet-Technik über viele
Generationen kompatibel ist. Für eine zuverlässige Verbindung ist neben
der richtigen Pinbelegung auch eine saubere mechanische Verarbeitung
entscheidend, da schlecht gecrimpte Stecker häufige FehlerQuellen sind.

#### Was ist ein Crossover-Kabel?

Ein *Patchkabel* (*Straight Through*) ist an beiden Enden gleich
aufgelegt (A-A oder B-B). Solche Kabel werden für die meisten
Standardverbindungen verwendet, z. B. PC zu Switch oder Switch zu
Router.

Ein *Crossover-Kabel* ist an einem Ende nach T568A und am anderen Ende
nach T568B aufgelegt. Dadurch werden Sende- und Empfangsadern gekreuzt.
Früher war das für direkte Verbindungen gleichartiger Geräte relevant
(z. B. PC zu PC oder Switch zu Switch). Heute erkennen viele Geräte den
Leitungstyp automatisch (*Auto-MDI/X*), sodass Crossover-Kabel deutlich
seltener benötigt werden.

In modernen Netzen ist es deshalb üblich, fast ausschliesslich
Patchkabel einzusetzen. Falls dennoch Verbindungsprobleme auftreten,
lohnt sich ein Blick auf Layer 1: Sind die Adern korrekt aufgelegt? Ist
das Kabel beschädigt? Ist die Strecke zu lang? Viele scheinbar
“logische” Netzwerkprobleme haben letztlich eine physikalische Ursache.

### Glasfaser (LWL)

LWL (*Lichtwellenleiter*) überträgt Daten als Lichtimpulse statt als
elektrische Signale. Dadurch ist die Übertragung praktisch unempfindlich
gegen elektromagnetische Störungen und erlaubt sehr hohe Datenraten über
grosse Distanzen. Typische Einsatzbereiche sind Backbone-Verbindungen,
Verbindungen zwischen Gebäuden, Provider-Netze und WAN-Strecken.

Eine Glasfaser besteht vereinfacht aus drei Bereichen: dem *Core*
(Kern), in dem das Licht geführt wird, dem *Cladding* (Mantel), das das
Licht durch Totalreflexion im Kern hält, und einer schützenden
Buffer-Schicht. Eine typische Singlemode-Angabe wie 9/125 um bedeutet 9
um Kerndurchmesser und 125 um Manteldurchmesser.

Ein grosser Vorteil von LWL ist die geringe Dämpfung pro Kilometer im
Vergleich zu elektrischen Leitungen. Dadurch sind sehr lange
Verbindungen möglich, ohne dass in kurzen Abständen aktive Verstärker
nötig werden. Gleichzeitig ist Glasfaser galvanisch getrennt, was bei
Potentialunterschieden zwischen Gebäuden oder bei blitzgeführdeten
Umgebungen ein Sicherheits- und Zuverlässigkeitsvorteil sein kann.

#### Singlemode vs. Multimode

Multimode-Fasern besitzen einen grösseren Kern (typisch 50/125 um oder
62.5/125 um). Dadurch können mehrere Lichtmoden gleichzeitig laufen, was
die Technik günstiger macht, aber Reichweite und maximale Datenrate
stärker begrenzt. Typische Wellenlängen liegen bei 850 nm, übliche
Einsätze sind LAN- und Gebäudeverkabelungen mit Strecken bis etwa einige
hundert Meter.

Singlemode-Fasern besitzen einen sehr kleinen Kern (typisch 9/125 um).
Es wird im Wesentlichen nur eine Lichtmode geführt, wodurch deutlich
höhere Reichweiten und sehr hohe Datenraten möglich sind. Typische
Wellenlängen sind 1310 nm und 1550 nm, eingesetzt wird Singlemode vor
allem im Backbone, bei Providern und in WAN-Umgebungen.

Multimode ist besonders wirtschaftlich auf kürzeren Strecken innerhalb
von Gebäuden oder Rechenzentren, während Singlemode für längere
Distanzen und skalierbare Infrastruktur bevorzugt wird. Die konkrete
Wahl hängt von der geplanten Strecke, den benötigten Datenraten und den
Gesamtkosten aus Transceivern, Patchfeldern und Verlegeaufwand ab. Für
langfristige Projekte wird oft bewusst eine leistungsfähigere
Infrastruktur eingeplant, um spätere Upgrades ohne neue Verkabelung zu
ermöglichen.

| **Merkmal**       | **Singlemode (OS1/OS2)**    | **Multimode (OM1-OM5)**                |
|:------------------|:----------------------------|:---------------------------------------|
| Kerndurchmesser   | ca. 9 um                    | 50 oder 62.5 um                        |
| LichtQuelle       | Laser (typ. 1310/1550 nm)   | LED/VCSEL (typ. 850/1300 nm)           |
| Reichweite        | Sehr hoch (km-Bereich)      | Eher kurz bis mittel (typ. Gebäude/DC) |
| Typischer Einsatz | Backbone, Campus, FTTH, WAN | Rechenzentrum, Inhouse-Verkabelung     |

<figure>

<figcaption>Singlemode vs. Multimode (Quelle: Wikimedia Commons, Datei:
Multimode_vs_Single_Mode_Fiber.png)</figcaption>
</figure>

#### Steckertypen bei Glasfaser

In der Praxis kommen mehrere Steckertypen vor, unter anderem LC, SC, ST,
FC und E2000. Farbkennzeichnungen helfen bei der schnellen Zuordnung:
Häufig steht Blau für Singlemode (UPC), Grün für Singlemode APC und
Beige/Weiss für Multimode-Systeme (herstellerabhängig).

Bei Glasfaser ist Sauberkeit ein zentrales Qualitätskriterium. Schon
kleine Verunreinigungen auf den Stirnflächen der Stecker können Dämpfung
erhöhen oder Reflexionen verursachen. Deshalb gehören Sichtkontrolle,
Reinigung und fachgerechte Messung (z. B. Dämpfungsmessung) zur
professionellen Inbetriebnahme und Wartung.

#### Sicherheitswarnung bei Glasfaser

In Glasfasersystemen werden häufig Laser als Lichtquelle eingesetzt.
Auch wenn die Leistung in vielen Anwendungen relativ gering ist, kann
direkte oder reflektierte Laserstrahlung das Auge schädigen, besonders
weil Infrarotstrahlung nicht sichtbar ist. Deshalb gilt: Niemals in
offene Faserenden, Steckverbinder oder aktive SFP-Module schauen.

Für die sichere Handhabung sollten unbenutzte Ports immer mit
Schutzkappen verschlossen, Steckflächen nur mit geeignetem
Reinigungswerkzeug gereinigt und Verbindungen vor Arbeiten möglichst
spannungsfrei geschaltet werden. Bei Messungen oder Fehlersuche sind
geeignete Prüfgeräte (z. B. Leistungsmesser/Visual Fault Locator) und
eine fachgerechte Arbeitsweise wichtig, statt “mit dem Auge” zu prüfen.

### Funk (WLAN)

WLAN (*Wireless Local Area Network*) nutzt auf Layer 1
elektromagnetische Wellen statt physischer Leitungen. Auch hier werden
technisch betrachtet Bits übertragen, nur eben drahtlos. WLAN basiert
auf IEEE 802.11 und ermöglicht mobile, flexible Netzzugänge ohne
Verkabelung bis zum Endgerät.

- **2.4 GHz:** Gute Reichweite und Durchdringung, aber störanfälliger
  und meist geringere nutzbare Datenrate.

- **5 GHz:** Höhere Datenraten, mehr Kanäle, geringere Reichweite als
  2.4 GHz.

- **6 GHz (Wi-Fi 6E/7):** Sehr viele freie Kanäle, hohe Datenraten,
  kurze Reichweite und hohe Dämpfung.

Bei WLAN gilt grundsätzlich: Höhere Frequenzen bieten oft mehr
Geschwindigkeit und mehr nutzbare Kanäle, haben jedoch meist eine
geringere Reichweite und schlechtere Wanddurchdringung. Störungen
entstehen unter anderem durch Wände, andere WLAN-Netze, Bluetooth-Geräte
und Mikrowellen.

Im Unterschied zu Kupfer oder LWL ist Funk ein geteiltes Medium: Mehrere
Geräte nutzen dieselbe Luftschnittstelle und beeinflussen sich
gegenseitig. Deshalb hängt die erreichbare Datenrate nicht nur vom
Standard ab, sondern stark von Kanalplanung, Abstand zum Access Point,
Auslastung und StörQuellen. Eine gute WLAN-Planung umfasst daher die
Wahl geeigneter Kanäle, sinnvolle Sendeleistung, ausreichende Anzahl von
Access Points und die Berücksichtigung baulicher Gegebenheiten.

WLAN ist auf Layer 1 physikalisch einzuordnen, auch wenn in der Praxis
natürlich weitere Protokollschichten beteiligt sind. Für den Anwender
ist besonders wichtig: Hohe Brutto-Datenraten auf dem Datenblatt
bedeuten nicht automatisch dieselbe Nettoleistung im Alltag. Reale
Bedingungen vor Ort sind entscheidend.

## Vergleich Kupfer vs. Glasfaser (LWL) vs. Funk

| **Kriterium**    | **Kupfer (Twisted Pair)**         | **Glasfaser (LWL)**                      | **Funk (WLAN)**                                      |
|:-----------------|:----------------------------------|:-----------------------------------------|:-----------------------------------------------------|
| Medium           | Elektrische Signale               | Lichtimpulse                             | Funkwellen                                           |
| Geschwindigkeit  | Hoch                              | Sehr hoch                                | Mittel bis hoch (abhängig von Standard und Umgebung) |
| Reichweite       | Typisch bis 100 m                 | Von hunderten Metern bis viele Kilometer | Variabel, stark umgebungsabhängig                    |
| Störanfälligkeit | Mittel, mit Schirmung reduzierbar | Gering (EMI-unempfindlich)               | Hoch, da geteiltes Medium                            |
| Kosten           | Gering bis mittel                 | Höher                                    | Mittel                                               |

Der Vergleich zeigt, dass es kein universell “bestes” Medium für alle
Fälle gibt. Kupfer ist kostengünstig, robust und für klassische
Arbeitsplatzanschlüsse sehr geeignet. Glasfaser spielt ihre Stärken bei
grossen Strecken, hoher Störsicherheit und sehr hohen Bandbreiten aus.
WLAN bietet maximale Flexibilität und Mobilität, ist aber stärker von
der Umgebung und der Netzplanung abhängig.

In realen Netzwerken werden diese Medien meist kombiniert: Endgeräte
nutzen WLAN oder Kupfer, Etagenverteiler sind per Kupfer oder LWL
angebunden, und das zentrale Backbone wird häufig mit Glasfaser
realisiert. Diese Kombination verbindet Wirtschaftlichkeit, Leistung und
Flexibilität.

### Wann nimmt man was?

- **Kupfer (Twisted Pair):** Arbeitsplatzverkabelung, kurze bis mittlere
  Strecken in Gebäuden, gutes Preis-Leistungs-Verhältnis.

- **LWL:** Backbone zwischen Verteilern/Gebäuden, lange Strecken, hohe
  Bandbreite und Umgebungen mit starker EMV-Belastung.

- **WLAN:** Mobile Endgeräte, flexible Erweiterung ohne neue Kabelwege,
  sinnvoll als Ergänzung zur strukturierten Verkabelung.

## Zusammenfassung

Der OSI-Layer 1 bildet die technische Basis jeder Netzkommunikation.
Hier wird festgelegt, wie Bits physikalisch übertragen werden, welche
Medien verwendet werden und welche Grenzwerte für Reichweite,
Störfestigkeit und Datenrate gelten. Obwohl diese Schicht keine
Dateninhalte interpretiert, entscheidet sie wesentlich über die
Gesamtleistung eines Netzwerks.

Aus technischer Sicht ist Layer 1 damit mehr als “nur Kabel”. Er umfasst
alle physikalischen Rahmenbedingungen, die eine Verbindung erst möglich
machen: Signalformen, Materialien, Stecksysteme, Dämpfung, Abschirmung
und Umgebungsbedingungen. Werden diese Punkte bei Planung und
Installation sauber berücksichtigt, arbeiten auch die höheren Schichten
stabil und effizient.

Die Wahl des passenden Mediums entscheidet direkt über Reichweite,
Stabilität, Datenrate und Kosten:

- Kupfer ist für viele Standard-LAN-Szenarien wirtschaftlich und
  praxisnah.

- LWL ist bei grossen Distanzen, hohen Bandbreiten und EMV-Anforderungen
  klar im Vorteil.

- WLAN erweitert Layer 1 um Mobilität, benötigt aber sorgfältige
  Funkplanung.

Für die Praxis bedeutet das: Gute Netzwerke entstehen durch die richtige
Kombination aus Medium, Topologie und sauberer Umsetzung. Gerade bei
Fehlersuche, Erweiterung oder Modernisierung lohnt sich deshalb immer
ein strukturierter Blick auf Layer 1.
