## Grundlagen
Eigenscahften:
- Datenvertraulichkeit: Geheime Daten bleiben geheim
- Datenintegrität: Unautorisierte Nutzer dürfen nicht Daten Manipulieren
- Systemverfügbarkeit: Sicherstellung einer Mindestverfüghbarkeit unabhängig von der aktuellen Lastsituation
- Datenschutz

Bedrohungen: Aufdeckung, Manipulation, Denial-of-Service, Speicherung und Kombination persönlicher Daten


## Eindringlinge
- Herumschnuffeln(Sniff around)
- Economic benefit
- Spionage
- Viren


## Kryptografie
Eine Nachricht in Klartext verschlüsseln

- Symmetrische Kryptografie (Secret-Key-Kryptografie)
	Schlüssel zur Verschlüsselung lässt sich aus dem Schlüssel zur Entschlüsselung bestimmen und umgekehrt
- Public-Key-Kryptografie
	Benutzer lässt ein Schlüsselpaar (Private Key KD, Public Key KE) so erstellen, dass KDKE(m) = KEKD(m) = m


## Einwegfunktionen und digitale Signaturen
- Einwegfunktionen: Wird x mit der Einwegfunktion f verschlüsselt, so ist die Bestimmung von x aus dem Ergebnis f(x) praktisch unmöglich
- Digitale Signaturen: Eindeutige Senderidentifikation
	- Dokumentbearbeitung mit Einweg-Hash-Funktion
		- MD5 (Message Digest): 16 Byte langes Ergebnis
		- SHA (Secure Hash Algorithm): 20 Byte langes Ergebnis
	- Hash different ==> manipulated


## Benutzer-Authentifizierung
Realisierung der Authentifikation durch etwas, was der Benutzer
- Weiß => z.B. Passwortabfrage
- Besitzt => z.B. Chipkarten
- Ist => z.B. biometrische Abfragen

#### Authentifizierung durch Passwörter
- Einfachste Implementierung basiert auf einer zentralen Liste, die den Loginnamen und das Passwort enthält
- Vergleich der eingegebenen und gespeicherten Daten: Bei Übereinstimmung wird der Zugriff gestattet

**Schwachstellen**
Angriffe durch Ausprobieren von Kombinationen (Login, Passwort)
- Hilfe durch Wörterbücher, Namenslisten,

Vorgehensweise
- Ermittlung aktiver, vernetzter Rechner, z.B. durch ping w.x.y.z => aktive Rechner antworten und dienen als Ziele
- Ausprobieren der Passwortkombinationen
- Bei Begrenzung möglicher sukzessiver Versuche => Starten vieler paralleler Threads und Verteilung auf mehrere Rechner
- Installation von Paket-Sniffern zur Überwachung des Netzwerkverkehrs und Aufspüren von interessanten Mustern


## Passwortsicherheit in UNIX
Passwort als Schlüssel zur Verschlüsselung eines festen Datenblocks
Passwortdatei: je eine Zeile mit Attributen und verschlüsseltem Passwort pro Benutzer

Beim Einloggen: Verschlüsselung sofort nach Passworteingabe. Niemand (auch nicht Sysadmin) kann die Passwörter in Klartext sehen
Allerdings ist der Verschlüsselungsalgorithmus bekannt
=> Wörterbuch kann entsprechend verschlüsselt und mit den Einträgen der Passwortdatei verglichen werden

**Probleme früherer Versionen**
-  Passwortdatei (/etc/passwd) mit verschlüsselten Passwörtern auf der Festplatte für Benutzer sichtbar
-  Datei wird kopiert und zum Offlineausprobieren der Passwörter genutzt
- Mittlerweile getrennte Speicherung der Passwörter in Shadow-Datei

#### /etc/passwd und /etc/shadow
**Passwd:** user:password (x =>/etc/shadow) user-id:group-id:complete name:home directory: default shell
**shadow:** username:coded pass: last_change:min_days: max_days:warn_days: inactive_days: expire_date


