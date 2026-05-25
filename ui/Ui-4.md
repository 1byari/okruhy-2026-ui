# LU rozklad matic, řešení soustavy lineárních rovnic pomocí LU rozkladu

## 1. Definice a princip LU rozkladu

Mějme matici $A \in \mathbb{R}^{m,m}$. Pokud existuje **dolní trojúhelníková matice** $L \in \mathbb{R}^{m,m}$ s jedničkami na diagonále a **horní trojúhelníková matice** $U \in \mathbb{R}^{m,m}$ tak, že platí:
$$A = LU$$
nazýváme tento součin **LU rozkladem**.

**Podmínka existence:** Matice má LU rozklad $\iff$ lze ji převést do HST (horního stupňovitého tvaru) pomocí pouze "operace (O3) dolů" (tj. přičítání násobku řádku k řádkům pod ním, bez prohazování řádků).
**Jednoznačnost:** Je-li matice regulární a má-li LU rozklad, potom je tento rozklad jednoznačný.

---

## 2. Odvození matice L (kroky Gaussovy eliminace)

Označme si $k$-tý sloupec matice $X_{k-1}$ (která vzniká z $A$ po $k-1$ GEM krocích) jako $x_k$. V $k$-tém kroku eliminace chceme vynulovat všechny prvky pod diagonálním prvkem $x_{kk}$. Chceme převést $x_k$ na $L_k x_k$ takto:
$$x_k = \begin{pmatrix} x_{1k} \\ \vdots \\ x_{kk} \\ x_{k+1,k} \\ \vdots \\ x_{mk} \end{pmatrix} \sim L_k x_k = \begin{pmatrix} x_{1k} \\ \vdots \\ x_{kk} \\ 0 \\ \vdots \\ 0 \end{pmatrix}$$

K tomu od $j$-tého řádku odečteme $l_{jk}$-násobek $k$-tého řádku, kde:
$$l_{jk} = \frac{x_{jk}}{x_{kk}} \quad \text{pro } (k < j \leq m)$$

Matice $L_k$, reprezentující tento krok, je rovna:
$$L_k = \begin{pmatrix} 1 & & & & & \\ & \ddots & & & & \\ & & 1 & & & \\ & & -l_{k+1,k} & 1 & & \\ & & \vdots & & \ddots & \\ & & -l_{m,k} & & & 1 \end{pmatrix}$$

Pokud si označíme vektor $l_k = (0, \dots, 0, l_{k+1,k}, \dots, l_{m,k})^T$, pak si můžeme zjednodušit zápis:
$$L_k = E - l_k e_k^T$$
Pro inverzní matici platí:
$$L_k^{-1} = E + l_k e_k^T$$
Důkaz: $(E + l_k e_k^T)(E - l_k e_k^T) = E - l_k e_k^T + l_k e_k^T - l_k \underbrace{e_k^T l_k}_{0} e_k^T = E$

Celkový proces (např. pro 3 kroky):
$$L_3 L_2 L_1 A = U \implies A = L_1^{-1} L_2^{-1} L_3^{-1} U \implies L = L_1^{-1} L_2^{-1} L_3^{-1}$$

Výsledná matice $L$ se získá jednoduše složením (sečtením) vektorů $l_k$:
$$L = E + l_1 e_1^T + \dots + l_{m-1} e_{m-1}^T = \begin{pmatrix} 1 & 0 & \dots & 0 \\ l_{21} & 1 & \dots & 0 \\ l_{31} & l_{32} & \ddots & \vdots \\ \vdots & \vdots & \ddots & 0 \\ l_{m1} & l_{m2} & \dots & 1 \end{pmatrix}$$

---

## 3. Příklad výpočtu LU rozkladu

Mějme matici $A \in \mathbb{R}^{4,4}$:
$$A = \begin{pmatrix} 3 & 1 & 1 & 1 \\ 6 & 4 & 4 & 3 \\ 15 & 9 & 10 & 7 \\ 9 & 7 & 5 & 7 \end{pmatrix}$$

**GEM (přímý chod) a ukládání koeficientů do $L$:**
1. Krok (eliminace 1. sloupce): Koeficienty jsou $2, 5, 3$.
   $$\sim \begin{pmatrix} 3 & 1 & 1 & 1 \\ 0 & 2 & 2 & 1 \\ 0 & 4 & 5 & 2 \\ 0 & 4 & 2 & 4 \end{pmatrix}$$
2. Krok (eliminace 2. sloupce): Koeficienty jsou $2, 2$.
   $$\sim \begin{pmatrix} 3 & 1 & 1 & 1 \\ 0 & 2 & 2 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & -2 & 2 \end{pmatrix}$$
