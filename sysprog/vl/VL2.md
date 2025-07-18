## Betriebssysteme
Mittler zwischen HW und SW

#### BS Katalog
- Urbrechungsverarbeitung(Interrupt handling)
- Prozessumschaltung
- Betriebsmittelverwaltung(Resource management)
- Programmallokation
- Dateiverwaltung(File management)
- Auftragsteuerung(Scheduling)
- Zuverlässigkeit

## Mechanismen und Methoden(Policies)
**Mechanismus:** Wie wird eine Aufgabe prinzipiell gelöst?
**Policy:** Welche Vorgaben/Parameter werden im konkreten fall eingesetzt?

Trennung wichtig
**Wünschenswert:** Genereller Mechanismus, so dass eine Veränderung der Policy durch Anpassung der Parametern umgesetzt werden kann.


#### Benutzermodus vs. Systemmodus
user vs. superuser

**unprivilegiert -> previligiert**
- beim Auftreten einer Unterbrechung
- beim Auslösen eines Fehlers (Division durch Null, Zugriffsversuch auf ein "Loch" im Adressraum, verbotene Instruktion)
- durch explizite Instruktion (x86: sysenter, ARM:svc)
- Ausführung wird an vom BS definierten Einsprungpunkten fortgesetzt
- ursprünglicher Prozessorzustand(Register etc.) wird gesichert



