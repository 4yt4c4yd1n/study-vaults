Speicherallokation = Zuordnung von Speicher
Speicher benötigt für:
- Prozesse
	- Stack
	- Heap
	- Code und Daten
- BS
	- BS Code und Daten
	- Verwaltungsstrukturen(PCBs etc.)


## Statische und dynamische Speicherallokation

Statisch: Feste Einteilung in mehrere Bereiche
- Nehmen einen Prozess vollständig auf
- Prozess blockiert => Auslagerung auf den externen Speicher
- Blockiert -> Bereit => Wiederanlagerung
- Nachteil: Nur ganze Prozesse swapped

Dynamisch: Prozessteile werden zur Laufzeit und erst bei Bedarf ein- und asugelagert
- Grundlage: Virtuelle Adressierung -> swap ohne Aufwand in anderem Speicherbereich => Realokierbare Prozesse
- Zugriff auf fehlendem Inhalt -> Interrupt(Seitenfehler, Page Fault) -> Nachladen des Inhalts initiiert


## Datenstruktur zur Adressraumverwaltung

Für jede virtuelle Adresse V liefern:
- Gültigkeit: Zugriff auf V soll nicht Speicherfehler auslösen
- Wenn gültig:
	- Zugehörige physische Adresse P
	- Lesen, Schreiben, Ausführen von dieser Adresse erlaubt?
	- Zugriff für unprivilegierte Instruktionen erlaubt?(Kernel reserves at boot & dynamically)
		=> + Maschinenwort (P speichern plus ein paar Bits extra)

Wie granular wollen wir V zuordnen können?
- Extrem 1: Ultrafein, d.h. jedes Byte einzeln
- Extrem 2: ultragrob, z.B. 64MB

Trade-off zwischen Effizienz des Lookups, Größe der Verwaltungsstruktur und anderen Überlegungen
meiste Architekturen: 4kB

=> virtuelle Verwaltungseinheit ist eine page
=> physisches counterpart ist page frame(Kachel)
=> page table


