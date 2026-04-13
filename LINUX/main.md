# Grundlagen

## Was ist Linux?

Linux ist ein freies, plattformunabhaengiges
Mehrbenutzer-Betriebssystem. In der Praxis verwendet man fast immer eine
Linux-Distribution. Eine Distribution kombiniert den Linux-Kernel mit
Programmen, Paketverwaltung und Werkzeugen zu einem sofort nutzbaren
Gesamtsystem.

Linux trennt klar zwischen Hardware und Anwendungen. Programme greifen
nicht direkt auf Geraete zu, sondern nutzen Systemaufrufe. Der Kernel
nimmt diese Aufrufe entgegen, verwaltet Ressourcen und steuert ueber
Treiber die Kommunikation mit der Hardware.

## Aufbau des Systems

### Kernel, Treiber und Systemaufrufe

Der Kernel ist das Kernstueck des Betriebssystems. Er stellt Funktionen
fuer Speicherverwaltung, Prozessverwaltung, Dateisysteme und
Geraetezugriff bereit. Wenn ein Programm z. B. Text ausgeben will,
startet es einen Systemaufruf. Erst das Betriebssystem fuehrt die Aktion
auf der Hardware aus.

Treiber sind die Schnittstelle zu konkreten Geraeten. Dadurch bleibt
Software weitgehend unabhaengig von einzelner Hardware, weil der Zugriff
immer ueber den Kernel und die passenden Treiber erfolgt.

### Shell und Prozesse

Die Shell ist eine textbasierte Benutzerschnittstelle. In dieser Arbeit
wird die **BASH** (Bourne Again Shell) verwendet, die auf vielen
Linux-Systemen Standardshell ist. Befehle in der Shell werden vom System
verarbeitet und als entsprechende Operationen ausgefuehrt.

Linux ist ein Multitasking-System: Mehrere Prozesse koennen scheinbar
gleichzeitig laufen. Ein Scheduler teilt die CPU-Zeit in sehr kleine
Zeitscheiben auf und weist sie Prozessen zu. Dadurch bleibt das System
reaktionsfaehig, auch wenn einzelne Programme viel Rechenzeit
benoetigen.

## Benutzer, Gruppen und Rechte

Linux ist ein Multiuser-System. Jeder Benutzer besitzt eine eindeutige
**User ID (UID)** und ist mindestens einer Gruppe mit **Group ID (GID)**
zugeordnet. Ueber Benutzer- und Gruppenzugehoerigkeit wird gesteuert,
wer welche Dateien lesen, veraendern oder ausfuehren darf.

Eine Sonderrolle hat der Benutzer **root** mit UID 0. Root besitzt
nahezu uneingeschraenkten Zugriff auf das gesamte System. Deshalb
arbeitet man im Alltag moeglichst mit einem normalen Benutzerkonto und
verwendet administrative Rechte nur gezielt.

## Dateien und Dateirechte

Datei- und Verzeichnisnamen sind unter Linux case-sensitive: `DATEI`,
`datei` und `Datei` sind verschiedene Namen. Punkte sind normale Zeichen
im Dateinamen. Namen, die mit einem Punkt beginnen, gelten als
versteckt.

Neben regulaeren Dateien gibt es weitere Dateiarten, unter anderem
Verzeichnisse, symbolische Links und Geraetedateien. Unter Linux gilt
grundsaetzlich: Vieles wird als Datei modelliert.

Fuer Zugriffsrechte wird haeufig die Kurzschreibweise `rwx` verwendet:

-   `r` (read): lesen

-   `w` (write): schreiben

-   `x` (execute): ausfuehren

Diese Rechte werden getrennt fuer Eigentuemer, Gruppe und andere
Benutzer vergeben.

## Dateisystem-Grundlagen

Ein Dateisystem organisiert Daten auf einem Datentraeger in Bloecken
(bzw. Clustern) und verwaltet, wo Dateien physisch liegen. Die Wahl des
Dateisystems beeinflusst unter anderem Performance, Zuverlaessigkeit und
Wiederherstellbarkeit nach Fehlern.

Unter Linux sind ext-Dateisysteme weit verbreitet, insbesondere ext4.
Journalfaehige Dateisysteme protokollieren Metadaten-Aenderungen und
erhoehen so die Konsistenz nach Abstuerzen.