## Verbesserung der Passwortsicherheit
- Verzögerungsstrategien
	- Erweiterung der Passwörter um eine n-Bit lange (UNIX n=12) Zufallszahl vor Verschlüsselung => Datei mit zu testenden Passwörtern wird um Faktor 2 n größer
	- Schwer zu erratende Passwörter, etwa mindestens 12 Zeichen, nicht im Wörterbuch, Klein/Großschreibung
		- Erzwingen einer regelmäßigen Änderung des Passwort
	- Einmal-Passwörter
	- Challenge-Response-Verfahren
		- Abfrage von vorher eingegebenen und verschlüsselt gespeicherten Fragen, z.B. Name der Klassenlehrerin in der achten Klasse?
		- Vereinbarter Algorithmus muss mit einem vom Server gesendeten Wert ausgeführt werden => Ergebnis = Passwort


## Authentifizierung durch Gegenstände
- Benutzer muss etwas haben: Schlüssel => Türschlösser
- Benutzer muss etwas haben und wissen: Karte + PIN
	- Magnetstreifenkarten: 140 Byte, oft ist das verschlüsselte Passwort dabei => riskant, da Lese/Schreibgeräte weit verbreitet
	- SmartCards: kleine CPU vorhanden
		- Aufbau einer Verbindung zum Server, der dann Daten sendet
		- SmartCard verarbeitet die Daten und sendet das Ergebnis als Passwort zurück => Berechnungsvorschrift geheim

## Biometrische Authentifizierung
Messung (Registrierung) und Vergleich (Identifizierung) von charakteristischen, schwer zu fälschenden Merkmalen des Benutzers
Speicherung der Merkmale auf zentralem Rechner/SmartCard

Entscheidend: Güte der Merkmale
- Weite Streuung (Haarfarbe kein gutes Merkmal)
- Keine gravierende Änderung im Laufe der Zeit
- Erkennung der Veränderungen (Sonnenbräune, Makeup, Erkältung, …)

Typische Merkmale
- Fingerlänge, Fingerabdrücke => Probleme bei Gipsvorlagen
- Scann der Netzhaut => Probleme beim Abfotografieren
- Automatische Unterschriftenanalyse => Probleme bekannt
- Stimmbiometrie => Täuschung durch Stimmaufnahmen


## Angriffe von innerhalb des Systems
- Eher sichere Systeme: lediglich der Benutzer, dessen Passwort geknackt wurde, erleidet Schaden
- Eher unsichere Systeme: der erste Zugang dient lediglich als Startpunkt, um wesentlich größeren Schaden einzurichten


## Login-Spoofing / Phishing
Schutzmöglichkeit: SCHLAU sein
- Technisch: Starten der Einloggprozedur mit einer Tastenkombination, die von einem Benutzerprogramm nicht abgefangen werden kann
-  Beispiel Windows: CTRL+ALT+DEL(Protected signal) loggt aktuellen Benutzer aus, startet Loginprozedur, Umgehung dieses Mechanismus ist nicht möglich


## Logische Bomben und versteckte Hintertüren
- Logische Bomben
	- Oft zur Erpressung des Arbeitsgebers eingesetzt
	- Solange das Passwort des Entwicklers täglich kommt oder sein Name auf der Gehaltsliste steht, passiert nichts
	- Bei Entlassung: Code wird aktiviert und richtet Schaden an
- Versteckte Hintertüren
- Veränderung der Einloggprozedur, so dass mit einem bestimmten Loginnamen das Einloggen jederzeit und auf allen vom Hersteller ausgelieferten Systemen möglich ist => Hintertür umgeht den gesamten Authentifizierungsprozess

Vermeidung von logischen Bomben/versteckten Hintertüren durch Einführung eines konsequenten, ausgiebigen Code Reviews möglich


## Buffer Overflows
Umlenkung der Rücksprungadresse auf ein eingeschleustes Programm
- Bei Funktionsaufrufen: Rücksprungadresse und Parameter auf den Stapel
- Für bestimmte Parameter – z.B. einen Dateinamen – wird Speicherplatz gemäß der Definition des Strings reserviert
- Wird diese Grenze durch einen extralangen Namen überschritten, so wird die Rücksprungadresse überschrieben und somit abgeändert
	-  Sprung in fremden/gesperrten Adressraum => Absturz
	- Pufferinhalt = Programm, das ausgeführt wird => Großer Schaden

