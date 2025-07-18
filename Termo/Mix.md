# Zustandsgröße
$z = z(x, y)$
Eine Zustandsgröße hängt nur vom Zustand des Systems ab, nicht vom Weg, auf dem dieser Zustand erreicht wurde
beschreiben eindeutig den Zustand des Systems
besitzen ein vollständiges Differential und erfüllen den Satz von Schwarz:
$\huge{dz = \left(\frac{{\partial z}}{\partial x}\right)_{y}dx + \left(\frac{{\partial z}}{\partial y}\right)_{y}dy} \quad \text{und}\quad \frac{{\partial^2z}}{\partial x\partial y} = \frac{{\partial^2z}}{\partial y\partial x}$
Daraus folgt, dass ZG wegunhabängig sind.
Bsp: $T, p, \rho, V, \dots$

### Intensive
bleiben bei der Teilung eines systems unverändert
Bsp: $\large{T, \rho, p, c\text{(Stoffmengekonzentration)},\eta \text{(Viskozit\"at)} n\text{(Brechungsindex)}, \mu \text{(chemisches Potential)},}$
$\large{\varepsilon \text{(Permittivit\"at)}, P\text{(dielektrische Polarisation)}}$
Elektrische Spannung $U$ ist ur in einer Parallelschaltung eine intensive Größe, in einer Reihenschaltung ist sie extensiv. Die elektrische Stromstärke $I$ ist nur in einer Parallelschaltung eine extensive Größe, in einer Reihenschaltung ist sie intensiv. 

### Extensive
Ändern sich mit der Größe des betrachteten Systems
Bsp:$$\begin{align}
m, n, N\text{(Teilchenzahl)}, V, U, F\text{(freie Energie)}, H\text{(Enthalpie)}, \\ G\text{(freie Enthalpie)}, Q\text{(elektrische Ladung)}, S\text{(Entropie)}
\end{align}$$
Zustandsgrößen sind auch dann extensiv, wenn sie proportional zu allen anderen als extensiv bekannten Zustandsgrößen sind. Diese Proportionalität gilt allerdings nur, solange alle nichtextensiven Zustandsgrößen konstant bleiben.

### Spezifische
auf m bezogen. Bsp:
$$\begin{align}
\text{spezifisches Volumen} \qquad \text{v}= \frac{\text{V}}{\text{m}} \\
\text{spezifische innere Energie} \qquad\text{u}=\frac{\text{U}}{\text{m}}
\end{align}$$

### Molare
auf n bezogen. Bsp:
$$\begin{align}
 
\huge{ \text{molares volumen}\qquad \text{v}_{m} = \frac{\text{V}}{n}} \\
 \huge{\text{molare innere Energie}\qquad \text{u}_{m} =\frac{\text{U}}{n}}

\end{align}$$


# Prozessgröße
- Beschreiben Energie / Masse, die die Systemgrenze bei einer Zustandsänderung überschreiten
- wegabhängig
- Bsp: $Q\text{(W\"arme)}, W\text{(Arbeit)}, \dot{m}\text{(Massenstrom)}, \dot{n}\text{(Stoffstrom)}$


# System
### offenes System
- Systemgrenzen durchlässig für Materie
- Systemgrenzen durchlässig für Energie
### geschlossenes System
- Systemgrenzen nicht durchlässig für Materie
- Systemgrenzen durchlässig für Energie
### abgeschlossenes System
- Systemgrenzen nicht durchlässig für Materie
- Systemgrenzen nicht durchlässig für Energie

# Prozess
### Stationär
- die Masse bzw. die Substanzmenge bleibt zeitlich konstant
- genauso viele Masse strömt hinein, wie das System verlässt
- wenn weder die Prozess-, noch Zustandsgrößen von der Zeit abhängen

### Stationäre Fließprozesse
![[Pasted image 20250403141537.png#invert]]
![[Pasted image 20250403141619.png#invert]]
$$\begin{align}
\huge{\Delta\dot{H}_{12} = P_{12} + \dot{Q}_{12}}\\ \\
\huge{\Delta \dot{H} = \dot{m}c_{p}\Delta T}
\end{align}$$
![[Pasted image 20250405171515.png#invert]]
![[Pasted image 20250406150111.png#invert]]
$\frac{dS}{dt} = 0$
$\huge{\dot{m}(h_{2} - h_{1}) = P_{21} + \dot{Q}_{21}}$

# Ideales Gas

- Teilchen ohne räumliche Ausdehnung
- keine Wechselwirkung zwischen Teilchen
- Wechselwirkung mit fester Wand: elastische Stöße
Näherung gut für kleine Drücke, hohe Temperaturen und kleine Atome

**Thermische Zustandsgleichung:**
$\huge{pv=RT} \Leftrightarrow pV = mRT \Leftrightarrow pv_{m}=R_{m}T\Leftrightarrow pV=nR_{m}T$
$\huge{R = c_{p}-c_{v}}$
**Kalorische Zustansgleichung:**
$\huge{u = \frac{f}{2}RT \Longleftrightarrow}u_{m}=\frac{f}{2}R_{m}T\Longrightarrow du = c_{v}dT$

$R = \frac{R_{m}}{M}, \quad f=$ Freiheitsgrade

### Kinetische Energie
![[Pasted image 20250403114203.png#invert]]

### Geschwindigkeit
$\huge{c = \sqrt{ \frac{1}{N} \sum^N_{i=1} c^2_{i}} }$
$\huge{ \bar{c} = \sqrt{ \frac{3kT}{m^*} } \cdot \sqrt{ \frac{N_{A}}{N_{A}} }= \sqrt{ \frac{3R_{m}T}{M} }}$
$k = \text{Boltzmann constant}$
# Zustandsgleichungen

definieren Zusammenhang von ZG des Systems im Gleichgewicht

### Thermische Zustandsgleichung
definieren Zusammenhang von _thermischen_ ZG des Systems($T, p, v$ und abgeleitete)

### Kalorsiche Zustandsgleichung
beschreiben energetische ZG des systems

## Partielle Ableitungen der thermischen Zustandsgleichung

![[Pasted image 20250403121404.png#invert]]
![[Pasted image 20250403121729.png#invert]]


# Zustandsänderung
![[Pasted image 20250403125412.png#invert]]

![[Pasted image 20250403125521.png#invert]]
![[Pasted image 20250403125540.png#invert]]
![[Pasted image 20250403134510.png#invert]]
reversible adiabat -> isentrop

# Enthalpie
$\huge{H \equiv U + pV \qquad \text{bzw.}\qquad h\equiv u+pv}$
zentrale thermodynamische Größe für energetische Betrachtungen technischer Prozesse
isentropic -> $h_{2} - h_{1} = w_{1,2}$
# spezifische Wärmekapazität $c$
![[Pasted image 20250403122642.png#invert]]
V konstant: ![[Pasted image 20250403124713.png#invert]]
![[Pasted image 20250403123726.png#invert]]
![[Pasted image 20250403142302.png#invert]]


# Kreisprozess
![[Pasted image 20250403132256.png#invert]]

# Volumenänderungsarbeit & Technische Arbeit
![[Pasted image 20250403134755.png#invert]]


# Entropie
![[Pasted image 20250404102836.png#invert]]
![[Pasted image 20250404111723.png#invert]]
![[Pasted image 20250404111623.png#invert]]
![[Pasted image 20250405145636.png#invert]]
![[Pasted image 20250405145643.png#invert]]
![[Pasted image 20250405145757.png#invert]]

![[Pasted image 20250405173059.png#invert]]


# Anergie und Exergie
![[Pasted image 20250404111903.png#invert]]
![[Pasted image 20250404111957.png#invert]]
$a = \text{Umgebung}$
$\huge{\Delta E_{v,34} = T_{u}\Delta S_{irr,34} = T_{u}\left( \dot{m}(s_{4}-s_{3}) -\frac{Q}{T_{L}} \right)}$
TL = isinin alindigi sicaklik
![[Pasted image 20250406151335.png#invert]]


# Wirkungsgrad

![[Pasted image 20250404114522.png#invert]]
### Thermischer Wirkungsgrad
![[Pasted image 20250404114553.png#invert]]

### Exergetischer Wirkungsgrad
![[Pasted image 20250404114748.png#invert]]

### Isentroper Wirkungsgrad
![[Pasted image 20250404114856.png#invert]]
$pv^k = \text{const.}$ dan v yi bul gerekli formule koy oyle devam et

# Leistungszahl
![[Pasted image 20250407101252.png#invert]]
![[Pasted image 20250407101116.png#invert]]


# Thermodynam. Mitteltemperatur(isobare Wärmeübertragung)
![[Pasted image 20250404121812.png#invert]]

# Reale Gase
### p-V-Diagramm
![[Pasted image 20250404122335.png#invert]]
liq, krit. Punkt,gas, Siedelinie, Tripellinie, Nassdampfgebiet
Koyu mavi = izoterm
Pembe = izobarik

### p-T-Diagramm
![[Pasted image 20250404122714.png#invert]]
Tripelpunkt, Dampfdruckkurve
Kritischer Punkt´tan sonra Überkritisches Gebiet
Pembe = Tripelpunkt

### p-$\rho$-Diagramm
![[Pasted image 20250404144207.png#invert]]

### T-s-Diagramm
![[Pasted image 20250404152433.png#invert]]

![[Pasted image 20250404123120.png#invert]]
Projeksiyondan dolayi alan -> cizgi -> nokta

### Lineare Interpolation
![[Pasted image 20250404123325.png#invert]]
### Interpolation im Zweiphasengebiet
![[Pasted image 20250404123420.png#invert]]
![[Pasted image 20250406131240.png#invert]]


# Joule-Prozess
![[Pasted image 20250404144928.png#invert]]
Carnot-Wirkungsgrad:![[Pasted image 20250404145110.png#invert]]


# Otto-Prozess
#### p-V
![[Pasted image 20250404160613.png#invert]]
#### p-v
![[Pasted image 20250404160634.png#invert]]

# Mischungen
![[Pasted image 20250407112156.png#invert]]

# Partialdruck
![[Pasted image 20250407112424.png#invert]]