## Verzeichnisstruktur unter Linux

Im Gegensatz zu Windows mit mehreren Laufwerksbuchstaben besitzt Linux
einen einzigen Verzeichnisbaum ab dem Wurzelverzeichnis `/`. Weitere
Datentraeger werden an passenden Stellen in diesen Baum eingehangen
(gemountet).

Wichtige Verzeichnisse sind zum Beispiel:

-   `/bin`: grundlegende Programme

-   `/boot`: Kernel und Startdateien

-   `/dev`: Geraetedateien

-   `/etc`: systemweite Konfiguration

-   `/home`: Home-Verzeichnisse normaler Benutzer

-   `/root`: Home-Verzeichnis des Benutzers root

-   `/tmp`: temporaere Dateien

-   `/usr`: Programme und systemweite Ressourcen

-   `/var`: veraenderliche Daten, z. B. Logs

Diese einheitliche Struktur ist ein grosser Vorteil von
Linux/Unix-Systemen, weil man sich auf unterschiedlichen Distributionen
schnell zurechtfindet.

## Distributionen und Desktop-Umgebungen

Linux liegt nicht als ein einziges fertiges Produkt vor, sondern in
vielen Distributionen. Bekannte Beispiele sind Debian, Fedora, openSUSE,
Ubuntu und Linux Mint. Sie unterscheiden sich unter anderem bei
Paketquellen, Release-Modell, Vorkonfiguration und Zielgruppe.

Auch die grafische Oberflaeche ist frei waehlbar. Verbreitete
Desktop-Umgebungen sind GNOME, KDE, XFCE, MATE und Cinnamon. Fuer den
Shell-Unterricht ist jedoch vor allem relevant, dass alle diese Systeme
eine leistungsfaehige Kommandozeile mit BASH bereitstellen.

# Die Linux-Shell (Bash)

## Grundidee der Shell

Die Shell ist die textbasierte Schnittstelle zwischen Benutzer und
Betriebssystem. In dieser Arbeit wird die Bash (Bourne Again Shell)
verwendet. Befehle werden in der Kommandozeile eingegeben und direkt
ausgefuehrt. Viele Verwaltungs- und Automatisierungsaufgaben lassen sich
damit schneller erledigen als ueber eine grafische Oberflaeche.

Nach dem Start befindet man sich ueblicherweise im Home-Verzeichnis. Mit
`pwd` (print working directory) kann jederzeit geprueft werden, in
welchem Verzeichnis man aktuell arbeitet.

## Navigation und Orientierung

Die wichtigsten Befehle fuer den Einstieg sind:

-   `ls`: listet Dateien und Verzeichnisse auf

-   `ls -l`: detaillierte Ansicht mit Rechten, Besitzer und Groesse

-   `ls -a`: zeigt auch versteckte Dateien (Namen mit Punkt am Anfang)

-   `cd <verzeichnis>`: wechselt in ein Verzeichnis

-   `cd ..`: wechselt eine Ebene nach oben

-   `cd /`: wechselt ins Wurzelverzeichnis

-   `cd`: wechselt ins Home-Verzeichnis

Zum Suchen von Dateien dient `find`, zum Beispiel
`find /etc -name fonts.conf`. Damit kann man gezielt in
Verzeichnisbaeumen nach Dateinamen suchen.

## Dateien lesen, erstellen und verwalten

Zum Lesen von Textdateien eignet sich `less`. Die Datei wird seitenweise
angezeigt, was bei groesseren Inhalten deutlich uebersichtlicher ist als
eine direkte Komplettausgabe.

Dateien und Ordner werden mit folgenden Befehlen erstellt:

-   `mkdir <name>`: neues Verzeichnis

-   `touch <name>`: leere Datei

Fuer die Verwaltung werden vor allem drei Befehle verwendet:

-   `cp <quelle> <ziel>`: kopieren

-   `mv <quelle> <ziel>`: verschieben oder umbenennen

-   `rm <datei>` und `rmdir <verzeichnis>`: loeschen

## Variablen und Ausgabe

Mit `echo` werden Texte oder Variableninhalte ausgegeben. Variablen
lassen sich direkt in der Shell setzen:

