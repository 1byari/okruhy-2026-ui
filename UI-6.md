# QR algoritmus pro výpočet vlastních čísel

## 1. Vlastní čísla a spektrální rozklad

❗️ Číslo $\lambda \in \mathbb{C}$ nazýváme **vlastním číslem matice** $A \in \mathbb{R}^{n,n} \iff \exists x \neq \theta \in \mathbb{C}^n$ splňující:
$$Ax = \lambda x$$
* $x$ — **vlastní vektor** matice $A$ příslušející vlastnímu číslu $\lambda$.
* Množina všech vlastních čísel $A$ se nazývá **spektrum matice** $A$, značíme $\sigma(A)$.

**Diagonalizace** čtvercové matice $A$, také známa jako **spektrální rozklad**:
$$A = P D P^{-1}$$
kde $D \in \mathbb{C}^{n,n}$ je diagonální matice a $P \in \mathbb{C}^{n,n}$ je regulární matice.

**[FYI]** Mějme matici $A \in \mathbb{R}^{n,n}$. Potom následující tvrzení jsou ekvivalentní:
* Matice $A$ je diagonalizovatelná.
* Součet geometrických násobností všech vlastních čísel je $n$.
* Existuje báze prostoru $\mathbb{C}^n$ složená z vlastních vektorů matice $A$.

> **Doplněk (Mocninná metoda):** Vezmu vektor $x$ a postupně jej násobím maticí $A$ zleva a normalizuji. Výsledek konverguje k vlastnímu vektoru, pak příslušné vlastní číslo se vypočítá jako Rayleighův podíl.

---

## 2. Základní podoba QR algoritmu

❗️ **QR algoritmus pro výpočet vlastních čísel:**

    A^{(0)} = A
    for k = 1, 2, ... do
        Q^{(k)} R^{(k)} = A^{(k-1)}   // QR rozklad
        A^{(k)} = R^{(k)} Q^{(k)}     // Vynásobení v opačném pořadí
    end for


❗️ Mějme čtvercovou matici $A \in \mathbb{R}^{n,n}$ a matice $A^{(k)}$ ($k=1,2,\dots$) konstruované QR algoritmem. **Matice $A^{(k)}$ mají stejná vlastní čísla jako matice $A$.**

**Důkaz (pro $A^{(0)} = A$ a $k=1$):**
$$Q^{(1)} R^{(1)} = A^{(0)}$$
Protože $Q^{(1)}$ je ortogonální (OG) matice, platí $(Q^{(1)})^{-1} = (Q^{(1)})^T$. Můžeme tedy vyjádřit $R^{(1)}$:
$$R^{(1)} = (Q^{(1)})^T A^{(0)}$$
Dosadíme do druhého kroku algoritmu:
$$A^{(1)} = R^{(1)} Q^{(1)} = (Q^{(1)})^T A^{(0)} Q^{(1)}$$
Tento vztah odpovídá **podobnostní transformaci**. Matice $A^{(1)}$ a $A^{(0)}$ jsou si podobné, a z věty o podobnosti matic plyne, že mají stejná vlastní čísla. Analogicky se to odvodí pro kroky $k-1$ a $k$.

**Konvergence:**
Matice $A^{(0)}, A^{(1)}, A^{(2)}, \dots$ konvergují k nějaké matici $B$.
* Pokud je $B$ diagonální (či horní trojúhelníková), tak její vlastní čísla budou přímo na diagonále.
* Pokud čísla pod diagonálou budou blízko k nule, můžeme algoritmus zastavit a získat dobrou aproximaci.

---

## 3. Zefektivnění QR algoritmu (Hessenbergův tvar)

❗️ V základní podobě by byl QR algoritmus značně výpočetně drahý. 
Chceme nejprve nalézt novou matici takovou, že má stejná vlastní čísla, ale QR algoritmus na ní vyžaduje méně operací násobení (aby matice $A^{(k)}, Q^{(k)}, R^{(k)}$ měly co nejvíce nul).

**Postup předzpracování matice:**
Najdeme Householderův reflektor tak, že v prvním sloupci zachová první prvek, druhý změní a ostatní vynuluje:
$$Q_1 = \begin{pmatrix} 1 & 0 \\ 0 & H_1 \end{pmatrix}$$
Potom po podobnostní transformaci dostaneme:
$$Q_1 A Q_1 = \begin{pmatrix} * & * & * & * \\ \alpha_1 & * & * & * \\ 0 & * & * & * \\ 0 & * & * & * \end{pmatrix}$$
Potom vytvoříme další transformační matici $Q_2$. Dostáváme:
$$Q_2 Q_1 A Q_1 Q_2 = \begin{pmatrix} * & * & * & * \\ \alpha_1 & * & * & * \\ 0 & \alpha_2 & * & * \\ 0 & 0 & * & * \end{pmatrix}$$
Tímto postupem dospějeme k matici v tzv. **Hessenbergově tvaru**.

---

## 4. Hessenbergův a tridiagonální tvar

❗️ O matici $A \in \mathbb{R}^{n,n}$ řekneme, že je **v Hessenbergově tvaru**, platí-li pro indexy $i, j \in \widehat{n}$:
$$j+1 < i \implies a_{ij} = 0$$
*(Všechny prvky pod první poddiagonálou jsou nulové).*
Pokud by byla matice $A$ symetrická, měla by nuly i „nad naddiagonálou“.

❗️ Matice $A \in \mathbb{R}^{n,n}$ je **tridiagonální**, platí-li pro $i, j \in \widehat{n}$:
$$|i - j| > 1 \implies a_{ij} = 0$$

### Shrnutí efektivního postupu:
1. Převedeme vstupní matici do Hessenbergova tvaru (případně symetrickou matici do tridiagonálního tvaru). Tento krok zachovává vlastní čísla.
2. Teprve na tento zredukovaný tvar aplikujeme samotný QR algoritmus.