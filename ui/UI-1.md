# Lineární zobrazení – Definice a vlastnosti

## 1. Základní definice a pojmy

Nechť $P$ a $Q$ jsou dva vektorové prostory (VP) nad stejným tělesem $T$. Zobrazení $A: P \to Q$ nazveme **lineární**, pokud současně platí:
1. **Aditivita:** $(\forall x, y \in P) (A(x+y) = Ax + Ay)$
2. **Homogenita:** $(\forall \alpha \in T) (\forall x \in P) (A(\alpha x) = \alpha Ax)$

Množinu všech lineárních zobrazení z $P$ do $Q$ značíme $\mathcal{L}(P, Q)$.

### Speciální typy zobrazení:
* **Lineární operátor:** Lineární zobrazení z VP $P$ do stejného VP $P$.
* **Lineární funkcionál:** Lineární zobrazení z VP $P$ do tělesa $T$.
* **Izomorfismus:** Lineární zobrazení, které je bijekce (prosté a na).
* **Identické zobrazení:** $E: V \to V$ je definováno jako $Ex = x$ pro $\forall x \in V$.
* **Souřadnicový izomorfismus:** Nechť soubor $B = (x_1, \dots, x_n)$ je báze VP $V$ nad $T$. Zobrazení $(\cdot)_B: V \to T^n$ definované jako $x \mapsto (x)_B$ pro $x \in V$, kde $(x)_B$ značí souřadnice $x$ vůči $B$.

---

## 2. Vlastnosti lineárních zobrazení

Nechť $A \in \mathcal{L}(P, Q)$ a $P, Q$ jsou VP nad $T$.

* **Obraz nulového vektoru:** $A\theta_P = \theta_Q$
* **Obraz lineárního obalu:** Je-li $M \subseteq P$, potom $A(\langle M \rangle) = \langle A(M) \rangle$. Pro soubor vektorů platí: $A(\langle x_1, \dots, x_n \rangle) = \langle Ax_1, \dots, Ax_n \rangle$.
* **Obrazy a vzory podprostorů:** * Obraz podprostoru je podprostor: $(\forall \tilde{P} \subset\subset P) (A(\tilde{P}) \subset\subset Q)$
  * Vzor podprostoru je podprostor: $(\forall \tilde{Q} \subset\subset Q) (A^{-1}(\tilde{Q}) \subset\subset P)$
* **Skládání zobrazení:** Mějme $A \in \mathcal{L}(P, Q)$ a $B \in \mathcal{L}(Q, R)$. Složení $BA$ je také lineární, tj. $BA \in \mathcal{L}(P, R)$.
* **Inverze:** Je-li $A \in \mathcal{L}(P, Q)$ izomorfismus, potom inverzní zobrazení $A^{-1}$ je také lineární, tj. $A^{-1} \in \mathcal{L}(Q, P)$.

---

## 3. Jádro, hodnost a defekt zobrazení

Nechť $A \in \mathcal{L}(P, Q)$.

* **Jádro zobrazení ($\ker A$):** $\ker A := \{ x \in P \mid Ax = \theta_Q \}$
* **Defekt zobrazení ($d(A)$):** $d(A) := \dim \ker A$
* **Hodnost zobrazení ($h(A)$):** $h(A) := \dim A(P)$
* **Základní omezení hodnosti:** $h(A) \leq \min \{ \dim P, \dim Q \}$

**Věta o dimenzi jádra a obrazu:** $$h(A) + d(A) = \dim P$$

### Injektivita a Surjektivita (pro $\dim P, \dim Q < \infty$):
1. **Zobrazení $A$ je injektivní (prosté):** $\iff \ker A = \{\theta_P\} \iff d(A) = 0 \iff h(A) = \dim P$
2. **Zobrazení $A$ je surjektivní (na):** $\iff A(P) = Q \iff \dim A(P) = \dim Q \iff h(A) = \dim Q$

**Důsledek:** Pokud $\dim P = \dim Q < \infty$, pak $A$ je injektivní $\iff A$ je surjektivní.

---

## 4. Lineární závislost (LZ) a nezávislost (LN)

Pro obecné lineární zobrazení platí:
* Obraz LZ souboru je LZ soubor.
* Vzor LN souboru je LN soubor.

