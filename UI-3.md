# Ortogonalita a ortogonální báze: definice, vlastnosti, využití při počítání vzdálenosti lineárních variet

## 1. Ortogonalita a ortogonální/ortonormální báze

Nechť $\mathcal{H}$ je prostor se skalárním součinem a $x, y \in \mathcal{H}$. 
* Vektor $x$ nazýváme **ortogonální na (nebo kolmý) $y$**, jestliže platí:
  $$\langle x \mid y \rangle = 0$$
* Soubor vektorů $(x_1, \dots, x_n)$ z $\mathcal{H}$ nazveme **OG (ortogonální)** $\iff$ pro $\forall i, j \in \widehat{n}, i \neq j$ platí:
  $$\langle x_i \mid x_j \rangle = 0$$
* Soubor vektorů $(x_1, \dots, x_n)$ z $\mathcal{H}$ nazveme **ON (ortonormální)** $\iff$ je OG a každý vektor má velikost 1. Neboli pro $\forall i, j \in \widehat{n}$ platí:
  $$\langle x_i \mid x_j \rangle = \begin{cases} 0 & \text{pro } i \neq j \\ 1 & \text{pro } i = j \end{cases}$$

**Důležitá vlastnost:** OG soubor nenulových vektorů je LN (lineárně nezávislý). ON soubor je vždy LN.

**Pythagorova věta:**
* Nechť $x \perp y$, pak platí: $\|x + y\|^2 = \|x\|^2 + \|y\|^2$
* Obecně, nechť $(x_1, \dots, x_n)$ je OG soubor, pak platí: $\|x_1 + \dots + x_n\|^2 = \|x_1\|^2 + \dots + \|x_n\|^2$

---

## 2. Fourierovy koeficienty

Nechť $\mathcal{X} = (x_1, \dots, x_n)$ je **ON báze** prehilbertova prostoru $\mathcal{H}$, potom pro každé $z \in \mathcal{H}$ lze vektor rozložit jako:
$$z = \sum_{i=1}^n \langle x_i \mid z \rangle x_i$$
Souřadnice vektoru $z$ vůči ON bázi $\mathcal{X}$ jsou tedy dány přímo Fourierovými koeficienty:
$$(z)_{\mathcal{X}} = (\langle x_1 \mid z \rangle, \langle x_2 \mid z \rangle, \dots, \langle x_n \mid z \rangle)$$

Vůči **OG bázi** platí předpis s normalizací:
$$z = \sum_{i=1}^n \frac{\langle x_i \mid z \rangle}{\langle x_i \mid x_i \rangle} x_i$$

---

## 3. Ortogonální projekce

* **OG projekce na přímku:**
  Mějme prehilbertův prostor $\mathcal{H}$ a vektor $v \neq \theta$ z $\mathcal{H}$. Zobrazení $\text{proj}_v : \mathcal{H} \to \mathcal{H}$ definované jako:
  $$\text{proj}_v(z) := \frac{\langle v \mid z \rangle}{\langle v \mid v \rangle} v \quad \text{pro } z \in \mathcal{H}$$
  se nazývá OG projekce vektoru $z$ na přímku $\langle v \rangle$. V maticovém zápisu: $\text{proj}_v(z) = \frac{1}{\|v\|^2} v \cdot v^T z$.

* **OG projekce na podprostor:**
  Je-li soubor $(x_1, \dots, x_k)$ OG báze podprostoru $P$ z $\mathbb{R}^n$, potom definujeme OG projekci na podprostor $P$ předpisem:
  $$\text{proj}_P(z) := \text{proj}_{x_1}(z) + \dots + \text{proj}_{x_k}(z) \quad \text{pro } z \in \mathbb{R}^n$$

---

## 4. Gramova-Schmidtova ortogonalizace (GSO)

Nechť $\mathcal{X} = (x_1, \dots, x_n) \subset \mathcal{H}$ je LN soubor vektorů. Potom existuje ON soubor takový, že pro $\forall k \in \widehat{n}$ platí $\langle x_1, \dots, x_k \rangle = \langle z_1, \dots, z_k \rangle$. 