-   `MeinText="Ich bin wiederverwendbar"`

-   `echo $MeinText`

Wichtige Umgebungsvariablen sind zum Beispiel `$HOME`, `$HOSTNAME`,
`$LOGNAME` und `$UID`. Sie stellen Systeminformationen bereit und sind
besonders in Shell-Skripten nuetzlich.

## Umleitungen und Pipes

Die Standardausgabe kann in Dateien umgeleitet werden:

-   `>`: schreibt neu und ueberschreibt vorhandenen Inhalt

-   `>>`: haengt an vorhandenen Inhalt an

Beispiel: `ls -l > info.txt`

Fehlerausgaben lassen sich getrennt behandeln, z. B. mit `2> /dev/null`.
Mit `|` (Pipe) wird die Ausgabe eines Befehls als Eingabe fuer den
naechsten genutzt, z. B. `ls -l | less`.

## Filtern, Hilfe und Administration

`cat` gibt Dateiinhalt auf der Standardausgabe aus. In Kombination mit
Wildcards oder Umleitung koennen auch mehrere Dateien zusammengefuehrt
werden.

`grep` filtert Zeilen nach Mustern. Das ist hilfreich, um in grossen
Ausgaben schnell relevante Zeilen zu finden oder irrelevante
auszuschliessen.

Fuer Dokumentation und Hilfe stehen drei Werkzeuge bereit:

-   `man <befehl>`: Handbuchseite

-   `info <befehl>`: erweiterte Info-Dokumentation

-   `help`: integrierte Hilfe der Shell

Administrationsaufgaben werden mit erhoehten Rechten ausgefuehrt. Dazu
dienen je nach System `su` oder `sudo`. Im Alltag sollte weiterhin mit
normalen Benutzerrechten gearbeitet werden.

# Debian mit WSL unter Windows einrichten

## Voraussetzung

Diese Anleitung gilt fuer einen Windows-PC mit PowerShell.

## Installation von WSL mit Debian

1.  Oeffne **PowerShell als Administrator**.

2.  Fuehre folgenden Befehl aus:

    -   `wsl –install -d debian`

3.  Starte den PC neu, falls Windows dazu auffordert.

4.  Starte danach **Debian** mit `wsl` in PowerShell.

## Erster Start und Benutzer

Bei der installation richtet sich Debian ein und fragt nach einem
Linux-Benutzer.

-   Waehle als Benutzername bitte deinen **Vornamen**.

-   Vergib ein Passwort und merke es dir gut.

Wichtig bei der Passworteingabe in Linux:

-   Es werden **keine Zeichen** angezeigt.

-   Es erscheinen auch **keine Sternchen**.

-   Die Eingabe funktioniert trotzdem normal. Nach dem Tippen einfach
    mit ENTER bestaetigen.

## Kurzer Funktionstest

Nach erfolgreicher Einrichtung kannst du in Debian zum Test folgende
Befehle ausfuehren:

-   `whoami`

-   `pwd`

-   `ls`

Wenn diese Befehle funktionieren, ist dein Debian-System unter WSL
betriebsbereit.

Vergiss nicht in dein Home-Verzeichniss zu wechslen. `cd  `

# Aufgaben zur Linux-Shell

1.  Lege in deinem Home-Verzeichnis ein Verzeichnis mit dem Namen `COPR`
    an.

2.  Gib den String `Hello, world!` in der Shell aus.

3.  Pruefe mit `man`, wie `echo` offiziell beschrieben wird.

4.  Gib die Strings `fee`, `fie`, `foe` und `fum` aus und nutze dabei
    die Befehls-Historie mit den Pfeiltasten.

5.  Loesche die sichtbare Ausgabe in der Kommandozeile.

6.  Gib den Text
    `Use "man echo". Tippe: Doppelte Anfuehrungszeichen verwenden.`
    korrekt aus.

7.  Finde mit `help` heraus, wie das Terminal fuer 5 Sekunden pausiert
    werden kann (`sleep`).

8.  Erzeuge mit `echo` und Umleitung zwei Dateien `line_1.txt` und
    `line_2.txt` mit beliebigem Inhalt.

9.  Kombiniere beide Dateien mit `cat` in eine dritte Datei.

