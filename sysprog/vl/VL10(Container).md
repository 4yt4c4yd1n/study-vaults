Herausforderungen der Software Entwicklung: Frequenz, Geschwindigkeit


## Microservices als de-facto Standard
Anwendungen werden auf mehrere/viele kleine Services verteilt, die unabhängig voneinander laufen

- Dienste implementieren Geschäftsfunktionen (Business functions, z.B. billing)
- Jeder Mikro-Dienst wird in einem eigenen Prozess ausgeführt
- Minimierte zentrale Verwaltung der Dienste
- Kommunizieren über leichtgewichtige Mechanismen miteinander, häufig HTTP/REST
- Microservices können
	- in verschiedenen Programmiersprachen geschrieben werden
	- unterschiedliche Datenspeichertechnologien verwenden
	- unabhängig voneinander durch vollautomatische Bereitstellung eingesetzt werden


## Virtualisierung mit Containern
- Container enthalten Prozesse, folgen dem Microservices-Design
- Container werden für kurze Zeit erstellt (kurzlebig), erledigen die Aufgabe und verschwinden wieder
- Kleine Updates ermöglichen eine schnelle Behebung bei Fehlern
	- Bei Problemen wird der fehlerhafte Container zerstört und neu bereitgestellt
	- Container sind zustandslos


## Container vs. virtuelle Maschinen
Container = leichtgewichtige Alternative mit System Level-Virtualisierung
- VMs mit vielen Vorteilen bei Hardwareauslastung und Isolation von Diensten
	- Virtuelle Maschinen sind meist mehrere Gigabytes groß (schwergewichtig)
	- Verbrauchen viele Ressourcen, da ein gesamter Rechner emuliert und ein Betriebssystemkern gestartet werden muss
- Container als Kompromiss
	- Isolation (und somit) Schutz der individuellen Container ist wichtig
	- ABER: Gleichzeitige Heterogenität bei Betriebssystemen muss nicht sein (Keine Wahlfreiheit beim eingesetzten BS)(Win and linux dont mix)
![[Pasted image 20250719094320.png#invert]]


## Isolation
Wie trennt man Container, die auf der gleichen Hardware und auf der gleichen Container-Engine laufen?

Nutzung mehrerer BS-Erweiterungen
- chroot (Unix-Version 7, 1979): Prozessisolierung durch Beschränkung des Dateizugriffs einer Anwendung auf ein Home-Verzeichnis und seine untergeordneten Verzeichnisse(changes root dir for the proc. and jails it)
- cgroups (control groups, Google 2006): Kernelfunktion zur Definition, Überwachung und Durchsetzung der maximalen Ressourcennutzung durch einen Prozess
	- Resource limiting: maximaler Speicher, Cache, Bandbreite, CPU quota, …
	- Priorisierung: Höhere Priorität berechtigt zu mehr CPU-Zeit / mehr E/A-Bandbreite
	- Accounting: Statistik über verbrauchte Ressourcen
	- Control: Einfrieren des Prozesses/der Familie von Prozessen, wenn Limits erreicht
- Namespace Isolation: Eltern-/Kindprozesse mit eigenen, für andere unsichtbaren PIDs


## Fallstudie: Docker
Docker-Container fassen die auszuführende Software mit allen notwendigen Komponenten (code, runtime, system tools, libraries) in einem Dateisystem zusammen

Architekturelemente
- Docker Daemon = verwaltet die Laufzeitumgebung von Docker-Containern
- Docker Images = Blaupause mit allen Informationen und Abhängigkeiten, die zum Ausführen einer Anwendung in einem Docker-Container benötigt werden
- Docker Repositories 


## Kubernetes (K8s, Steuermann)
Portable Plattform für die Verwaltung von Container-basierten Diensten
De-facto-Standard für die Verwaltung und Orchestrierung von Container-Workload-Clustern

- Service Discovery und Load Balancing
	- Verwaltung mehrerer Container und Verteilung der Arbeitslast auf diese Container
- Speicherorchestrierung
	- Flexible Anbindung an virtualisierte (logische) Speichereinheiten
- Automatisierte Rollouts und Rollbacks
	- Schrittweise Bereitstellung neuer Container, Bewertung der Leistung und - im Falle von Fehlern - Zurückrollen auf die letzte funktionierende Version
- Automatisches Bin-Packing
	- Entscheidung über den besten Platz für eine containerisierte Aufgabe in Bezug auf CPU/GPU, Speicher, etc. → beste Nutzung der verfügbaren Ressourcen
- Selbstheilung (Self-Healing)
- Neustart ausgefallener Container, Migration von Containern, Stoppen nicht reagierender Container usw.


## SW-Entwicklung und Betrieb
- Entwicklung
	- SW-Developer schreiben Code für Produkt und Funktionen ohne Kenntnisse der zukünftigen IT-Infrastruktur
- IT-Betrieb
	- Bereitstellung von Anwendungen und Diensten ohne Kenntnis des Codes / ohne Beteiligung an der Entwicklung
- Qualitätssicherung
	- Testen neuer Funktionen und des Produkts sowie Sicherstellen einer guten Benutzererfahrung
Ungleichgewicht zwischen Entwicklungs- und Betriebsteams führt zu Konflikten
und Ineffizienzen


## Devops
![[Pasted image 20250719095456.png#invert]]
- Continuous Integration
	- Änderungen von verschiedenen Entwicklern so oft wie möglich in den Hauptcode integrieren
- Continuous Delivery
	- Bereitstellen der Anwendung zu jedem beliebigen Zeitpunkt und Automatisieren des Freigabeprozesses
- Continuous Deployment
	- Jede Änderung, die die Testpipeline durchläuft, wird automatisch bereitgestellt
- Infrastructure as Code
	- Verwendung von SW-Entwicklungsparadigmen zur Verwaltung und Konfiguration der Infrastruktur


#### Continuous Integration
![[Pasted image 20250719095648.png#invert]]

