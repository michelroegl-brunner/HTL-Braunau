- [Inhaltsverzeichnis](#inhaltsverzeichnis)
- [Grundlagen](#grundlagen)
  - [Was ist Router-on-a-Stick?](#was-ist-router-on-a-stick)
  - [VLANs, Trunking und 802.1Q](#vlans-trunking-und-802.1q)
  - [Rolle des Default Gateways](#rolle-des-default-gateways)
  - [Was macht ein Subinterface am
    Router?](#was-macht-ein-subinterface-am-router)
- [Topologie](#topologie)
  - [Standardaufbau pro Gruppe](#standardaufbau-pro-gruppe)
- [IP-Plan](#ip-plan)
  - [Adressierungstabelle](#adressierungstabelle)
  - [Interface-Belegung (Verkabelung)](#interface-belegung-verkabelung)
- [Aufgabe: Router-on-a-Stick](#aufgabe-router-on-a-stick)
  - [Teil A: Switch konfigurieren](#teil-a-switch-konfigurieren)
  - [Teil B: Router konfigurieren](#teil-b-router-konfigurieren)
  - [Teil C: Endgeraete konfigurieren und
    testen](#teil-c-endgeraete-konfigurieren-und-testen)
  - [Kontrollbefehle](#kontrollbefehle)
- [Netzzeichnung](#netzzeichnung)
  - [Logische Darstellung mit IPs](#logische-darstellung-mit-ips)

# Inhaltsverzeichnis

# Grundlagen

## Was ist Router-on-a-Stick?

Router-on-a-Stick ist ein Inter-VLAN-Routing-Verfahren, bei dem ein
Router mit nur einem physischen Interface mehrere VLANs routet. Statt
vieler einzelner physischer Links wird ein gemeinsamer Trunk zwischen
Switch und Router verwendet.

Der Router nutzt dafuer Subinterfaces (z. B. `Gi0/0.10` oder
`Fa0/0.10`). Jedes Subinterface ist genau einem VLAN zugeordnet und
erhaelt die Gateway-IP fuer dieses VLAN.

## VLANs, Trunking und 802.1Q

VLANs trennen ein Layer-2-Netz logisch in mehrere Broadcast-Domaenen.
Damit Frames verschiedener VLANs ueber eine gemeinsame Leitung
uebertragen werden koennen, wird IEEE 802.1Q Tagging verwendet.

Zwischen Switch und Router wird ein Trunk-Port konfiguriert. Der Switch
taggt die Frames je VLAN, und der Router ordnet sie ueber
`encapsulation dot1Q <VLAN-ID>` den passenden Subinterfaces zu.

## Rolle des Default Gateways

Endgeraete in einem VLAN senden Verkehr zu anderen Netzen an ihr Default
Gateway. In dieser Uebung ist das Gateway pro VLAN jeweils die Adresse
`.254` auf dem zugehoerigen Router-Subinterface.

## Was macht ein Subinterface am Router?

Ein Subinterface ist ein **virtuelles Interface** auf einem physischen
Router-Port. Dadurch kann ein einzelner Port (z. B. `Gi0/0` oder
`Fa0/0`) mehrere logisch getrennte VLAN-Netze bedienen.

Jedes Subinterface bekommt:

- eine VLAN-Zuordnung via `encapsulation dot1Q <VLAN-ID>`

- eine eigene IP-Adresse als Default Gateway des VLANs

Beispiel:

    interface gi0/0.10 (oder fa0/0.10)
     encapsulation dot1Q 10
     ip address 10.10.10.254 255.255.255.0

    interface gi0/0.20 (oder fa0/0.20)
     encapsulation dot1Q 20
     ip address 10.10.20.254 255.255.255.0

Ohne Subinterfaces koennte der Router die VLAN-Tags am Trunk nicht
sauber den jeweiligen Netzen zuordnen.

# Topologie

## Standardaufbau pro Gruppe

Jede Gruppe arbeitet mit derselben Standard-Topologie:

- 1x Cisco 3550 Switch

- 1x Cisco 2800 Series Router

- 2 VLANs (VLAN 10 und VLAN 20)

- 1 PC pro VLAN

Adressregel:

- PC: `.1`

- Switch-SVI: `.253`

- Router-Subinterface (Gateway): `.254`

# IP-Plan

## Adressierungstabelle

| Geraet    | Interface                | VLAN | IP-Adresse   | Maske         | Default Gateway |
|:----------|:-------------------------|:-----|:-------------|:--------------|:----------------|
| PC-VLAN10 | NIC                      | 10   | 10.10.10.1   | 255.255.255.0 | 10.10.10.254    |
| PC-VLAN20 | NIC                      | 20   | 10.10.20.1   | 255.255.255.0 | 10.10.20.254    |
| SW-3550   | VLAN 10 (SVI)            | 10   | 10.10.10.253 | 255.255.255.0 | 10.10.10.254    |
| SW-3550   | VLAN 20 (SVI)            | 20   | 10.10.20.253 | 255.255.255.0 | 10.10.20.254    |
| RTR-2800  | Gi0/0.10 (oder Fa0/0.10) | 10   | 10.10.10.254 | 255.255.255.0 | \-              |
| RTR-2800  | Gi0/0.20 (oder Fa0/0.20) | 20   | 10.10.20.254 | 255.255.255.0 | \-              |

## Interface-Belegung (Verkabelung)

| Verbindung               | Linke Seite    | Rechte Seite                |
|:-------------------------|:---------------|:----------------------------|
| PC-VLAN10 zu Switch      | PC-VLAN10 NIC  | SW-3550 Fa0/1               |
| PC-VLAN20 zu Switch      | PC-VLAN20 NIC  | SW-3550 Fa0/2               |
| Switch zu Router (Trunk) | SW-3550 Fa0/24 | RTR-2800 Gi0/0 (oder Fa0/0) |

# Aufgabe: Router-on-a-Stick

## Teil A: Switch konfigurieren

1.  VLAN 10 und VLAN 20 auf dem Switch anlegen.

2.  Einen Access-Port fuer PC-VLAN10 auf VLAN 10 konfigurieren.

3.  Einen Access-Port fuer PC-VLAN20 auf VLAN 20 konfigurieren.

4.  Den Uplink zum Router als Trunk konfigurieren.

5.  Optional: SVI-Adressen fuer VLAN 10 und VLAN 20 gemaess Tabelle
    setzen.

## Teil B: Router konfigurieren

1.  Physisches Interface `Gi0/0` (oder `Fa0/0`) aktivieren.

2.  Subinterface `Gi0/0.10` (oder `Fa0/0.10`) erstellen:

    - `encapsulation dot1Q 10`

    - IP-Adresse `10.10.10.254/24`

3.  Subinterface `Gi0/0.20` (oder `Fa0/0.20`) erstellen:

    - `encapsulation dot1Q 20`

    - IP-Adresse `10.10.20.254/24`

## Teil C: Endgeraete konfigurieren und testen

1.  IP-Adresse und Gateway auf beiden PCs gemaess Tabelle eintragen.

2.  Pruefen:

    - PC-VLAN10 pingt `10.10.10.254`

    - PC-VLAN20 pingt `10.10.20.254`

    - PC-VLAN10 pingt PC-VLAN20

    - PC-VLAN20 pingt PC-VLAN10

3.  Wenn ein Ping fehlschlaegt: VLAN-Zuordnung, Trunk, Subinterfaces und
    Gateways kontrollieren.

## Kontrollbefehle

    show vlan brief
    show interfaces trunk
    show ip interface brief
    show running-config
    ping <ziel-ip>

# Netzzeichnung

## Logische Darstellung mit IPs

    PC-VLAN10 (10.10.10.1/24, GW 10.10.10.254) -- Access VLAN 10 --+
                                                                    |
                                                                 SW-3550
                                                         SVI10: 10.10.10.253
                                                         SVI20: 10.10.20.253
                                                                    |
                                                              802.1Q Trunk
                                                                    |
                                                            RTR-2800 Gi0/0 (oder Fa0/0)
                                                       Gi0/0.10 (oder Fa0/0.10): 10.10.10.254
                                                       Gi0/0.20 (oder Fa0/0.20): 10.10.20.254
                                                                    |
    PC-VLAN20 (10.10.20.1/24, GW 10.10.20.254) -- Access VLAN 20 --+
