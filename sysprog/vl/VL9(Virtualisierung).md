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


