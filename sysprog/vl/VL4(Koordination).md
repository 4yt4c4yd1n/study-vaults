## Explizite / Implizite Prozessinteraktion
**Implizite Interaktion:** wenn ein Prozess eine Systemfunktion aufruft und diese mit anderen Prozessen interagiert.
![[Pasted image 20250716034025.png#invert]]

![[Pasted image 20250716034051.png#invert]]


#### Konkurrenz
2+ Prozesse nutzen ein Betriebsmittel
- Synchronisationsmechanismen notwendig 
- Koordinierung -> Serialisierung der Zugriffsversuche
- n Prozesse -> n-1 Prozesse verzögert

Aufgabe: Sperren/Freigeben einer Variable
Voraussetzung: Zugriff auf gemeinsamen Speicher

Einfachste Form: Reihenfolgebeziehung(Signalisierung)
![[Pasted image 20250716035033.png#invert]]


#### Aktives Warten
```c
// Signalisierung mit Busy-Waiting
struct Signal { // Version 1
	boolean s;
} so = { false }; //Initialisierung
void signal(struct Signal *so) {
	so->s = true;
}
void wait(struct Signal *so) {
	while (so->s == false) { ; } //Warten, bis gesetzt
	so->s = false;
}
```
![[Pasted image 20250716035303.png#invert]]

==verschwendet Prozessorkapazität und blockiert die CPU==


#### Blockierendes Warten
**Lösung**
- muss warten -> Prozess in Zustand blockiert versetzen
- der Prozess muss explizit durch das Signal herausgeholt werden
```c
// Signalisierung mit blockierendem Warten
struct Signal { // Version 2
	boolean s;
	process *wp;
} so = { false, NULL }; // Initialisierung
void signal(struct Signal *so) {
	so->s = true;
	if (so->wp != NULL) { // Wartet ein Prozess?
		deblock(so->wp); // Blockierung aufheben
		so->wp = NULL; // Struktur zurücksetzen
	}
}
void wait(struct Signal *so) {
	if (so->s == false) { // noch kein Signal?
		so->wp = MYSELF;
		block();
	}
so->s = false;
}
```
```pseudo
block(){
	Request OS to change current process state from RUNNING to BLOCKED
	Save ctx to PCB
	Invoke scheduler
}
deblock(process* wp){
	Request OS to change current process state from BLOCKED to READY
}
```

![[Pasted image 20250716040341.png#invert]]

#### Wechselseitige Synchronisierung
![[Pasted image 20250716041834.png#invert]]



## Kritische Abschnitte
Operationsfolgen, bei denen eine nebenläufige oder verzahnte Ausführung zu Fehlern führen kann
Absicherung durch Sperrvariable
Prozesse müssen sich gegenseitig ausschließen(mutual exclusion)
![[Pasted image 20250716042307.png#invert]]


## Thread-safe
- Vermeidung gemeinsamer Zustände
	- Speichern von Statusinformationen in lokalen Variablen
	- jeder Thread hat seine eigene private Kopie aller Variablen
	- nur schreibgeschützte Daten werden gemeinsam genutzt
- Synchronisation, wenn gemeinsamer Zustand notwendig
	- Mutex
	- Check if the shared variable is _still_ equal to `value_at_start`. If it is, write `new_result` to it.
	- **`Load-Linked` (LL)**: You load a value from a memory address. The CPU "links" to this address, essentially placing a watch on it for your thread.
	- **`Store-Conditional` (SC)**: You attempt to store a new value to that same address. The store will only succeed **if no other thread has written to that address** since your `Load-Linked` instruction.



## Semaphor
Semaphore stellen eine Zählsperre dar und bestehen aus
- Nicht-negativ intialisiertem Zähler
- Liste mit Verweisen auf involvierte Prozesse

Grundoperation P(S)(Passieren des Semaphors)
- Der aktuelle Wert des Semaphorzählers wird dekrementiert
- Wenn Zähler nicht-negativ: Passieren der Sperre, eintritt in den kritischen Bereich
- Wenn Zähler negativ: Blockieren

Grundoperation V(S)(Verlassend des Semaphors)
- Austritt aus dem kritischen bereich
- Wenn Zähler negativ: Deblockieren eines wartenden Prozesses
- Der aktuelle Wert des Semaphorzählers wird Inkrementiert

![[Pasted image 20250716045908.png#invert]]
![[Pasted image 20250716045920.png#invert]]

![[Pasted image 20250716050246.png#invert]]
![[Pasted image 20250716050619.png#invert]]
### ## The Core Idea

Imagine you have a shared file or piece of data. There are two types of processes:

- **Readers (Leser)**: They only read the data. They don't change it.
    
- **Writers (Schreiber)**: They modify or write to the data.
    

The problem is how to manage access to this data safely and efficiently.

---

### ## The Rules of Access

The slide lays out the fundamental rules for the "critical section" (the part of the code that accesses the shared data):

1. **One Writer at a Time**: If a Writer is in the critical section, no other process (Reader or Writer) can be there. This is because writing changes the data, and allowing others access at the same time would lead to corruption. ✍️
    
2. **Many Readers are Okay**: Any number of Readers can be in the critical section at the same time. Since they are only reading, they don't interfere with each other. 📚
    
3. **Readers and Writers are Mutually Exclusive**: If Readers are in the critical section, no Writer can enter until they are all gone.
    

In short: **You can have either 1 Writer OR multiple Readers, but never both at once.**

---

### ## The Fairness Problem: Who Goes Next?

The last point on the slide addresses a tricky situation: What happens when a Writer finishes, and a queue of both new Writers and new Readers are waiting to get in? There are two common strategies:

- **Writer Preference (`Schreibervorrang`)**: Give priority to a waiting Writer. A Writer waiting in line gets to go next, even if Readers have been waiting longer.
    
    - **Pro**: Important updates (writes) happen quickly.
        
    - **Con**: Readers might wait forever if Writers keep arriving (this is called "starvation").
        
- **Reader Preference (`Leservorrang`)**: Give priority to all waiting Readers. All Readers waiting in line are let in.
    
    - **Pro**: Maximizes concurrency since many readers can run at once.
        
    - **Con**: A Writer might wait forever if new Readers keep arriving and joining the active group.

![[Pasted image 20250716052715.png#invert]]
![[Pasted image 20250716052219.png#invert]]
mutex1 Readernr i kitliyo
writer gelince r kitleniyor, yeni reader giremiyor(writerlar acliktan olmuyor)\[bu writer gelene kadar baska reader siraya girmis olabilir\]
mutex3 readerlari tek tek geciriyo
w veriyi kitliyo
![[Pasted image 20250716052703.png#invert]]
mutex2 writernr i kitliyo
r readerlari kitliyo
w veriyi kitliyo

## Monitore
Umgang mit Sperren ist fehleranfällig, da Programmierer diese explizit und in korrekter Weise setzen müssen
Monitor-Objekten: Methoden und Datenstrukturen, die zu jedem Zeitpunkt nur von einem Prozess benutzt werden dürfen

![[Pasted image 20250716053712.png#invert]]