3. Krok (eliminace 3. sloupce): Koeficient je $-2$.
   $$\sim \begin{pmatrix} 3 & 1 & 1 & 1 \\ 0 & 2 & 2 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 2 \end{pmatrix} = U$$

**Výsledné matice:**
$$L = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 2 & 1 & 0 & 0 \\ 5 & 2 & 1 & 0 \\ 3 & 2 & -2 & 1 \end{pmatrix}, \quad U = \begin{pmatrix} 3 & 1 & 1 & 1 \\ 0 & 2 & 2 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 2 \end{pmatrix}$$

---

## 4. Řešení soustavy lineárních rovnic pomocí LU rozkladu

Řešme soustavu $m$ rovnic o $m$ neznámých pro regulární matici $A \in \mathbb{R}^{m,m}$ a pravou stranu $b \in \mathbb{R}^m$:
$$Ax = b$$
Využitím rozkladu obdržíme:
$$LUx = b$$
Pokud si označíme $y = Ux$, obdržíme soustavu s neznámou $y \in \mathbb{R}^m$:
$$Ly = b$$

Postup:
1. Vyřešíme $Ly = b$ postupným dosazováním **od prvního řádku k poslednímu** (přímá substituce).
2. Poté vyřešíme $Ux = y$ postupným dosazováním **od posledního řádku k prvnímu** (zpětná substituce).

### Příklad (pokračování)
Pravá strana $b = (2, 7, 15, 16)^T$.

**1. Řešení $Ly = b$:**
$$\begin{pmatrix} 1 & 0 & 0 & 0 \\ 2 & 1 & 0 & 0 \\ 5 & 2 & 1 & 0 \\ 3 & 2 & -2 & 1 \end{pmatrix} y = \begin{pmatrix} 2 \\ 7 \\ 15 \\ 16 \end{pmatrix} \implies y = \begin{pmatrix} 2 \\ 3 \\ -1 \\ 2 \end{pmatrix}$$

**2. Řešení $Ux = y$:**
$$\begin{pmatrix} 3 & 1 & 1 & 1 \\ 0 & 2 & 2 & 1 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 2 \end{pmatrix} x = \begin{pmatrix} 2 \\ 3 \\ -1 \\ 2 \end{pmatrix} \implies x = \begin{pmatrix} 0 \\ 2 \\ -1 \\ 1 \end{pmatrix}$$

---

## 5. LU rozklad s pivotací

Pokud matici nelze rozložit přímo (např. nula na diagonále), využíváme pivotaci. 

* **Permutační matice $P \in \mathbb{R}^{m,m}$:** Nechť $\pi$ je permutace na $\widehat{m}$.
  $$P_{ij} = \begin{cases} 1, & \text{pokud } \pi(i) = j \\ 0, & \text{jinak} \end{cases}$$
* **LU rozklad s částečnou pivotací:** Zápis matice $A \in \mathbb{R}^{m,m}$ jako součin $PA = LU$. Permutujeme řádky tak, aby v $k$-tém kroku byl na pozici $x_{kk}$ v absolutní hodnotě největší prvek z daného zpracovávaného sloupce. Platí, že pro $\forall i > j$ z $\widehat{m}$ je $|L_{ij}| \leq 1$.
* **LU rozklad s úplnou pivotací:** Permutujeme řádky i sloupce, aby na pozici $x_{kk}$ byl v absolutní hodnotě největší prvek z celé nezpracované části matice. Zápis je $PAQ = LU$.

### Příklad na částečnou pivotaci
Mějme matici:
$$A = \begin{pmatrix} 1 & 1 & 2 & 1 \\ -3 & -1 & -2 & 3 \\ 3 & 3 & 3 & 3 \\ 0 & 2 & 3 & 1 \end{pmatrix}$$

V průběhu eliminace dochází k výměnám řádků (označeno šipkami v zápiscích), což se projeví v permutační matici $P$ a upraví podobu matice $L$. Výsledné matice tohoto rozkladu ($PA = LU$):

$$P = \begin{pmatrix} 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 1 & 0 & 0 & 0 \end{pmatrix}, \quad L = \begin{pmatrix} 1 & 0 & 0 & 0 \\ -1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 0 \\ \frac{1}{3} & 0 & \frac{1}{2} & 1 \end{pmatrix}, \quad U = \begin{pmatrix} 3 & 3 & 3 & 3 \\ 0 & 2 & 1 & 6 \\ 0 & 0 & 2 & -5 \\ 0 & 0 & 0 & \frac{5}{2} \end{pmatrix}$$