# Metrika – definice a příklady. Metoda nejbližších sousedů (kNN). Aglomerativní hierarchické shlukování. Evaluace modelů.

Hlavní výzvou strojového učení je **schopnost generalizace** – aby natrénovaný
model dobře fungoval i na nových vstupech, které nikdy neviděl. Mnoho metod
(kNN, shlukování) přitom potřebuje měřit *vzdálenost* mezi datovými body.
Začneme proto metrikou, pak se podíváme na kNN, aglomerativní shlukování a
nakonec na metriky pro evaluaci modelů.

---

## 1. Metrika (vzdálenost)

Buď $M$ libovolná množina. Zobrazení $d : M \times M \to \mathbb{R}$ nazýváme **metrikou**, pokud pro $\forall x, y, z \in M$ platí:

1. **Pozitivní definitnost:** $d(x,y) \ge 0 \ \land \ (d(x,y) = 0 \iff x = y)$
2. **Symetrie:** $d(x,y) = d(y,x)$
3. **Trojúhelníková nerovnost:** $d(x,z) \le d(x,y) + d(y,z)$

### Příklady metrik na $\mathbb{R}^p$
**Minkowského vzdálenost** (obecná $p$-norma jako metrika; parametr značíme $r$, abychom se vyhnuli kolizi s počtem sousedů $k$ v kNN):
$$L_r(x, y) = \| x - y \|_r = \sqrt[r]{\sum_{i=1}^{p} |x_i - y_i|^r}$$

Speciální případy:
* $r = 2 \implies$ **Euklidovská vzdálenost**.
* $r = 1 \implies$ **Manhattanská vzdálenost** (City-block).
* $r \to \infty \implies$ **Čebyševova vzdálenost** $\max_i |x_i - y_i|$.

Pro nominální (neuspořádané) hodnoty se používá **diskrétní metrika** $d(x,y) = 0$ pokud $x = y$, jinak $1$.

---

## 2. Metoda nejbližších sousedů (kNN – k-Nearest Neighbors)

Chceme predikovat hodnotu vysvětlované proměnné pro datový bod $x \in \mathbb{R}^p$.
* V trénovacích datech najdeme $k$ nejbližších bodů k bodu $x$ (podle zvolené metriky).
* **Důležité:** U kNN ve skutečnosti **učení neprobíhá** (lazy learning). Trénovací data jsou sama o sobě modelem. O to více je pak výpočetně náročná samotná predikce.

### Klasifikace vs. regrese u kNN
* **Regrese:** jako výsledek bereme **průměr / medián** hodnot nejbližších sousedů.
* **Klasifikace:** bereme nejčastější hodnotu (**majority vote**).

### Hyperparametry kNN
* **$k$** – počet nejbližších sousedů. Čím větší $k$, tím menší šance na přeučení.
* **Volba metriky** – nejčastěji Minkowského (Euklidovská / Manhattanská).
* **Váhy nejbližších sousedů** – např. bližší soused má větší vliv.
* **Předzpracování příznaků – normalizace / standardizace:**
  * **Normalizace:** $x_i \leftarrow \frac{x_i - \min x}{\max x - \min x}$
    *(Této metodě škodí outlieři a zahazujeme informaci o absolutních vztazích mezi příznaky – např. že jeden je 2× větší než druhý.)*
  * **Standardizace:** $x_i \leftarrow \frac{x_i - \bar{x}}{S_x}$ (kde $\bar{x}$ je výběrový průměr a $S_x$ výběrový rozptyl).
    *(Také citlivá na outliery.)*
* **Nominální příznaky:** kNN neumí dobře pracovat s neuspořádanými kategoriemi. Lze použít diskrétní metriku (zda se rovnají) nebo one-hot encoding.

### Prokletí dimenzionality (Curse of dimensionality)
Pojem odkazující na problémy objevující se v případě vysokého počtu příznaků (mnohadimenzionálního prostoru). U kNN se projevují tyto efekty:
1. **Řídnutí dat:** Se zvyšováním dimenzí exponenciálně řídnou a navzájem se vzdalují datové body. (Pro zachování stejné hustoty by bylo nutné masivně navýšit počet bodů.)
2. **Zmenšování relativních rozdílů:** S rostoucí dimenzí se pro klasické metriky zmenšují relativní rozdíly mezi vzdálenými a blízkými body.
3. **„Všichni sousedé daleko":** Ve velké dimenzi pak zprůměrováním sousedů nedostaneme dobrý odhad.

---

## 3. Aglomerativní hierarchické shlukování (Clustering)

Algoritmus „zdola nahoru" (bottom-up).

* Máme $N$ datových bodů.
* Na začátku každý datový bod považujeme za samostatný shluk (tj. máme $N$ shluků).
* **Krok:** Najdeme 2 shluky, které jsou k sobě nejblíž, a ty spojíme do jednoho.
* Krok opakujeme, dokud:
  * nemáme požadovaný počet $k$ shluků,
  * nebo není překročena limitní hodnota vzdálenosti pro spojení,
  * nebo nám nezbude jeden velký shluk.

### Měření vzdálenosti shluků (Linkage metody)
* **Metoda nejbližšího souseda (Single linkage):** $D(A,B) = \min_{x \in A, y \in B} d(x,y)$
  *Generuje shluky jako dlouhé řetězce.*
* **Metoda nejvzdálenějšího souseda (Complete linkage):** $D(A,B) = \max_{x \in A, y \in B} d(x,y)$
  *Tvoří kompaktní shluky.*
