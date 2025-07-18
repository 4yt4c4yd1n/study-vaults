Begriffsdefinitionen
- Datei (file): Sammlung von Daten auf stabilem Speicher unter einem gewählten Namen
- Verzeichnis, Ordner (directory, folder): Enthält eine Sammlung von Dateien und anderen Verzeichnissen -> Baumstruktur
- Dateisystem (file system): enthält einen Verzeichnisbaum; DS-Typ definiert allgemeine Charakteristika (max. Dateinamenlänge, max. Dateigröße, Abstraktions-Algorithmus)
- Partition: Bereich (zusammenhängende Menge von Blöcken) einer Festplatte, der ein Dateisystem enthalten kann
- Festplatte: persistentes Speichermedium, heutzutage meist flash-basiert (SSD) oder magnetspeicher-basiert (HDD), üblicherweise in Partitionen unterteilt

## Dateiattribute
- Name: Symbolischer Bezeichner, aussagekräftig für Benutzer und üblicherweise zweigeteilt
	1. Benutzervergebener Name der Datei
	2. Dateierweiterung: Hinweis zu Typ und Format des Inhalts
- Bezeichner: Eindeutige – üblicherweise numerische – Kennung der Datei im Dateisystem
- Typ: UNIX unterscheidet z.B. reguläre Dateien, Verweise (Links), spezielle Gerätedateien, Pipes, Sockets…
- Position: Speicherort(e) der Datei (Blockadresse(n) in der Partition)
- Größe
- Benutzer- und Zugriffsinformationen: Wer darf lesen / schreiben / ausführen, …
- Zeitpunkte: Erstellung, letzter Zugriff, letzte Modifikation, …

## Ausführbare Programme
exec-Systemaufruf: mevcut prosesi dosyadaki programla degistir
OS program dosyasini okumali ve analiz etmeli

#### Möglichkeit 1: Stapelverarbeitungsdatei
Erste Zeile Shebang

#### Möglichkeit 2: ausführbare Programme
![[Pasted image 20250627151026.png|inlR]]Kompiliertes Programm
- Erse 4 bytes -> spezielle Bytefolge(magic)
- Headers -> Inhaltsverzeichnis
	- Beginn & Größe des Programmcodes & der Daten usw.
	- Entry point
- Code(Text)
- Data
- Symbol table(nützlich zum Debuggen)
- Liste von Funktionen von anderen Bibliotheken

