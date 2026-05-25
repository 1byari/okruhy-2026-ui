# QR rozklad: výpočet a využití při výpočtu odhadu metodou nejmenších čtverců

## 1. Ortogonální matice a jejich vlastnosti

* **Matice s ortonormálními sloupci:** Matici $Q \in \mathbb{R}^{m,n}$ ($m \ge n$) nazveme maticí s ortonormálními sloupci, pokud platí:
  $$Q^T Q = E_n$$
  kde $E_n$ je jednotková matice z $\mathbb{R}^{n,n}$. Neboli pro sloupce $q_i, q_j$ platí $q_i^T q_j = 0$ pro $i \neq j$ a $q_i^T q_i = 1$.
* **Ortogonální matice:** Čtvercovou matici $Q \in \mathbb{R}^{n,n}$ nazýváme ortogonální (OG), pokud platí:
  $$Q^T = Q^{-1}$$

**Vlastnosti ortogonálních matic ($Q \in \mathbb{R}^{n,n}$):**
1. Je-li $Q$ OG matice, je její transpozice $Q^T$ také OG matice: 
   $$QQ^T = (Q^T)^T Q^T = Q^T Q = Q^T (Q^T)^T = E \implies (Q^T)^{-1} = (Q^T)^T$$
2. Součin OG matic $Q_1, Q_2, \dots, Q_k \in \mathbb{R}^{n,n}$ je opět OG matice:
   $$(Q_1 Q_2 \dots Q_k)^T (Q_1 Q_2 \dots Q_k) = E$$
3. Zachování skalárního součinu (úhlů): Pro každé $x, y \in \mathbb{R}^n$ platí:
   $$(Qx)^T(Qy) = x^T y$$
4. Zachování normy (délky): Pro každý vektor $x \in \mathbb{R}^n$ platí:
   $$\|Qx\|_2 = \|x\|_2$$
5. Determinant OG matice:
   $$\det Q = \pm 1$$
   *(Důkaz: $\det(Q^TQ) = \det(E) = 1 \implies \det(Q^T)\det(Q) = (\det(Q))^2 = 1$)*
6. Vlastní čísla: Pro každé vlastní číslo $\lambda$ OG matice platí $|\lambda| = 1$.
   *(Důkaz aplikací normy: $Qx = \lambda x \implies \|Qx\|_2 = \|\lambda x\|_2 \implies \|x\|_2 = |\lambda|\|x\|_2 \implies 1 = |\lambda|$)*

---

## 2. Definice QR rozkladu

Mějme $m \ge n$ a matici $A \in \mathbb{R}^{m,n}$.

### Úplný QR rozklad
Zápis jako součin:
$$A = QR$$
kde $Q \in \mathbb{R}^{m,m}$ je ortogonální matice a $R \in \mathbb{R}^{m,n}$ je horní trojúhelníková matice.

### Redukovaný QR rozklad
Zápis jako součin:
$$A = \hat{Q}\hat{R}$$
kde $\hat{Q} \in \mathbb{R}^{m,n}$ je matice s ortonormálními sloupci a $\hat{R} \in \mathbb{R}^{n,n}$ je horní trojúhelníková matice.

**Souvislost úplného a redukovaného rozkladu:**
$$A = \left( q_1 \mid q_2 \mid \dots \mid q_n \mid q_{n+1} \mid \dots \mid q_m \right) 
\begin{pmatrix} 
r_{11} & r_{12} & \dots & r_{1n} \\ 
0 & r_{22} & \ddots & \vdots \\ 
\vdots & \ddots & \ddots & r_{nn} \\
0 & \dots & \dots & 0
\end{pmatrix} 
= \hat{Q}\hat{R}$$
Vektory $q_{n+1}, \dots, q_m$ (doplnění do úplné OG matice $Q$) hledáme jako ortonormální bázi ortogonálního doplňku $\langle q_1, \dots, q_n \rangle^\perp$.
Platí, že hodnost matice se zachovává: $h(A) = h(\hat{R})$.

---

## 3. Výpočet QR rozkladu

### A. Pomocí Gramovy-Schmidtovy ortogonalizace
Získáme přímo prvky redukovaného rozkladu $A = \hat{Q}\hat{R}$:
$$( a_1 \mid a_2 \mid \dots \mid a_n ) = ( q_1 \mid q_2 \mid \dots \mid q_n ) 
\begin{pmatrix} 
r_{11} & r_{12} & \dots & r_{1n} \\ 
0 & r_{22} & \dots & r_{2n} \\ 
\vdots & \ddots & \ddots & \vdots \\
0 & \dots & 0 & r_{nn}
\end{pmatrix}$$
* $a_1 = r_{11}q_1 \implies q_1 = \frac{1}{r_{11}} a_1$
* $a_2 = r_{12}q_1 + r_{22}q_2 \implies q_2 = \frac{1}{r_{22}} (a_2 - r_{12}q_1)$
* Obecně pro koeficienty $R$: $r_{ij} = q_i^T a_j$ a na diagonále $r_{ii} = \|a_i - r_{1i}q_1 - \dots - r_{i-1,i}q_{i-1}\|_2$.