![[Pasted image 20250717170626.png#invert]]


- Wo kann dieser Fehler auftreten?
	- Überall, wo Felder fester Größe für Benutzereingaben benutzt werden (Dateinamen, Umgebungsvariablen, Datenabfragen, …)
- Wie findet man solche Sicherheitslücken?
	- Extremeingaben bei allen Aufforderungen: Stürzt das Programm ab, so wahrscheinlich wegen eines Pufferüberlaufs
	- Analyse des Fehlerreports (z.B. core Datei) kann u.U. Rückschlüsse auf den Stackaufbau zu dem Zeitpunkt erlauben
	- Noch einfacher geht es bei offenen Quellen (in beiden Richtungen, sowohl Ausnutzen als auch Verhindern)
- Verhinderung
	- Nutzung von Routinen, die Stringlänge limitieren (fgets statt gets, strncpy statt strcpy)
	- Entwicklung mit Memory-safe Sprachen: Rust (aber auch Go, Java, C#, Swift, Python)


## Angriffe von außerhalb des Systems
Vernetzung => vielfältige Angriffsmöglichkeiten von außerhalb
![[Pasted image 20250717172415.png]]


## Schutzmechanismen
 Grundlage für die Sicherheit ist das Domänenkonzept (domain)
- Domäne = Menge von Paaren (Objekt, Rechte)
	=> Jedes Paar spezifiziert ein eindeutig identifiziertes Objekt (HW, Prozesse, Dateien, Semaphore, …) und Erlaubnis (Rechte) für darauf ausführbare Operationen
- Domäne kann einem Benutzer entsprechen, aber auch allgemeiner Natur sein
	=> Ein Prozess läuft zu jedem Zeitpunkt in einer einzigen Schutzdomäne
	=> Domänenwechsel möglich, Regel hochgradig vom System abhängig

Domänenkonzept bei UNIX
- Domäne eines Prozesses wird durch BenutzerID (UID) und GruppenID (GID) festgelegt
	=> für jede Kombination (UID, GID) lässt sich eine Liste mit allen benutzbaren Objekten (HW, SW, …) erstellen
- Prozessaufteilung in Benutzer- und Kernteil, die in unterschiedlichen Domänen laufen
	=> Systemaufruf wie z.B. sudo (superuser do) zur temporären Anhebung der Privilegien einer Nutzerin

## Realisierung des Domänenkonzepts
Matrix
![[Pasted image 20250717172803.png#invert]]
 ineffizient, da zu viele leere Felder vorhanden

#### Zugriffskontrolllisten (Access Control Lists, ACL)
Eine Liste pro Objekt spezifiziert, welcher Prozess, Benutzer, … (allgemein: Subjekt) welche Operation (für dieses Objekt) ausführen darf
- Unix: Unterscheidung zwischen Rechten für den Eigentümer der Datei, die Mitglieder einer Gruppe und die restlichen Benutzer
- Windows: Beliebig viele Einträge => genauere, fein spezifizierte Kontrolle
![[Pasted image 20250717174019.png#invert|500]]


#### Capabilities
- Jedem Subjekt wird seine eigene Domäne angehängt
- Verwaltung in einer Liste, wobei jeder Eintrag folgendes angibt
	- Referenz auf das Objekt
	- Menge der zugelassenen Operationen
- Liste wird z.B. durch Verschlüsselung geschützt

- Tagged Architecture: Spezielles Bit zeigt an, ob das Speicherwort eine Capability enthält  => Änderung nur im Kernmodus möglich
- C-Listen werden innerhalb des BS-Kerns gehalten und über ihre Position referenziert
- Verschlüsselte C-Listen im Benutzermodus => Manipulation durch Benutzer schwer

![[Pasted image 20250717174053.png#invert|500]]


## Absicherung eines Betriebssystems nach NIST
Basisschritte nach NIST (National Institute of Standards and Technology), um die die Sicherheitsanforderungen für BS adäquat zu erfüllen

- Installieren, konfigurieren, patchen des BS
- Konfigurieren von Benutzern, Gruppen und Berechtigungen
- Konfigurieren von Ressourcenkontrollen
- Installieren und Konfigurieren zusätzlicher Sicherheitskontrollen, wie z. B. Virenschutz, Honeypots, Host-basierte Firewalls und Intrusion Detection Systeme
- Entfernen von unnötigen Diensten, Anwendungen und Protokollen
- Testen


## Sicherheitsanforderungen
Trusted Computer System Evaluation Criteria (TCSEC): Orange Book (1983)
D – minimaler Schutz (niedrigste Klasse)
C1 – Sicherheitsschutz nach Ermessen (Aktuelle Einstufung von Linux)
C2 – Kontrollierter Zugriffsschutz (Aktuelle Einstufung von Windows)
B1 – Sicherheitsschutz mit Etiketten
B2 – Strukturierter Schutz
B3 – Sicherheitsdomänen
A1 – verifizierter Entwurf (die höchste Klasse)

#### C2-Standard Umsetzung
- Sicheres Anmelden mit Antispoofing-Maßnahmen
- Frei einstellbare Zugriffskontrollen
- Privilegierte Zugriffskontrollen
- Schutz des Adressraumes für jede einzelne Person
- Stackseiten müssen vor Einlagerung mit Nullwerten belegt werden(Keine Information über vorherigen Besitzer)
- Security auditing

## Vertrauenswürdige Systeme
Gründe für Existenz nicht-sicherer Systeme
- Bequemlichkeit der Benutzer: Bereitschaft auf einzelne potentiell gefährliche Features zu verzichten, ist gering
- Komplexität der Systeme

Trusted Computing Base
- Minimale HW und SW, die einer Sicherheitsspezifikation folgt, diese vollständig umsetzt und nicht kompromittiert werden kann
- Bestandteile: HW, Betriebssystemkern, alle Programme mit Superuserrechten


## Der Kern eines Betriebssystems
![[Pasted image 20250717175453.png#invert]]



## Modularisierung und Hierarchien
Zerlegung in Modulen (hierarchische Anwendung) derart, dass
- Hohe Interaktion innerhalb des Moduls
- Geringe Interaktion zwischen den Modulen
- Einfache Schnittstellen zwischen den Modulen
- Module sind aufgrund ihrer begrenzten Größe und Komplexität leicht verständlich
 Hierarchische Ansätze wie etwa Bäume helfen zur Erhöhung der Skalierbarkeit und Reduktion der Komplexität


## Zerlegung der Funktionalität in Schichten
- Einfache, universeller nutzbare Funktionen weiter unten, komplexere und spezifischere Funktionen weiter oben
- Jede Schicht repräsentiert eine Abstraktion niedrigerer Schichten
- Jede Schicht mit einer Schnittstelle für die höheren Schichten
- Anwendungsneutral, Unterstützung für spezifische Anforderungen möglichst in der obersten Schicht
![[Pasted image 20250717175909.png#invert]]


## Sanduhr-Architektur für Betriebssysteme
![[Pasted image 20250717175953.png#invert]]


## Orthogonalität und SPOT
Orthogonalität
- Funktionen und Konzepte eines BS sollten unabhängig voneinander sein.

Regel Single Point of Truth (SPOT)
- Keine Kopien oder Wiederholungen
- Für Code: Jede Funktionalität wird genau einmal implementiert.
- Für Daten: Jede Information, die vom System verwaltet wird, hat genau eine Repräsentation.
- Die Verwendung von SPOT vermeidet Inkonsistenzen.


## Strukturen der Betriebssysteme
- Monolithisches System
- (historisch: Geschichtetes System)
- Hypervisor mit Virtuellen Maschinen
- Mikrokern
- Exokern


#### Monolithische Systeme
- Gesamte Funktionalität in einem großen Programm vereint: Unterbrechungsbehandlung, Systemaufrufbehandlung, E/A-Treiber, Scheduler, Dateisysteme, Netzwerkprotokolle, ...
- Vorteil: einfach zu konstruieren
- Nachteile:
	- Menge an Quellcode sehr groß
	- Keine Trennung zwischen Komponenten
		- Anfällig für Sicherheitsprobleme, Abstürze
		- Der gesamte Kern muss vertrauenswürdig sein
		- Problem mit Treibern von Drittherstellern

#### Monolithische Systeme mit Modulen
Linux enthält Treiber für Tausende von Geräten, ein PC enthält eher nur ein paar Dutzend Geräte → Kern unnötig groß

Idee:
- Treiber nicht in Linux-Kern integrieren, sondern separat kompilieren & bereithalten
- Treiber werden bei Bedarf von BS in Speicher geladen, wenn BS ein entsprechendes Gerät vorfindet
- Übertragbar auf Dateisysteme, Netzwerkprotokolle, …


## Geschichtete Systeme
![[Pasted image 20250717181720.png#invert]]
Abhängigkeit nur von höherer zu niederer Schicht
Monolithisches Design, aber mit interner Struktur


## Mikrokern
Idee eines minimalen Kerns durch Auslagerung von BS-Funktionen als normale Prozesse im Benutzerraum
- Server-Dienste: Dateiserver, Grafikanzeigeserver, Druckserver, …
- Paradigma Mikrokern: nur Funktionalität, die Systemmodus unbedingt benötigt, verbleibt im Kern
![[Pasted image 20250717205319.png#invert]]

- Schlanker und effizienter Kern
- Fehlerbegrenzung: wenn ein Dienst abstürzt, kann er neugestartet werden und hat kaum Einfluss auf den Rest des Systems
- Sicherheit und Stabilität: Kern wird nicht durch fehlerhafte Dienste beeinträchtigt
- Sicherheitskritische Teil des Systems kleiner und leichter zu verifizieren
- Flexibilität und Erweiterbarkeit: Dienste können auch im laufenden Betrieb hinzugefügt und entfernt werden
- Nachteile
	- Komplexe Implementierung
	- Langsamere Systemaufrufe, da Prozessumschaltung


## Hypervisor und Virtuelle Maschinen
IBM: CP/CMS, der erste Hypervisor

- Komponenten
1. Control Program (CP): Ausführung auf der realen Hardware, bildet für darauf laufende Software die echte Systemhardware nach
2. Cambridge Monitor System (CMS): häufig genutztes (Single-User-) Betriebssystem in den von CP bereitgestellten virtuellen Maschinen

Systemaufrufe von Anwendungen landen in CMS, Zugriffe auf E/A-Geräte durch CMS von CP überwacht
![[Pasted image 20250717210210.png#invert]]


## Fallstudie: Windows
Ursprünglich auf Betrieb von mehreren Teilsystemen ausgelegt (z.B. Unix, OS/2, Windows)
Betriebssystemkern oft als Mikrokern bezeichnet, enthält allerdings Systemkomponenten, die nach Mikrokerndefinition nicht notwendigerweise integriert werden müssen

Aufteilung der Systemkomponenten in Schichten
- Hardwareabstraktionsschicht (HAL)
- Kern mit zentralen Aufgaben
- Executive
- Gerätetreiber

![[Pasted image 20250717210336.png#invert]]


## Fallstudie: Unix
Unix: Schichten-basiertes System mit monolithischem Kern
Kernmodus
- Alle Befehle mit Zugriff auf Hardware
- Kritische Dienste wie Scheduler, Module-Loader, Prozessmanagement, Semaphore, Tabelle mit Systemaufrufen, …

![[Pasted image 20250717210550.png#invert]]


## Fallstudie: Android
- Entstanden auf Basis des Linux-Kernel 2.6
- Freie und quelloffene Software
- SDK verfügbar zur Entwicklung von Anwendungen für Android-Plattformen in Java

Linux-Kernel: Grundlage der Android-Plattform
- Verwaltet alle zur Laufzeit benötigten Treiber z.B. für Anzeige, Kamera, Bluetooth, Audio, Speicher
- Abstraktionsschicht zwischen Gerätehardware und anderen Android-Komponenten
![[Pasted image 20250717210801.png#invert]]


- Hardware Abstraction Layer (HAL): Bibliotheken mit Schnittstellen für HW-komponenten, z. B. Kamera
- Android Runtime (ART): stützt sich auf den Linux- Kernel für zugrunde liegende Funktionen wie Threading und Low-Level-Arbeitsspeicherverwaltung
- Native C/C++-Bibliotheken
- Java API-Framework: Zugang zum Funktionsumfang des BS, z.B. GUI, Ressourcenmanager, Benachrichtigungen, Aktivitätsmanager für Lebenszyklus von Apps, Inhaltsanbieter (Zugriff auf Daten anderer Apps, z.B. Kontakte)
- System-Apps: Kern-Apps für E-Mail, SMS, Kalender, Internet, Kontakte, …