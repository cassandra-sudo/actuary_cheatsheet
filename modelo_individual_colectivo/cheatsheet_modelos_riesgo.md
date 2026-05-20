# Cheatsheet: Modelo Individual vs. Modelo Colectivo
### Teoría del Riesgo · Matemáticas Actuariales

---

## 1. Conceptos Fundamentales

**Riesgo** en seguros: variable aleatoria $S$ que representa el monto total de reclamaciones que enfrenta una compañía aseguradora durante un periodo de vigencia.

El problema central en ambos modelos es determinar la **distribución de probabilidad de $S$**, sus momentos y su función generadora de momentos.

| Característica | Modelo Individual | Modelo Colectivo |
|---|---|---|
| Unidad de análisis | Cada póliza $j = 1, \ldots, n$ | El portafolio como un todo |
| Variable de frecuencia | $D_j \sim \text{Ber}(q_j)$ | $N$ con distribución discreta |
| Variable de severidad | $C_j > 0$ (monto si hay reclamo) | $Y_1, Y_2, \ldots$ i.i.d. positivas |
| Reclamación real póliza $j$ | $D_j C_j$ (variable mixta) | — |
| Riesgo agregado | $S = \sum_{j=1}^{n} D_j C_j$ | $S = \sum_{j=1}^{N} Y_j$ |
| Número de asegurados | $n$ fijo y conocido | No determinado (aleatorio) |
| Supuesto clave | $(D_j, C_j)$ independientes; $D_j \perp C_j$ | $N \perp Y_j$; $Y_j$ i.i.d. |
| Ventaja | Heterogeneidad individual | Flexibilidad; más tratable matemáticamente |
| Desventaja | $n$ fijo; convoluciones difíciles | Pierde comportamiento individual |

---

## 2. Modelo Individual

**Definición 1.1.** El monto de reclamaciones agregadas en el modelo individual es:

$$\boxed{S = \sum_{j=1}^{n} D_j C_j}$$

donde $D_j \sim \text{Ber}(q_j)$, $C_j > 0$, todas independientes, $D_j \perp C_j$.

### 2.1 Función de distribución de la reclamación $j$-ésima

**Proposición 1.1.** Con $F_j(x) = P(D_j C_j \leq x)$ y $G_j(x) = P(C_j \leq x)$:

$$
\boxed{
F_j(x)=
\begin{cases}
1 - q_j(1-G_j(x)) & \text{si } x \ge 0 \\
0 & \text{si } x < 0
\end{cases}}
$$

*La distribución de $D_j C_j$ es mixta: masa puntual $(1-q_j)$ en cero y parte continua/discreta de $C_j$ escalada por $q_j$.*

### 2.2 Momentos y FGM del riesgo $S$

**Proposición 1.2.**

| Cantidad | Fórmula |
|---|---|
| Esperanza | $E(S) = \displaystyle\sum_{j=1}^{n} q_j\, E(C_j)$ |
| Varianza | $\text{Var}(S) = \displaystyle\sum_{j=1}^{n} q_j\left[\text{Var}(C_j) + p_j\, E^2(C_j)\right]$ |
| FGM de $D_j C_j$ | $M_{D_j C_j}(t) = 1 - q_j\left(1 - M_{C_j}(t)\right)$ |
| FGM de $S$ | $M_S(t) = \displaystyle\prod_{j=1}^{n} \left[1 - q_j\left(1 - M_{C_j}(t)\right)\right]$ |

### 2.3 Aproximación normal

Cuando $n$ es grande y el portafolio es homogéneo (variables $D_j C_j$ i.i.d. con segundo momento finito):

$$P(S \leq x) \approx \Phi\!\left(\frac{x - E(S)}{\sqrt{\text{Var}(S)}}\right)$$

**Cuidado:** esta aproximación asigna probabilidad positiva en $(-\infty, 0)$, lo cual es incongruente con $S \geq 0$. Es adecuada solo cuando $E(S) - 4\sqrt{\text{Var}(S)} \geq 0$.

---

## 3. Fórmula de De Pril

Método recursivo exacto para calcular $g_x = P(S = x)$ cuando los montos de reclamación son discretos $\{1, 2, \ldots\}$.

### 3.1 De Pril [i] — Portafolio heterogéneo

Sea $n_{ij}$ = número de pólizas con probabilidad de reclamación $q_j$ y monto de reclamación $i$.

