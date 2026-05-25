# Skalární součin a norma vektoru: definice, vlastnosti a příklady

## 1. Skalární součin

Buď $V$ vektorový prostor (VP) nad $T = \mathbb{R}$ nebo $\mathbb{C}$. Zobrazení $\langle \cdot \mid \cdot \rangle : V \times V \to T$ nazýváme **(obecný) skalární součin**, platí-li pro $\forall x, y, z \in V$ a $\forall \alpha \in T$:

1. **Zobrazení je lineární v druhém argumentu**, tj.:
   $$\langle x \mid y + z \rangle = \langle x \mid y \rangle + \langle x \mid z \rangle$$
   $$\langle x \mid \alpha y \rangle = \alpha \langle x \mid y \rangle$$
2. **Platí tzv. Hermitovská symetrie:**
   $$\langle x \mid y \rangle = \overline{\langle y \mid x \rangle}$$
3. **Zobrazení je pozitivně definitní**, tzn.:
   $$\langle x \mid x \rangle \geq 0 \quad \land \quad (\langle x \mid x \rangle = 0 \iff x = \theta)$$

*(Pozn.: Dvojici $(V, \langle \cdot \mid \cdot \rangle)$ nazýváme VP se skalárním součinem nebo **prehilbertův prostor** $\mathcal{H}$.)*

### Základní vlastnosti
1. **Sdružená linearita v prvním argumentu:**
   $$\langle x + y \mid z \rangle = \langle x \mid z \rangle + \langle y \mid z \rangle$$
   $$\langle \alpha x \mid z \rangle = \overline{\alpha} \langle x \mid z \rangle$$
2. **Skalární součin s nulovým vektorem je nula:**
   $$\langle x \mid \theta \rangle = 0 = \langle \theta \mid x \rangle$$

**Důkazy (Dk):**
* K vlastnosti 1 (sčítání): 
  $$\langle x + y \mid z \rangle \stackrel{\text{ax.2}}{=} \overline{\langle z \mid x + y \rangle} \stackrel{\text{ax.1}}{=} \overline{\langle z \mid x \rangle + \langle z \mid y \rangle} \stackrel{\text{vl. } \overline{z}}{=} \overline{\langle z \mid x \rangle} + \overline{\langle z \mid y \rangle} \stackrel{\text{ax.2}}{=} \langle x \mid z \rangle + \langle y \mid z \rangle$$
* K vlastnosti 1 (násobení skalárem):
  $$\langle \alpha x \mid z \rangle \stackrel{\text{ax.2}}{=} \overline{\langle z \mid \alpha x \rangle} \stackrel{\text{ax.1}}{=} \overline{\alpha \langle z \mid x \rangle} \stackrel{\text{vl. } \overline{z}}{=} \overline{\alpha} \overline{\langle z \mid x \rangle} = \overline{\alpha} \langle x \mid z \rangle$$
* K vlastnosti 2:
  $$\langle x \mid \theta \rangle = \langle x \mid 0 \cdot \theta \rangle = 0 \cdot \langle x \mid \theta \rangle = 0$$

---

## 2. Příklady skalárních součinů

* **Standardní skalární součin:** Na $T^n$ definujeme skalární součin předpisem:
  $$x \cdot y := \sum_{j=1}^n \overline{x}_j \cdot y_j = x^T y$$
  kde $x = (x_1, \dots, x_n)$ a $y = (y_1, \dots, y_n)$ z $T^n$.

* **Frobeniův skalární součin:** Na VP matic $T^{m,n}$ definujeme pro matice $A$ s prvky $a_{ij}$ a $B$ s prvky $b_{ij}$ skalární součin:
  $$\langle A \mid B \rangle := \sum_{i=1}^m \sum_{j=1}^n \overline{a}_{ij} \cdot b_{ij}$$

* **Příklad na $\mathbb{R}^2$:** Pro $x = (x_1, x_2)$ a $y = (y_1, y_2)$ lze definovat skalární součin např. jako:
  $$\langle x \mid y \rangle := x^T \begin{pmatrix} 2 & 1 \\ 1 & 1 \end{pmatrix} y = 2x_1y_1 + x_1y_2 + x_2y_1 + x_2y_2$$

---

## 3. Norma vektoru

Buď $V$ VP nad $T = \mathbb{R}$ nebo $\mathbb{C}$. Zobrazení $\| \cdot \| : V \to \mathbb{R}$ nazýváme **norma**, pokud pro $\forall x, y \in V$ a $\alpha \in T$ platí:

1. **Norma je vždy nezáporná:** $\| x \| \geq 0$
2. **Pouze nulový vektor má nulovou normu:** $\| x \| = 0 \iff x = \theta$
3. **Norma je homogenní v absolutní hodnotě:** $\| \alpha x \| = |\alpha| \cdot \| x \|$
4. **Trojúhelníková nerovnost:** $\| x + y \| \leq \| x \| + \| y \|$

Pro $x, y \in V$ číslo $\| x \| \in \mathbb{R}$ nazýváme **velikostí vektoru** $x$ a číslo $d(x, y) := \| x - y \|$ **vzdáleností** vektorů $x$ a $y$.

### Norma indukovaná skalárním součinem
Ve VP $\mathcal{H}$ (v prehilbertově prostoru) se skalárním součinem $\langle \cdot \mid \cdot \rangle$ je definováno zobrazení $\| \cdot \| : \mathcal{H} \to \mathbb{R}$ pro $x \in \mathcal{H}$ jako:
$$\| x \| := \sqrt{\langle x \mid x \rangle}$$
Toto nazýváme **normou indukovanou skalárním součinem**.

### Schwarzova nerovnost
Pro každé $x, y \in \mathcal{H}$ platí Schwarzova nerovnost (skalární součin může být roven nejvýše součinu velikostí jednotlivých vektorů):
$$|\langle x \mid y \rangle| \leq \| x \| \cdot \| y \|$$

---

## 4. Příklady norem

* **Eukleidovská norma:** Pro $x = (x_1, \dots, x_n) \in T^n$ je eukleidovská norma rovna:
  $$\| x \| = \sqrt{\overline{x}_1 x_1 + \dots + \overline{x}_n x_n} = \sqrt{|x_1|^2 + \dots + |x_n|^2}$$

* **$p$-norma:** Na $T^n$ definujeme pro $p \in \langle 1, \infty)$ tzv. $p$-normu předpisem pro $x = (x_1, \dots, x_n) \in T^n$:
  $$\| x \|_p = 
  \begin{cases} 
  (|x_1|^p + \dots + |x_n|^p)^{1/p} & \text{pro } p < \infty, \\ 
  \max \{ |x_1|, \dots, |x_n| \} & \text{pro } p = \infty 
  \end{cases}$$
