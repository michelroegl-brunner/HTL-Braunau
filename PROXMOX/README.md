- [Grundlagen der Virtualisierung](#grundlagen-der-virtualisierung)
  - [Was ist Virtualisierung?](#was-ist-virtualisierung)
  - [Typ-1-Hypervisoren (Bare-Metal)](#typ-1-hypervisoren-bare-metal)
  - [Typ-2-Hypervisoren (Hosted)](#typ-2-hypervisoren-hosted)
  - [Vergleich Typ 1 und Typ 2](#vergleich-typ-1-und-typ-2)
- [Proxmox VE im Überblick](#proxmox-ve-im-überblick)
  - [Was ist Proxmox VE?](#was-ist-proxmox-ve)
  - [Vergleich: Proxmox VE, VMware ESXi und Microsoft
    Hyper-V](#vergleich-proxmox-ve-vmware-esxi-und-microsoft-hyper-v)
  - [Wann welche Lösung?](#wann-welche-lösung)
- [Hardware für Proxmox](#hardware-für-proxmox)
  - [CPU](#cpu)
  - [Arbeitsspeicher (RAM)](#arbeitsspeicher-ram)
  - [Speicher](#speicher)
  - [Netzwerk](#netzwerk)
  - [Fernverwaltung (IPMI / iDRAC /
    iLO)](#fernverwaltung-ipmi-idrac-ilo)
- [RAID-Grundlagen](#raid-grundlagen)
  - [Was ist RAID?](#was-ist-raid)
  - [RAID-Level im Überblick](#raid-level-im-überblick)
  - [Hardware-RAID und Software-RAID](#hardware-raid-und-software-raid)
- [Installationsanleitung](#installationsanleitung)
  - [ISO herunterladen](#iso-herunterladen)
  - [Bootfähigen USB-Stick erstellen](#bootfähigen-usb-stick-erstellen)
  - [BIOS/UEFI vorbereiten](#biosuefi-vorbereiten)
  - [Proxmox VE installieren](#proxmox-ve-installieren)
  - [Erster Login im Webinterface](#erster-login-im-webinterface)
  - [Subscription-Hinweis und freie
    Paketquellen](#subscription-hinweis-und-freie-paketquellen)
- [Erste Schritte nach der
  Installation](#erste-schritte-nach-der-installation)
  - [System aktuell halten](#system-aktuell-halten)
  - [ISO-Images hochladen](#iso-images-hochladen)
  - [Erste virtuelle Maschine
    anlegen](#erste-virtuelle-maschine-anlegen)
  - [Netzwerkbrücken verstehen](#netzwerkbrücken-verstehen)
  - [Backups einrichten](#backups-einrichten)
  - [Snapshots nutzen](#snapshots-nutzen)

# Grundlagen der Virtualisierung

## Was ist Virtualisierung?

Virtualisierung bezeichnet die Technik, mehrere voneinander isolierte
Betriebssysteminstanzen auf einer einzigen physischen Maschine zu
betreiben. Eine spezielle Software, der sogenannte **Hypervisor**,
verwaltet dabei den Zugriff auf die vorhandene Hardware und teilt deren
Ressourcen – CPU-Zeit, Arbeitsspeicher, Speicherplatz und Netzwerk –
zwischen den einzelnen virtuellen Maschinen (VMs) auf.

Jede VM verhält sich aus Sicht des installierten Betriebssystems wie
eigenständige Hardware. Die VMs sind vollständig voneinander isoliert:
Ein Fehler oder Absturz in einer VM beeinflusst die anderen Maschinen
nicht. Dies macht Virtualisierung besonders wertvoll für
Serverumgebungen, Testlabore und Schulungsumgebungen, weil viele Dienste
auf weniger physischen Geräten konsolidiert werden können.

## Typ-1-Hypervisoren (Bare-Metal)

Bei einem **Typ-1-Hypervisor** läuft die Virtualisierungsschicht direkt
auf der Hardware, ohne darunter liegendes Host-Betriebssystem. Der
Hypervisor selbst übernimmt die Ressourcenverwaltung und stellt den VMs
virtualisierte Hardware bereit.

Bekannte Typ-1-Lösungen sind:

- **Proxmox VE**: quelloffen, Debian-basiert, kombiniert KVM und LXC

- **VMware ESXi**: kommerziell, weit verbreitet im Unternehmensumfeld

- **Microsoft Hyper-V**: in Windows Server und Windows 10/11 Pro
  integriert

Da kein allgemeines Betriebssystem zwischen Hypervisor und Hardware
liegt, sind Typ-1-Hypervisoren sehr effizient und für den
Produktiveinsatz auf Servern ausgelegt.

## Typ-2-Hypervisoren (Hosted)

Ein **Typ-2-Hypervisor** wird als normale Anwendung auf einem
vorhandenen Host-Betriebssystem installiert. Er nutzt die Dienste des
Host-Betriebssystems, um Ressourcen an VMs weiterzureichen. Diese
zusätzliche Schicht bringt etwas mehr Overhead mit sich, vereinfacht
aber die Installation und Nutzung erheblich.

Typische Typ-2-Lösungen sind:

- **VMware Workstation**: kommerziell, leistungsfähig, für Entwickler
  und Tester

- **Oracle VirtualBox**: quelloffen, plattformübergreifend, für
  Einsteiger und Schulungen gut geeignet

Typ-2-Hypervisoren laufen problemlos auf normalen Arbeitsplatzrechnern
und erfordern keine spezielle Serverkonfiguration. Dafür eignen sie sich
weniger für den dauerhaften Produktivbetrieb vieler VMs gleichzeitig.

## Vergleich Typ 1 und Typ 2

Der wichtigste Unterschied liegt in der Architektur und im typischen
Einsatzbereich:

- **Leistung**: Typ-1-Hypervisoren haben weniger Overhead, weil kein
  Host-Betriebssystem dazwischen liegt. VMs erhalten direkteren Zugriff
  auf die Hardware.

- **Stabilität und Isolation**: Ohne Host-Betriebssystem gibt es weniger
  Angriffsfläche und weniger Fehlerquellen.

- **Einsatzgebiet**: Typ 1 wird in Rechenzentren und auf dedizierten
  Servern eingesetzt. Typ 2 eignet sich für Entwicklung, Tests und
  Schulungen auf dem eigenen Laptop oder PC.

- **Verwaltung**: Typ-2-Lösungen wie VirtualBox sind durch ihre
  grafische Oberfläche einfacher zu bedienen. Typ-1-Lösungen wie Proxmox
  werden meist über ein Webinterface oder die Kommandozeile verwaltet.

- **Kosten**: VirtualBox und Proxmox VE sind kostenlos nutzbar. VMware
  ESXi und VMware Workstation sind kommerzielle Produkte.

# Proxmox VE im Überblick

## Was ist Proxmox VE?

**Proxmox Virtual Environment (Proxmox VE)** ist eine quelloffene
Virtualisierungsplattform, die auf Debian GNU/Linux basiert. Sie
kombiniert zwei bewährte Virtualisierungstechnologien:

- **KVM (Kernel-based Virtual Machine)**: vollständige
  Hardwarevirtualisierung für beliebige Betriebssysteme, z. B. Windows,
  Linux oder FreeBSD

- **LXC (Linux Containers)**: leichtgewichtige Container, die denselben
  Kernel wie der Host nutzen und mit sehr wenig Overhead betrieben
  werden können

Die Verwaltung erfolgt über ein intuitives Webinterface, das alle
wichtigen Aufgaben abdeckt: VMs und Container anlegen, starten und
stoppen, Ressourcen zuweisen, Snapshots erstellen und Backups einplanen.
Zusätzlich steht eine vollständige Kommandozeilenschnittstelle bereit.

Proxmox VE unterstützt den Betrieb mehrerer Server in einem gemeinsamen
**Cluster**. Im Cluster können VMs zwischen Knoten migriert werden, ohne
sie auszuschalten (*Live Migration*), und der gemeinsame Speicher
(*Shared Storage*) wird zentral verwaltet.

## Vergleich: Proxmox VE, VMware ESXi und Microsoft Hyper-V

Alle drei Produkte sind Typ-1-Hypervisoren für den Servereinsatz,
unterscheiden sich aber deutlich in Lizenzmodell, Funktionsumfang und
Zielgruppe.

**Proxmox VE**:

- vollständig quelloffen (AGPLv3), keine Lizenzkosten

- optionaler Support-Vertrag für Updates aus dem Enterprise-Repository

- kombiniert KVM und LXC in einer Plattform

- sehr gute ZFS-Integration

- Community-Support über Foren und Wiki

**VMware ESXi (vSphere)**:

- kommerzielles Produkt von Broadcom (ehemals VMware)

- Marktführer in großen Unternehmensumgebungen

- sehr ausgereifte Clustering- und Hochverfügbarkeitsfunktionen (vSphere
  HA, vMotion)

- kostspielige Lizenzierung, besonders seit der Übernahme durch Broadcom

- sehr breite Hardware-Zertifizierung

**Microsoft Hyper-V**:

- in Windows Server und Windows 10/11 Pro/Enterprise enthalten

- enge Integration mit Active Directory und Windows-Diensten

- gut geeignet für reine Windows-Umgebungen

- Microsoft Azure nutzt Hyper-V als Basis

- für Linux-VMs möglich, aber weniger optimiert als KVM

## Wann welche Lösung?

Die Wahl des Hypervisors hängt von Anforderungen, Budget und vorhandener
Infrastruktur ab:

- **Proxmox VE** empfiehlt sich für kleine bis mittlere Umgebungen,
  Schulen, Labore und überall dort, wo Lizenzkosten vermieden werden
  sollen. Auch als Einstieg in professionelle Virtualisierung ist
  Proxmox ideal.

- **VMware ESXi** ist die erste Wahl in großen Unternehmensumgebungen
  mit hohen Anforderungen an Hochverfügbarkeit und einem entsprechenden
  Budget.

- **Hyper-V** bietet sich an, wenn bereits Windows-Server-Lizenzen
  vorhanden sind und die Umgebung hauptsächlich aus Windows-VMs besteht.

- **VirtualBox / VMware Workstation** eignen sich für
  Entwicklungsrechner, Schulungsumgebungen und schnelle lokale Tests.

# Hardware für Proxmox

## CPU

Proxmox VE benötigt eine 64-Bit-CPU mit
Hardwarevirtualisierungs-Unterstützung. Bei Intel-Prozessoren heißt
diese Funktion **VT-x** (Virtualization Technology), bei AMD-Prozessoren
**AMD-V** (SVM). Diese Erweiterungen können im BIOS/UEFI aktiviert
werden, sind aber auf modernen Prozessoren oft bereits standardmäßig
eingeschaltet.

Typische CPU-Empfehlungen für Proxmox-Server:

- **Einstieg / Homelab**: Intel Core i5/i7 der aktuellen Generation, AMD
  Ryzen 5/7 – viele Kerne für gleichzeitige VMs

- **Kleiner Produktivserver**: Intel Xeon E-Serie (z. B. E-2300), AMD
  EPYC Embedded

- **Unternehmensserver**: Intel Xeon Scalable (Gold/Platinum), AMD EPYC
  – mehr Kerne, ECC-RAM-Unterstützung, IPMI

Für ZFS-Nutzung sollte die CPU über genügend Kerne und
AES-NI-Unterstützung verfügen, da ZFS rechenintensiv sein kann.

## Arbeitsspeicher (RAM)

RAM ist in einer Virtualisierungsumgebung oft der erste Engpass, da jede
VM ihren eigenen zugewiesenen Speicher benötigt.

- **Absolutes Minimum**: 8 GB (nur für Tests, sehr wenige VMs)

- **Empfehlung Homelab**: 32 GB

- **Produktivbetrieb**: 64 GB und mehr, je nach Anzahl und Größe der VMs

- **ECC-RAM**: im Serverbereich dringend empfohlen, besonders bei ZFS –
  Bitfehler im RAM können bei ZFS zu Datenverlust führen

Proxmox unterstützt Ballooning: VMs geben nicht genutzten RAM temporär
an den Host zurück, was die Gesamtausnutzung verbessert.

## Speicher

Die Wahl des Speichermediums beeinflusst Leistung und Ausfallsicherheit
erheblich.

- **SSD**: empfohlen für das Proxmox-Betriebssystem und VM-Festplatten;
  deutlich schneller als HDDs

- **NVMe**: für maximale Performance, besonders bei vielen
  gleichzeitigen I/O-Zugriffen

- **HDD**: für große Datenspeicher (Backups, ISO-Images) geeignet, aber
  langsam für VM-Festplatten

- **ZFS**: Proxmox bringt sehr gute ZFS-Unterstützung mit. ZFS übernimmt
  RAID, Kompression, Snapshots und Integritätsprüfung direkt im
  Dateisystem. Für ZFS werden mindestens zwei Festplatten sowie
  ausreichend RAM (1 GB RAM pro 1 TB Speicher als Faustregel) empfohlen.

## Netzwerk

- **Mindestens eine Netzwerkkarte**: 1 GbE reicht für den Einstieg,
  10 GbE ist für Produktivumgebungen empfehlenswert

- **Mehrere NICs**: ermöglichen die Trennung von Management-Traffic,
  VM-Traffic und Storage-Traffic in separate Netzwerke

- **Bonding / LACP**: Zusammenschluss mehrerer Netzwerkports für mehr
  Durchsatz oder Redundanz

- **VLAN-Unterstützung**: Proxmox kann VLANs über Linux-Bridges
  konfigurieren, um VMs in verschiedene Netzwerke zu segmentieren

## Fernverwaltung (IPMI / iDRAC / iLO)

Im Servereinsatz ist ein **Baseboard Management Controller (BMC)**
unverzichtbar. Er erlaubt den Zugriff auf den Server auch dann, wenn das
Betriebssystem nicht mehr reagiert:

- **IPMI**: offener Standard, auf vielen Serverboards vorhanden

- **iDRAC** (Dell) und **iLO** (HPE): herstellerspezifische
  Implementierungen mit zusätzlichen Funktionen wie virtueller Konsole
  und Remote-Boot

Für Homelab-Setups mit Consumer-Hardware kann **PiKVM** oder ein
einfacher USB-KVM-Switch eine kostengünstige Alternative sein.

# RAID-Grundlagen

## Was ist RAID?

**RAID** (Redundant Array of Independent Disks) bezeichnet die
Kombination mehrerer physischer Festplatten zu einem logischen Verbund.
Damit werden zwei Ziele verfolgt:

- **Ausfallsicherheit (Redundanz)**: Der Verbund überlebt den Ausfall
  einer oder mehrerer Festplatten ohne Datenverlust.

- **Leistungssteigerung**: Lese- und Schreibzugriffe können auf mehrere
  Platten verteilt werden.

Wichtig: RAID ist **kein Backup**. Es schützt vor dem physischen Ausfall
einer Festplatte, aber nicht vor versehentlichem Löschen, Ransomware
oder Controller-Defekten. Regelmäßige Backups bleiben unverzichtbar.

## RAID-Level im Überblick

**RAID 0 – Striping**:

- Daten werden auf alle Festplatten verteilt (gestreift)

- Maximale Leistung, keine Redundanz

- Fällt eine Platte aus, sind alle Daten verloren

- Geeignet für temporäre Daten oder Cache, nicht für produktive Systeme

**RAID 1 – Mirroring**:

- Daten werden auf mindestens zwei Platten gespiegelt

- Hohe Ausfallsicherheit: Eine Platte darf ausfallen

- Nutzbarer Speicher: 50 % der Gesamtkapazität

- Gut geeignet für Systemplatte und kritische Daten

**RAID 5 – Striping mit Parität**:

- Mindestens drei Festplatten, Paritätsdaten werden verteilt gespeichert

- Eine Platte darf ausfallen, ohne Datenverlust

- Nutzbarer Speicher: Gesamtkapazität minus einer Platte

- Guter Kompromiss zwischen Leistung, Kapazität und Redundanz

**RAID 6 – Striping mit doppelter Parität**:

- Mindestens vier Festplatten, zwei Paritätssätze

- Zwei Platten dürfen gleichzeitig ausfallen

- Geringere Schreibleistung als RAID 5 durch aufwändigere Parität

- Empfohlen bei großen Arrays mit vielen Platten

**RAID 10 – Kombination aus RAID 1 und RAID 0**:

- Mindestens vier Festplatten (gespiegelte Striping-Paare)

- Hohe Leistung und gute Ausfallsicherheit

- Nutzbarer Speicher: 50 % der Gesamtkapazität

- Beliebt für Datenbankserver und hochperformante Systeme

## Hardware-RAID und Software-RAID

Bei einem **Hardware-RAID** übernimmt ein dedizierter RAID-Controller
die Verwaltung des Arrays. Für das Betriebssystem erscheint der Verbund
als einzelne logische Festplatte. Hardware-RAID-Controller verfügen oft
über einen eigenen Cache mit Batterie-Backup für bessere
Schreibleistung.

**Software-RAID** wird vom Betriebssystem verwaltet, ohne spezielle
Hardware. In Proxmox VE wird dies meist durch **ZFS** (ZFS RAID, auch
*RAIDZ* genannt) realisiert. ZFS bietet neben RAID auch:

- automatische Integritätsprüfung aller Daten (Checksummen)

- transparente Kompression

- effiziente Snapshots

- Copy-on-Write-Mechanismus verhindert Datenverlust bei Schreibfehlern

Für neue Proxmox-Installationen wird ZFS als Software-RAID-Lösung
empfohlen, weil es tief in Proxmox integriert ist und keinen
zusätzlichen RAID-Controller erfordert.

# Installationsanleitung

## ISO herunterladen

Das aktuelle Proxmox-VE-ISO ist kostenlos auf der offiziellen Website
erhältlich:

- <https://www.proxmox.com/de/downloads>

Die ISO-Datei trägt einen Namen wie `proxmox-ve_9.x-x.iso`. Es empfiehlt
sich, nach dem Download die angegebene SHA256-Prüfsumme zu verifizieren,
um die Integrität der Datei sicherzustellen.

## Bootfähigen USB-Stick erstellen

Ein USB-Stick mit mindestens 2 GB Kapazität wird benötigt. Das ISO muss
als bootfähiges Medium auf den Stick geschrieben werden – einfaches
Kopieren reicht nicht aus.

**Unter Windows mit Rufus**:

1.  Rufus herunterladen: <https://rufus.ie>

2.  Rufus starten (keine Installation notwendig)

3.  Unter *Laufwerk* den USB-Stick auswählen

4.  Unter *Boot-Auswahl* das Proxmox-ISO auswählen

5.  Partitionsschema: `GPT` (für UEFI) oder `MBR` (für ältere
    BIOS-Systeme)

6.  Auf *Start* klicken – alle vorhandenen Daten auf dem Stick werden
    gelöscht

7.  Es werden Administratorrechte benötigt.

## BIOS/UEFI vorbereiten

Vor dem Start des Installers müssen im BIOS/UEFI des Zielrechners einige
Einstellungen überprüft werden:

1.  **Boot-Reihenfolge**: USB-Stick als erstes Bootmedium einstellen

2.  **Virtualisierung aktivieren**: `Intel VT-x` bzw. `AMD-V (SVM)` muss
    aktiviert sein

3.  **Secure Boot**: für Proxmox VE typischerweise deaktivieren, da der
    Kernel nicht standardmäßig signiert ist

4.  **RAID-Controller**: Falls ein Hardware-RAID-Controller vorhanden
    ist, das Array dort vorher konfigurieren. Soll ZFS (Software-RAID)
    verwendet werden, müssen die Festplatten als Einzellaufwerke
    sichtbar sein (AHCI/IT-Modus).

## Proxmox VE installieren

Nach dem Booten vom USB-Stick erscheint das Installationsmenü:

1.  **Installationsart wählen**: `Install Proxmox VE (Graphical)` für
    die geführte grafische Installation

2.  **EULA akzeptieren**

3.  **Zieldatenträger und Dateisystem wählen**:

    - `ext4` oder `xfs`: einfache Dateisysteme für einzelne Festplatten

    - `ZFS (RAID0/1/10/RAIDZ-1/2/3)`: empfohlen bei mehreren
      Festplatten; die gewünschte RAID-Konfiguration und die Platten
      werden hier ausgewählt

4.  **Standort und Zeitzone** einstellen, z. B. `Europe/Vienna`

5.  **Passwort und E-Mail-Adresse** für den `root`-Benutzer festlegen

6.  **Netzwerk konfigurieren**:

    - Netzwerkschnittstelle auswählen (Management-Interface)

    - **Hostname** (FQDN) eintragen, z. B. `pve01.local`

    - IP-Adresse, Subnetzmaske, Gateway und DNS-Server eintragen

7.  **Zusammenfassung prüfen** und Installation mit *Install* starten

8.  Der Rechner startet nach der Installation neu – USB-Stick vorher
    entfernen

## Erster Login im Webinterface

Nach dem Neustart ist Proxmox VE über den Browser erreichbar:

- URL: `https://<IP-Adresse>:8006`

- Benutzername: `root`

- Authentifizierungsbereich: `Linux PAM standard authentication`

- Passwort: das während der Installation gewählte Root-Passwort

Der Browser zeigt eine Zertifikatswarnung, weil Proxmox ein selbst
signiertes Zertifikat verwendet. Diese Warnung kann für den internen
Gebrauch akzeptiert werden. Für den Produktivbetrieb empfiehlt sich ein
gültiges Zertifikat, z. B. über *Let’s Encrypt* (im Proxmox-Webinterface
direkt konfigurierbar).

## Subscription-Hinweis und freie Paketquellen

Nach dem ersten Login erscheint ein Hinweis, dass keine Subscription
vorhanden ist. Dieser Hinweis kann in den meisten Fällen ignoriert
werden. Für den Heimgebrauch und Schulungsumgebungen stehen die freien
Paketquellen zur Verfügung.

Um das Enterprise-Repository zu deaktivieren und das freie
Community-Repository zu aktivieren, werden folgende Schritte in der
Shell (`pve` $\rightarrow$ *Shell* im Webinterface) ausgeführt:

1.  Enterprise-Repository deaktivieren:

    - `echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" >`  
      `/etc/apt/sources.list.d/pve-community.list`

    - `echo "" > /etc/apt/sources.list.d/pve-enterprise.list`

2.  Ceph-Enterprise-Repository (falls nicht benötigt) ebenso
    deaktivieren:

    - `echo "" > /etc/apt/sources.list.d/ceph.list`

3.  System aktualisieren:

    - `apt update && apt dist-upgrade -y`

# Erste Schritte nach der Installation

## System aktuell halten

Regelmäßige Updates sind für die Sicherheit und Stabilität des
Proxmox-Hosts essenziell. Updates können entweder über das Webinterface
(*Knoten* $\rightarrow$ *Updates*) oder über die Shell eingespielt
werden:

- `apt update && apt dist-upgrade -y`

Nach Kernel-Updates ist ein Neustart erforderlich. Proxmox zeigt im
Webinterface einen entsprechenden Hinweis an.

## ISO-Images hochladen

Um VMs mit einem Betriebssystem zu installieren, werden ISO-Images
benötigt. Diese werden im Webinterface hochgeladen:

1.  Im linken Baum den Speicher auswählen, z. B. `local`

2.  Auf *ISO Images* klicken

3.  *Upload* für eine lokale Datei oder *Download from URL* für direkten
    Download

## Erste virtuelle Maschine anlegen

1.  Oben rechts auf *Create VM* klicken

2.  **General**: VM-ID und Name vergeben

3.  **OS**: ISO-Image auswählen und Betriebssystemtyp festlegen
    (z. B. `Microsoft Windows` oder `Linux`)

4.  **System**: BIOS und Maschinentyp auswählen – für moderne Systeme
    `OVMF (UEFI)` und `q35` empfohlen

5.  **Disks**: Festplattengröße und Speicher-Backend wählen

6.  **CPU**: Anzahl der virtuellen Kerne zuweisen

7.  **Memory**: RAM-Menge in MiB festlegen

8.  **Network**: Netzwerkbrücke auswählen (standardmäßig `vmbr0`)

9.  Auf *Finish* klicken, VM erscheint im linken Baum

10. VM starten: im linken Baum anklicken, dann *Start*

11. Über *Console* die grafische Ausgabe der VM aufrufen und das
    Betriebssystem installieren

## Netzwerkbrücken verstehen

Proxmox verwendet **Linux Bridges** für die VM-Vernetzung. Standardmäßig
wird während der Installation die Bridge `vmbr0` angelegt, die mit dem
physischen Netzwerkport verbunden ist. VMs, die dieser Bridge zugewiesen
werden, sind im selben Netzwerk wie der Proxmox-Host erreichbar.

Weitere Bridges können für isolierte Netzwerke, VLANs oder separate
Subnetzwerke angelegt werden. Die Konfiguration erfolgt im Webinterface
unter *Knoten* $\rightarrow$ *Netzwerk*.

## Backups einrichten

Proxmox VE bringt einen integrierten Backup-Mechanismus mit. Backups
können manuell oder zeitgesteuert erstellt werden:

- **Manuell**: VM auswählen $\rightarrow$ *Backup* $\rightarrow$ *Backup
  now*

- **Geplant**: Im Rechenzentrum-Bereich unter *Backup* einen Zeitplan
  anlegen

- **Backup-Speicher**: Backups können lokal, auf einem NFS-/SMB-Share
  oder auf einem *Proxmox Backup Server* gespeichert werden

Drei Backup-Modi stehen zur Verfügung:

- **Stop**: VM wird kurz gestoppt – konsistenteste Methode, aber kurze
  Downtime

- **Suspend**: VM wird eingefroren, Backup erstellt, danach weiter –
  kurze Pause sichtbar

- **Snapshot**: laufende VM wird gesichert – keine Downtime, aber unter
  Umständen weniger konsistent

## Snapshots nutzen

Snapshots speichern den aktuellen Zustand einer VM zu einem bestimmten
Zeitpunkt. Sie sind besonders nützlich, bevor an einer VM Änderungen
vorgenommen werden (Updates, Konfigurationsänderungen):

- VM auswählen $\rightarrow$ *Snapshots* $\rightarrow$ *Take Snapshot*

- Einen beschreibenden Namen vergeben, z. B. `vor-update-2026-04`

- Bei Problemen kann der frühere Zustand mit *Rollback*
  wiederhergestellt werden

Snapshots sind kein Ersatz für Backups, da sie auf denselben Festplatten
wie die VM liegen.