$$\boxed{g_x = \frac{1}{x}\sum_{i=1}^{x \wedge I}\sum_{k=1}^{\lfloor x/i \rfloor} g_{x-ik}\, h(i,k), \quad x \geq 1}$$

$$\boxed{g_0 = \prod_{i=1}^{I}\prod_{j=1}^{J}(1-q_j)^{n_{ij}}}$$

donde:

$$h(i,k) = i(-1)^{k-1} \sum_{j=1}^{J} n_{ij}\left(\frac{q_j}{1-q_j}\right)^k$$

### 3.2 De Pril [ii] — Portafolio homogéneo ($X_1, \ldots, X_n$ i.i.d.)

Con $f_j = P(X = j)$ y $f_0 \neq 0$:

$$\boxed{g_0 = (f_0)^n, \qquad g_x = \frac{1}{f_0}\sum_{j=1}^{x}\left[\frac{j(n+1)}{x} - 1\right] f_j\, g_{x-j}, \quad x \geq 1}$$

*Primeros términos:* $g_1 = \binom{n}{1}f_1(f_0)^{n-1}$; $g_2 = \binom{n}{2}(f_1)^2(f_0)^{n-2} + \binom{n}{1}f_2(f_0)^{n-1}$.

---

## 4. Modelo Colectivo

**Definición 1.2.** El monto agregado o riesgo en el modelo colectivo es:

$$\boxed{S = \sum_{j=1}^{N} Y_j, \quad (S = 0 \text{ si } N = 0)}$$

donde $Y_1, Y_2, \ldots$ son i.i.d. positivas, independientes de $N \in \{0, 1, 2, \ldots\}$.

### 4.1 Función de distribución

**Proposición 1.4.** Con $G^{*0}(x) = \mathbf{1}_{[x \geq 0]}$ y $G^{*n}$ la $n$-convolución de $G$:

$$\boxed{F(x) = \sum_{n=0}^{\infty} G^{*n}(x)\, P(N = n)}$$

### 4.2 Momentos y FGM del riesgo $S$

**Proposición 1.5.** Con $\mu_n = E(Y^n)$:

| Cantidad | Fórmula |
|---|---|
| Esperanza | $E(S) = E(N)\, E(Y)$ |
| Segundo momento | $E(S^2) = E(N)\, E(Y^2) + E(N(N-1))\, E^2(Y)$ |
| Varianza | $\text{Var}(S) = \text{Var}(N)\, E^2(Y) + E(N)\, \text{Var}(Y)$ |
| FGM de $S$ | $M_S(t) = M_N\!\left(\ln M_Y(t)\right)$ |

---

## 5. Casos Particulares del Modelo Colectivo

| Modelo | Distribución de $N$ | $E(S)$ | $\text{Var}(S)$ | $M_S(t)$ |
|---|---|---|---|---|
| **Binomial compuesto** | $\text{Bin}(n,p)$ | $np\mu$ | $np(\mu_2 - p\mu^2)$ | $(1 - p + pM_Y(t))^n$ |
| **Bin. negativo compuesto** | $\text{BinNeg}(k,p)$ | $k(\tfrac{1}{p}-1)\mu$ | $k(\tfrac{1}{p}-1)[\mu_2 + (\tfrac{1}{p}-1)\mu^2]$ | $\left(\frac{p}{1-(1-p)M_Y(t)}\right)^k$ |
| **Poisson compuesto** | $\text{Poisson}(\lambda)$ | $\lambda\mu$ | $\lambda\mu_2$ | $\exp[\lambda(M_Y(t)-1)]$ |

*El modelo Poisson compuesto es el más utilizado en la práctica actuarial.*

---

## 6. Modelo Colectivo Poisson Compuesto: Propiedades Clave

### 6.1 Asociado al modelo individual

Dado un modelo individual con $n$ pólizas, se construye el modelo colectivo Poisson compuesto asociado mediante:

$$\lambda = \sum_{j=1}^{n} q_j, \qquad G(x) = \sum_{j=1}^{n} \frac{q_j}{\lambda}\, G_j(x)$$

Se cumple: $E(S^i) = E(S^c)$ pero $\text{Var}(S^i) \leq \text{Var}(S^c)$.

El modelo colectivo Poisson compuesto es el **límite en distribución** del modelo individual cuando $n \to \infty$ y $q_j \to 0$ (pólizas grandes, siniestralidad pequeña).