### B. Pomocí Householderových reflektorů
Matice odrazu (zrcadlení) podle nadroviny kolmé k vektoru $v$:
$$H x = x - 2 \text{proj}_v(x) = x - 2 \frac{v^T x}{\|v\|^2} v = \left( E - 2 \frac{v v^T}{\|v\|^2} \right) x$$
Householderův reflektor je dán maticí: $H = E - 2 \frac{v v^T}{\|v\|^2}$.
* Postupným násobením matice $A$ zleva reflektory $Q_1, Q_2, \dots, Q_k$ nulujeme prvky pod diagonálou.
* Výsledek eliminace: $Q_k \dots Q_2 Q_1 A = R$.
* Celková matice $Q$ se získá jako: $Q = (Q_k \dots Q_1)^{-1} = (Q_k \dots Q_1)^T$.
* *Poznámka ke konstrukci normály:* Abychom vektor $x$ překlopili do směru osy $e_1$, volíme normálu transformace tak, aby reflektovaný vektor měl pouze první složku nenulovou.

### C. Pomocí Givensovy rotace
Vždy pracuje pouze se dvěma řádky (vhodnější pro "řídké" matice s mnoha nulami, i když obecně vyžaduje více operací pro husté matice).
* Postupně se vytváří jedna nula pomocí matice rotace o úhel $\varphi$:
  $$\begin{pmatrix} \cos\varphi & \sin\varphi \\ -\sin\varphi & \cos\varphi \end{pmatrix}$$
* Tato matice $2 \times 2$ je symetricky umístěna do jednotkové matice $E$ příslušného rozměru.
* Úhel rotace se nepočítá přímo přes úhlové funkce, ale hodnoty $\cos\varphi$ a $\sin\varphi$ se určí přímo ze složek upravovaného sloupce, aby po aplikaci rotace vznikla požadovaná nula.

---

## 4. Metoda nejmenších čtverců (MNČ / OLS) a využití QR rozkladu

Mějme $A \in \mathbb{R}^{m,n}$ a vektor pravé strany $b \in \mathbb{R}^m$.
Vektor $x \in \mathbb{R}^n$ nazýváme **řešením soustavy $Ax = b$ ve smyslu nejmenších čtverců**, pokud pro $\forall y \in \mathbb{R}^n$ platí:
$$\|b - Ax\| \le \|b - Ay\|$$

**Ekvivalentní definice:** $x \in \mathbb{R}^n$ je řešením $\iff$ je řešením soustavy:
$$Ax = \text{proj}_{\langle a_1, \dots, a_n \rangle} b$$
**Důsledek:** Mějme $A \in \mathbb{R}^{m,n}$, $b \in \mathbb{R}^m$. Potom existuje **právě jedno** řešení soustavy $Ax = b$ ve smyslu MNČ $\iff$ soubor sloupců matice $A$ je lineárně nezávislý ($h(A) = n$).

### Normální rovnice
Vektor $x \in \mathbb{R}^n$ je řešením ve smyslu nejmenších čtverců $\iff$ splňuje tzv. **normální rovnice**:
$$A^T A x = A^T b$$
Pokud má matice $A$ plnou sloupcovou hodnost ($h(A) = n$), matice $A^T A$ je regulární a řešení lze jednoznačně vyjádřit jako:
$$x = (A^T A)^{-1} A^T b$$

*(Statistický zápis: Hledáme váhy $w$, které minimalizují $RSS(w) = \|y - Xw\|^2$. Odtud odhad $\hat{w}_{OLS} = (X^T X)^{-1} X^T y$.)*

### Využití QR rozkladu k řešení MNČ
Normální rovnice mohou být numericky nestabilní. Mnohem výhodnější je použít pro řešení QR rozklad matice $A$:
$$Ax = b$$
Dosadíme úplný rozklad $A = QR$:
$$QRx = b$$
Vynásobíme zleva maticí $Q^T$ (přičemž víme, že $Q^TQ = E$):
$$Rx = Q^T b$$
Získali jsme snadno řešitelnou soustavu rovnic (matice $R$ je horní trojúhelníková, řeší se zpětnou substitucí). V případě redukovaného rozkladu $A = \hat{Q}\hat{R}$ dostáváme přímo rovnici pro přesný výpočet odhadu:
$$\hat{R}x = \hat{Q}^T b$$