## Verzeichnisbaum
![[Pasted image 20250627151943.png#invert]]
Hierarchische Organization von Verzeichnissen
- Sicherheit -> Zugriffsrechte & Benutzeridentifikation
- jeder Benutzer hat alle Namen zur verfügung

Pfade
- Dateispez. über Pfadangabe und Dateiname

## Mounting
Alle Partitionen werden in eine einzige Verzeichnisstruktur eingebunden
![[Pasted image 20250627152339.png#invert]]

## UNIX-Verweise(Links)
- Hard Link: kein Unterschied zwischen "Quelle" und "Ziel" des links
	- Beim Löschen einer der beiden ist die Datei am anderen Ort noch da
	- Hard Links nur innerhalb eines Dateisystems möglich

- Soft Link: symbolischer Bezichner(also den Dateinamen) des Ziels
	- Weiterleitung
	- Wenn Originaldatei gelöscht wird, ist der Link "dangling"
	- Über den gesamten Verzeichnisbaum möglich


## Partitionierung einer Festplate(MBR-Schema)
![[Pasted image 20250627152838.png#invert]]
- Sektor 0 der Festplatte: MBR(Master Boot Record)
	- 446 Bytes Boot-Code
	- 4\*16 bytes Partitionstabelle
	- 2 bytes magic
- Jede Partition enthält ein Dateisystem
	- Bootblock: Code zum Laden des OS
	- Superblock: Schlüsselparameter des DS(magic zur Typ-Identifikation, Anzahl Blöcke, administrative Informationen)
	- Liste der freien Blöcke
	- Verwaltungsstrukturen
	- Eigentliche Daten


## Datenblöcke
![[Pasted image 20250627153256.png#invert]]
- Vorteile
	- Lesen einer Datei mit einer kontinuierlichen Operation
	- Direkter Zugriff auf einzelne Blöcke möglich
	- Einfachste Verwaltung, da lediglich die Länge und die Indexnummer des ersten Datenblocks bekannt sein müssen
- Nachteile
	- Fragmentierung des Datenträgers durch Löschen/Anlegen von Dateien unterschiedlicher Länge
	- Probleme, wenn mehrere Dateien zum Schreiben offen gehalten werden müssen
	- Nur in speziellen Fallen brauchbar
- Weiterentdeckung duch Beschreiben von CD-ROMs/DVDs/Blu-Rays
![[Pasted image 20250627154324.png#invert]]


## Belegung durch verkettete Listen: Interne Verkettung
![[Pasted image 20250627153739.png#invert]]
- Dateien können unter Nutzung geeigneter Datenstrukturen beliebig auf die Datenblöcke verteilt werden
	- Interne Verkettung
	- Externe Verkettung
	- Indexblöcke
- Interne Verkettung ohne Verwaltungsdatenblöcke
	- Startblock ist direkt im Dateiverzeichnis abgelegt
	- Jeder Block verweist auf seinen Nachfolger
	- Ausschließlich sequentielles Lesen möglich
- Vorteile
	- Keine externe Fragmentierung, Größenvorgabe nicht nötig
	- Einfaches Anlegen von Dateien
- Nachteile
	- Teil des Blocks reserviert für Zeiger -> interne Fragmentierung
	- Ineffizienter Zugriff: Auswertung vorgehender Blöcke beim Zugriff notwendig
	- Dateiverlust durch Zerstörung eines Blocks.
- Abhile: Gruppierung mehrere Blöcke in Cluster
![[Pasted image 20250627154259.png#invert]]


## Externe Verkettung
MS-DOS: FAT(File Allocation Table)
![[Pasted image 20250627161321.png#invert]]
- Je eine Zeile pro Block des Dateisystems mit Verweis auf Adresse des nächsten Blocks
- FAT wird an fest reservierter Stelle einer Partition gehalten und laufend aktualisiert
- Startblock einer Datei wird im Verzeichniseintrag abgelegt
- Dateiende durch NULL-Zeiger

- Nachteile
	- Ineffizienter Zugriff, da bei jedem Zugriff ein Lesevorgang in der FAT notwendig ist -> Sprung an Partitionsanfang
	- Bei Fehler im FAT droht massiver Datenverlust; daher redundant vorhanden


## Indexblöcke
Zentrale Tabelle wie FAT  ist Single Point of Failure und skaliert nicht mit Größe der Festplatte

Idee: Tabelle dezentralisieren -> einzelne Tabelle(Index) für jede Datei
Verzeichniseintreag verweist nicht auf ersten Datenblock, sondern auf einen sogenannten Indexblock
- Indexblock: enthält Blockliste für diese Datei(als Array)
- Zerstörung des Indexblocks -> Verlust nur dieser einen Datei

Problem: Indexblock hat nur Platz für begrenzte Zahl Einträge
Idee: Indizes im Haupt-Indexblock verweisen auf weitere Indexblöcke anstatt auf Datenblöcke![[Pasted image 20250627162419.png#invert]]
![[Pasted image 20250627162727.png#invert]]

Zugriffsschema bei Indexblöcken
- Lese den Inexblock
- Lese die i-te Zeile für den Zugriff auf den i-ten Block aus
- Folge dem Zeiger zur Position des Blocks

Belegung eines ganzen Blocks nur für Indexblock: ungünstig
- übliche Blockgröße: 4kB
- Platz für 1024 32-bit-Indizes
- Für kleine Dateien: massive Platzverschwendung
	-> Nutzung des Indexblocks auch für Metadaten

## Inodes(bsp. ext2/ext3)
![[Pasted image 20250627163246.png#invert]]

ext2/ext3: Kombiniertes Schema
- Direct Blocks: Datenblock-Indizes
- Single Indirect: Verweis auf Block, der weitere Datenblock-Indizes enthält
- Double Indirect: 2 Zwischenstufen
- Triple Indirect: ...

## Inodes ext4 Extents
Extents: Einsparung von Metadata, indem viele zusammenhängende Blöcke kompakt adressiert werden
- Extent: Länge des Blocks + Zugeordnete Start-Dateisystemblocknummer(DS)

Inode-Struktur: Platz für 4 Extents(statt 15 Block Indizes)
![[Pasted image 20250627163750.png#invert]]

Auch möglich: Verweis auf einen separaten Block mit weiteren Extents-Eintragen, die den verwalteten Bereich weiter unterteilen
Inodes selbst sind namenlos

## Realisierung von Verzeichnissen
Verzeichnisse müssen mindestens Namen und Referenz auf jedes enthaltene Objekt enthalten
- Einfache Organisation: Alle Metadata werden direkt im Verzeichniseintrag gespeichert
- Inode -> Dateinamen & Nummer der jeweiligen Inode aufnehmen


## Verwaltung des Plattenspeichers
Wie groß soll ein Block sein?

- Spuren, Sektoren, Zylinder -> geräteabhängig, ungeeignet

![[Pasted image 20250627164540.png#invert]]


## Disk Quotas
- Maximal erlaubte Anzahl von Dateien und Blöcken
- Unterscheidung zwischen Soft- und Hard-Limits

- Soft Limits: Überschreitung x-Mal möglich
- Hard Limits: keine Überschreitung erlaubt

Realisierung der Quotas mit zwei Tabellen
- Tabelle 1: Details der geöffneten Datei, Benutzer, Zeiger auf Quotatabelle
- Tabelle 2: Belegung gesamt, Angaben Soft/Hard Limits, Anzahl verbleibender Warnungen


## Inkonsistenzen und Log-Basierte Dateisysteme
Aktuelle Situation
- CPU-Geschwindigkeit steigt sehr schnell an
- Festplatten immer größer, Geschwindigkeitszuwachs kann mit dem CPU-Zuwachs nicht mithalten
	--> Extensive Nutzung von großen Caches
- Strategien bei Schreiben gecachter Daten auf Festplatte
	- Write through(Synchronous Write): Abschaltung des Cache -> Daten und Metadaten werden sofort geschrieben
- Careful Write: Verzögertes Schreiben
	- Alle Schreiboperationen erfolgen spätestens nach vorgegebener Maximalverzögerung
	- Inkonsistenzen beim Systemausfall möglich
- Lazy Write: Änderungen werden zunächst nur im Cache durchgeführt
	- Zyklische Analyse des Speichermediums -> nach Systemausfall gravierende Inkonsistenzen möglich


## Inkonsistenzen
Häufige und großflächige Nutzung von Caches erhöht die Gefahr durch Konsistenzprobleme

Bsp.: Löschen einer Datei
1. Dateieintrag aus dem Verzeichnis entfernen
2. Inode der Datei freigeben
3. Von der Inode belegte Datenblöcke freigeben(DS-Freiliste updaten)

- Systemcrash zwischen Schritt 1 und 2? -> Orphaned Inode
- Systemcrach zwischen Schritt 2 und 3? -> vermisste Blocks


## Konsistenz
Konsistenzüberprüfung
- Vergleich der Daten in Verzeichnissen mit den Datenblöcken auf der Festplatte und ggf. Behebung der Inkonsistenzen
- Kritisch bei Inodes, Verzeichnisblöcken, Listen freier Segmente
- Realisierung mit Hilfsprogrammen wie scandisk, fsck, ...

![[Pasted image 20250627170643.png#invert]]
Vorgehensweise bei Blöcken
- Zwei Tabellen mit Zählern, wie häufig ein Block vorkommt in
	1. Einer Datei
	2. Liste mit freien Segmenten
- Auslesen aller Inodes und Überprüfung aller Blöcke
- Bei Konsistenz ist jeder Block in genau einer Tabelle

- Vermisster Block: Speicherplatzverschwendung -> Freiliste aktualisieren
- Datenblock doppelt als frei ausgewiesen -> Freiliste aktualisieren
- Block kommt in zwei Dateien vor -> kritischer Fall
	- Wird eine Datei gelöscht, so wird der Block als frei gekennzeichnet
	- Block wird kopiert -> beide Dateien haben eigenen Block
	- Meldung an Benutzer, da eine Datei evtl. durcheinander gebracht wurde


## Log-basierte(Journaling) Dateisysteme
- DS enthält einen abgeteilten Bereich: das "Logbuch"
- Logbuch = Ringpuffer mit den nächsten geplanten Operationen
- Wenn eine Operation ausgeführt werden soll:
	- Beschreibung der Operation ins Logbuch schreiben
	- Eine Prüfsumme der Beschreibung ans Logbuch anhängen
	- Cache der Festplatte leeren
	- Operation durchführen
	- Operation im Logbuch als vollständig abgeshclossen markieren
- Beim Hochfahren des Systems(womöglich nach Crash): Replay aller nicht abgeschlossenen Operationen im Logbuch

#### Stufen von Journaling
- Vollständig: Daten und Metadaten werden über das Journal verarbeitet
	- Vorteil: volle Konsistenz-Garantie
	- Nachteil: alle Daten werden 2x ins Datensystem geschrieben
- Metadata-Only: Inhalt von Datenblöcken wird nicht ins Journal geschrieben
	- Vorteil: deutlich erhöhte Performance
	- Nachteil: möglicher Datenverlust, selbst wenn die Metadaten dank Journal konsistent bleiben
		- Beispiel: Anhängen von Text an eine Datei
		- Bei Absturz: neue Größe der Datei korrekt vom Journal replayed, aber neu angehängte Inhalte nicht im Journal
- Journal muss nicht zwangsläufig Teil des DS sein
	- Journal auf anderer Festplatte
	- Journal in Batterie-gestütztem RAM


## Backup
- Backup: Datensicherung auf redundante Speichermedien
- Restore: Wiederherstellung verlorener/veränderter Daten anhand der Kopien nach
	- Festplattencrash und anderen Katastrophen
	- Datenverlust nach Fehlbedienung
- Backuparten: Vollständige und inkrementielle Sicherung
	- Vollständig: Alle Daten werden auf Bakupmedien kopiert
	- Inkrementell: Sicherung aller Daten die sich seit der letzten Sicherung geändert haben
	- Üblich: Wöchentliche vollständige Sicherung und tägliches inkrementelles Backup
	- Sicherung eines Zyklus(~ 3-4 Wochen) auf unterschiedlichen Medien
	- Von Zeit zu Zeit -> Vollständige Sicherung, die nicht überschrieben wird
- Physische Datensicherung(Dump)
	- Beginnend mit Block 0 werden alle Blöcke der Festplatte auf ein Band geschrieben
	- Überspringen freier Blöcke -> Speicherung der Blocknummer notwendig
	- Zugriff auf Bitmap erforderlich, um defekte und daher ausgeblendete Blöcke zu erkennen
	- Vorteil: einfach und schnell
	- Nachteil: keine inkrementelle Sicherung möglich
- Logische Datensicherung
	- Beginnend bei einem oder mehreren vorgegebenen Verzeichnissen werden alle Unterverzeichnisse rekursiv durchlaufen und die darin enthaltenen Dateien gesichert
	- Auf dem Band werden sorgfältig identifizierte Dateien abgelegt -> Restoration gesicherter Dateien einfach
	- Strandardvorgehensweise bei den meisten Betriebssystemen

![[Pasted image 20250627173144.png#invert]]

![[Pasted image 20250627173213.png#invert]]
- Rekursives Durchlaufen aller Verzeichnisse. Aufbau einer Bitmap, die mit den Nummern der Inodes indiziert wird. Für jede modifizierte Datei wird der entsprechende Eintrag markiert. Alle Verzeichnisse werden markiert.
- Erneuter rekursiver Durchlauf un Aufhebung der Markierung aller Verzeichnisse, die keine modiefizierten Datei enthalten
- Nun ist es bekannt, welche Verzeichnisse und Dateien gesichert werden müssen
- Analog für Dateien: Erweiterung um Attribute un Sicherung
- 