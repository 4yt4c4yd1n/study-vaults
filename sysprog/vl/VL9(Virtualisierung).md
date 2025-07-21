Logische Repräsentation von Ressourcen ohne Einschränkung durch physikalische Verfügbarkeit
BS kommuniziert also nicht mit Hardware, sondern mit einem anderen Stück Software, das so tut, als wäre es echte Hardware: der Hypervisor(auch: Virtual Machine Monitor)

## Virtualisierung in Rechenzentren
![[Pasted image 20250718091219.png#invert]]
![[Pasted image 20250718091229.png#invert]]


## Volle Virtualisierung
![[Pasted image 20250718091522.png#invert]]


## Arten der Virtualisierung
![[Pasted image 20250718091616.png#invert]]
![[Pasted image 20250718091647.png#invert]]


## Vergleich Hypervisor / Betriebssystem
![[Pasted image 20250718091835.png#invert]]

## Hypervisor-Implementierung
Hypervisor muss also eine perfekte Illusion der echten Hardware erschaffen, komplett mit (je VM):
- Virtuellem Prozessorstatuswort (vPSW)
- Seitentabellen-basierter virtueller Adressierung
- Simulierten bzw. nach Bedarf zugeteilten Geräten wie: Zeitgeber, Tastatur/Maus, …


## Virtualisierbarkeit: Deprivilegierung des BS
Was braucht man, damit die Virtualisierung funktioniert?
- CPU mit zwei Sicherheitsmodi (privilegiert / unprivilegiert)
- Speicherschutzmechanismus
- trap-Mechanismus (bestimmte Ereignisse versetzen die CPU in privilegierten Modus und springen zu einer Behandlungs-routine)


## Abarbeitung von Interrupts mit und ohne Virtualisierung

- Ohne Virtualisierung:
	- BS kontrolliert Maskierung von Interrupts
	- Wenn Interrupt aktiv => Sprung in privilegierten Modus zu Interrupt-Behandlungsroutine des BS
- Mit Virtualisierung:
	- Hypervisor kontrolliert Maskierung von Interrupts
	- Wenn Interrupt aktiv =>
		- Sprung in privilegierten Modus zu Interrupt-Behandlungsroutine des Hypervisors
		- Hypervisor prüft, ob Interrupt für ihn selbst oder eines der BS
		- Wenn für ein BS: prüft, ob es gerade Interrupts akzeptiert
		- Ja: Interrupt an BS zustellen // Nein: Interrupt „aufheben“


## Grundideen bei effizienter Virtualisierung

welche Wörter dürfen ausgeführt werden und welche nicht?
- Analysiere den Programmcode und entdecke die Bits für den protected Mode
- Führe diese und nur diese mit Hilfe des Emulators aus
- Führe alle anderen Befehle direkt auf der Hardware aus

Popek&Goldberg: alle „unschädlichen“ Instruktionen werden ohne Intervention direkt vom Prozessor ausgeführt


## Kategorien von CPU Befehlen
![[Pasted image 20250718105101.png#invert]]
privilegierte Instruktion (Ipriv): eine Instruktion, die bei Ausführung im unprivilegierten Modus Trap auslöst

Beispiel: x86 cli/sti („clear/set interrupt flag“: Maskieren/Demaskieren von Interrupts)
Wenn Prozesse/VMs Interrupts abschalten dürften, könnte das BS/Hypervisor nicht mehr schedulen
Andere Beispiele: Set CPU Timer (S/370) legt fest, wann der laufende Prozess die CPU abgeben muss

sensitive Instruktion (Isens): eine Instruktion,
- die den Zustand der (echten!) Hardware ändert oder sichtbar macht (control sensitive) oder
- die sich unterschiedlich verhält, je nachdem, in welchem CPU-Modus sie ausgeführt wird (behavior sensitive)

Beispiel: x86 popf („pop flags“: lade Prozessorstatuswort vom Stack)
- Wenn privilegiert: schreibe Wert vom Stack direkt in PSW
- Wenn unprivilegiert: lasse systemrelevante Bits im PSW unverändert (z.B. Interrupt Flag), übernehme übrige Bits (z.B. Bedingungsbits) vom Stack ins PSW

## Theorem Popek und Goldberg
Effizientes VMM ist realisierbar, falls {sensitive instructions} => {privileged instructions}
![[Pasted image 20250718105651.png#invert]]
Alle Befehle, welche die korrekte Funktionsfähigkeit der Maschine
beeinflussen können(sensitive), werden abgefangen und an Hypervisor weitergeleitet
- Nicht-privilegierte Befehle werden aus Effizienzgründen direkt ausgeführt
- Einfachste Virtualisierung ist trap-and-emulate (klassische Virtualisierung)

- Effizienter VMM: Alle nichtsensitiven Befehle werden direkt auf der CPU ausgeführt (native Ausführung)
- Trap and emulate
	- Guest OS führt sensitiven Befehl aus
	- Befehl wird mit trap abgefangen
	- VMM emuliert den eigentlichen Befehl
![[Pasted image 20250718110553.png#invert]]


## einzelne x86-Instruktionen
- sti (Interrupts demaskieren) => Ipriv?
	Ja, weil Trap.
- sti (Interrupts demaskieren) => Isens?
	Ja, modifiziert Hardware-Zustand (Zulassen von Interrupts).


Gedankenexperiment 1: keine Virtualisierung
- Annahmen:
	- BS enthält sti-Instruktion
	- Aktuell sind Interrupts maskiert (= Annahme deaktiviert)
	- BS möchte Interrupts mittels sti wieder demaskieren
- Ergebnis:
	- sti wird in privilegiertem Modus ausgeführt
	- PSW.IF = 1; Interrupts erfolgreich demaskiert


Gedankenexperiment 2: Virtualisierung
- Annahmen:
	- BS enthält sti-Instruktion
	- Aktuell sind für dieses BS virtuelle Interrupts maskiert
	- BS möchte Interrupts mittels sti wieder demaskieren
- Ergebnis:
	- sti wird in unprivilegiertem Modus ausgeführt => Trap!
	- PSW.IF wird nicht geändert, da Trap ausgelöst wurde
	- Hypervisor ändert virtuellen Zustand: vPSW.IF = 1; Interrupts für dieses BS erfolgreich demaskiert!


- popf (PSW vom Stack laden) $\in$ Ipriv?
	Nein, kein Trap.
- popf (PSW vom Stack laden) $\in$ Isens?
	Ja, verhält sich unterschiedlich je nach Modus (behavior sensitive)!
=> popf verhindert Virtualisierbarkeit der x86-Architektur!

popf de trap yok ondan virtual olmuyo


## Lösung zur Virtualisierung
- Problem: sensitive Instruktionen im BS, die nicht privilegiert sind
- Einzige Lösung: Entfernen dieser Instruktionen aus BS
	1. Anpassungen am BS-Quellcode, so dass dieser statt der problematischen Instruktionen explizite Aufrufe zum Hypervisor enthält („Paravirtualisierung“)
	2. Binary Translation: BS-Maschinencode wird vor der ersten Kontrollübergabe durch Hypervisor auf problematische Instruktionen untersucht und diese durch explizite Hypervisor-Aufrufe ersetzt
=> Trap & Emulate-Hypervisor unter diesen Umständen kaum praktikabel, da Overhead viel zu groß


## Speicherverwaltung bei voller Virtualisierung
![[Pasted image 20250719092747.png#invert]]

Umsetzung in Praxis: Shadow page tables
- Guest OSs verwaltet eigene Seitentabelle (aus Kompabilitätsgründen, wird nicht weiter genutzt)
- Jede Änderung an dieser Tabelle werden getrappt und die Einträge in die neue Seitentabelle, die von Hypervisor errichtet und verwaltet wird, kopiert → Diese Tabelle nennt man VMM’s shadow page table

![[Pasted image 20250719093154.png#invert]]


## Virtualisierung von E/A
Bei der heute standardmäßig verwendeten Paravirtualisierung kooperieren
das BS und der Hypervisor
- BS weiß von der Virtualisierung und erzeugt Aufrufe an den Hypervisor → kein aufwendiges Auffangen mehr notwendig
- CPU-Unterstützung für Virtualisierung durch zusätzliche Sicherheitsringe / -modi, durch verbesserte MMUs (Memory Management Units), etc.