Postup konstrukce OG báze (vektory $z_i$):
* $z_1 = x_1$
* $z_2 = x_2 - \text{proj}_{z_1}(x_2)$
* $z_3 = x_3 - \text{proj}_{z_1}(x_3) - \text{proj}_{z_2}(x_3)$
* $\dots$

*(Pozn.: Pro zisk ON báze pak tyto ortogonální vektory přenásobíme vhodným násobkem – znormujeme je.)*

---

## 5. Ortogonální doplněk

Je-li $M \subset \mathbb{R}^n$, definujeme tzv. **OG doplněk $M^\perp$** jako množinu vektorů kolmých na všechny vektory z $M$:
$$M^\perp := \{ x \in \mathbb{R}^n \mid (\forall v \in M)(\langle x \mid v \rangle = 0) \}$$

**Vlastnosti OG doplňku (je-li $P$ podprostor $\mathbb{R}^n$):**
1. $P^\perp$ je podprostor v $\mathbb{R}^n$.
2. $P \cap P^\perp = \{\theta\}$.
3. Každý vektor z $\mathbb{R}^n$ lze zapsat jako součet vektoru z $P$ a z $P^\perp$:
   $(\forall x \in \mathbb{R}^n) (\exists v \in P) (\exists w \in P^\perp) (x = v + w)$
4. Tento rozklad je **jednoznačný**.
5. $(P^\perp)^\perp = P$

---

## 6. Vzdálenosti množin, variet a vektorů

**Definice vzdálenosti:**
Pro dvě množiny $M, N \subset \mathbb{R}^n$ je vzdálenost $d(M,N) = \inf \{ \|x - y\| \mid x \in M, y \in N \}$.
* Posunutí vzdálenost nezmění: $d(M, N) = d(v+M, v+N)$.
* Pro varietu a vektor platí: $d(z, a+P) = d(z-a, P)$.

**Vzdálenost vektoru od podprostoru:**
Mějme $z \in \mathbb{R}^n$ a podprostor $P \subset \mathbb{R}^n$. Vektor $z$ lze rozložit na $z = v + w$, kde $v \in P$ a $w \in P^\perp$. Potom platí:
$$d(z, P) = \|w\|$$
* Výpočet: $w := z - \text{proj}_P(z)$
* *(Alternativa: najít $P^\perp$ a bázi sjednocení $P \cup P^\perp = \mathbb{R}^n$, najít souřadnice a spočítat $w$.)*

**Vzdálenost dvou variet:**
Mějme variety $M = a + \langle x_1, \dots, x_k \rangle$ a $N = b + \langle y_1, \dots, y_l \rangle$.
$$d(M,N) = d(a-b, \langle x_1, \dots, x_k, y_1, \dots, y_l \rangle)$$
Problém se redukuje na vzdálenost vektoru od podprostoru:
$$d = \| (a-b) - \text{proj}_P(a-b) \|$$
*(Pro tento postup musí být báze $P$ ortogonální – tj. z generátorů vynecháme LZ vektory a provedeme Gram-Schmidtovu ortogonalizaci).*

---

### Ukázkový příklad z poznámek:
Spočtěte vzdálenost bodu $(1,1,2,-3)$ od variety $(1,0,1,-1) + \langle (1,-2,0,1) \rangle$.

1. Převod na vzdálenost vektoru od podprostoru (posunutí):
   $z = (1,1,2,-3) - (1,0,1,-1) = (0,1,1,-2)$
   Podprostor $P = \langle (1,-2,0,1) \rangle$.
2. Výpočet OG projekce $\text{proj}_P(z)$:
   $$\text{proj}_P(z) = \frac{(1,-2,0,1) \cdot (0,1,1,-2)}{(1,-2,0,1) \cdot (1,-2,0,1)} (1,-2,0,1) = \frac{-4}{6} (1,-2,0,1) = -\frac{2}{3}(1,-2,0,1)$$
3. Výpočet kolmice $w$ a její normy:
   $$w = z - \text{proj}_P(z) = (0,1,1,-2) - \left(-\frac{2}{3}\right)(1,-2,0,1) = \frac{1}{3}(2,-1,3,-4)$$
   $$d = \|w\| = \frac{1}{3} \sqrt{2^2 + (-1)^2 + 3^2 + (-4)^2} = \frac{\sqrt{30}}{3}$$