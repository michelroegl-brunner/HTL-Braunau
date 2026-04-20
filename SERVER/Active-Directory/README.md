- [Grundlagen Active Directory](#grundlagen-active-directory)
  - [Was ist Active Directory?](#was-ist-active-directory)
  - [Kernkonzepte](#kernkonzepte)
    - [Domain](#domain)
    - [Forest und Tree](#forest-und-tree)
    - [Organisationseinheiten (OUs)](#organisationseinheiten-ous)
    - [Benutzer und Gruppen](#benutzer-und-gruppen)
    - [Gruppenrichtlinien (GPO)](#gruppenrichtlinien-gpo)
    - [Domain Controller](#domain-controller)
  - [Einsatzgebiete](#einsatzgebiete)
- [Netzwerkplanung mit Proxmox](#netzwerkplanung-mit-proxmox)
  - [Konzept: Isoliertes Labornetz](#konzept-isoliertes-labornetz)
  - [IP-Adressplan](#ip-adressplan)
  - [Proxmox: Interne Bridge `vmbr1`
    anlegen](#proxmox-interne-bridge-vmbr1-anlegen)
  - [Übersicht der virtuellen
    Maschinen](#übersicht-der-virtuellen-maschinen)
- [Aufgabenstellung](#aufgabenstellung)
  - [Aufgabe 1: Proxmox-VM für den Domain Controller
    anlegen](#aufgabe-1-proxmox-vm-für-den-domain-controller-anlegen)
    - [Ziel](#ziel)
    - [Voraussetzungen](#voraussetzungen)
    - [Aufgabe](#aufgabe)
    - [Hinweis](#hinweis)
  - [Aufgabe 2: Windows Server installieren und Domain Controller
    promoten](#aufgabe-2-windows-server-installieren-und-domain-controller-promoten)
    - [Ziel](#ziel-1)
    - [Aufgabe – Teil A: Windows Server
      installieren](#aufgabe-teil-a-windows-server-installieren)
    - [Aufgabe – Teil B: Statische IP-Adresse
      setzen](#aufgabe-teil-b-statische-ip-adresse-setzen)
    - [Aufgabe – Teil C: AD DS-Rolle installieren und DC
      promoten](#aufgabe-teil-c-ad-ds-rolle-installieren-und-dc-promoten)
    - [Snapshot empfohlen](#snapshot-empfohlen)
  - [Aufgabe 3: DHCP-Server
    einrichten](#aufgabe-3-dhcp-server-einrichten)
    - [Ziel](#ziel-2)
    - [Aufgabe](#aufgabe-1)
  - [Aufgabe 4: Organisationseinheiten, Benutzer und Gruppen
    anlegen](#aufgabe-4-organisationseinheiten-benutzer-und-gruppen-anlegen)
    - [Ziel](#ziel-3)
    - [Unternehmensstruktur](#unternehmensstruktur)
    - [Beispielbenutzer](#beispielbenutzer)
    - [Aufgabe](#aufgabe-2)
  - [Aufgabe 5: Fileshares pro Abteilung
    einrichten](#aufgabe-5-fileshares-pro-abteilung-einrichten)
    - [Ziel](#ziel-4)
    - [Aufgabe](#aufgabe-3)
  - [Aufgabe 6: Logon-Skript pro Abteilung
    einrichten](#aufgabe-6-logon-skript-pro-abteilung-einrichten)
    - [Ziel](#ziel-5)
    - [Aufgabe – Teil A: Logon-Skripte
      erstellen](#aufgabe-teil-a-logon-skripte-erstellen)
    - [Aufgabe – Teil B: GPO pro Abteilung erstellen und
      verknüpfen](#aufgabe-teil-b-gpo-pro-abteilung-erstellen-und-verknüpfen)
    - [Hinweis:
      PowerShell-Ausführungsrichtlinie](#hinweis-powershell-ausführungsrichtlinie)
  - [Aufgabe 7: Windows-Client-VM erstellen und der Domain
    beitreten](#aufgabe-7-windows-client-vm-erstellen-und-der-domain-beitreten)
    - [Ziel](#ziel-6)
    - [Aufgabe – Teil A: VM anlegen](#aufgabe-teil-a-vm-anlegen)
    - [Aufgabe – Teil B: Netzwerk
      überprüfen](#aufgabe-teil-b-netzwerk-überprüfen)
    - [Aufgabe – Teil C: Domain
      beitreten](#aufgabe-teil-c-domain-beitreten)
    - [Aufgabe – Teil D: Domainbenutzer
      testen](#aufgabe-teil-d-domainbenutzer-testen)
- [Hilfestellung und
  Implementierungshinweise](#hilfestellung-und-implementierungshinweise)
  - [Windows-Client per PowerShell der Domain
    hinzufügen](#windows-client-per-powershell-der-domain-hinzufügen)
  - [Netzwerkdiagramm und Datenfluss beim
    Login](#netzwerkdiagramm-und-datenfluss-beim-login)
  - [Häufige Fehlerquellen](#häufige-fehlerquellen)
    - [DNS falsch konfiguriert](#dns-falsch-konfiguriert)
    - [Kerberos-Zeitdifferenz](#kerberos-zeitdifferenz)
    - [Logon-Skript wird nicht
      ausgeführt](#logon-skript-wird-nicht-ausgeführt)
    - [Domain-Beitritt schlägt fehl](#domain-beitritt-schlägt-fehl)
  - [Gruppenrichtlinien –
    Bonusaufgaben](#gruppenrichtlinien-bonusaufgaben)
  - [Empfohlene Reihenfolge und
    Snapshot-Strategie](#empfohlene-reihenfolge-und-snapshot-strategie)

# Grundlagen Active Directory

## Was ist Active Directory?

**Active Directory (AD)** ist ein von Microsoft entwickelter
Verzeichnisdienst, der erstmals mit Windows Server 2000 eingeführt
wurde. Ein Verzeichnisdienst ist ein zentraler Speicher für
Informationen über Objekte in einem Netzwerk – Benutzer, Computer,
Drucker, Gruppen und viele weitere Ressourcen – und stellt Mechanismen
zur Authentifizierung und Autorisierung bereit.

Active Directory basiert auf dem offenen Protokoll **LDAP** (Lightweight
Directory Access Protocol) und nutzt **Kerberos** als primäres
Authentifizierungsprotokoll. Ergänzend wird **DNS** (Domain Name System)
intensiv genutzt: Ohne funktionierenden DNS-Dienst arbeitet Active
Directory nicht zuverlässig.

In der Praxis ist Active Directory der De-facto-Standard für das
Identitäts- und Zugriffsmanagement in Windows-basierten
Unternehmensumgebungen. Es findet sich in nahezu jeder Organisation, die
mehr als ein Dutzend Windows-Rechner betreibt.

## Kernkonzepte

### Domain

Eine **Domain** (Domäne) ist die grundlegende administrative Einheit in
Active Directory. Sie bündelt alle Objekte – Benutzer, Computer, Gruppen
– unter einem gemeinsamen Namensraum, z. B. `bannanenimport.local`.
Innerhalb einer Domain gelten gemeinsame Sicherheitsrichtlinien.

### Forest und Tree

Mehrere Domains können zu einer **Tree** (Baumstruktur) verbunden
werden, wenn sie denselben DNS-Namensraum teilen. Mehrere Trees bilden
zusammen einen **Forest** (Gesamtstruktur), der die oberste
Sicherheitsgrenze in Active Directory darstellt. Für Schulungsumgebungen
genügt in der Regel eine einzige Domain ohne Forest-Verbund.

### Organisationseinheiten (OUs)

**Organisationseinheiten** (Organizational Units, OUs) sind Container
innerhalb einer Domain, mit denen Objekte strukturiert und gruppiert
werden. OUs spiegeln häufig die Unternehmensstruktur wider – etwa nach
Abteilungen oder Standorten. Auf OUs können gezielt
**Gruppenrichtlinien** (GPOs) angewendet werden.

### Benutzer und Gruppen

**Benutzerkonten** repräsentieren Personen oder Dienste und enthalten
Anmeldeinformationen sowie Profildetails. **Gruppen** fassen mehrere
Benutzer oder Computer zusammen und erleichtern die Verwaltung von
Berechtigungen erheblich: Anstatt jedem einzelnen Benutzer Rechte zu
erteilen, werden diese der Gruppe zugewiesen.

Es gibt zwei Haupttypen von Gruppen:

- **Sicherheitsgruppen**: für Berechtigungsverwaltung und
  Gruppenrichtlinien

- **Verteilergruppen**: für E-Mail-Verteilerlisten, keine
  Berechtigungsverwaltung

Gruppen haben außerdem einen **Gültigkeitsbereich**:

- **Lokal**: nur innerhalb der Domain nutzbar

- **Global**: domainweit, kann in anderen Domains verwendet werden

- **Universal**: forest-weit nutzbar

### Gruppenrichtlinien (GPO)

**Group Policy Objects (GPOs)** sind Richtlinienpakete, die auf Benutzer
oder Computer angewendet werden. Sie steuern hunderte von Einstellungen:
Passwortrichtlinien, Sperrbildschirm-Timeouts, Softwareverteilung,
Netzlaufwerk-Mappings, Startskripte und vieles mehr. GPOs werden auf
Ebene von Domain, Standort oder OU verknüpft.

### Domain Controller

Der **Domain Controller (DC)** ist der zentrale Server, auf dem Active
Directory läuft. Er authentifiziert Benutzer und Computer beim Anmelden,
beantwortet LDAP-Anfragen und repliziert die AD-Datenbank bei mehreren
DCs. In kleinen Umgebungen genügt ein einziger DC; für
Produktivumgebungen empfehlen sich mindestens zwei für
Ausfallsicherheit.

## Einsatzgebiete

Active Directory wird in Unternehmen und Bildungseinrichtungen für eine
Vielzahl von Aufgaben eingesetzt:

- **Zentrales Benutzermanagement**: Benutzerkonten, Passwörter und
  Profilinformationen werden an einem Ort verwaltet. Ein Benutzer kann
  sich mit denselben Anmeldedaten an beliebigen Computern der Domain
  anmelden.

- **Single Sign-On (SSO)**: Nach einer einmaligen Anmeldung am
  Betriebssystem haben Benutzer Zugriff auf alle Ressourcen, für die sie
  berechtigt sind – ohne erneute Passwortabfrage.

- **Netzwerkressourcen verwalten**: Drucker, Dateifreigaben und
  Anwendungen werden zentral bereitgestellt und den richtigen Benutzern
  und Gruppen zugewiesen.

- **Sicherheitsrichtlinien**: GPOs erzwingen einheitliche
  Sicherheitseinstellungen auf allen Computern der Domain –
  z. B. Mindestpasswortlänge, Bildschirmsperre oder deaktivierter
  USB-Zugriff.

- **Softwareverteilung**: Anwendungen können automatisch auf Computern
  oder für Benutzer installiert werden.

- **Audit und Compliance**: Anmeldevorgänge, Zugriffsversuche und
  administrative Änderungen werden protokolliert.

# Netzwerkplanung mit Proxmox

## Konzept: Isoliertes Labornetz

Für diese Übung wird ein vollständig isoliertes Netzwerk in Proxmox
eingerichtet. Der Vorteil: Die VMs sind vom restlichen Netzwerk und vom
Internet getrennt, was einerseits die Sicherheit erhöht und andererseits
eine realistische Unternehmensumgebung simuliert, in der der Domain
Controller alle Netzwerkdienste (DNS, DHCP) selbst übernimmt.

Das Konzept sieht folgendermaßen aus:

      +------------------------------- Proxmox Host -------------------------------+
      |                                                                            |
      |   vmbr0 (mit physischer NIC)          vmbr1 (intern, kein Bridge-Port)     |
      |   --> Internet / Schulnetz            --> isoliertes AD-Labornetz          |
      |                                                                            |
      |              +---------------+        +------------------+                 |
      |              |     DC01      |        |    CLIENT01      |                 |
      |              | Windows Srv   |        | Windows 10/11    |                 |
      |              | 192.168.100.1 |        | 192.168.100.x    |                 |
      |              |  (DHCP, DNS)  |        |  (via DHCP)      |                 |
      |              +-------+-------+        +--------+---------+                 |
      |                      |                         |                           |
      |                      +----------vmbr1----------+                           |
      |                                                                            |
      +----------------------------------------------------------------------------+

Der Domain Controller `DC01` erhält eine feste IP-Adresse und betreibt
gleichzeitig den DNS- und DHCP-Dienst für das interne Netz. Der
Windows-Client `CLIENT01` bekommt seine IP-Konfiguration automatisch per
DHCP.

## IP-Adressplan

<div class="center">

| **Gerät**                | **IP-Adresse**                     | **Bemerkung**           |
|:-------------------------|:-----------------------------------|:------------------------|
| DC01 (Domain Controller) | `192.168.100.1`                    | statisch, Gateway + DNS |
| DHCP-Bereich (Clients)   | `192.168.100.50 – 192.168.100.150` | dynamisch per DHCP      |
| CLIENT01                 | `192.168.100.50` (Beispiel)        | per DHCP zugewiesen     |
| Subnetzmaske             | `255.255.255.0`                    | `/24`                   |

</div>

## Proxmox: Interne Bridge `vmbr1` anlegen

Damit das isolierte Netz funktioniert, muss in Proxmox eine zweite
Linux-Bridge ohne physischen Netzwerkport angelegt werden:

1.  Im Proxmox-Webinterface den Knoten (Server) im linken Baum auswählen

2.  Auf *Netzwerk* klicken

3.  Oben auf *Erstellen* $\rightarrow$ *Linux Bridge* klicken

4.  Folgende Einstellungen vornehmen:

    - **Name**: `vmbr1`

    - **Bridge-Ports**: *leer lassen* (kein physischer Port – rein
      intern)

    - **VLAN aware**: optional, für diese Übung nicht notwendig

    - Kommentar: `AD-Labornetz intern`

5.  Auf *Erstellen* klicken, dann oben auf *Änderungen übernehmen*

Die neue Bridge `vmbr1` erscheint nun in der Netzwerkliste und steht
beim Erstellen von VMs als Netzwerkschnittstelle zur Verfügung.

## Übersicht der virtuellen Maschinen

<div class="center">

| **VM-Name** | **Betriebssystem**  | **vCPU** | **RAM** | **Disk** |
|:------------|:--------------------|:---------|:--------|:---------|
| DC01        | Windows Server 2022 | 2        | 4 GB    | 60 GB    |
| CLIENT01    | Windows 10 / 11     | 2        | 4 GB    | 50 GB    |

</div>

Beide VMs werden ausschließlich mit `vmbr1` verbunden. Ein zusätzlicher
Netzwerkadapter an `vmbr0` ist für diese Aufgabe nicht erforderlich.

# Aufgabenstellung

## Aufgabe 1: Proxmox-VM für den Domain Controller anlegen

### Ziel

Eine virtuelle Maschine für den späteren Windows Server / Domain
Controller wird in Proxmox erstellt.

### Voraussetzungen

- Proxmox VE ist installiert und erreichbar

- Die interne Bridge `vmbr1` wurde wie in Kapitel 2 beschrieben angelegt

- Ein Windows-Server-2022-ISO ist auf dem Proxmox-Speicher vorhanden  
  (ISO-Upload: Speicher `local` $\rightarrow$ *ISO Images* $\rightarrow$
  *Upload*)

### Aufgabe

1.  Klicke oben rechts im Proxmox-Webinterface auf *VM erstellen*

2.  Konfiguriere die VM mit folgenden Einstellungen:

    - **Allgemein**: Name `DC01`, VM-ID nach Wahl (z. B. `100`)

    - **OS**: Windows-Server-2022-ISO auswählen, Typ
      `Microsoft Windows`, Version `11/2022`

    - **System**: BIOS `OVMF (UEFI)`, Maschine `q35`, TPM 2.0 hinzufügen
      (für Windows Server 2022 empfohlen)

    - **Festplatten**: 60 GB, Bus/Gerät `VirtIO SCSI`, Storage nach
      Verfügbarkeit

    - **CPU**: 2 Kerne, Typ `host`

    - **Speicher**: 4096 MiB (4 GB)

    - **Netzwerk**: Bridge `vmbr1`, Modell `VirtIO (paravirtualized)`

3.  Auf *Fertigstellen* klicken – die VM erscheint im linken Baum

4.  Erstelle vor dem ersten Start einen Snapshot: VM auswählen
    $\rightarrow$ *Snapshots* $\rightarrow$ *Snapshot aufnehmen*, Name:
    `leer-vor-installation`

5.  Starte die VM und öffne die Konsole über *Console*

### Hinweis

VirtIO-Treiber werden von Windows während der Installation
möglicherweise nicht erkannt. In diesem Fall muss zusätzlich eine zweite
virtuelle CD mit dem **VirtIO-Treiber-ISO** (kostenlos von
[fedorapeople.org](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/))
eingebunden werden, damit der Festplatten- und Netzwerktreiber gefunden
wird.

## Aufgabe 2: Windows Server installieren und Domain Controller promoten

### Ziel

Windows Server 2022 wird installiert, mit einer statischen IP-Adresse
konfiguriert, und der Server wird zum Domain Controller der Domain
`bannanenimport.local` befördert.

### Aufgabe – Teil A: Windows Server installieren

1.  Starte die VM `DC01` und starte die Windows-Server-2022-Installation
    über die Konsole

2.  Wähle **Windows Server 2022 Standard (Desktop Experience)** – die
    Version mit grafischer Oberfläche

3.  Wähle *Benutzerdefinierte Installation*, wähle das virtuelle
    Laufwerk aus (VirtIO-Treiber ggf. nachladen)

4.  Vergib ein sicheres Administratorpasswort

5.  Nach dem ersten Anmelden: Computernamen auf `DC01` ändern  
    (*Systemsteuerung* $\rightarrow$ *System* $\rightarrow$
    *Computername ändern*) und neu starten

### Aufgabe – Teil B: Statische IP-Adresse setzen

1.  Öffne *Netzwerk- und Freigabecenter* $\rightarrow$
    *Adaptereinstellungen*

2.  Rechtsklick auf den Ethernet-Adapter $\rightarrow$ *Eigenschaften*
    $\rightarrow$ *Internet Protocol Version 4 (TCP/IPv4)*

3.  Setze folgende Werte:

    - IP-Adresse: `192.168.100.1`

    - Subnetzmaske: `255.255.255.0`

    - Standardgateway: *leer lassen* (kein Internet in diesem Netz)

    - Bevorzugter DNS: `127.0.0.1`

### Aufgabe – Teil C: AD DS-Rolle installieren und DC promoten

1.  Öffne den **Server-Manager** $\rightarrow$ *Rollen und Features
    hinzufügen*

2.  Wähle *Rollenbasierte oder featurebasierte Installation*

3.  Wähle **Active Directory Domain Services** (AD DS) – weitere
    Features werden automatisch vorgeschlagen, bestätigen

4.  Installiere die Rolle und klicke danach auf das Warndreieck im
    Server-Manager: *Diesen Server zu einem Domänencontroller
    heraufstufen*

5.  Im Konfigurationsassistenten:

    - **Neue Gesamtstruktur hinzufügen**

    - Root-Domänenname: `bannanenimport.local`

    - Funktionsebene: *Windows Server 2016* oder höher

    - DNS-Server und Globaler Katalog: aktiviert lassen

    - DSRM-Passwort (Directory Services Restore Mode) vergeben und
      notieren

6.  Installation abschließen – der Server startet automatisch neu

7.  Nach dem Neustart mit `BANANENIMPORT\Administrator` anmelden

### Snapshot empfohlen

Erstelle nach erfolgreichem DC-Promote einen Snapshot in Proxmox:
`dc-promote-fertig`

## Aufgabe 3: DHCP-Server einrichten

### Ziel

Der Domain Controller übernimmt auch den DHCP-Dienst für das interne
Netz, sodass Windows-Clients automatisch eine IP-Adresse erhalten.

### Aufgabe

1.  Öffne den **Server-Manager** $\rightarrow$ *Rollen und Features
    hinzufügen*

2.  Wähle die Rolle **DHCP-Server** und installiere sie

3.  Nach der Installation: Warndreieck im Server-Manager $\rightarrow$
    *DHCP-Konfiguration abschließen* $\rightarrow$ Autorisierung mit dem
    Domain-Administratorkonto bestätigen

4.  Öffne das **DHCP-Verwaltungstool** (*Start* $\rightarrow$ *DHCP*)

5.  Lege einen neuen Bereich (Scope) an:

    - Rechtsklick auf *IPv4* $\rightarrow$ *Neuer Bereich*

    - Name: `AD-Labornetz`

    - Startadresse: `192.168.100.50`

    - Endadresse: `192.168.100.150`

    - Subnetzmaske: `255.255.255.0`

    - Ausschlüsse: keine (oder `192.168.100.1` ist ohnehin außerhalb)

    - Leasedauer: Standard (8 Tage) oder kürzer für Laborumgebungen
      (z. B. 1 Tag)

6.  Konfiguriere die **Bereichsoptionen**:

    - **003 Router (Gateway)**: `192.168.100.1`

    - **006 DNS-Server**: `192.168.100.1`

    - **015 DNS-Domänenname**: `bannanenimport.local`

7.  Aktiviere den Bereich (Scope aktivieren)

## Aufgabe 4: Organisationseinheiten, Benutzer und Gruppen anlegen

### Ziel

Die Unternehmensstruktur der fiktiven **Bananeimport Fruchticus GmbH**
wird in Active Directory abgebildet. Es werden Organisationseinheiten,
Sicherheitsgruppen und Benutzerkonten für vier Abteilungen angelegt.

### Unternehmensstruktur

<div class="center">

| **Abteilung** | **OU-Name**   | **Gruppe**        |
|:--------------|:--------------|:------------------|
| IT-Abteilung  | `IT`          | `GRP_IT`          |
| Buchhaltung   | `Buchhaltung` | `GRP_Buchhaltung` |
| Vertrieb      | `Vertrieb`    | `GRP_Vertrieb`    |
| Personal      | `Personal`    | `GRP_Personal`    |

</div>

### Beispielbenutzer

<div class="center">

| **Anzeigename** | **Benutzername** | **Abteilung** |
|:----------------|:-----------------|:--------------|
| Anna Berger     | `aberger`        | IT            |
| Klaus Huber     | `khuber`         | IT            |
| Eva Maier       | `emaier`         | Buchhaltung   |
| Stefan Gruber   | `sgruber`        | Buchhaltung   |
| Lisa Wagner     | `lwagner`        | Vertrieb      |
| Markus Bauer    | `mbauer`         | Vertrieb      |
| Sandra Fischer  | `sfischer`       | Personal      |
| Thomas Müller   | `tmueller`       | Personal      |

</div>

### Aufgabe

1.  Öffne **Active Directory-Benutzer und -Computer** (*Start*
    $\rightarrow$ *Windows-Verwaltungstools* $\rightarrow$ *Active
    Directory-Benutzer und -Computer*)

2.  Lege unter `bannanenimport.local` eine übergeordnete OU
    `Bananenimport-Fruchticus` an:

    - Rechtsklick auf `bannanenimport.local` $\rightarrow$ *Neu*
      $\rightarrow$ *Organisationseinheit*

    - Name: `Bananenimport-Fruchticus`, Häkchen bei *Container vor
      versehentlichem Löschen schützen*

3.  Lege unter `Bananenimport-Fruchticus` für jede Abteilung eine eigene
    OU an (`IT`, `Buchhaltung`, `Vertrieb`, `Personal`)

4.  Lege in jeder Abteilungs-OU eine **globale Sicherheitsgruppe** an:

    - Rechtsklick auf OU $\rightarrow$ *Neu* $\rightarrow$ *Gruppe*

    - Name nach obiger Tabelle, Gruppenbereich *Global*, Gruppentyp
      *Sicherheit*

5.  Lege für jeden Benutzer aus der Tabelle ein Konto an:

    - Rechtsklick auf zugehörige OU $\rightarrow$ *Neu* $\rightarrow$
      *Benutzer*

    - Vorname, Nachname und Anmeldenamen eintragen

    - Passwort setzen (z. B. `Bananenimport-Fruchticus2024!`), *Benutzer
      muss Kennwort bei nächster Anmeldung ändern* aktivieren

6.  Füge jeden Benutzer der entsprechenden Abteilungsgruppe hinzu:

    - Doppelklick auf die Gruppe $\rightarrow$ Reiter *Mitglieder*
      $\rightarrow$ *Hinzufügen*

    - Benutzernamen eingeben und bestätigen

## Aufgabe 5: Fileshares pro Abteilung einrichten

### Ziel

Jede Abteilung erhält einen eigenen Netzwerkordner auf dem DC. Nur
Mitglieder der jeweiligen Abteilungsgruppe dürfen auf den Ordner
zugreifen.

### Aufgabe

1.  Öffne den **Datei-Explorer** auf `DC01` und erstelle folgende
    Ordnerstruktur:

    - `C:\Shares\IT`

    - `C:\Shares\Buchhaltung`

    - `C:\Shares\Vertrieb`

    - `C:\Shares\Personal`

2.  Gib jeden Ordner als Netzwerkfreigabe frei:

    - Rechtsklick auf Ordner $\rightarrow$ *Eigenschaften* $\rightarrow$
      Reiter *Freigabe* $\rightarrow$ *Erweiterte Freigabe*

    - Häkchen bei *Diesen Ordner freigeben*, Freigabename z. B. `IT`,
      `Buchhaltung` usw.

    - Unter *Berechtigungen*: `Jeder` entfernen, jeweilige Gruppe
      (`GRP_IT` usw.) mit Berechtigung *Ändern* hinzufügen

3.  Setze zusätzlich die **NTFS-Berechtigungen** (Sicherheitsreiter):

    - Klicke auf *Bearbeiten* im Reiter *Sicherheit*

    - Entferne `Benutzer` (falls vorhanden), behalte `Administratoren`
      und `SYSTEM`

    - Füge die jeweilige Gruppe hinzu: Berechtigung *Ändern* (beinhaltet
      Lesen, Schreiben, Ausführen)

4.  Überprüfe die Freigabe: Öffne auf einem anderen Computer (später
    `CLIENT01`) den Pfad `\\DC01\IT` – nur Benutzer aus `GRP_IT` sollten
    Zugriff haben

## Aufgabe 6: Logon-Skript pro Abteilung einrichten

### Ziel

Jeder Benutzer soll beim Anmelden automatisch das Netzlaufwerk seiner
Abteilung als Laufwerk `Z:` eingebunden bekommen. Dies wird über ein
PowerShell-Skript realisiert, das per Gruppenrichtlinie (GPO) auf
OU-Ebene angewendet wird.

### Aufgabe – Teil A: Logon-Skripte erstellen

Erstelle für jede Abteilung ein eigenes PowerShell-Skript. Die Skripte
werden im SYSVOL-Ordner gespeichert, damit alle Domaincomputer darauf
zugreifen können.

Speicherpfad auf dem DC:

<div class="center">

`C:\Windows\SYSVOL\sysvol\bannanenimport.local\scripts\`

</div>

Erstelle dort folgende vier Dateien:

**Datei `logon_IT.ps1`:**

``` sh
# Netzlaufwerk fuer die IT-Abteilung einbinden
$laufwerk = "Z:"
$pfad     = "\\DC01\IT"

# Vorhandenes Laufwerk Z: trennen (falls vorhanden)
if (Test-Path $laufwerk) {
    net use $laufwerk /delete /yes
}

# Netzlaufwerk verbinden
net use $laufwerk $pfad /persistent:no

Write-Host "Laufwerk $laufwerk wurde mit $pfad verbunden."
```

Erstelle analoge Skripte `logon_Buchhaltung.ps1`, `logon_Vertrieb.ps1`
und `logon_Personal.ps1` – jeweils mit dem entsprechenden Freigabepfad
(`\\DC01\Buchhaltung` usw.).

### Aufgabe – Teil B: GPO pro Abteilung erstellen und verknüpfen

1.  Öffne die **Gruppenrichtlinienverwaltung** (*Start* $\rightarrow$
    *Windows-Verwaltungstools* $\rightarrow$
    *Gruppenrichtlinienverwaltung*)

2.  Navigiere zu *bannanenimport.local* $\rightarrow$
    *Bananenimport-Fruchticus* $\rightarrow$ *IT*

3.  Rechtsklick auf die OU `IT` $\rightarrow$ *Gruppenrichtlinienobjekt
    hier erstellen und verknüpfen*

4.  Name: `GPO_Logon_IT`

5.  Rechtsklick auf `GPO_Logon_IT` $\rightarrow$ *Bearbeiten*

6.  Im Gruppenrichtlinien-Editor navigiere zu:  
    *Benutzerkonfiguration* $\rightarrow$ *Windows-Einstellungen*
    $\rightarrow$ *Skripts (Anmelden/Abmelden)*

7.  Doppelklick auf *Anmelden* $\rightarrow$ *Hinzufügen*

8.  Klicke auf *Dateien anzeigen* – der SYSVOL-Skriptordner öffnet sich

9.  Kopiere `logon_IT.ps1` in diesen Ordner (falls nicht bereits dort)

10. Trage als Skriptname `logon_IT.ps1` ein, klicke *OK*

11. Wiederhole die Schritte für `Buchhaltung`, `Vertrieb` und `Personal`

### Hinweis: PowerShell-Ausführungsrichtlinie

Damit PowerShell-Skripte als Logon-Skript ausgeführt werden dürfen, muss
die Ausführungsrichtlinie angepasst werden. Dies kann ebenfalls per GPO
erzwungen werden:

*Computerkonfiguration* $\rightarrow$ *Administrative Vorlagen*
$\rightarrow$ *Windows-Komponenten* $\rightarrow$ *Windows PowerShell*
$\rightarrow$ *Skriptausführung aktivieren* $\rightarrow$ *Allen
Skripten erlauben*

## Aufgabe 7: Windows-Client-VM erstellen und der Domain beitreten

### Ziel

Ein Windows-Client (`CLIENT01`) wird als zweite VM in Proxmox angelegt,
konfiguriert und der Domain `bannanenimport.local` beigetreten.
Anschließend wird die gesamte Umgebung getestet.

### Aufgabe – Teil A: VM anlegen

1.  Klicke im Proxmox-Webinterface auf *VM erstellen*

2.  Konfiguriere die VM:

    - **Allgemein**: Name `CLIENT01`, VM-ID z. B. `101`

    - **OS**: Windows-10- oder Windows-11-ISO, Typ `Microsoft Windows`

    - **System**: `OVMF (UEFI)`, `q35`

    - **Festplatte**: 50 GB, `VirtIO SCSI`

    - **CPU**: 2 Kerne, Typ `host`

    - **Speicher**: 4096 MiB

    - **Netzwerk**: Bridge `vmbr1`, Modell `VirtIO`

3.  VM starten, Windows installieren

4.  Computername auf `CLIENT01` setzen und neu starten

### Aufgabe – Teil B: Netzwerk überprüfen

1.  Öffne die Eingabeaufforderung und prüfe die IP-Konfiguration:

    - `ipconfig /all` – der Client sollte eine IP aus dem DHCP-Bereich  
      (`192.168.100.50–150`) bekommen haben, DNS sollte `192.168.100.1`
      sein

2.  Teste die Verbindung zum DC:

    - `ping 192.168.100.1` – muss erfolgreich sein

    - `ping DC01.bannanenimport.local` – muss DNS auflösen und antworten

### Aufgabe – Teil C: Domain beitreten

1.  Öffne *Systemsteuerung* $\rightarrow$ *System* $\rightarrow$
    *Einstellungen für Computername, Domäne und Arbeitsgruppe*

2.  Klicke auf *Ändern*

3.  Wähle *Domäne*, trage `bannanenimport.local` ein

4.  Bestätige mit den Anmeldedaten des Domain-Administrators
    (`Administrator` + Passwort)

5.  Nach Bestätigung neu starten

### Aufgabe – Teil D: Domainbenutzer testen

1.  Melde dich am Client mit einem Domainbenutzerkonto an, z. B.:

    - Benutzername: `BANANENIMPORT\aberger` (oder nur
      `aberger@bannanenimport.local`)

    - Passwort: das bei der Erstellung vergebene Passwort

2.  Überprüfe nach dem Login:

    - Ist Laufwerk `Z:` eingebunden? $\rightarrow$ Datei-Explorer öffnen

    - Zeigt `Z:` den richtigen Netzwerkpfad (`\\DC01\IT`)?

    - Kann eine Testdatei im Abteilungsordner erstellt werden?

    - Kann ein Benutzer einer anderen Abteilung **nicht** auf diesen
      Ordner zugreifen?

# Hilfestellung und Implementierungshinweise

## Windows-Client per PowerShell der Domain hinzufügen

Auf dem Client kann der Domain-Beitritt ebenfalls über die PowerShell
erfolgen:

``` sh
$cred = Get-Credential -UserName "BANANENIMPORT\Administrator" `
    -Message "Domain-Administrator-Passwort eingeben"

Add-Computer `
    -DomainName "bannanenimport.local" `
    -Credential $cred `
    -NewName    "CLIENT01" `
    -Restart
```

## Netzwerkdiagramm und Datenfluss beim Login

Beim Anmelden eines Domainbenutzers laufen folgende Schritte ab:

1.  Der Client sendet eine **Kerberos AS-REQ** an den DC
    (`192.168.100.1`)

2.  Der DC prüft die Anmeldedaten in der AD-Datenbank und stellt ein
    **Ticket Granting Ticket (TGT)** aus

3.  Das Betriebssystem verarbeitet die verknüpften **GPOs** für den
    Benutzer und den Computer

4.  Das **Logon-Skript** wird ausgeführt: Laufwerk `Z:` wird gemappt

5.  Der Benutzer ist angemeldet und hat Zugriff auf seinen
    Abteilungsordner

## Häufige Fehlerquellen

### DNS falsch konfiguriert

Active Directory ist extrem DNS-abhängig. Der häufigste Fehler ist, dass
der Windows-Client den falschen DNS-Server eingetragen hat.
Sicherstellen:

- DNS auf dem Client (per DHCP oder manuell): `192.168.100.1`

- Test: `nslookup bannanenimport.local` muss `192.168.100.1` zurückgeben

- Test: `nslookup DC01.bannanenimport.local` muss `192.168.100.1`
  zurückgeben

### Kerberos-Zeitdifferenz

Kerberos toleriert maximal **5 Minuten** Zeitdifferenz zwischen Client
und DC. Ist die Uhr eines Clients falsch gestellt, schlägt die Anmeldung
mit einem kryptischen Fehler fehl. Lösung:

- Sicherstellen, dass DC und Client dieselbe Zeitzone und eine
  synchronisierte Uhrzeit haben

- Im isolierten Lab ohne Internet: Auf dem DC einen internen NTP-Dienst
  konfigurieren oder Zeit manuell angleichen

### Logon-Skript wird nicht ausgeführt

- PowerShell-Ausführungsrichtlinie prüfen (muss per GPO auf
  *RemoteSigned* oder *Unrestricted* gesetzt sein)

- Skriptpfad in der GPO exakt prüfen – Groß-/Kleinschreibung beachten

- GPO-Verknüpfung prüfen: Ist die GPO mit der richtigen OU verknüpft?

- Mit `gpresult /r` auf dem Client überprüfen, welche GPOs angewendet
  wurden

- GPO-Verarbeitung erzwingen: `gpupdate /force`

### Domain-Beitritt schlägt fehl

- Ping zum DC (`192.168.100.1`) muss funktionieren

- DNS muss korrekt aufgelöst werden (`nslookup bannanenimport.local`)

- Administratorpasswort korrekt?

- Windows-Firewall auf dem DC: *Datei- und Druckerfreigabe* muss
  aktiviert sein  
  (*Systemsteuerung* $\rightarrow$ *Windows Defender Firewall*
  $\rightarrow$ *App durch Firewall zulassen*)

## Gruppenrichtlinien – Bonusaufgaben

Wer die Grundaufgaben fertiggestellt hat, kann folgende zusätzliche GPOs
konfigurieren:

**Passwortrichtlinie für die Domain:**

- Gruppenrichtlinienverwaltung $\rightarrow$ *Default Domain Policy*
  $\rightarrow$ Bearbeiten

- *Computerkonfiguration* $\rightarrow$ *Windows-Einstellungen*
  $\rightarrow$ *Sicherheitseinstellungen* $\rightarrow$
  *Kontorichtlinien* $\rightarrow$ *Kennwortrichtlinien*

- Empfehlung: Mindestlänge 10 Zeichen, Komplexitätsanforderungen
  aktiviert, maximales Kennwortalter 90 Tage

**Sperrbildschirm-Timeout:**

- Neue GPO auf OU-Ebene oder Domain-Ebene anlegen

- *Benutzerkonfiguration* $\rightarrow$ *Administrative Vorlagen*
  $\rightarrow$ *Systemsteuerung* $\rightarrow$ *Anpassung*
  $\rightarrow$ *Kennwortgeschützten Bildschirmschoner aktivieren* und
  *Wartezeit für den Bildschirmschoner festlegen* (z. B. 10 Minuten)

**Softwareverteilung (MSI-Pakete):**

- *Computerkonfiguration* $\rightarrow$ *Software-Einstellungen*
  $\rightarrow$ *Softwareinstallation*

- MSI-Datei auf einer Netzwerkfreigabe ablegen und per GPO zuweisen –
  wird beim nächsten Computerstart automatisch installiert

**USB-Zugriff sperren (z. B. für Buchhaltung):**

- GPO auf OU `Buchhaltung` anwenden

- *Computerkonfiguration* $\rightarrow$ *Administrative Vorlagen*
  $\rightarrow$ *System* $\rightarrow$ *Wechseldatenträgerzugriff*
  $\rightarrow$ *Alle Wechseldatenträgerklassen: Jeglichen Zugriff
  verweigern*

## Empfohlene Reihenfolge und Snapshot-Strategie

Für eine stabile Laborumgebung empfiehlt sich folgende Vorgehensweise:

<div class="center">

| **Schritt** | **Aufgabe**                                      | **Snapshot-Name**             |
|:-----------:|:-------------------------------------------------|:------------------------------|
|      1      | vmbr1 in Proxmox angelegt                        | –                             |
|      2      | DC01 VM erstellt, noch leer                      | `dc01-leer`                   |
|      3      | Windows Server installiert, Computername gesetzt | `dc01-winserver-basis`        |
|      4      | DC-Promote abgeschlossen                         | `dc01-dc-fertig`              |
|      5      | DHCP konfiguriert                                | `dc01-dhcp-fertig`            |
|      6      | OUs, Gruppen, Benutzer angelegt                  | `dc01-ad-objekte`             |
|      7      | Shares und Berechtigungen gesetzt                | `dc01-shares-fertig`          |
|      8      | GPOs und Logon-Skripte konfiguriert              | `dc01-gpo-fertig`             |
|      9      | CLIENT01 VM erstellt, noch leer                  | `client01-leer`               |
|     10      | Windows installiert, Domain-Beitritt             | `client01-domain-beigetreten` |

</div>

Mit dieser Snapshot-Strategie kann jederzeit zu einem sauberen Zustand
zurückgekehrt werden, ohne die gesamte Umgebung neu aufbauen zu müssen.