## Kommunikation mit dem Betriebssystem
![[Pasted image 20250427114326.png#invert]]

#### Systemaufruf
Ablauf:
1. Anwendung bereiter Systemaufruf vor(Register mit Parametern belegen, architekturspezsifisch)
2. Anwendung führt spezielle Instruktion aus(svc/...) -> Trap
3. Ausführung springt zu BS-Behandlungsroutine(-> privilegierter Modus) für Systemaufrufe
4. BS analysiert Parameter, identifiziert gewünschte Funktionalität
5. BS prüft Berechtigung, Ressourcen, ... führt ggf. gewünschte Funktion durch
6. BS setzt Anwendung fort(Rückkehr in unprivilegierten Modus)


![[Pasted image 20250427114806.png#invert]]


#### Unterbrechungen
Interrupt: Signal informiert CPU über das Ende einer Aktivität
- Der Bus verfügt über (mindestens) eine Unterbrechungsleitung. Prüfung nach jedem CPU-Befehl, ob ein Signal anliegt. Falls ja:
	-  Sofortiger Sprung in eine Prozedur zur Auswertung der Unterbrechung
	- Abhängig von Auswertung werden die erforderlichen Aktionen durchgeführt / veranlasst

![[Pasted image 20250427115100.png#invert]]

#### Unterbrechungsanalyse
- wer (welches Gerät) die Unterbrechung verursacht hat (Quelle),
- warum die Unterbrechung ausgelöst wurde (z.B. Ende der Übertragung, Fehler)

![[Pasted image 20250427115210.png#invert]]


#### Sequentielle Unterbrechungsbehandlung
Yolda daha sorun varsa
![[Pasted image 20250427115330.png#invert]]


#### Geschachtelte Unterbrechungsbehandlung
Klassifikation von Unterbrechungen in Prioritätsklassen (statisch)

![[Pasted image 20250427115450.png#invert]]


## Prozesse
- dynamische Objekte, die Aktivitäten in einem System repräsentieren
-  funktionale und strukturierende Beschreibungseinheiten in System- und Anwendungssoftware

**Prozess = virtueller Rechner spezialisiert zur Ausführung eines bestimmten
Programms (Instanz eines Programms, laufendes Programm)**

![[Pasted image 20250427115644.png#invert]]
Ein oder mehrere Threads (Aktivitätsträger)


#### Adressräume
- Physischer Adressraum (einmal pro Maschine)
	- Hardwarekomponenten im Rechner und in Peripheriegeräten (RAM, Register, Controllerspeicher, …)

- Virtueller Adressraum (i.d.R. einmal pro Prozess)
	- vom Betriebssystem erzeugt und konfiguriert
	- Jeder Prozess hat einen eigenen virtuellen Adressraum, der i.d.R. viel größer ist als der tatsächlich vorhandene physische Adressreaum
	- enthält die für die Ausführung nötigen Instruktionen und Daten

#### Pysischer Adressraum
![[Pasted image 20250427115852.png#invert]]

#### Virtueller Adressraum
![[Pasted image 20250427115958.png#invert]]
- Programmtext: Instruktionen des Programms
- Statische Daten: globale Variablen, lokale Variablen mit static-Modifier
-  Dynamische Daten (Heap)
	- zur Laufzeit explizit reservierbar Speicherbereich
	- wächst und schrumpft nach Bedarf
- Dynamische Daten (Stack)
	- je aufgerufene Funktion: lokale Variablen, Aufrufparameter
	- wächst, je tiefer die Aufrufkette ist (Rekursion!)
	- im Gegensatz zum Heap Benutzung „automatisch“

![[Pasted image 20250427120353.png#invert]]

#### Ausführung von Prozessen
**Stapelbetrieb ->** Sequentiell
Problem: Während der Kommunikation mit z.B. E/A-Geräten bleibt die CPU ungenutzt -> Leerlaufzeiten und ineffiziente Ausführung, große Aufträge blockieren das gesamte System

**Multiprogrammierung**
Bei Warten auf Geräteantwort wird der nächste bereite Prozess gestartet/fortgesetzt (dispatcht)
Mehrere Prozesse werden verzahnt abgearbeitet (Nebenläufigkeit)\[Concurrency]

#### Modellierung der Multiprogrammierung
Annahmen:
- Ein Prozess verbringt einen Anteil p seiner Zeit mit Warten auf E/A-Operationen
- Wahrscheinlichkeit pn = n Prozesse warten gleichzeitig auf E/A-Ende
- Ausnutzung der CPU: A = 1 – pn
- n = Grad der Multiprogrammierung (Degree of Multiprogramming)

![[Pasted image 20250427120924.png#invert]]

#### Prozesszustände
![[Pasted image 20250427121048.png#invert]]

- Rechnend, Laufend (Running): Der Prozess ist im Besitz des physikalischen
Prozessors und wird aktuell ausgeführt
- Bereit (Ready): Der Prozess hat alle notwendigen Betriebsmittel und wartet auf die
Zuteilung des/eines Prozessors
- Blockiert, Wartend (Waiting): Der Prozess wartet auf die Erfüllung einer Bedingung,
z.B. Beendigung einer E/A-Operation und bewirbt sich derzeit nicht um den
Prozessor
- Beendet (Terminated): Der Prozess hat alle Berechnung beendet und die zugeteilten
Betriebsmittel freigegeben

**Erlaubte Übergänge:**
- Add: Ein neu erzeugter Prozess wird in die Klasse Bereit aufgenommen
- Assign: Infolge des Kontextwechsels wird der Prozessor zugeteilt
- Block: Aufruf einer blockierenden E/A-Operation oder Synchronisation bewirkt, dass der Prozessor entzogen wird
- Ready: Nach Beendigung der blockierenden Operation wartet der Prozess auf erneute Zuteilung des Prozessors
- Resign: Einem laufenden Prozess wird der Prozessor – aufgrund eines Timer-Interrupts, z.B. Zeitscheibe abgelaufen – entzogen, oder er gibt den Prozessor freiwillig ab
- Retire: Der laufende Prozess terminiert und gibt alle Ressourcen wieder frei

#### Erweitertes Zustandsmodell
Wegen Speichermangel werden oft ganze Prozesse (d.h. der Inhalt ihrer Adressräume) auf die Festplatte ausgelagert
Zusatzübergänge Swap in und Swap out

#### Nebenläufigkeit und Paralellität
- Nebenläufigkeit (Concurrency = Concurrent Execution)
	- Logisch simultane Verarbeitung von Operationsströmen
- Parallelität
	- Die Operationsströme werden tatsächlich simultan ausgeführt
	- Mehrfache Verarbeitungselemente, d.h. Prozessoren oder andere unabhängige
Architekturelemente, sind zwingend notwendig


Parallelität ist eine Teilmenge der Nebenläufigkeit

![[Pasted image 20250427121826.png#invert]]


#### Prozessverwaltung in Betriebssystemen
- Implementierung von Prozessen in BS durch Datenstruktur Prozesskontrollblock (Process Control Block, PCB)
-  PCB = verwaltungstechnischer Repräsentant des Prozesses
-  Elemente eines PCB (unter anderem):
	- Prozessidentifikation (häufig: PID, Prozessnummer)
	- Identifikation des Besitzers (z.B. Nutzerkennung)
	- Bereich zur Sicherung der aktuellen Registerwerte, wenn Prozess nicht im Zustand Laufend
	- Zustandsvariable (Prozesszustand): Bereit / Laufend / …
	- Informationen über zugeteilte Betriebsmittel
	- Konfiguration des virtuellen Adressraums
	- Verweise auf Eltern- bzw. Kindprozesse


#### Prozessumschaltung
1. Aktuell aktiver Prozess A wird aus Zustand Laufend in anderen Zustand versetzt (Grund z.B. Zeitscheibe verbraucht, Interrupt, blockierender Systemaufruf, …)
	1. Registerinhalte in PCB_A ablegen (inkl. Ort, wo A unterbrochen wurde = Befehlszähler zum Zeitpunkt der Unterbrechung)
	2. Prozesszustand aktualisieren (→ Blockiert/Bereit/…)

2. Ein bereiter Prozess B wird in den Zustand Laufend versetzt
	1. Prozesszustand aktualisieren (→ Laufend)
	2. Umschalten des virtuellen Adressraums gemäß Konfiguration in PCB_B
	3. Laden der Registerinhalte aus PCB_B
	 Fortsetzen von B an dessen gespeichertem Befehlszähler

Auswahlprozess beinhaltet die dynamische Auswertung von verschiedenen
Kriterien, z.B.
- Prozessnummer (zyklisches Umschalten)
- Ankunftsreihenfolge
- Fairness und Priorität (Konstant / Dynamisch)
- Einhaltung von geforderten Fertigstellungspunkten
![[Pasted image 20250427122516.png#invert]]
![[Pasted image 20250427122532.png#invert]]
![[Pasted image 20250427122544.png#invert]]
![[Pasted image 20250427122600.png#invert]]
![[Pasted image 20250427122617.png#invert]]



## Thread
- Gewöhnlich einem Prozess fest zugeordnet
- Entspricht einem Kontrollfluss (von ggf. mehreren) dieses Prozesses
- Operiert im selben virtuellen Adressraum und mit denselben Betriebsmitteln
(Ausnahme: Stack) wie alle anderen Threads des Prozesses
- Hat eigene Priorität, Zustand (bereit, laufend, blockiert, …), Befehlszähler,
Registerwerte, die aber nicht in PCB, sondern in einer Threadtabelle gespeichert
werden

![[Pasted image 20250427123034.png#invert]]

![[Pasted image 20250427123147.png#invert]]

Grundsätzlich werden Threads aufgeteilt in
- Kernel-Level-Threads (KL-Threads): realisiert im Kernadressraum
- User-Level-Threads (UL-Threads): realisiert im Benutzeradressraum

KL-Threads haben folgende Eigenschaften
- Werden im Betriebssystem realisiert
- Erzeugung, Umschaltung, Zerstörung erfordert Aufruf von Verwaltungsfunktionen des Betriebssystems → Systemaufruf
- Jeder KL-Thread vom BS mit eigenem Kontrollblock verwaltet (Threadtabelle im BS)
- KL-Threads werden vom BS einzeln dispatcht

UL-Threads haben folgende Eigenschaften
- Vollständige Realisierung im Adressraum der Anwendung
- (mindestens ein) KL-Thread als Träger: UL-Threads sind dem Betriebssystem völlig unbekannt
	- BS „sieht“ (und dispatcht) nur den KL-Thread
	- Zwischen den dem KL-Thread zugeordneten UL-Threads kann dann ohne Beteiligung des BS gewechselt werden
	- Speichern/Laden des Zustands wird von einer Threading-Hilfsbibliothek („Laufzeitumgebung“) durchgeführt
- Erzeugung, Umschaltung, Zerstörung von UL-Threads ähnelt einem
Prozeduraufruf → kein Wechsel der Privilegstufe: Operationen sehr schnell

Auch ein UL-Thread benötigt bestimmte Betriebsmittel (BM), z.B. eigenen Stack
Laufzeitumgebung kümmert sich um Allokation und fordert ggf. Ressourcen vom BS an

![[Pasted image 20250427123913.png#invert]]
KL-Thread als Träger für UL-Threads
- Vorteile:
	- Scheduling-Algorithmus beliebig
	- Sehr schnelle Verwaltung
- Nachteile:
	- Blockierender Systemaufruf blockiert alle UL-Threads
	- Nicht geeignet für Multicore (BS kann nureinen KL-Thread einer CPU zuordnen)

- Many2One
- One2One
- Many2Many


#### Fork/Join
**Fork:** Erzeugung einer perfekten Kopie (Child) des aufrufenden Prozesses (Parent) mit folgenden Eigenschaften
- Gleiches Programm
- Gleiche Daten (gleiche Werte in Variablen)
- Gleicher Programmzähler (nach der Kopie)
- Gleicher Eigentümer
- Gleiches aktuelles Verzeichnis
- Gleiche Dateien geöffnet (selbst Schreib-, Lesezeiger ist gemeinsam)
- Unterschiedliche Prozessnummer (PID)

**Join:** Zusammenführung der beiden Zweige mittels wait im Parent (im Pseudocode auch join)
![[Pasted image 20250427125933.png#invert]]


## Prozesshierarchien
- Eltern-Kind-Beziehung: Ein Prozess erzeugt einen weiteren Prozess
- Vorgänger-Nachfolger-Beziehung: Ein Prozess darf erst starten, wenn ein anderer Prozess beendet ist
- Kommunikationsbeziehung: Zwei (oder mehr) Prozesse kommunizieren miteinander
- Wartebeziehung: Ein Prozess wartet auf etwas, was von einem anderen Prozess kommt
- Dringlichkeitsbeziehung: Ein Prozess ist wichtiger (dringlicher) als ein anderer
+mehr

Viele mögliche Ereignisse zur Erzeugung von Prozessen:
- Initialisierung des Systems: Meistens Hintergrundprozesse (Daemons) wie Terminaldienst, Mailserver, Webserver, …
- Prozesserzeugung durch andere Prozesse: Aufteilung des Prozesses in mehrere, nebenläufige oder parallele Aktivitäten, die als eigene KL-Threads initialisiert werden
-  Benutzerbefehle zum Starten eines Prozesses (Kommandozeile oder GUI

Technischer Ablauf in allen Fällen gleich
- Bestimmter Prozess analysiert die Eingabe (z.B. von Benutzern oder Konfigurationsdateien)
- Prozess sendet einen Systemaufruf zur Prozesserzeugung und teilt dem BS mit, welches Programm darin ausgeführt werden soll


![[Pasted image 20250427130501.png#invert]]

![[Pasted image 20250427131455.png#invert]]

Drücken des Resetknopfs lädt den PC mit der Adresse eines kleinen Bootstrapprogramms (im ROM)
1. Bootstrapprogramm lädt den Bootblock (Plattenblock 0)
2. Bootblockprogramm lädt das Kernelbinärprogramm (z.B.: /boot/vmlinux)
3. Bootblockprogramm übergibt die Kontrolle an den Kernel

.
1. ![[Pasted image 20250427131646.png#invert]]
2. ![[Pasted image 20250427131730.png#invert]]
3. ![[Pasted image 20250427131808.png#invert]]
4. ![[Pasted image 20250427131902.png#invert]]


## Shell
Hilfsprogramm aus einzelnen Befehlen oder aus Shell-Programmen (genannt Script) zum Starten von Anwendungen

###### Basisfunktionen moderner Shells
- Erstellen und Starten von Scripts mit Befehlen
- Bedingungen (if, case) und Schleifen (while, for)
- interne Befehle (cd, read) und Variablen ($HOME)
- Manipulation der Umgebungsvariablen für die neuen Prozesse
- Ein-/Ausgabeumlenkung, Starten mehrerer Prozesse, Verkettung über Pipes
- Starten von Prozessen im Hintergrund, Stoppen und erneutes Starten von Prozessen (job control)
- Vervollständigung von Befehlen, Dateinamen und Variablen (completion)
- Wiederholung und Editieren früherer Befehle (command history)
- Testen von Dateieigenschaften (test)

