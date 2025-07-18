
Räumliche und zeitliche Zuordnung von Aktivitäten zu Instanzen, welche diese
Aktivitäten durchführen können

Scheduling historisch in drei Bereiche eingeteilt:
- Long-Term Scheduling (LTS): behandelt die Erzeugung (ausreichend Ressourcen vorhanden?) und Zerstörung von Prozessen
- Medium-Term Scheduling (MTS): behandelt das Ein- und Auslagern von Prozessen
- Short-Term Scheduling (STS): behandelt das Zuweisen der CPU(s) zu bereiten Prozessen
- (und Dispatching: Durchführen der CPU-Zuweisung)

Der Begriff „Scheduling“ meint gewöhnlich „CPU Scheduling“, also STS

![[Pasted image 20250504124710.png#invert]]

- Statisch: Keine weiteren Prozesse kommen hinzu
- Dynamisch: neue Prozesse kommen während der Ausführung hinzu
- Off-line (irgendwann vorher): Alle Prozesse – auch zukünftige Ankünfte – sind bekannt -> vollständige Information liegt vor
- On-line (zur Laufzeit): Lediglich aktuelle Prozesse bekannt -> Entscheidungsfindung auf Grund unvollständiger Information

Zu erreichendes Ziel, d.h. zu optimierende Zielfunktion, wie z.B.
- Länge des Ablaufplans (min)
- Maximale Antwortzeit (min)
- Mittlere (gewichtete) Antwortzeit (min)
- Anzahl Prozessoren (min)
- Durchsatz (max)
- Prozessorauslastung (max)
- Maximale Verspätung (min)

**Zeitscheibenbetrieb:** Jeder Prozess erhält im festen Takt ein Zeitfenster zugeteilt

### Round Robin: 
Wahl der Zeitscheibenlänge t ist Optimierungsproblem
- Für großes t nähert sich RR der Reihenfolgestrategie FCFS
- Für kleines t schlägt der Aufwand für das häufige Umschalten negativ zu Buche

![[Pasted image 20250504125603.png#invert]]



**Prio-NP:** Neuankömmlinge werden nach ihrer Priorität in die Bereitliste eingeordnet

**Prio-P:** Wie PRIO-NP, jedoch findet Verdrängungsprüfung statt

**SJN(Shortest Job Next):** Prozess mit kürzester Bedienzeit wird als nächster bis zum Ende oder zur freiwilligen Aufgabe bearbeitet
**SRTN(Shortest Remaining Time Next)**

**HRRN(Highest Response Ratio Next):** $\text{rr} := \huge{\frac{\text{Wartezeit + Bedienzeit}}{\text{Bedienzeit}}}$
### Prioritätsinvertierung
![[Pasted image 20250504130248.png#invert]]
**Lösung: „Prioritätsvererbung“ (priority inheritance):** A bekommt Priorität von C, solange A das Betriebsmittel besitzt, auf das C wartet


### Multilevel-Scheduling
Kombination unterschiedlicher Scheduling-Verfahren

- Unterteilung der Menge bereiter Prozesse in mehrere Listen
- Prozesse werden – je nach gewünschter Betriebsform – bei ihrer Erzeugung einer Liste zugeordnet
- Jede Liste kann mit einer eigenen Strategie verwaltet werden
- Listen nach Priorität geordnet

### Multilevel-Feedback-Scheduling
Optimistische Annahme: Prozesse sind kurz, also schnell fertig (-> SJN), beginnen also mit hoher Priorität
- Lange (CPU-intensive) Prozesse -> niedrigere Priorität!
	- behindern sonst Interaktivität des Systems
- Kurze (E/A-intensive; „schubweise“) Prozesse -> höhere Priorität!
	- das sind gerade die interaktiven Prozesse (auf Tastendruck warten; kurz rechnen, z.B. Buchstabe an Textdokument anhängen; dann wieder warten)
- Optional: „Warte“-Historie eines bereiten Prozesses (→ HRRN)
	- Prozesse in den untersten Ebenen kommen u.U. nie dran, wenn immer höher priorisierte bereit sind (starvation)
	- Abhilfe: Aging (Erhöhung der Priorität bei langer Wartezeit)

### Zeitpunkte
![[Pasted image 20250504131406.png#invert]]

Überschreitung der Sollzeit ($\huge{e_i \gt d_i}$): Verspätung (lateness) $\huge{L_i=e_i–d_i}$

![[Pasted image 20250504131633.png#invert]]
##### Minimierung der maximalen Verspätung
- Bei 1-CPU-Systemen ohne Verdrängung und ohne Abhängigkeiten zwischen Prozessen
	- Scheduling durch Permutation der Prozesse


- Theorem EDD (Earliest Due Date), Jacksons Regel
	- Relaxation: Alle Prozesse können zu jedem Zeitpunkt beginnen
	- Jeder Ablaufplan, in dem die Prozesse nach nicht fallenden Sollzeitpunkten geordnet ausgeführt werden, ist optimal bzgl. Lmax
	- Lediglich Sortiervorgang mit O(n log n) notwendig
- Theorem EDF (Earliest Deadline First)
	- Jeder Ablaufplan, in dem zu jedem Zeitpunkt der Prozess mit dem frühesten Sollzeitpunkt unter allen ablaufbereiten Prozessen zugeordnet wird, ist optimal bezüglich der maximalen Verspätung
![[Pasted image 20250504132057.png#invert]]

### Periodische Prozesse
Jeder Prozess ist gekennzeichnet durch Periode bzw. die dazu reziproke Rate

Ist die Schedulingaufgabe lösbar (schedulability test, feasibility test)?
- Für jeden periodischen Prozess muss gelten: $\huge{0 < b_i \leq d_i \leq p_i}$
- Bei mehreren periodischen Prozessen muss gelten: $\huge{\sum_{i} \frac{b_{i}}{p_{i}} \leq 1}$
$\huge{p_{i}}$ = Periode

##### Ratenmonotones Verfahren
Statische Priorität für jeden Prozess, die umgekehrt proportional ist zu der Periode, höhere Priorität verdrängt niedrige Priorität
**Voraussetzung:** Prozesse sind unabhängig und Sollzeitpunkte fallen mit den Perioden zusammen

**Satz:** Menge von n periodischen Prozessen kann durch ein ratenmonotones Verfahren eingeplant werden, wenn folgendes gilt (hinreichende Bedingung): $\huge{\sum_{i} \frac{b_{i}}{p_{i}} \leq n\left( 2^{\frac{1}{n}} -1 \right)}$

![[Pasted image 20250504133019.png#invert]]

### Unix-Scheduling
UNIX/Linux-Scheduling basiert auf Multilevel-Feedback-Scheduling

Scheduling-Verfahren (policies)
- SCHED_OTHER: basiert auf PRIO-P, für „normale“ Prozesse
	- Statische Priorität = 20, dynamische Priorität (nice): \[-20 19]
	- Verdrängung durch Prozesse mit höherer statischer Priorität, nicht verbrauchte Zeitscheibe bleibt als „Guthaben“ erhalten
- SCHED_FIFO: FCFS, für „Echtzeitprozesse“ (Real-Time FIFO)
	- Statische Priorität: 1-99, bei gleicher Priorität Wahl des ersten Prozesses, Verdrängung durch Prozesse mit höherer statischer Priorität
- SCHED_RR: Round-Robin, für „Echtzeitprozesse“ (Real-Time RR)
	- Statische Priorität: 1-99, bei gleicher statischer Priorität -> RR
	- Nach Ablauf der Zeitscheibe -> Einordnung ans Ende der runqueue
	- Verdrängung durch Prozesse mit höherer statischer Priorität

### Linux-Scheduling
1. Entferne alle Prozesse aus der Wartschlange runqueue, deren Zustand nicht TASK_RUNNING ist.
2. Bewerte jeden lauffähigen Prozess aus der runqueue und wähle den Prozess mit der höchsten Bewertung
3. Wenn alle Zeitkonten (Quantum) der lauffähigen Prozesse (blockierte werden nicht berücksichtigt) abgelaufen sind, dann berechne die Zeitkonten aller Prozesse neu
	- Quantum: Bestimmte Anzahl sog. Uhrticks üblicherweise 20, ein Uhrtick etwa 10ms beträgt
	- Bestimme den auszuführenden Prozess und rufe Dispatcher auf
4. CPU wird entzogen, wenn
	- Quantum vollständig verbraucht (Quantum=0)
	- Thread blockiert wegen E/A
	- Thread höherer Priorität ist ablaufbereit

##### Prozessbewertung
Bewertung eines Prozesses durch die Funktion goodness(...)
- Rückgabe –1: Freiwillige Abgabe der CPU
- Rückgabe 0: Quantum aufgebraucht
- Rückgabe \[1-1000]: Normaler Prozess
- Rückgabe > 1000: Echtzeitprozess
- Je größer der Rückgabewert, desto besser ist die Bewertung

Berechnung der Funktion goodness()
- Bei normalen Prozessen
- Allgemein: Güte = quantum + priorität
	- Gleicher Prozess hat noch Zeit übrig: Güte = quantum + priorität + 1
	- Gleicher Adressraum wie aktueller Prozess: Güte = quantum + priorität + 1
- Bei Echtzeit-Prozessen: Güte = 1000 + Priorität

Neuberechnung: Quantum_neu = Quantum_alt/2+Basispriorität

### Windows-Scheduling
Kombination aus Prozess- und Threadpriorität ergibt 32 Werte \[0, 31]
- Systemprioritäten 16..31 Þ Zuweisung durch Administrator
- Benutzerprioritäten 0..15 (Änderung mit API SetThreadPriority)

- Verwaltung der Prioritätsliste mit 32 Prioritäten
	- Jeder Eintrag enthält Liste mit allen wartenden Threads der gleichen Priorität
	- Durchlaufen der Liste von Priorität 31 bis Priorität 0
	- Bei nichtleerem Eintrag -> führe die Prozesse nach Round-Robin aus
- Spezielle Threads
	- Null Thread: läuft im Hintergrund, überschreibt Speicherseiten mit Nullen
	-Zeitscheibenlängen

- Standardeinstellung: 20ms, Einzelprozessor-Server: 120ms
- Multiprozessor-Systeme: abhängig von Taktfrequenz
- Einstellungen können um Faktor 2, 4 oder 6 erhöht werden
	- Kürzere Zeitscheiben bevorzugen interaktive Benutzer
	- Längere Zeitscheiben erfordern weniger Kontextwechsel und sind effizienter
- Verringerung der Priorität
	- Verbraucht ein Thread seine gesamte nächste Zeitscheibe, so wird seine Priorität um eine Einheit verringert, solange die aktuelle Priorität höher ist als seine Basispriorität
 Idle Thread: Läuft, wenn keine anderen Threads inkl. Null Thread aktiv
