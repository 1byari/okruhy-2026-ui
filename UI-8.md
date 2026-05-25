# Rozhodovací stromy, náhodné lesy, AdaBoost

## 1. Rozhodovací stromy

**Rozhodovací strom** je model, který data klasifikuje (nebo predikuje hodnotu)
postupným větvením podle hodnot příznaků. V každém vnitřním uzlu se testuje
jeden příznak, větve odpovídají možným odpovědím a listy obsahují predikci.

### Výhody
* Nenáročnost na přípravu dat.
* Jednoduché a srozumitelné.
* Rychlé učení.
* Dobře interpretovatelné.

### Nevýhody
* **Nerobustní:** Drobná změna v trénovacích datech může způsobit zásadní změnu struktury stromu.
* Většina implementací podporuje pouze binární stromy.
* Najít optimální strom je **NP-complete** problém.
* **Snadno se přeučí** (overfitting).

---

## 2. Konstrukce stromu

* **Vstup:** $N$-řádková tabulka s hodnotami pro binární proměnnou a $p$ binárních příznaků $X_0, \dots, X_{p-1}$.
* Pro konstrukci stromu využíváme **ID3 algoritmus** (případně C4.5, C5).

**Algoritmus ID3:**
1. Je **greedy** (hladový).
2. Vybírá jeden příznak, který rozdělí data na 2 části tak, že vzniklé rozdělení maximalizuje vybrané kritérium (rekurzivně se opakuje pro zbylé příznaky).
3. **Zastavovací kritérium rekurze:** max. hloubka, vyčerpání příznaků apod.
4. **Kritérium pro větvení:** Entropie nebo Gini index.

---

## 3. Kritéria pro větvení

### Entropie
Je to míra neuspořádanosti.
* **Binární případ** (množina nul a jedniček, poměr počtu $0/1$ je $p_0+p_1=1$):
  $$H(D) = -p_0 \log p_0 - p_1 \log p_1 = -p_0 \log p_0 - (1-p_0) \log(1-p_0)$$
* **Nebinární případ** (s $k$ různými hodnotami):
  $$H(D) = -\sum_{i=0}^{k-1} p_i \log p_i$$

*(Pozn.: Základ logaritmu se obvykle volí $2$ – entropie pak vyjde v bitech. Volba základu výsledek dělení nemění, jen ho škáluje.)*

**Informační zisk (IG):**
Spočítáme $IG$ pro všechny $X_i$ (všechny příznaky) a rozdělíme strom podle příznaku s nejvyšším $IG$.
$$IG(D, X_i) = H(D) - t_0 H(D_0) - t_1 H(D_1)$$
*(Kde $D_0, D_1$ jsou podmnožiny $D$ po rozdělení příznakem $X_i$, a $t_i = \frac{|D_i|}{|D|}$ je podíl počtu prvků v $D_i$ a $D$).*

### Gini index
Alternativa k entropii, má podobné vlastnosti.
$$GI(D) = 1 - \sum_{i=0}^{k-1} p_i^2 = \sum_{i=0}^{k-1} p_i(1-p_i)$$

---

## 4. Různé typy příznaků

* **Nebinární (nominální) příznaky:** Používá se **one-hot encoding**.
  * *Nevýhody:* Zvyšuje dimenzi datasetu, může se projevit na kvalitě modelu, vhodné jen pro nominální příznaky (mezi kategoriemi není žádný vztah).
* **Spojité příznaky:**
  * Rozdělení dat na 2 skupiny pomocí operace $<$.
  * Je nutné najít číslo, se kterým porovnáváme.
  * Lze použít vícekrát (např. nejdříve `věk < 30`, potom `věk < 15` atd.).

---

## 5. Hyperparametry stromu

* `max_depth` – hloubka stromu.
* `criterion` – kritérium větvení (gini, entropie, ...).
* `min_samples_split` – min. počet prvků potřebných pro rozdělení uzlu.
* `min_samples_leaf` – min. počet prvků potřebných, aby uzel byl listem.
* `max_features` – počet příznaků uvažovaných pro každý split.