## Seitenadressierung
Seitenadressierung(paging) -> virtuelle und physikalische Speicher in Stücke fester länge einteilen
![[Pasted image 20250717090030.png#invert]]

Neben der physikalischen Adresse enthält jeder Eintrag der Seitentabelle noch Informationen über
- Seite im Hauptspeicher vorhanden?(Presence bit P)(If 0 in swapfile)
- Wurde auf die Seite bereits zugegriffen?(Reference bit R or access bit A)(for swap targeting)
- Wurde die Seite modifiziert?(Dirty bit D or modification bit M)(If not modified just drop(same version already on the disk), if modified save to disk)
![[Pasted image 20250717090605.png#invert]]
PRD + Physical Frame Number(pointer) + RWX perms + additional

## Adressübersetzung mit Seitentabelle
Notwendige Angaben
- Wo befindet sich die Seitentabelle => Tabellenbasisregister
- Welche Seite wird angesprochen => Seitennumer
- Adresse innerhalb der Seite => Wortadresse(offset, displacement)
![[Pasted image 20250717091502.png#invert]]

FrameAddress = content_of(Tabellenbasisadresse + Seite)
FinalAddress = Concatenate(FrameAddress, Wortadresse)

![[Pasted image 20250717092859.png#invert]]
![[Pasted image 20250717092910.png#invert]]
![[Pasted image 20250717092926.png#invert]]
![[Pasted image 20250717092950.png#invert]]

Für 64-Bit-Architekturen erst recht nicht handhabbar ($2^{52}$!)

Idee 1: invertierte Tabelle
- Ein Eintrag pro Kachel, nicht pro Seite
- Größe der Tabelle skaliert mit dem tatsächlich vorhandenen Speicher, nicht mit der virtuellen Adressraum-Größe

Problem: wenn das eine Kachel-Tabelle ist, wie löst man die Aufgabe „gegeben V, suche P“?
- Naiv: lineare Suche durch die gesamte Tabelle
- Etwas trickreicher: durch Einsatz von Hash-Verfahren muss nur ein Teil der Einträge durchsucht werden… dennoch ineffizient

Idee 2: mehrstufige Tabelle
- Anstatt alle 20 Bits mit einer einzigen $2^{20}$-Tabelle zu übersetzen, mache zwei Schritte à 10 Bits
- Erste Tabelle enthält $2^{10}$ Einträge
- Nur beim Übersetzen -> 1. Tabelle verweist auf eine 2. Tabelle zur Übersetzung der übrigen Bits dieses Adressbereichs

- Leerer Adressraum: eine Tabelle à $2^{10}$
- Adressraum mit 5 Seiten: max 6 Tabellen à $2^{10}$
- Voll besetzter Adressraum: $2^{10}$+1 Tabellen à $2^{10}$

![[Pasted image 20250717094554.png#invert]]

![[Pasted image 20250717094702.png#invert]]
![[Pasted image 20250717094722.png#invert]]
![[Pasted image 20250717094737.png#invert]]
![[Pasted image 20250717094758.png#invert]]
![[Pasted image 20250717094813.png#invert]]
![[Pasted image 20250717094855.png#invert]]


## Speicherzuweisung und Auslagerung
BS verwaltet sowohl die Kacheln selbst als auch die Adressräume

2 Fälle, Strategie benötigen:
- Nachschub: ein neuer Prozess startet oder ein bestehender fordert zusätzlichen Speicher an
- Verdrängung: es besteht Speicherknappheit, so dass durch Auslagerung Platz geschaffen werden muss

#### Nachschubstrategien
- Demand paging: Seiten werden erst bei Bedarf nachgeladen
	- Bedarf äußert sich durch Zugriff auf ungültige virtuelle Adresse(=> Seitenfehler tritt auf, BS "löst" das Problem)
- Pre-paging: Transport einer Seite in den Arbeitsspeicher so, dass die Seite zum Referenzierungszeitpunkt zur Verfügung steht
	- Voraussetzung: exaktes Prozessverhalten im Voraus bekannt, meist nicht erfüllt
- Kombinierte Ansätze
	- Beim Prozess-Start: Pre-Paging(für Heap und Stack)
	- Während Laufzeit: Demand Paging


#### Verdrängung
Möglichkeit 1: Prozesse vollständig ein- und auslagern => ineffizient

Möglichkeit 2: Prozessteile (Seiten!) werden  zur Laufzeit und erst bei Bedarf ein- und ausgelagert
Ausgelagerte Seiten werden in der Seitentabelle als „ungültig“ markiert, beim Zugriff darauf wird ein Seitenfehler ausgelöst

Für einen ungültigen Eintrag muss nur gelten: Gültig-Bit P = 0

Diese können also z.B. dazu genutzt werden, die Blocknummer auf der Festplatte zu speichern, wohin der Seiteninhalt ausgelagert wurde!
Wenn das D-Bit in der Seitentabelle nicht gesetzt ist, kann das Schreiben auf die Festplatte u.U. unterbleiben!

![[Pasted image 20250717100441.png#invert]]

#### Demand Paging
![[Pasted image 20250717100814.png#invert]]
![[Pasted image 20250717100827.png#invert]]
![[Pasted image 20250717100857.png#invert]]


#### Verdrängungsstrategien
Seitenfehler durch intelligente Verdrängung minimieren

Welche Kachel soll geleert und der Inhalt ausgelagert werden?
- Lokale Auswahlstrategie: Es wird eine Kachel geleert, welche dem den Seitenfehler verursachenden Prozess zugeordnet ist
- Globale Auswahlstrategie: Eine beliebige Kachel – auch von fremden Prozessen – darf geleert werden

Modellierung der Seitenersetzung
Ist $r_{i}$ die Nummer der Seite, auf der zum Zeitpunkt $t$ zugegriffen wird, so heist $R = r_{1}, r_{2}, r_{3}, \dots, r_{n}$ Seitenreferenzfolge

**Optimalste Auswahlstrategie**
Verdränge diejenige Seite, die am längsten nicht mehr benötigt werden wird

**Realisierbare Strategien**
Lokalitätsprinzip = Zugriffsverhalten in unmittelbarer Vergangenheit = gute Schätzung für das Verhalten in nächster Zukunft
- Räumliche Lokalität: Nach Zugriff auf Adresse a ist ein Zugriff auf eine Adresse in der Nähe von a sehr Wahrscheinlich
- Zeitliche Lokalität: Nach einem Zugriff auf Adresse a ist ein erneuter Zugriff auf a sehr wahrscheinlcich

Warum?
- Sequentielle Ausführung von Anweisungen, Schleifen
- Ausführung bestimmter Programmteile nur in Ausnahmefällen
- 90/10-Regel: Prozesse verbringen 90% der Zeit in 10% des Adressraums



Verdrängung der Seite, die
- Am längsten im Speicher war(FIFO)
- Am längsten nicht benutzt wurde(Least Recently Used, LRU)
- Am wenigsten benutzt wurde(Least Frequently Used, LFU)
- Innerhalb eines Zeitraums nicht referenziert(Recently Not Used, RNU)

#### FIFO-Strategie
Anomalie der FIFO-Strategie
- Bei steigender Kachelzahl kann die Anzahl von Seitenfehlern steigen(!)

#### LFU-Strategie
- Ein Zähler pro Seite
- Bei gleicher Frequenz FIFO

#### LRU-Strategie
- Seitenzugriffs-Stapel: Seite, auf die zuletzt zugegriffen wurde, wird auf oberste Stapelposition gelegt => oberste k Seiten im Speicher
- Falls Seite schon im Speicher: im Stapel wieder nach oben befördern
-  Bei Auslagerung: k-te Seite austauschen; neu referenzierte Seite kommt auf oberste Position => vorhandene Seiten rutschen nach unten, k-te Seite rutscht aus dem Stapel

#### RNU-Strategie
Definition des Zeitraums über ein Fenster, das k zuletzt referenzierte Elemente umfasst
Kritische Größe: Fensterbreite k>0, aber k klein
![[Pasted image 20250717105044.png#invert]]

Von den Strategien zeigt LRU im Durchschnitt die beste Leistung, d.h. geringste Seitenfehlerrate

Probleme bei der Realisierung
-  Alle realisierbaren Strategien erfordern bei jedem Zugriff gewisse Datenoperationen (Stapeloperationen, Zählerinkrementierung, …)
-  Durchführung dieser Operationen (vollständig in Software, mit Unterstützung von Hardware) ist zu aufwendig
-  Daher werden hauptsächlich Annäherungsverfahren realisiert


## Angenäherte LRU/NRU-Strategie
- Hardwareunterstützung beim Seitenzugriff
	- Jede Seite besitzt Zugriffsbit (A) in der Seitentabelle
	- Das Zugriffsbit wird beim Lesen der Seite von der CPU gesetzt
	- Keine Information über den Zeitpunkt eines Zugriffs
- Fehlende Zeitinformation wird durch eine periodische Rücksetzung der Zugriffsbits simuliert
- Beispiel: Second-Chance-Algorithmus (Clock-Algorithmus)

#### Second-Chance-Algorithmus
- Sortiere die Seiten/Referenzbits gemäß Einlagerungsdauer
- Durchlaufe zyklisch den Vektor mit Referenzbits
	- Falls das Referenzbit der aktuellen Seite = 1
		- Setze das Referenzbit auf 0
		- Betrachte die nächste Seite
	- Falls das Referenzbit der aktuellen Seite = 0
		- Verdränge die Seite
		- Setze die Suche mit der nächsten Seite fort


## Solaris 2
Speicherverwaltung mit dem Prozess pageout
- Erster Zeiger scannt die Seiten und setzt die Referenzbits auf 0
- Später läuft der zweite Zeiger nach und gibt alle Seiten mit Referenzbit immer noch 0 wieder frei

Wichtige Parameter
-  Scanrate: Scangeschwindigkeit (Seiten/Sekunde)
	- Anpassung an aktuellen Systemzustand (slowscan = 100 Seiten/s bis fastscan = Gesamtanzahl von Seiten/2 aber max = 8192)
- Handspread: Statischer Abstand in Seiten zwischen den Zeigern
	- Tatsächlicher Abstand ergibt sich durch Kombination von scanrate und handspread, z.B. scanrate = 100 und handspread = 1024 => 10s zwischen den beiden Zeigern
	- Bei höher Belastung Abstände von einigen Tausendstel nicht ungewöhnlich
- Erweiterung: Überspringe Seiten von Shared Libraries


## Paging-Daemon
Grundidee:
- Trennung von Seitenverdrängung und Seiteneinlagerung
-  Speicherverwaltung hält eine vorab festgelegte Anzahl an Kacheln frei => Seitenfehler können ohne Verdrängung beseitigt und neue Adressräume direkt angelegt werden

Realisierung:
- Paging-Daemon wird periodisch aktiviert (Üblicher Wert: alle 250 ms)
- Überprüfung, ob die a-priori vorgegebene Anzahl freier Kachel unterschritten wurde (Üblicher Wert: ca. 25% der Kachel frei)
	- Falls ja => Auslagerung von Seiten auf die Festplatte gemäß der eingesetzten Verdrängungsstrategie
	- Falls nein => Blockierung des Paging-Daemons bis zum nächsten Sollzeitpunkt

#### Paging Daemon mit Solaris 2
Wichtige Parameter
-  Lotsfree: mindestens 1/4 des Speichers frei, Überprüfung alle 0.25s
-  Desfree: Fällt die Anzahl freier Seite unter diesen Schwellwert (Durchschnitt über 30s) => Start Swapping, Überprüfung alle 0.1s
-  Minfree: Nur noch minimale Menge freier Seiten vorhanden => Aufruf der Seitenersetzung bei jeder Speicheranforderung
![[Pasted image 20250717110310.png#invert]]


## Page-Fault-Frequency-Modell
Seitenfehlerrate = Anzahl Seitenfehler / Zeiteinheit
Für jeden Prozess wird die Seitenfehlerrate r gemessen
-  Einführung von zwei Schwellwerten r1 (obere Intervallgrenze) und r2 (untere Intervallgrenze)
- Einstellung der Kachelanzahl S in Abhängigkeit von Seitenfehlerrate
	- Verringere Anzahl Kacheln, falls r < r1, d.h. S =S -1
	- Erhöhe Anzahl Kacheln, falls r > r2, d.h. S = S + 1

![[Pasted image 20250717110435.png#invert]]