**Pro prosté (injektivní) zobrazení $A$ navíc platí:**
* Obraz LN souboru je LN soubor.
* Vzor LZ souboru je LZ soubor.
* Důsledek pro soubor $(x_1, \dots, x_n)$: $(x_1, \dots, x_n) \text{ je LN } \iff (Ax_1, \dots, Ax_n) \text{ je LN}$.
* Zachování dimenze: $A \text{ je prosté } \implies \dim A(M) = \dim M$. (Obecně platí jen $\dim A(M) \leq \dim M$).

**Řešení rovnice $Ax = b$:**
Nechť $A \in \mathcal{L}(P, Q)$, $b \in Q$. Existuje-li vektor $\tilde{x} \in P$ takový, že $A\tilde{x} = b$, pak množina všech řešení (vzor) je dána jako:
$$A^{-1}b = \tilde{x} + \ker A$$

---

## 5. Matice lineárního zobrazení

Nechť $A \in \mathcal{L}(P, Q)$, $\mathcal{X} = (x_1, \dots, x_n)$ je báze $P$ a $\mathcal{Y} = (y_1, \dots, y_m)$ je báze $Q$.

**Definice matice zobrazení $^\mathcal{X}A^\mathcal{Y} \in T^{m, n}$:**
$$(^\mathcal{X}A^\mathcal{Y})_{:, i} := (Ax_i)_\mathcal{Y}$$
$$^\mathcal{X}A^\mathcal{Y} = \left( (Ax_1)_\mathcal{Y} \dots (Ax_n)_\mathcal{Y} \right)$$
*(Pozn.: $^\mathcal{X}A^\mathcal{X} = ^\mathcal{X}A$)*

**Vlastnosti matic zobrazení:**
* **Hodnost:** $h(A) = h(^\mathcal{X}A^\mathcal{Y})$
* **Skládání zobrazení:** Pro $A \in \mathcal{L}(Q, V)$ a $B \in \mathcal{L}(P, Q)$ s bázemi $\mathcal{X}$ (pro $P$), $\mathcal{Y}$ (pro $Q$), $\mathcal{W}$ (pro $V$) platí:
  $$^\mathcal{X}(AB)^\mathcal{W} = ^\mathcal{Y}A^\mathcal{W} \cdot ^\mathcal{X}B^\mathcal{Y}$$
* **Inverzní matice:** Je-li $A$ izomorfismus, pak je matice $^\mathcal{X}A^\mathcal{Y}$ regulární a platí:
  $$(^\mathcal{X}A^\mathcal{Y})^{-1} = ^\mathcal{Y}(A^{-1})^\mathcal{X}$$

---

## 6. Matice přechodu a změna báze

Nechť $\mathcal{X} = (x_1, \dots, x_n)$ a $\mathcal{Y} = (y_1, \dots, y_n)$ jsou báze prostoru $P$. 

**Matice přechodu** od báze $\mathcal{X}$ k bázi $\mathcal{Y}$ je matice identického operátoru $^\mathcal{X}E^\mathcal{Y} \in T^{n, n}$:
$$^\mathcal{X}E^\mathcal{Y} = \left( (x_1)_\mathcal{Y} \ (x_2)_\mathcal{Y} \ \dots \ (x_n)_\mathcal{Y} \right)$$

**Vlastnosti matice přechodu:**
1. Přepočet souřadnic: Pro $\forall x \in P$ platí $^\mathcal{X}E^\mathcal{Y} (x)_\mathcal{X} = (x)_\mathcal{Y}$.
2. Matice $^\mathcal{X}E^\mathcal{Y}$ je regulární.
3. Inverze: $(^\mathcal{X}E^\mathcal{Y})^{-1} = ^\mathcal{Y}E^\mathcal{X}$
4. Skládání (tranzitivita): $^\mathcal{Y}E^\mathcal{Z} \cdot ^\mathcal{X}E^\mathcal{Y} = ^\mathcal{X}E^\mathcal{Z}$

**Věta o změně báze pro matici zobrazení:**
Nechť $A \in \mathcal{L}(P, Q)$, $\mathcal{X}, \tilde{\mathcal{X}}$ jsou báze $P$ a $\mathcal{Y}, \tilde{\mathcal{Y}}$ jsou báze $Q$. Potom platí:
$$^{\tilde{\mathcal{X}}}A^{\tilde{\mathcal{Y}}} = ^\mathcal{Y}E^{\tilde{\mathcal{Y}}} \cdot ^\mathcal{X}A^\mathcal{Y} \cdot ^{\tilde{\mathcal{X}}}E^\mathcal{X}$$
