![[Pasted image 20250716153107.png#invert |200]]

![[Pasted image 20250716154112.png#invert]]

![[Pasted image 20250716154141.png#invert]]


## Verständigung
Beispiele für Protokolle:
- Kritischer Abschnitt
	- Vereinbarung, den kritischen Abschnitt durch Nutzung von Sperren unter mutex zu stellen
- Dezentrale Bus-Arbitrierung
	- Sendewillige Komponenten(bus request) legen im speziellen Koordinationsprotokoll(Bus-Arbitrierung) fest, wer als nächstes senden darf.
- Verteilte Systeme
	- Knoten melden per Broadcast Bedarf an, Abstimmung basierend auf logische Zeit legt Zugriffsreihenfolge fest


## Unkoordinierte Nutzung
Aufwand für Kollisionauflösung kann geringer sein als der permanente Aufwand für eine vorherige Abstimmung
Einsatz dort, wo
- eine Kollision unwahrscheinlich ist
- der durch die Kollision entstandene Schaden reparabel ist

![[Pasted image 20250716155633.png#invert]]


## Klassifikation von Betriebsmitteln
Unterscheidung: Real / Logisch / Virtuell

- Reale BM: Physisch existent(SSD, CPU, GPU)
- Logische BM: Abstraktion des realen BM(Datei -> Abstraktion der Platte, Fenster -> Abstraktion des Bildschirms)
- Virtuelle BM: Anzahl simuliert > Anzahl real(Virtueller Speicher, Virtuelle Verbindung)

- Persistenz
	- Wiederverwendbar: BM werden nach Nutzung freigegeben und können von anderen Prozessen genutzt werden
	- Verbrauchbar: Einige logische BM wie Signale, Zeitstempel werden durch die Nutzung verbraucht
- Kapazität
	- Begrenzt: BM mus bewirtschaftet werden(explizites Freigeben/Belegen)
	- Unbegrenzt: höchstens An-/Abmelden einer Nutzung


## Auswahlstrategien
- nf(t): zum Zeitpunkt t freie BM-Einheiten
- n(i): Prozess i geforderten BM-Einheiten
- W(t): Warteschlange der angemeldeten Anforderungen/Prozesse

Strategien:
- First-fit-request: Durchsuche die Warteschlange bis n(i) ≤ nf(t)
- Best-fit-request: Durchsuche die Warteschlange vollständig und finde i welche die Restkapazität minimiert

![[Pasted image 20250716161633.png#invert]]

#### Starvation
Gefahr, dass Prozesse mit großen Anforderungen sehr lange warten müssen
Idee: Verwende Fenster dynamischer Größe
- Initialbreite $:L_{max}$
- Nach jeder erfolgreichen Belegung wird die Fensterbreite folgendermaßen reduziert

$$
L = \begin{cases}
L-1, & \text{falls}  & L>1  & \text{und erste anforderung nicht ber\"ucksichtigt} \\
L_{max}  & \text{sonst}
\end{cases}
$$
Nach spätestens $L_{max-1}$ Zugriffe gilt Fenstergröße = 1, d.h. die vorderste Anforderung muss berücksichtigt werden

![[Pasted image 20250716162740.png#invert]]


## Verklemmung(Deadlock)
Verklemmung = Situation, in der Prozesse sich gegenseitig behindern und blockieren und deshalb nicht weiter ausgeführt werden können

Praxis: Blockierte Prozesse und Threads

Erkennung und Behandlung: Vier notwendige und hinreichende Bedingungen überwachen

Ziel: Vermeidung von Deadlocks durch einen Systementwurf, der eine Verklemmung erst gar nicht auftreten lässt

![[Pasted image 20250716163110.png#invert|500]]

#### Wartegraph
Wartegraph(wait-for graph): gerichteter Graph mit Prozessen als Knoten und Wartebeziehungen als Pfeile
Verklemmung = Charakterisiert durch Zyklus im Wartegraphen
![[Pasted image 20250716163313.png#invert]]

Notwendige 3 Bedingungen für das Auftreten einer Verklemmung:
1. Mutex: Jedes involvierte BM ist entweder exklusiv belegt oder frei
2. hold-and-wait: Die Prozesse haben bereits BM belegt, wollen zusätzlich weitere BM belegen und warten darauf -> Notwendige BM wird nicht auf einmal angefordert
3. no pre-emption: Die bereits belegten BM können den Prozessen nicht wieder entzogen werden, sondern müssen von den Prozessen selbst explizit zurückgegeben werden

Hinreichende Bedingung für Deadlocks:
4. circular wait: Eine geschlossene Kette von zwei oder mehr wartenden Prozessen muss existieren, wobei ein Prozess BM vom nächsten haben will, die dieser belegt und die deshalb nicht mehr frei sind


## Beschreibung der BM-Situation
![[Pasted image 20250716164030.png#invert]]
Satir: Proses
Sutun: Kaynak

B nin sutunlarini topla, v den cikar bos kaynak vektoru oluyo
G = B  + R

#### Bedingungen
- Es kann nicht mehr belegt sein, als vorhanden ist
- Kein Prozess kann mehr anfordern als insgesamt vorhanden
	$a_{ij} + b_{ij} \leq v_{j}$
- Ein Prozess, der eine derzeit nicht erfüllbare Anforderung gestellt hat, wird bis zur Erfüllbarkeit blockiert

Eine Prozessmenge $\text{P} = \{\text{P1, P2, ... , Pm}\}$ heißt verklemmt, genau dann wenn:
$\huge{\exists I \subseteq \{1, \dots , m\}: \forall i \in I: \vec{a_{i}}}\nleq\vec{v} - \sum_{c \in I} \vec{b}_{c}$

Selbst wenn wir alle anderen Prozesse terminieren und Ressourcen freigeben, wären die Anforderungen der Prozesse in der Teilmenge nicht erfüllbar:
$\huge{\exists I \subseteq \{1, \dots , m\}: \forall i \in I: \vec{a_{i}}}\nleq\vec{f} - \sum_{c \in I} \vec{b}_{c}$


## Betriebsmittelgraph
![[Pasted image 20250716170013.png#invert]]
es gibt nur Kanten von P nach B oder umgekehrt

Menge von Punkten im Knoten = Anzahl der insgesamt verfügbaren Einheiten eines Betriebsmitteltyps(Ausgangsgrad)
Ein Zyklus im BM-Graph weist auf eine potentielle Verklemmungssituation hin

BM-Graph reduzierbar <-> es gibt einen Prozess, dessen Anforderungen sofort erfüllbar sind und alle seine Kanten entfernt werden können
Verklemmungstheorem für BM-Graphen: Aktuelle BM-Situation verklemmt <-> dazugehöriger BM-Graph ist nicht vollständig reduzierbar

![[Pasted image 20250716170417.png#invert]]
![[Pasted image 20250716170428.png#invert]]


## Verklemmungsvermeidung
Wie kann man eine Verklemmung vermeiden?
- Aktuelle Situation kennen -> BM-Graph
- Restanforderungen der Prozesse müssen bekannt sein
- Eine Belegungsstrategie anwenden, so dass kein Wartezyklus entsteht

Ziel: finde eine Beendigungsreihenfolge der Prozesse derart, dass jede Anforderung durch das erfüllt werden kann, was von früher beendeten Prozessen freigegeben wird

#### Bankier-Algorithmus
![[Pasted image 20250716171120.png#invert]]
Yani en az bir prosese lazim olacak kadar kaynak varsa safe

![[Pasted image 20250716171353.png#invert]]
![[Pasted image 20250716171436.png#invert]]
Sichere Reihenfolge (z. B.): P3 P1 P2 P4

![[Pasted image 20250716171547.png#invert]]


## Beseitigung von Verklemmungen
Reaktion: Erkennung Deadlock (meist durch Stillstand zahlreicher Prozesse) und Ausführung einer Aktion

- Prozesse abbrechen: ein unbeteiligter Prozess, der das benötigte BM hat, wird anhand einer Strategie selektiert und abgebrochen
	- Hoffnung, alles arrangiert sich selbst wieder
	- Auswahlstrategien basierend auf Größe der Anforderung, Umfang belegter BM, Dringlichkeit, Restbedienzeit, Aufwand des Abbruchs
-  Prozesse zurücksetzen: Ein oder mehrere Prozesse werden auf den letzten gültigen Checkpoint zurückgesetzt und neu gestartet
-  BM entziehen: einem Prozess werden von anderen Prozessen benötigte BM entzogen
	 => nur für bestimmte BM-Arten
Allerdings: mögliche Datenverluste und -inkonsistenzen