### 6.2 Propiedad de cierre bajo suma

**Proposición 1.9.** Si $S_1 \sim \text{Poisson comp}(\lambda_1, G_1)$ y $S_2 \sim \text{Poisson comp}(\lambda_2, G_2)$ son independientes, entonces:

$$S_1 + S_2 \sim \text{Poisson comp}\!\left(\lambda_1 + \lambda_2,\; \frac{\lambda_1}{\lambda}\, G_1 + \frac{\lambda_2}{\lambda}\, G_2\right)$$

### 6.3 Clasificación de reclamaciones

Si $S \sim \text{Poisson comp}(\lambda, G)$ y se clasifican los montos en $m$ categorías $A_1, \ldots, A_m$ con $p_k = P(Y \in A_k)$, entonces los recuentos $N_1, \ldots, N_m$ son **independientes** con $N_k \sim \text{Poisson}(\lambda p_k)$.

---

## 7. Ejercicios

**Ejercicio 1 — Momentos en modelo individual.**
Una compañía tiene una cartera de 3 pólizas con los siguientes parámetros:

| Póliza $j$ | $q_j$ | $E(C_j)$ | $E(C_j^2)$ |
|---|---|---|---|
| 1 | 0.10 | 500 | 350,000 |
| 2 | 0.15 | 800 | 800,000 |
| 3 | 0.05 | 1,200 | 2,000,000 |

(a) Calcule $E(S)$ y $\text{Var}(S)$ bajo el modelo individual.
(b) ¿Puede usarse la aproximación normal para estimar $P(S > 400)$? Justifique.

---

**Ejercicio 2 — Fórmula de De Pril [ii].**
Sean $X_1, X_2, X_3, X_4$ variables aleatorias i.i.d. con distribución:

$$P(X = 0) = 0.6, \quad P(X = 1) = 0.3, \quad P(X = 2) = 0.1$$

Sea $S = X_1 + X_2 + X_3 + X_4$.

(a) Calcule $g_0, g_1, g_2, g_3$ usando la fórmula de De Pril [ii].
(b) Verifique $g_1$ calculando directamente $P(S = 1)$ con argumentos combinatorios.

---

**Ejercicio 3 — Modelo colectivo Poisson compuesto.**
El número de reclamaciones $N \sim \text{Poisson}(3)$ y el monto de cada reclamación $Y \sim \text{Exp}(\beta)$ con $E(Y) = 200$ (en miles de pesos).

(a) Obtenga $E(S)$, $\text{Var}(S)$ y $M_S(t)$.
(b) Mediante la propiedad $M_S(t) = \exp[\lambda(M_Y(t) - 1)]$, identifique la distribución de $S$ cuando $N \sim \text{Poisson}(\lambda)$ e $Y \sim \text{Gamma}(\alpha, \beta)$. ¿Sigue $S$ una distribución conocida?

---

**Ejercicio 4 — Comparación individual vs. colectivo.**
Un portafolio homogéneo tiene $n = 200$ pólizas, cada una con probabilidad de reclamación $q = 0.02$ y monto de reclamación $C \sim \text{Exp}(1/1000)$.

(a) Calcule $E(S^i)$ y $\text{Var}(S^i)$ bajo el modelo individual.
(b) Construya el modelo colectivo Poisson compuesto asociado. Determine $\lambda$ y la distribución de $Y$.
(c) Calcule $E(S^c)$ y $\text{Var}(S^c)$. Compare con los valores del inciso (a) y explique la diferencia en varianzas.

---

**Ejercicio 5 — Suma de riesgos y clasificación Poisson.**
Una aseguradora tiene dos líneas de negocio independientes:
- Línea 1: $S_1 \sim \text{Poisson comp}(5, G_1)$ donde $Y^{(1)} \sim \text{Uniforme}(0, 1000)$.
- Línea 2: $S_2 \sim \text{Poisson comp}(3, G_2)$ donde $Y^{(2)} \sim \text{Exp}(1/500)$.

(a) Determine la distribución de $S = S_1 + S_2$.
(b) Si se clasifican las reclamaciones de $S_1$ en dos categorías: "menor a 500" y "mayor o igual a 500", determine las distribuciones marginales del número de reclamaciones en cada categoría y verifique su independencia.
(c) Calcule $E(S)$ y $\text{Var}(S)$.
