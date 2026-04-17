- [OSI-Modell und Layer 2](#osi-modell-und-layer-2)
  - [Grundlagen und Einordnung](#grundlagen-und-einordnung)
  - [MAC-Adressen](#mac-adressen)
    - [Aufbau und Struktur](#aufbau-und-struktur)
    - [Sonderbits in der MAC-Adresse](#sonderbits-in-der-mac-adresse)
    - [Adresstypen im Überblick](#adresstypen-im-überblick)
    - [Broadcast-Adresse](#broadcast-adresse)
    - [MAC-Adressen in der Praxis](#mac-adressen-in-der-praxis)
  - [Ethernet](#ethernet)
    - [Geschichte und Standard](#geschichte-und-standard)
    - [Der Ethernet-Frame](#der-ethernet-frame)
    - [EtherType – Protokollkennung](#ethertype-protokollkennung)
    - [Jumbo Frames](#jumbo-frames)
    - [Half-Duplex, Full-Duplex und
      CSMA/CD](#half-duplex-full-duplex-und-csmacd)
  - [Switches](#switches)
    - [Was ist ein Switch?](#was-ist-ein-switch)
    - [Die drei Grundoperationen eines
      Switches](#die-drei-grundoperationen-eines-switches)
    - [Collision Domains und Broadcast
      Domains](#collision-domains-und-broadcast-domains)
  - [Switching Table
    (MAC-Adresstabelle)](#switching-table-mac-adresstabelle)
    - [Aufbau der Tabelle](#aufbau-der-tabelle)
    - [Ablauf: Wie lernt ein Switch?](#ablauf-wie-lernt-ein-switch)
    - [Aging-Timer](#aging-timer)
    - [CAM-Table-Overflow
      (MAC-Flooding-Angriff)](#cam-table-overflow-mac-flooding-angriff)
  - [VLANs – Virtual Local Area
    Networks](#vlans-virtual-local-area-networks)
    - [Was ist ein VLAN?](#was-ist-ein-vlan)
    - [IEEE 802.1Q – VLAN-Tagging](#ieee-802.1q-vlan-tagging)
    - [Access Port und Trunk Port](#access-port-und-trunk-port)
  - [Spanning Tree Protocol (STP)](#spanning-tree-protocol-stp)
    - [Das Problem: Loops im
      Layer-2-Netz](#das-problem-loops-im-layer-2-netz)
    - [STP-Grundprinzip](#stp-grundprinzip)
    - [STP-Port-Zustände](#stp-port-zustände)
    - [RSTP – Rapid Spanning Tree
      Protocol](#rstp-rapid-spanning-tree-protocol)
  - [Zusammenfassung](#zusammenfassung)

# OSI-Modell und Layer 2

## Grundlagen und Einordnung

Layer 2 des OSI-Modells wird als *Data Link Layer* oder
**Sicherungsschicht** bezeichnet. Während Layer 1 ausschliesslich für
die physikalische Übertragung von Bits zuständig ist, übernimmt Layer 2
die erste inhaltliche Strukturierung dieser Bits: Er fasst den rohen
Bitstrom zu sinnvollen Einheiten zusammen, den sogenannten *Frames*
(Rahmen). Damit stellt er Layer 3 (dem Netzwerk-Layer) einen
zuverlässigeren, adressierbaren Dienst zur Verfügung.

Die Protokolldateneinheit (PDU) auf Layer 2 ist der **Frame**. Im
Gegensatz zu Layer 3, der mit logischen IP-Adressen arbeitet, verwendet
Layer 2 **physikalische Adressen** – die sogenannten MAC-Adressen. Diese
sind eindeutig jedem Netzwerkinterface zugeordnet und ermöglichen die
direkte Kommunikation innerhalb eines Netzsegments.

#### Aufgaben des Data Link Layer

Layer 2 übernimmt mehrere klar definierte Aufgaben:

- **Framing:** Der Bitstrom von Layer 1 wird in strukturierte Frames
  unterteilt, die Anfang und Ende klar kennzeichnen.

- **Physikalische Adressierung:** Jeder Frame enthält eine Quell- und
  eine Ziel-MAC-Adresse, sodass im Netzsegment festgestellt werden kann,
  woher ein Frame stammt und für wen er bestimmt ist.

- **Fehlererkennung:** Durch Prüfsummen (z. B. CRC/FCS) kann Layer 2
  erkennen, ob ein Frame während der Übertragung beschädigt wurde.
  Fehlerhafte Frames werden verworfen.

- **Flusskontrolle:** In manchen Implementierungen regelt Layer 2 den
  Datenfluss zwischen Sender und Empfänger, um Überlastung zu vermeiden.

- **Medienzugriffssteuerung (MAC):** Layer 2 regelt, wer wann auf das
  gemeinsame Übertragungsmedium zugreift. Dies ist besonders bei
  Halbduplex-Übertragungen wichtig.

#### Einordnung im OSI-Modell

Layer 2 gliedert sich klassisch in zwei Unterschichten:

- **LLC (Logical Link Control):** Die obere Teilschicht sorgt für die
  verbindungsorientierte oder verbindungslose Kommunikation und stellt
  den Übergang zu Layer 3 her.

- **MAC (Media Access Control):** Die untere Teilschicht verwaltet die
  physikalischen Adressen und steuert den Zugriff auf das
  Übertragungsmedium.

Im Alltag ist vor allem die MAC-Unterschicht präsent, da sie direkt mit
den Ethernet-Frames und den MAC-Adressen verknüpft ist, mit denen wir
täglich arbeiten.

## MAC-Adressen

### Aufbau und Struktur

Eine MAC-Adresse (*Media Access Control Address*) ist eine 48 Bit
(6 Byte) lange physikalische Adresse, die jedem Netzwerkinterface
weltweit eindeutig zugeordnet ist. Sie wird üblicherweise hexadezimal
geschrieben, wobei die sechs Bytes durch Bindestriche oder Doppelpunkte
getrennt werden:

<div class="center">

`AA:BB:CC:DD:EE:FF` oder `AA-BB-CC-DD-EE-FF`

</div>

Die 48 Bits teilen sich in zwei gleichgrosse Hälften:

- **OUI (Organizationally Unique Identifier):** Die ersten 3 Bytes
  (24 Bit) kennzeichnen den Hersteller des Netzwerkinterfaces. Die IEEE
  vergibt diese Kennung an Hersteller. Beispiele: `00:1A:2B` steht für
  Cisco, `AC:DE:48` für ein Apple-Gerät.

- **NIC-spezifischer Teil:** Die letzten 3 Bytes (24 Bit) werden vom
  Hersteller frei vergeben und eindeutig jedem Interface zugeordnet.

<figure>

<figcaption>Aufbau einer MAC-48-Adresse mit OUI und Interface-Teil
(Quelle: Wikimedia Commons, Datei: MAC-48_Address.svg, CC BY-SA
2.5)</figcaption>
</figure>

### Sonderbits in der MAC-Adresse

Das erste Byte der MAC-Adresse enthält zwei besondere Steuerbits, die im
niedrigstwertigen Bereich liegen:

- **Bit 0 (Unicast/Multicast-Bit):** Ist dieses Bit `0`, handelt es sich
  um eine *Unicast*-Adresse – der Frame ist für genau ein Gerät
  bestimmt. Ist es `1`, ist es eine *Multicast*-Adresse: Der Frame
  richtet sich an eine Gruppe von Geräten, die sich für diese
  Multicast-Gruppe registriert haben.

- **Bit 1 (G/L-Bit, Global/Locally Administered):** Ist dieses Bit `0`,
  ist die Adresse global eindeutig (vom Hersteller vergeben). Ist es
  `1`, wurde die Adresse lokal manuell konfiguriert oder per Software
  zugewiesen.

### Adresstypen im Überblick

| **Adresstyp**        | **Beispiel**              | **Bedeutung**                          |
|:---------------------|:--------------------------|:---------------------------------------|
| Unicast              | `AC:DE:48:12:34:56`       | Genau ein Empfänger-Interface          |
| Multicast            | `01:00:5E:xx:xx:xx`       | Gruppe von Empfängern (IPv4-Multicast) |
| Broadcast            | `FF:FF:FF:FF:FF:FF`       | Alle Geräte im Netzsegment             |
| Locally Administered | z. B. `02:xx:xx:xx:xx:xx` | Manuell vergeben, lokal gültig         |

### Broadcast-Adresse

Die Adresse `FF:FF:FF:FF:FF:FF` ist die Layer-2-Broadcast-Adresse. Ein
Frame mit dieser Zieladresse wird von **allen Geräten** im selben
Netzsegment angenommen und verarbeitet. Typische Anwendungsfälle sind
ARP-Anfragen (*Address Resolution Protocol*), bei denen ein Gerät nach
der MAC-Adresse zu einer bekannten IP-Adresse fragt, ohne den Empfänger
vorher zu kennen.

Broadcasts werden von Switches weitergeleitet (innerhalb des VLANs),
aber von Routern nicht – ein Router bildet eine
*Broadcast-Domain-Grenze*. Das Konzept der Broadcast-Domain ist daher
eng mit Layer 2 verknüpft.

### MAC-Adressen in der Praxis

Obwohl MAC-Adressen theoretisch weltweit eindeutig und fest in der
Hardware gespeichert sind, lassen sich viele Betriebssysteme und
Netzwerkadapter so konfigurieren, dass sie eine andere (“gespoofde”)
MAC-Adresse nach aussen signalisieren. Dies kann für Tests oder
Datenschutzmassnahmen nützlich sein, birgt aber auch Sicherheitsrisiken
(z. B. Umgehung von MAC-basierten Zugangsbeschränkungen). Deshalb sollte
eine rein MAC-basierte Sicherheitskontrolle immer durch weitere
Mechanismen ergänzt werden.

## Ethernet

### Geschichte und Standard

Ethernet ist die heute bei weitem am häufigsten eingesetzte
LAN-Technologie und bildet die technische Grundlage für den Grossteil
moderner Netzwerkkommunikation. Die Ursprünge liegen in den 1970er
Jahren bei Xerox PARC, wo Robert Metcalfe und seine Kollegen das
Grundprinzip des CSMA/CD-basierten Medienzugriffs entwickelten. **1980**
präsentierten Xerox, Intel und DEC gemeinsam den ersten
Ethernet-Standard (“DIX Ethernet” oder “Ethernet II”) mit einer
Datenrate von 10 Mbit/s.

**1983** veröffentlichte das IEEE den Standard *IEEE 802.3*, der
Ethernet formal standardisierte. Seitdem wurde der Standard stetig
weiterentwickelt: von 10 Mbit/s über 100 Mbit/s (Fast Ethernet, 802.3u),
1 Gbit/s (Gigabit Ethernet, 802.3z/ab), 10 Gbit/s (802.3ae) bis hin zu
100 Gbit/s und mehr für Rechenzentren. Heute ist Ethernet nicht mehr auf
Koaxialkabel beschränkt, sondern läuft auf Twisted-Pair-Kabeln,
Glasfaser und in Datenzentren auf speziellen
Hochgeschwindigkeitsverbindungen.

### Der Ethernet-Frame

Die Grundstruktur des Ethernet-II-Frames (der im Alltag verwendeten
Variante) sieht folgendermassen aus:

<figure>

<figcaption>Ethernet-II-Frame-Struktur (Quelle: Wikimedia Commons,
Datei: Ethernet_II_Frame_Structure.png, CC BY-SA 4.0)</figcaption>
</figure>

| **Feld**                    | **Grösse**   | **Bedeutung**                                                                     |
|:----------------------------|:-------------|:----------------------------------------------------------------------------------|
| Preamble                    | 7 Byte       | Abwechselnde 1/0-Bits zur Taktsynchronisation des Empfängers                      |
| SFD (Start Frame Delimiter) | 1 Byte       | Markiert den Beginn des eigentlichen Frames (`10101011`)                          |
| Destination MAC             | 6 Byte       | MAC-Adresse des Empfängers (Unicast, Multicast oder Broadcast)                    |
| Source MAC                  | 6 Byte       | MAC-Adresse des Senders                                                           |
| EtherType / Länge           | 2 Byte       | Wert $\geq$ 1536: EtherType (Protokoll-ID); Wert $<$ 1536: Nutzdatenlänge (802.3) |
| Payload (Nutzdaten)         | 46–1500 Byte | Nutzdaten, z. B. ein IP-Paket. Mindestens 46 Byte (ggf. mit Padding aufgefüllt)   |
| FCS (Frame Check Sequence)  | 4 Byte       | CRC-32-Prüfsumme über Dst/Src/EtherType/Payload zur Fehlererkennung               |

Preamble und SFD werden in der Regel nicht als Teil des eigentlichen
Frames gezählt – die Mindestgrösse des “eigentlichen” Frames beträgt
deshalb **64 Byte** (6+6+2+46+4), die maximale Grösse **1518 Byte**
(ohne 802.1Q-Tag). Frames unter 64 Byte gelten als *Runt Frames* und
werden verworfen.

### EtherType – Protokollkennung

Das EtherType-Feld im Ethernet-Frame teilt dem Empfänger mit, welches
Layer-3-Protokoll im Payload steckt. Wichtige Werte:

| **EtherType-Wert** | **Protokoll**                     |
|:-------------------|:----------------------------------|
| `0x0800`           | IPv4                              |
| `0x86DD`           | IPv6                              |
| `0x0806`           | ARP (Address Resolution Protocol) |
| `0x8100`           | 802.1Q VLAN-Tag                   |
| `0x8847`           | MPLS (Unicast)                    |

### Jumbo Frames

In Standardnetzwerken ist die maximale Nutzlast (MTU) auf 1500 Byte
begrenzt. In Hochleistungs-Umgebungen (z. B. Rechenzentren,
Storage-Netzwerke) werden häufig *Jumbo Frames* mit einer MTU von bis zu
9000 Byte eingesetzt. Das reduziert den Overhead durch weniger
Frame-Header pro übertragener Datenmenge und entlastet die CPU. Wichtig:
Alle beteiligten Geräte im Pfad müssen Jumbo Frames unterstützen und
entsprechend konfiguriert sein.

### Half-Duplex, Full-Duplex und CSMA/CD

In frühen Ethernet-Netzwerken arbeiteten alle Geräte an einem
gemeinsamen Koaxialkabel oder über einen Hub im **Halbduplex-Modus**:
Senden und Empfangen war nicht gleichzeitig möglich. Um Kollisionen zu
handhaben, wurde *CSMA/CD* (*Carrier Sense Multiple Access with
Collision Detection*) eingesetzt:

1.  Gerät horcht, ob das Medium frei ist (*Carrier Sense*).

2.  Ist es frei, beginnt es zu senden (*Multiple Access*).

3.  Kollision erkannt: Alle Sender stoppen, senden ein Jam-Signal und
    warten eine zufällige Zeit (*Backoff*), bevor sie es erneut
    versuchen (*Collision Detection*).

Moderne geswitchte Netzwerke arbeiten fast ausschliesslich im
**Full-Duplex-Modus**: Senden und Empfangen erfolgen gleichzeitig über
separate Leitungspaare. Kollisionen sind damit praktisch ausgeschlossen,
und CSMA/CD spielt in diesem Kontext keine Rolle mehr. Die maximale
theoretische Datenrate wird vollständig ausgenutzt.

## Switches

### Was ist ein Switch?

Ein **Switch** ist ein aktives Layer-2-Netzwerkgerät, das Frames anhand
von MAC-Adressen gezielt an den richtigen Port weiterleitet. Im
Gegensatz zu einem **Hub**, der jeden empfangenen Frame einfach an alle
anderen Ports dupliziert (“flooding”), trifft ein Switch eine
intelligente Weiterleitungsentscheidung: Er lernt, welche MAC-Adresse an
welchem Port erreichbar ist, und sendet Frames nur an den jeweils
benötigten Port.

<figure>

<figcaption>Kollisions- und Broadcast-Domains bei Hub, Switch
(Halbduplex) und Switch (Vollduplex) (Quelle: Wikimedia Commons, Datei:
Ethernet-broadcast-collision.svg, CC BY-SA 4.0)</figcaption>
</figure>

### Die drei Grundoperationen eines Switches

Jeder Layer-2-Switch arbeitet nach denselben drei Grundprinzipien:

#### Learning (Lernen)

Wenn ein Frame am Switch eintrifft, liest der Switch die
**Quell-MAC-Adresse** des Frames aus und trägt sie zusammen mit dem
eingehenden Port und einem Zeitstempel in die *MAC-Adresstabelle* (auch
*CAM-Tabelle*) ein. So weiss der Switch in Zukunft, an welchem Port
dieses Gerät erreichbar ist.

#### Flooding (Fluten)

Ist die **Ziel-MAC-Adresse** eines eingehenden Frames noch **nicht** in
der MAC-Adresstabelle vorhanden (unbekanntes Ziel), sendet der Switch
den Frame an **alle Ports ausser dem Eingangsport**. Ebenso werden
Broadcast-Frames (`FF:FF:FF:FF:FF:FF`) und Multicast-Frames (sofern kein
IGMP Snooping konfiguriert ist) stets geflutet. Dieses Verfahren
garantiert, dass der Frame sein Ziel auch dann erreicht, wenn der Switch
den entsprechenden Port noch nicht kennt.

#### Forwarding / Filtering (Weiterleiten und Filtern)

Ist die Ziel-MAC-Adresse in der Tabelle bekannt, leitet der Switch den
Frame **gezielt** an den zugehörigen Port weiter. Liegt Quell- und
Ziel-Port auf demselben Port (z. B. beide Geräte hängen an einem
weiteren Switch), wird der Frame verworfen (*Filtering*), da der
Empfänger den Frame bereits direkt erhalten hat.

### Collision Domains und Broadcast Domains

Ein wichtiger Vorteil von Switches gegenüber Hubs liegt im Umgang mit
Kollisions- und Broadcast-Domains:

| **Gerät** | **Collision Domain**                            | **Broadcast Domain**                                  |
|:----------|:------------------------------------------------|:------------------------------------------------------|
| Hub       | Eine gemeinsame Collision Domain für alle Ports | Eine gemeinsame Broadcast Domain                      |
| Switch    | Jeder Port bildet eine eigene Collision Domain  | Alle Ports in einem VLAN teilen eine Broadcast Domain |
| Router    | Jeder Port bildet eine eigene Collision Domain  | Jeder Port bildet eine eigene Broadcast Domain        |

Durch die Segmentierung der Collision Domains eliminiert ein Switch
Kollisionen bei Full-Duplex-Betrieb vollständig. Die Broadcast Domain
bleibt jedoch zunächst erhalten – alle Ports in einem VLAN empfangen
Broadcasts. Erst ein Router oder eine Layer-3-Switch-Funktion grenzt
Broadcast-Domains voneinander ab.

## Switching Table (MAC-Adresstabelle)

### Aufbau der Tabelle

Die MAC-Adresstabelle (auch *CAM-Table*, *Forwarding Database* oder kurz
FDB) ist das Kernelement eines Switches. Sie enthält die Zuordnung von
MAC-Adressen zu Ports. Ein typischer Eintrag besteht aus folgenden
Feldern:

| **Feld**    | **Bedeutung**                                                                                                     |
|:------------|:------------------------------------------------------------------------------------------------------------------|
| MAC-Adresse | Die gelernte Quell-MAC-Adresse des Endgeräts                                                                      |
| Port        | Der physikalische Port, an dem das Gerät mit dieser MAC-Adresse zuletzt einen Frame gesendet hat                  |
| VLAN        | Das VLAN, in dem dieser Eintrag gültig ist (MAC-Adressen können in verschiedenen VLANs getrennt verwaltet werden) |
| Alter (Age) | Zeitstempel oder Zähler seit dem letzten Auftreten; nach Ablauf des *Aging-Timers* wird der Eintrag gelöscht      |

Auf einem Cisco-Switch lässt sich die MAC-Adresstabelle beispielsweise
mit folgendem Befehl anzeigen:

    Switch# show mac address-table

### Ablauf: Wie lernt ein Switch?

Das folgende Beispiel zeigt Schritt für Schritt, wie ein Switch mit vier
angeschlossenen Geräten (PC-A an Port 1, PC-B an Port 2, PC-C an Port 3,
PC-D an Port 4) seine MAC-Tabelle aufbaut:

1.  **Ausgangszustand:** Die MAC-Tabelle ist leer. Kein Gerät ist bisher
    bekannt.

2.  **PC-A sendet einen Frame an PC-C.** Der Switch empfängt den Frame
    an Port 1 und trägt die Quell-MAC von PC-A in die Tabelle ein:
    `MAC-A `$\rightarrow$` Port 1`. Die Ziel-MAC von PC-C ist unbekannt
    $\Rightarrow$ Flooding an Ports 2, 3 und 4.

3.  **PC-C antwortet.** Der Switch empfängt den Antwort-Frame an Port 3
    und lernt: `MAC-C `$\rightarrow$` Port 3`. Die Ziel-MAC ist jetzt
    bekannt (Port 1) $\Rightarrow$ Forwarding nur an Port 1.

4.  **PC-B sendet an PC-A.** Der Switch lernt
    `MAC-B `$\rightarrow$` Port 2`, leitet den Frame direkt an Port 1
    weiter.

5.  **Ergebnis:** Der Switch kennt nun MAC-A, MAC-B und MAC-C.
    Zukünftige Frames zwischen diesen Geräten werden gezielt
    weitergeleitet, ohne andere Ports zu belasten. PC-D ist noch
    unbekannt.

### Aging-Timer

Einträge in der MAC-Tabelle sind nicht dauerhaft. Nach einer definierten
Zeit ohne Aktivität – dem *Aging-Timer* (typisch 300 Sekunden) – werden
Einträge automatisch gelöscht. Hintergrund: Geräte können den Port
wechseln (z. B. Laptop umgesteckt), und der Switch müsste sonst immer
noch den alten, falschen Port verwenden. Durch das Aging bleibt die
Tabelle dynamisch und aktuell.

### CAM-Table-Overflow (MAC-Flooding-Angriff)

Die MAC-Adresstabelle eines Switches hat eine begrenzte Grösse (abhängig
vom Modell: oft einige tausend bis hunderttausend Einträge). Ein
Angreifer kann durch gezieltes Senden von Frames mit vielen
unterschiedlichen (gefälschten) Quell-MAC-Adressen die Tabelle
vollständig füllen (*MAC-Flooding*). Ist die Tabelle voll, kann der
Switch neue Adressen nicht mehr lernen und muss alle Frames fluten – er
verhält sich wie ein Hub. Dadurch kann der Angreifer den Datenverkehr
aller anderen Geräte mitlesen.

Schutz bieten *Port Security*-Funktionen auf verwalteten Switches, die
die maximale Anzahl von MAC-Adressen pro Port begrenzen und bei
Überschreitung den Port abschalten oder Alarm auslösen.

## VLANs – Virtual Local Area Networks

### Was ist ein VLAN?

Ein **VLAN** (*Virtual Local Area Network*) erlaubt es, ein
physikalisches Netzwerk logisch in mehrere voneinander isolierte
Netzsegmente aufzuteilen, ohne dass dafür separate physikalische
Hardware erforderlich ist. Geräte in verschiedenen VLANs können **nicht
direkt auf Layer 2 kommunizieren** – sie verhalten sich so, als wären
sie an physikalisch getrennten Switches angeschlossen. Für die
Kommunikation zwischen VLANs ist ein Layer-3-Gerät (Router oder
Layer-3-Switch) erforderlich.

Typische Gründe für den Einsatz von VLANs:

- **Sicherheit:** Trennung von Abteilungen (z. B. Verwaltung,
  Produktion, Gäste-WLAN).

- **Performance:** Reduzierung der Broadcast-Domain – weniger Geräte
  empfangen Broadcasts, die sie nicht benötigen.

- **Flexibilität:** Logische Gruppenbildung unabhängig von der
  physikalischen Position der Geräte.

- **Netzmanagement:** Einfacheres Troubleshooting durch übersichtliche
  Segmentierung.

### IEEE 802.1Q – VLAN-Tagging

Der Standard *IEEE 802.1Q* definiert, wie VLAN-Informationen in
Ethernet-Frames kodiert werden. Dazu wird ein 4 Byte grosser
**VLAN-Tag** in den Frame eingefügt, direkt nach der Quell-MAC-Adresse:

<figure>

<figcaption>Ethernet-Frame vor und nach Einfügen des 802.1Q-VLAN-Tags
(Quelle: Wikimedia Commons, Datei: Ethernet_802.1Q_Insert.svg, CC BY-SA
3.0)</figcaption>
</figure>

Der 4-Byte-Tag gliedert sich in folgende Felder:

| **Feld** | **Grösse** | **Bedeutung**                                                                                    |
|:---------|:-----------|:-------------------------------------------------------------------------------------------------|
| TPID     | 16 Bit     | Tag Protocol Identifier: immer `0x8100`, kennzeichnet den Frame als 802.1Q-getaggt               |
| PCP      | 3 Bit      | Priority Code Point (IEEE 802.1p): Priorisierung des Frames (0–7, 7 = höchste Priorität)         |
| DEI      | 1 Bit      | Drop Eligible Indicator: Kennzeichnet Frames, die bei Überlast bevorzugt verworfen werden dürfen |
| VID      | 12 Bit     | VLAN Identifier: Nummer des VLANs (0–4095, nutzbar: 1–4094)                                      |

Da der Tag den Frame vergrössert, steigt die maximale Framegrösse bei
802.1Q-getaggten Frames auf **1522 Byte**.

### Access Port und Trunk Port

Auf einem Switch unterscheidet man grundsätzlich zwei Port-Typen:

- **Access Port:** Verbindet ein Endgerät (PC, Drucker, IP-Telefon) mit
  dem Switch. Der Port gehört genau einem VLAN an. Eingehende Frames
  werden vom Switch intern mit dem VLAN-Tag versehen; ausgehende Frames
  werden ohne Tag an das Endgerät weitergegeben. Das Endgerät selbst
  “sieht” keinen 802.1Q-Tag.

- **Trunk Port:** Verbindet zwei Switches (oder einen Switch mit einem
  Router/Layer-3-Switch) miteinander. Über einen Trunk können Frames
  **mehrerer VLANs gleichzeitig** übertragen werden. Jeder Frame trägt
  dabei seinen 802.1Q-Tag, damit das empfangende Gerät weiss, zu welchem
  VLAN der Frame gehört.

Diese Unterscheidung ist fundamental: Würde man einen Trunk-Port
fälschlicherweise als Access-Port konfigurieren (oder umgekehrt),
entstünden VLAN-Leckagen oder Konnektivitätsprobleme. Korrekte
Portkonfiguration ist daher ein zentraler Bestandteil des
Switch-Managements.

## Spanning Tree Protocol (STP)

### Das Problem: Loops im Layer-2-Netz

In professionellen Netzwerken werden Switches häufig redundant
verkabelt: Gibt es zwei physikalische Pfade zwischen zwei Switches, ist
einer davon als Backup gedacht. Auf Layer 3 lösen Routing-Protokolle
solche Redundanzen elegant – auf **Layer 2 hingegen entstehen ohne
Schutzmaßnahme gefährliche Schleifen (Loops)**.

Ein Layer-2-Loop hat fatale Folgen:

- **Broadcast-Sturm:** Broadcast-Frames werden endlos im Kreis gesendet
  und amplifiziert, bis das Netz zusammenbricht.

- **MAC-Tabellen-Instabilität:** Switches “sehen” dieselbe MAC-Adresse
  abwechselnd an verschiedenen Ports (je nachdem, von welcher Seite der
  Loop gerade kommt) und aktualisieren die Tabelle ständig
  (*MAC-Flapping*).

- **Netzausfall:** Die Bandbreite wird vollständig durch
  Schleifenverkehr verbraucht; reguläre Frames können nicht mehr
  übertragen werden.

### STP-Grundprinzip

Das **Spanning Tree Protocol (STP)**, standardisiert in *IEEE 802.1D*,
löst dieses Problem, indem es alle redundanten Pfade automatisch erkennt
und genau einen davon blockiert, bis der aktive Pfad ausfällt. Der
Algorithmus berechnet einen loop-freien Baum (*Spanning Tree*), der alle
Switches miteinander verbindet.

Die wichtigsten Konzepte:

- **Root Bridge:** Jedes STP-Netz hat genau eine Root Bridge – der
  “Wurzel-Switch” des Baums. Die Root Bridge wird durch die niedrigste
  *Bridge ID* bestimmt (Kombination aus Priorität und MAC-Adresse). Alle
  anderen Switches orientieren sich an ihr.

- **Root Port:** Jeder Nicht-Root-Switch wählt den Port mit dem
  günstigsten Pfad zur Root Bridge als *Root Port* – dieser ist stets im
  Forwarding-Zustand.

- **Designated Port:** Pro Netzsegment gibt es genau einen *Designated
  Port*, der Frames in Richtung Root Bridge weiterleitet. Auch dieser
  ist im Forwarding-Zustand.

- **Blocked Port:** Alle übrigen Ports, die zu Loops führen würden,
  werden in den *Blocking*-Zustand versetzt. Sie empfangen zwar
  STP-Nachrichten (BPDUs), leiten aber keine regulären Datenpakete
  weiter.

<figure>

<figcaption>Beispiel einer Spanning-Tree-Topologie in einem geswitchten
Ethernet (Quelle: Wikimedia Commons, Datei: Spanning_tree_topology.png,
CC BY-SA 3.0)</figcaption>
</figure>

<figure>

<figcaption>STP in Aktion: Root Bridge und blockierter Port (Quelle:
Wikimedia Commons, Datei: Spanning_tree_protocol_at_work_1.svg, CC BY
3.0)</figcaption>
</figure>

### STP-Port-Zustände

Ein STP-Port durchläuft beim Aktivieren mehrere Zustände:

| **Zustand** | **Beschreibung**                                                   |
|:------------|:-------------------------------------------------------------------|
| Blocking    | Port empfängt BPDUs, leitet keine Frames weiter. Schutz vor Loops. |
| Listening   | Port verarbeitet BPDUs aktiv, lernt aber noch keine MAC-Adressen.  |
| Learning    | Port lernt MAC-Adressen, leitet aber noch keine Frames weiter.     |
| Forwarding  | Port leitet Frames normal weiter. Regulärer Betriebszustand.       |
| Disabled    | Port ist administrativ deaktiviert.                                |

Der Übergang von Blocking bis Forwarding dauert bei klassischem STP
(802.1D) insgesamt bis zu **50 Sekunden**. In modernen Netzwerken ist
das zu langsam.

### RSTP – Rapid Spanning Tree Protocol

*IEEE 802.1w* (Rapid STP, RSTP) ist die Weiterentwicklung von STP und
bietet deutlich schnellere Konvergenz – typischerweise unter **1–2
Sekunden**. RSTP verwendet denselben Algorithmus, optimiert aber die
Aushandlung zwischen Switches durch einen direkten Handshake-Mechanismus
statt dem reinen Timeout-basierten Ansatz. RSTP ist vollständig
rückwärtskompatibel zu STP.

In aktuellen Netzwerken und auf modernen Switches wird RSTP
standardmässig verwendet. *MSTP* (Multiple Spanning Tree Protocol,
802.1s) geht noch einen Schritt weiter und erlaubt es, verschiedene
VLANs auf unterschiedliche Spanning Trees zu mappen, um Lastverteilung
über redundante Links zu ermöglichen.

## Zusammenfassung

Layer 2 des OSI-Modells bildet die erste intelligente Schicht der
Netzwerkkommunikation. Während Layer 1 rohe Bits überträgt,
strukturiert, adressiert und kontrolliert Layer 2 diese Daten auf
Segment-Ebene. Die zentralen Konzepte dieses Layers sind eng miteinander
verknüpft:

**MAC-Adressen** bilden die Grundlage für alle Layer-2-Kommunikation.
Ihre 48-Bit-Struktur mit OUI und Interface-Teil, kombiniert mit den
Sonderfunktionen für Unicast, Multicast und Broadcast, ermöglicht eine
eindeutige Adressierung innerhalb eines Netzsegments.

**Ethernet** und der zugehörige Frame-Aufbau standardisieren die
Struktur jeder übertragenen Dateneinheit. Die Felder Destination-MAC,
Source-MAC, EtherType und FCS machen den Frame selbstbeschreibend und
prüfbar. Mit über 40 Jahren Weiterentwicklung ist Ethernet heute die
dominierende LAN-Technologie von 10 Mbit/s bis 400 Gbit/s.

**Switches** ersetzen Hubs als zentrales Layer-2-Verbindungsgerät. Durch
gezieltes Forwarding auf Basis der MAC-Adresstabelle werden
Kollisionsdomänen eliminiert und das Netz effizienter genutzt. Die drei
Grundoperationen Learning, Flooding und Forwarding sind die
Kernmechanismen jedes Switches.

**VLANs** erlauben die logische Segmentierung eines physikalischen
Netzes. Das 802.1Q-Tagging ermöglicht den Transport mehrerer VLANs über
einen einzigen Trunk-Link. Access Ports und Trunk Ports definieren, wie
Endgeräte und Switch-zu-Switch-Verbindungen konfiguriert werden.

**STP und RSTP** schützen Layer-2-Netze vor Loops und den damit
verbundenen Broadcast-Stürmen. STP blockiert redundante Pfade
automatisch und aktiviert sie im Fehlerfall – RSTP macht das deutlich
schneller und ist heute der Standard in professionellen Netzwerken.

| **Konzept**     | **Kernaussage**                                                    |
|:----------------|:-------------------------------------------------------------------|
| PDU             | Frame (Layer 2), enthält Src-MAC, Dst-MAC, Nutzdaten, FCS          |
| MAC-Adresse     | 48 Bit, 6 Byte, OUI + Interface-Teil, global eindeutig             |
| Ethernet        | IEEE 802.3, Frame-Struktur, EtherType, Full-Duplex, bis 400 Gbit/s |
| Switch          | Layer-2-Gerät, lernt MAC-Adressen, leitet Frames gezielt weiter    |
| Switching Table | MAC $\rightarrow$ Port-Zuordnung, Aging-Timer, CAM-Table           |
| VLAN            | Logische Netztrennung, 802.1Q-Tag, Access/Trunk-Port               |
| STP / RSTP      | Loop-Vermeidung, Root Bridge, Blocked Port, Konvergenz             |

Für das Verständnis aller höheren Protokollschichten ist Layer 2
unverzichtbar. IP-Pakete auf Layer 3, TCP-Segmente auf Layer 4 – sie
alle reisen innerhalb von Ethernet-Frames über das Netzwerk. Wer Layer 2
versteht, versteht, wie Daten tatsächlich von Port zu Port fliessen.
