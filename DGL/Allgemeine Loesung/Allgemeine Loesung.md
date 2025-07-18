---
review-frequency: high
reviewed: 2025-03-28
---

![[Pasted image 20250323120901.png#invert]]
$\alpha\pm i\omega$ karak. polinomun kokleri

## Inhomogen
$$
\begin{gather}
\huge{\dots = ue^{\mu t}}\\
\huge{y_{p}= ve^{\mu t}} \\
\huge{v = \frac{u}{p(\mu)}}
\end{gather}
$$
p == char. poly.


Homojen genel cozumunu bul
Inhomojene method of **undetermined coefficients** yap
![[Pasted image 20250328103317.png#invert]]
$\huge{cosh(x) = \frac{e^{x}}{2}+ \frac{e^{-x}}{2}}$
Gerekli turevleri hesapla
Turevleri yerine koy
Inhomojende bilmedigin kisimlaari bul

Genel = Homojen + Inhomojen

## Farkli reel
$\huge{c_1e^{a_1t} + c_2e^{a_2t}}$

## Anfangwert
$\huge{y(t_0)=y_0}$
$\huge{y(t) = e^{\alpha(t-t_0)}y_0}$

# $c = re^{i\phi}$
$\huge{re^{\alpha t}(cos(\omega t + \phi) + i\cdot sin(\omega t + \phi))}$
$\huge{re^{\alpha t}}$ = zeitabh¨angiger Radius 
$\huge{\omega}$ = Kreisfrequenz
$\huge{\alpha}$ = D¨ampfungs- oder Wachstumsrate
$\huge{\phi}$ = Phasenverschiebung

## Variable Koeffizienten
![[Pasted image 20250323123758.png#invert]]
![[Pasted image 20250323123809.png#invert]]


## Seperable
y' = dy/dx
dx karsiya gider integrali alinir

## Vektor
Diagonalisierbar ise! e^At
Char polinom:
$X(\lambda)=\det(\lambda I-A)$


1. Eigenvalue bul $det(A - \lambda I) = 0$
2. Eigenvektor bul $(A -\lambda_iI)v = 0$
3. Genel cozum = $c_1e^{\lambda_1t}v_1 + ...$


$\huge{e^{ait} = (\cos(at)+i\sin(at))}$
$\huge{\cos(at) = \frac{e^{ait}}{2}+\frac{e^{-ait}}{2} }$
$\huge{\cos(at) = \frac{e^{ait}}{2i}-\frac{e^{-ait}}{2i} }$