* **Párová vzdálenost (Average linkage):** $D(A,B) = \frac{1}{|A||B|} \sum_{x \in A, y \in B} d(x,y)$
  *Kompromis předchozích dvou.*
* **Wardova metoda** (v $\mathbb{R}^p$): minimalizuje nárůst vnitřního rozptylu shluků. Velmi účinná metoda.

### Dendrogram
* Grafická vizualizace procesu hierarchického shlukování.
* Strom, kde listy jsou počáteční jednoprvkové shluky a kořen reprezentuje finální shluk všech bodů.
* Výška, ve které se v grafu uzly (shluky) spojují, odpovídá vzdálenosti těchto shluků. Umožňuje vizuálně odhadnout ideální počet shluků.

**Výhody:** Získání dendrogramu, hierarchická struktura (při změně počtu shluků se pouze dělí existující, struktura se nemění celá).
**Nevýhody:** Výpočetně velmi náročné kvůli velkému počtu porovnání – $O(N^3)$, v nejlepším případě $O(N^2)$ pro single/complete linkage.

---

## 4. Evaluace modelů

Kvalitu modelu měříme různými metrikami; volba metriky závisí na úloze (regrese vs. klasifikace) a na charakteru dat.

### Evaluace regrese
* **MSE (Mean Squared Error):** $MSE = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2$ — penalizuje velké odchylky, velmi citlivé na outliery.
* **RMSE:** $\sqrt{MSE}$ — ve stejných jednotkách jako $y$.
* **RMSLE:** $\sqrt{\frac{1}{N} \sum (\log y_i - \log \hat{y}_i)^2}$ — relativní míra odchylek, jen pro nezáporné hodnoty.
* **MAE (Mean Absolute Error):** $MAE = \frac{1}{N} \sum_{i=1}^{N} |y_i - \hat{y}_i|$
* **$R^2$ (koeficient determinace):** $R^2 = 1 - \frac{RSS}{SST} = 1 - \frac{\sum(y_i - \hat{y}_i)^2}{\sum(y_i - \bar{y})^2}$ — vyjadřuje, jaký podíl variability cílové proměnné model vysvětluje.

### Evaluace klasifikace

**Matice záměn (Confusion matrix)** pro binární klasifikaci s pozitivní třídou $1$:

|                          | **Predikce $\hat{y}=1$** | **Predikce $\hat{y}=0$** |
| ------------------------ | :---------------------: | :---------------------: |
| **Skutečnost $y=1$**     |    **TP** (True Positive)    |   **FN** (False Negative)   |
| **Skutečnost $y=0$**     |   **FP** (False Positive)    |   **TN** (True Negative)    |

Z confusion matrix odvozujeme:
* **TPR** (True Positive Rate) = **Recall** = **Sensitivity:** $TPR = \frac{TP}{TP + FN}$
* **FPR** (False Positive Rate): $FPR = \frac{FP}{FP + TN}$
* **PPV** (Positive Predictive Value) = **Precision:** $PPV = \frac{TP}{TP + FP}$

**Skalární metriky:**
* **Accuracy (ACC):** $ACC = \frac{TP + TN}{N}$. *(Může být velmi zavádějící u nevyvážených tříd!)*
* **$F_1$ skóre:** $F_1 = 2 \cdot \frac{PPV \cdot TPR}{PPV + TPR}$ — harmonický průměr Precision a Recall, řeší problém nevyvážených tříd.
* **Log Loss:** $L(y, \hat{p}) = -y \log \hat{p} - (1-y) \log(1-\hat{p})$ — pracuje s pravděpodobnostmi $\hat{p}$, ne s tvrdou predikcí.

**ROC křivka a AUC:**
* **ROC křivka** zobrazuje závislost mezi TPR a FPR při změně rozhodovacího prahu (obvykle $0{,}5$, ale lze měnit).
* Pro dobrý model bude graf rychle stoupat k levému hornímu rohu.
* **AUC (Area Under Curve):** Plocha pod ROC křivkou.
  * Úplně náhodný model: $AUC = 0{,}5$
  * Dokonalý model: $AUC = 1{,}0$
  * Většina reálných modelů je mezi $0{,}5$ a $1{,}0$.

---

## 5. Ukázkový příklad: výpočet metrik z confusion matrix

Mějme binární klasifikátor s následující confusion matrix na testovacích datech ($N=100$):

|                  | $\hat{y}=1$ | $\hat{y}=0$ |
| ---------------- | :--------: | :--------: |
| $y=1$            |   $TP=40$  |   $FN=10$  |
| $y=0$            |   $FP=5$   |   $TN=45$  |

Výpočty:
1. **Accuracy:** $ACC = \frac{40 + 45}{100} = 0{,}85$
2. **Precision (PPV):** $PPV = \frac{40}{40 + 5} = \frac{40}{45} \approx 0{,}889$
3. **Recall (TPR):** $TPR = \frac{40}{40 + 10} = \frac{40}{50} = 0{,}8$
4. **$F_1$:** $F_1 = 2 \cdot \frac{0{,}889 \cdot 0{,}8}{0{,}889 + 0{,}8} \approx 0{,}842$
5. **FPR:** $FPR = \frac{5}{5 + 45} = 0{,}1$

Model tedy správně označí 80 % pozitivních případů (Recall), z označených jako pozitivní je 88,9 % skutečně pozitivních (Precision), a falešně-pozitivních je 10 % z negativních případů.