10. Zeige den Inhalt der dritten Datei mit `less` an.

11. Liste alle nicht versteckten Dateien auf, die mit `l` beginnen.

12. Liste alle Dateien inklusive versteckter Dateien in der langen Form
    auf.

13. Erstelle `foo.txt` mit dem Inhalt `hello world`, kopiere die Datei
    und nenne die Kopie `bar.txt`.

14. Notiere die noetigen Befehle, um eine Datei `foo` in `bar`
    umzubenennen und danach eine Kopie `foobar` zu erzeugen.

# Linux Shell Skripte

## Einführung in Bash-Skripte

Ein Bash-Skript ist eine Textdatei mit Shell-Befehlen, die nacheinander
ausgefuehrt werden. Am Beginn steht ueblicherweise der sogenannte
Shebang:

-   `#!/bin/bash`

Damit wird festgelegt, mit welcher Shell das Skript interpretiert werden
soll. Danach koennen beliebige Befehle folgen,
z. B. `echo ’Servus Linux’`.

Damit ein Skript direkt gestartet werden kann, muss es ausfuehrbar sein,
zum Beispiel mit `chmod +x <datei>`. Der Start erfolgt dann ueber:

-   `./<dateiname>`

-   `./<dateiname> <parameter1> <parameter2>`

## Parameter und Exit-Codes

Beim Aufruf koennen Parameter uebergeben werden. Im Skript werden sie
ueber Stellungsparameter angesprochen:

-   `$0`: Name des Skripts

-   `$1`, `$2`, ...: uebergebene Parameter

-   `$#`: Anzahl aller Parameter

-   `$?`: Exit-Code des zuletzt ausgefuehrten Befehls

Ein Exit-Code von `0` bedeutet meist Erfolg, ein Wert ungleich `0` weist
auf einen Fehler hin. Mit `exit <code>` kann ein Skript einen eigenen
Rueckgabewert setzen.

## Bedingungen mit test und if

Fuer logische Pruefungen werden in Bash meist `test` oder die
Kurzschreibweise `[ ... ]` verwendet. Typische Pruefungen sind:

-   String-Vergleich: `==`

-   Zahlenvergleich: `-eq`, `-lt`, `-gt`

-   Datei existiert: `-e`

-   Verzeichnis existiert: `-d`

-   String leer/nicht leer: `-z`, `-n`

Darauf baut die `if`-Anweisung auf:

-   `if [ <bedingung> ]; then ... else ... fi`

Wichtig ist die korrekte Schreibweise mit Leerzeichen innerhalb der
eckigen Klammern.

## Schleifen in Bash

Fuer Wiederholungen werden vor allem `for`- und `while`-Schleifen
verwendet.

**for** eignet sich gut fuer bekannte Wiederholungszahlen oder Listen,
zum Beispiel zum Durchlaufen von Parametern. **while** wird genutzt,
solange eine Bedingung wahr ist, etwa fuer Zaehler-Schleifen.

Beide Schleifentypen sind zentrale Werkzeuge, um wiederkehrende Aufgaben
in Skripten kompakt und sauber zu loesen.

## Funktionen und Variablenbereiche

Funktionen strukturieren Skripte und machen Code wiederverwendbar.
Parameter werden innerhalb von Funktionen ebenfalls ueber `$1`, `$2`,
...angesprochen.

Rueckgaben erfolgen mit `return`. Fuer den Variablenbereich gilt:

-   globale Variablen sind im ganzen Skript sichtbar

-   lokale Variablen werden in Funktionen mit `local` deklariert

So lassen sich Seiteneffekte vermeiden und Skripte besser wartbar
halten.

## Debugging und praktische Hinweise

Zum Nachvollziehen der Befehlsausfuehrung ist `bash -x <skript>` sehr
hilfreich. Damit wird beim Lauf angezeigt, welche Zeilen gerade
ausgefuehrt werden und welche Werte Variablen annehmen.

Fuer robustere Skripte sollten Eingaben frueh geprueft werden (Anzahl
und Gueltigkeit der Parameter, Existenz von Dateien/Verzeichnissen,
Rechtepruefung bei Admin-Aufgaben).

# Aufgaben zu Shell-Skripten