---

## 6. Regrese pomocí rozhodovacího stromu

* Hodnotu vysvětlované proměnné spočítáme jako **průměr / medián** hodnot listů (průměr je citlivý na outliery).
* Namísto ID3 se využívá **CART**:
  * Maximalizuje se / minimalizuje chybová funkce: $MSE(D) - t_L MSE(D_L) - t_R MSE(D_R)$
* **Metriky:**
  * $MSE(y) = \frac{1}{N} \sum_{i=0}^{N-1} (y_i - \bar{y})^2$
  * $MAE(y) = \frac{1}{N} \sum_{i=0}^{N-1} |y_i - \bar{y}|$ *(Pozor: MAE nejde derivovat.)*

---

## 7. Ensemble metody (Bagging, Boosting, náhodné lesy)

**Základní myšlenka:** Namísto jednoho modelu (např. Decision Tree) použijeme více modelů a jejich predikce nějak zkombinujeme do finálního rozhodnutí.

❗️ U ensemble metod je důležité, aby jednotlivé modely **nebyly stejné**, ale naopak co nejpestřejší.

Dva nejobvyklejší způsoby: **Bagging** a **Boosting**.

### Bagging a náhodné lesy
1. Ze vstupního datasetu $D$ vytvoříme $n$ datasetů $D_1, \dots, D_n$ (obvykle stejně velkých jako $D$) pomocí **bootstrap** (výběr s opakováním).
2. Na každém datasetu $D_i$ naučíme rozhodovací strom $T_i$.
   * *Je doporučeno použít spíše menší hloubku (`max_depth`).*
3. Každý datový bod proženeme všemi stromy $T_1, \dots, T_n$ a od každého uložíme rozhodnutí $y_1, \dots, y_n$.
4. **Klasifikace:** Finální rozhodnutí dostaneme pomocí **majority vote** (jakého rozhodnutí $0/1$ je více).
5. **Regrese:** Predikce se bere jako **průměr** z predikcí všech stromů.

**Hyperparametry lesa:**
* `n_estimators` – počet stromů v lese.
* `max_depth` – u lesa je často lepší použít nízkou hodnotu.
* `max_features` – počet náhodně vybraných příznaků pro větvení.

---

## 8. Boosting a AdaBoost

Stromy se konstruují **postupně** – každý další se učí na datech, kde mají vyšší váhu body, které předchozí klasifikoval špatně.

❗️ **Algoritmus AdaBoost (binární klasifikace):**

    w_i = 1/N                                    // počáteční váhy
    for m = 1, 2, ..., n_estimators do
        T^(m) = naučit strom na datech s váhami w_i
        e^(m) = součet vah špatně klasifikovaných bodů
        if e^(m) == 0: break
        alpha^(m) = learning_rate * log((1 - e^(m)) / e^(m))
        for každý špatně klasifikovaný bod i:
            w_i = w_i * exp(alpha^(m))
        normalizuj w tak, aby sum(w) == 1
    end for

**Rozhodnutí modelu pro datový bod $x$:**
* Výsledkem je `n_estimators` rozhodovacích stromů $T^{(1)}, T^{(2)}, \dots$
* Každému stromu $T^{(m)}$ přiřadíme váhu $\alpha^{(m)}$.
* Sečteme váhy všech stromů, které predikují $y=1$, a to samé pro $y=0$.
* Rozhodnutí bude pro tu možnost, pro kterou je vyšší součet vah.

**Poznámky k AdaBoost:**
* Verze pro více než binární klasifikaci se nazývá *AdaBoost-SAMME*.
* Nemusí nutně používat rozhodovací stromy, lze použít jakýkoli model s parametrem `sample_weight`.
* Ve `sklearn` je výchozí volba strom s hloubkou 1 (**stump**).
* Nižší `learning_rate` znamená větší odolnost vůči přeučení (regularizace), ale obvykle je nutné zvýšit počet stromů (`n_estimators`).
