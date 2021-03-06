# ABL eMH
Die Instanz ABL eMH repräsentiert eine einzelne Wallbox vom Typ eMH.


### Inhaltsverzeichnis

1. [Funktionsumfang](#1-funktionsumfang)
2. [Voraussetzungen](#2-voraussetzungen)
3. [Software-Installation](#3-software-installation)
4. [Einrichten der Instanzen in IP-Symcon](#4-einrichten-der-instanzen-in-ip-symcon)
5. [Statusvariablen und Profile](#5-statusvariablen-und-profile)
6. [WebFront](#6-webfront)
7. [PHP-Befehlsreferenz](#7-php-befehlsreferenz)

### 1. Funktionsumfang

- Abfragen aller relevanten Parameter der Wallbox, wie Zustand, Fehler oder aktuelle Ladeströme
- Sperren/Entsperren des Anschlusses
- Setzen des maximalen Ladestroms
- Setzen der Geräte ID via Broadcast-Befehl (dabei darf nur eine einzelne Wallbox am Bus hängen!)

__Hinweis:__
Die eMH1 kann ab Baujahr Q2-2021 in der 11kW-Version keine Ströme mehr messen,
da die notwendigen Strommesswandler nicht mehr verbaut werden. Die 22kW-Version ist davon nicht betroffen.


### 2. Voraussetzungen

- IP-Symcon ab Version 5.6
- Installierte ABL Chargepoint Splitter Instanz zur Busanbindung


### 3. Software-Installation

* Über den Module Store das 'ABL eMH'-Modul installieren.
* Alternativ das Repository manuell in das Module Verzeichnis der Symcon Installation kopieren.


### 4. Einrichten der Instanzen in IP-Symcon

 Unter 'Instanz hinzufügen' kann das 'ABL eMH'-Modul mithilfe des Schnellfilters gefunden werden.  
	- Weitere Informationen zum Hinzufügen von Instanzen in der [Dokumentation der Instanzen](https://www.symcon.de/service/dokumentation/konzepte/instanzen/#Instanz_hinzufügen)


__Konfigurationsseite__:

Name         | Beschreibung
------------ | ------------------
Geräte ID    | Angabe der GeräteID der Wallbox



### 5. Statusvariablen und Profile

Die Statusvariablen/Kategorien werden automatisch angelegt. Das Löschen einzelner kann zu Fehlfunktionen führen.



#### Statusvariablen

Name                        | Typ     | Beschreibung
--------------------------- | ------- | ------------
Fahrzeug angeschlossen      | Bool    | true, wenn ein fahrzeug angeschlossen ist
Fahrzeug hat Ladefreigabe   | Bool    | true, wenn der Ladepunkt eine Ladung freigibt
Fahrzeug lädt               | Bool    | true, wenn das Fahrzeug wirklich lädt      
Anschluss gesperrt          | Bool    | true, wenn der Anschluss gesperrt ist
Zustand                     | String  | aktueller Gerätezustand (A1, B1, B2, C2...)
Ladestrom L1                | Float   | aktueller Ladestrom L1 in Ampere
Ladestrom L2                | Float   | aktueller Ladestrom L2 in Ampere
Ladestrom L3                | Float   | aktueller Ladestrom L3 in Ampere
maximaler Ladestrom         | Int     | aktuell eingestellter max. Ladestrom (änderbar)
Gerätefehler                | Bool    | true, wenn ein Gerätefehler vorliegt (Fehlercode siehe Zustand)
Kommunikationsfehler        | Bool    | true, wenn die Wallbox in den letzten 10 Sekunden nicht geantwortet hat
Schalteingang EN1           | Bool    | aktueller Status des Schalteingangs EN1 im Gerät
Schalteingang EN2           | Bool    | aktueller Status des Schalteingangs EN2 im Gerät
Gerätetyp                   | String  | ausgelesender Gerätetyp
Seriennummer                | String  | ausgelesene Seriennummer



#### Profile

Name               | Typ
------------------ | -------
ABLEMH.MaxCurrent.16  | Integer
ABLEMH.MaxCurrent.32  | Integer
ABLEMH.BoolState      | Bool
ABLEMH.BoolErrorState | Bool
ABLEMH.OutletState	| String



### 6. WebFront

Die Variablen "Anschluss gesperrt" und "maximaler Ladestrom" können über das Webfront eingestellt werden


### 7. PHP-Befehlsreferenz

#### `boolean ABLEMH_RequestStatus(integer $InstanzID);`
Fragt den Zustand der Wallbox ab. Wird durch Instanz automatisch regelmäßig aufgerufen.

Beispiel: `ABLEMH_RequestStatus(12345);`
<br><br>


#### `boolean ABLEMH_GetDeviceIdent(integer $InstanzID);`
Fragt den Gerätetyp und die Seriennummer ab. Wird durch Instanz automatisch einmalig beim Start abgefragt.

Beispiel: `ABLEMH_GetDeviceIdent(12345);`
<br><br>


#### `boolean ABLEMH_SetLockOutlet(integer $InstanzID, bool $value);`
Sperrt die Wallbox, so dass kein Laden möglich ist.

Beispiel: `ABLEMH_SetLockOutlet(12345, true);`
<br><br>


#### `boolean ABLEMH_SetMaxCurrent(integer $InstanzID, int $value);`
Setzt den maximalen Ladestrom in Ampere.

Beispiel für 16A: `ABLEMH_SetMaxCurrent(12345, 16);`
<br><br>


#### `boolean ABLEMH_SetDeviceID(integer $InstanzID, int $value);`
Setzt die GeräteID des angeschlossenen Gerätes. Dies erfolgt per Braodcast.
Es darf sich zu der Zeit daher nur ein Gerät am Bus befinden!

Beispiel, ID 2 setzen: `ABLEMH_SetDeviceID(12345, 2);`
<br><br>


#### `boolean ABLEMH_ResetDevice(integer $InstanzID);`
Startet die Wallbox neu.

Beispiel: `ABLEMH_ResetDevice(12345);`
<br><br>