## Leicht

1.  **Einfache Rechenmaschine (`rechner.sh`)**\
    Nimm zwei Zahlen als Parameter entgegen und berechne Summe,
    Differenz, Produkt und Division in einer gut lesbaren Ausgabe.\
    *Beispiel Eingabe:* `./rechner.sh 10 5`\
    *Beispiel Ausgabe:*
    `Summe: 15, Differenz: 5, Produkt: 50, Division: 2`\
    *Erweiterung:* Gib eine Fehlermeldung aus, wenn Parameter fehlen
    oder ungueltig sind.

2.  **Dateizaehler in einem Verzeichnis (`count_files.sh`)**\
    Nimm ein Verzeichnis als Parameter, pruefe dessen Existenz und
    zaehle Dateien sowie Unterordner.\
    *Beispiel Eingabe:* `./count_files.sh ./testdaten`\
    *Beispiel Ausgabe:* `Dateien: 8, Ordner: 3`\
    *Erweiterung:* Zaehle optional nur Dateien mit einer bestimmten
    Endung (z. B. `.txt`).

## Mittel

3.  **Backup-Skript (`backup.sh`)**\
    Kopiere ein angegebenes Verzeichnis in ein Backup-Verzeichnis und
    versehe den Zielordner mit Zeitstempel
    (z. B. `backup_2026-02-27_14-30`).\
    *Beispiel Eingabe:* `./backup.sh ./projekt ./backups`\
    *Beispiel Ausgabe:*
    `Backup erstellt: ./backups/backup_2026-02-27_14-30`\
    *Erweiterung:* Loesche alte Backups automatisch (aelter als 7 Tage).

4.  **Logfile-Analyzer (`log_analyzer.sh`)**\
    Analysiere eine Logdatei und zaehle, wie oft `ERROR`, `WARNING` und
    `INFO` vorkommen. Gib die Ergebnisse formatiert aus.\
    *Beispiel Eingabe:* `./log_analyzer.sh app.log`\
    *Beispiel Ausgabe:* `ERROR: 4, WARNING: 7, INFO: 21`\
    *Erweiterung:* Werte optional nur einen per Parameter gewaehlten
    Log-Level aus.

5.  **Shell-Skript zum Bewegen von Dateien (`mvprot.sh`)**\
    Entwickle ein Skript, das im aktuellen Verzeichnis alle Dateien
    einer angegebenen Endung in ein neu erstelltes Unterverzeichnis
    verschiebt. Das Skript besitzt drei Parameter: Dateiendung, Name des
    Unterverzeichnisses und ein Flag, ob ein Protokoll erstellt werden
    soll. Bei aktivem Protokoll sollen Anzahl und Namen der verschobenen
    Dateien in eine Protokolldatei im Quellverzeichnis geschrieben
    werden.\
    *Beispiel Eingabe:* `./mvprot.sh txt archiv ja`\
    *Beispiel Ausgabe:*
    `5 Dateien nach ./archiv verschoben. Protokoll: ./mvprot.log`\
    *Hinweis:* Die Anzahl kann z. B. ueber Kommandosubstitution
    ermittelt werden, etwa mit `AnzahlDateien=$(ls *.txt | wc -w)`.

## Schwer

6.  **Benutzer-Management-Skript (`user_manager.sh`)**\
    Pruefe, ob das Skript mit Root-Rechten laeuft, und unterstuetze das
    Hinzufuegen sowie Loeschen von Benutzern (`add`/`delete`). Pruefe
    dabei auch, ob ein Benutzer bereits existiert.\
    *Beispiel Eingabe:* `sudo ./user_manager.sh add max`\
    *Beispiel Ausgabe:* `Benutzer max wurde erfolgreich angelegt.`\
    *Erweiterung:* Passwort automatisch generieren und Benutzer einer
    Gruppe zuordnen.

# Abgabe

Die Ausarbeitung ist als **PDF-Datei** abzugeben.

Pro Aufgabe ist ein Screenshot der Ausgabe des Befehls zu machen. Neu
verwendete Befehle sind in kurzen Stichworten zu beschreiben.

Bitte lade deine fertige PDF in die entsprechende **Teams-Aufgabe**
hoch.

Test
