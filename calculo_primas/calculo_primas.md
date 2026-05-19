# Principios para el Cálculo de Primas
### Matemáticas Actuariales · Capítulo 3

---

## ¿Qué es una prima?

Una **prima** es el pago anticipado que un asegurado entrega a una compañía aseguradora a cambio de cobertura contra un riesgo $S$ durante la vigencia de la póliza. Matemáticamente, la prima es una **función numérica** de la variable aleatoria $S$ —o de su distribución—, y la denotamos $p$, $p_S$ o $p(S)$.

El objetivo de este capítulo es estudiar las distintas formas en que se puede calcular esta función de manera matemáticamente rigurosa, dejando de lado aspectos administrativos o de mercado.

---

## 1. Propiedades deseables de una prima

No toda función $p(S)$ es un buen principio de cálculo de primas. Antes de estudiar métodos concretos, conviene establecer las propiedades que cualquier método razonable debería satisfacer.

| Propiedad | Enunciado formal | Interpretación |
|---|---|---|
| **Simplicidad** | — | La prima debe ser fácil de calcular y comprensible para todas las partes involucradas |
| **Consistencia** | $p(S + c) = p(S) + c$ para $c > 0$ | Si el riesgo crece en una constante, la prima crece en la misma cantidad |
| **Aditividad** | $p(S_1 + S_2) = p(S_1) + p(S_2)$ para $S_1, S_2$ independientes | La prima de un portafolio combinado es la suma de las primas individuales; combinar riesgos no genera ventajas artificiales |
| **Invarianza de escala** | $p(aS) = a\, p(S)$ para $a > 0$ | Si el riesgo cambia de moneda o escala, la prima se ajusta proporcionalmente |
| **Cota inferior** | $p(S) \geq E(S)$ | La prima no puede ser menor a la reclamación promedio (prima pura de riesgo) |
| **Cota superior** | Si $S \leq M$, entonces $p(S) \leq M$ | La prima no puede exceder el máximo posible del riesgo |

---

## 2. La condición de ganancia neta

### ¿Por qué no basta cobrar la prima pura $p = E(S)$?

Considere un portafolio homogéneo de $n$ pólizas del mismo riesgo $S$, con reclamaciones $S_1, \ldots, S_n$ i.i.d., prima $p$ por póliza y capital inicial $u$. Al término de la vigencia, el capital de la aseguradora es:

$$X_n = u + np - \sum_{j=1}^{n} S_j = u + \sum_{j=1}^{n}(p - S_j)$$

**Caso $p = E(S)$:** En promedio $E(X_n) = u$; la aseguradora no gana ni pierde. Sin embargo, puede demostrarse (usando caminatas aleatorias) que casi seguramente:

$$\limsup_{n\to\infty} X_n = +\infty \qquad \text{y} \qquad \liminf_{n\to\infty} X_n = -\infty$$

Esto significa que el capital oscila indefinidamente, pudiendo tomar valores arbitrariamente negativos. La compañía eventualmente **quebrará**.

**Caso $p \neq E(S)$:** Por la Ley de los Grandes Números:

$$\lim_{n\to\infty} \frac{1}{n} X_n = p - E(S)$$

Por lo tanto:

$$\lim_{n\to\infty} X_n = \begin{cases} +\infty & \text{si } p > E(S) \\ -\infty & \text{si } p < E(S) \end{cases}$$

### Conclusión fundamental

> **Condición de ganancia neta** (*net profit condition*): Todo método de cálculo de primas debe satisfacer
> $$\boxed{p > E(S)}$$
> Esta condición es necesaria para la supervivencia financiera de la aseguradora.

---

## 3. Principios de cálculo de primas

### 3.1 Principio del valor esperado

$$\boxed{p = (1 + \theta)\, E(S), \quad \theta > 0}$$

El parámetro $\theta$ se llama **factor de recargo** (*safety loading*) y absorbe costos administrativos, comerciales y márgenes de utilidad. Es el principio más sencillo y siempre satisface la condición de ganancia neta.

**Desventaja importante:** asigna la misma prima a dos riesgos con igual media pero distinta distribución. Si $\text{Var}(S_1) \gg \text{Var}(S_2)$ y $E(S_1) = E(S_2)$, este principio no los distingue.

---

### 3.2 Principio de la varianza

$$\boxed{p = E(S) + \theta\, \text{Var}(S), \quad \theta > 0}$$

Corrige la principal desventaja del principio anterior incorporando la **dispersión del riesgo**. Riesgos más volátiles pagan una prima mayor. Satisface la condición de ganancia neta pues $\text{Var}(S) \geq 0$.

**Desventaja:** las unidades de $\text{Var}(S)$ son el cuadrado de las unidades del riesgo, lo que hace que el recargo $\theta \cdot \text{Var}(S)$ no sea directamente comparable con $E(S)$.

---

### 3.3 Principio de la desviación estándar

$$\boxed{p = E(S) + \theta\, \sqrt{\text{Var}(S)}, \quad \theta > 0}$$

Resuelve el problema de unidades del principio anterior: ahora el recargo tiene las mismas unidades que el riesgo. Siempre se cumple:

$$p_{\text{desv. est.}} \leq p_{\text{varianza}}$$

pues $\sqrt{\text{Var}(S)} \leq \text{Var}(S)$ cuando $\text{Var}(S) \geq 1$ (en unidades monetarias apropiadas, la varianza típicamente supera a la desviación estándar).

---

### 3.4 Principio de utilidad cero

Este principio surge de la **teoría de la utilidad**. Una función de utilidad $v(x)$ modela el valor subjetivo que la aseguradora asocia a una riqueza $x$. Se requiere que $v$ sea:

- estrictamente creciente: $v'(x) > 0$ (más riqueza siempre es preferible)
- cóncava: $v''(x) \leq 0$ (aversión al riesgo: el valor marginal de la riqueza decrece)

**Definición.** La prima bajo este principio es el valor $p$ que satisface:

$$\boxed{v(u) = E[v(u + p - S)]}$$

donde $u$ es el capital inicial de la aseguradora. La ecuación expresa **indiferencia**: la utilidad del capital actual sin seguro debe ser igual a la utilidad esperada del capital después de cobrar $p$ y asumir el riesgo $S$.

**Demostración de que $p \geq E(S)$:** Por la desigualdad de Jensen para funciones cóncavas:
$$v(u) = E[v(u + p - S)] \leq v(E[u + p - S]) = v(u + p - E(S))$$

Como $v$ es estrictamente creciente (y por tanto su inversa también), se concluye $p \geq E(S)$.

**Funciones de utilidad comunes:**

| Nombre | Fórmula | Dominio |
|---|---|---|
| Exponencial | $v(x) = 1 - e^{-\alpha x},\; \alpha > 0$ | $x \geq 0$ |
| Cuadrática | $v(x) = x - \alpha x^2,\; \alpha > 0$ | $0 \leq x \leq \frac{1}{2\alpha}$ |
| Logarítmica | $v(x) = \alpha \ln x,\; \alpha > 0$ | $x > 0$ |
| Potencia fraccional | $v(x) = x^\alpha,\; 0 < \alpha \leq 1$ | $x \geq 0$ |

**Caso especial — Principio exponencial.** Con $v(x) = 1 - e^{-\alpha x}$, la ecuación $v(u) = E[v(u + p - S)]$ se simplifica:

$$1 - e^{-\alpha u} = E\left[1 - e^{-\alpha(u + p - S)}\right]$$

Operando algebraicamente:

$$e^{-\alpha u} = e^{-\alpha(u+p)} E[e^{\alpha S}] = e^{-\alpha(u+p)} M_S(\alpha)$$

$$\Longrightarrow \quad \boxed{p = \frac{1}{\alpha} \ln M_S(\alpha)}$$

Nótese que esta prima **no depende del capital inicial** $u$, lo cual es una propiedad muy conveniente en la práctica.

---

### 3.5 Principio del valor medio

Utiliza una **función de valor** $v(x)$ con propiedades opuestas a la función de utilidad: $v(0) = 0$, estrictamente creciente y estrictamente **convexa** ($v''(x) > 0$).

La prima se define como el valor $p$ que satisface:

$$\boxed{v(p) = E[v(S)]}$$

Equivalentemente, $p = v^{-1}(E[v(S)])$.

La interpretación es que la aseguradora **asigna el mismo valor a cobrar la prima** que al promedio del valor de las reclamaciones. Por la desigualdad de Jensen (ahora con función convexa, aplicada a $v^{-1}$ que es cóncava):

$$p = v^{-1}(E[v(S)]) \geq E[v^{-1}(v(S))] = E(S)$$

**Ejemplo.** Con $v(x) = e^{\alpha x} - 1$, la ecuación $v(p) = E[v(S)]$ da:
$$e^{\alpha p} - 1 = E[e^{\alpha S} - 1] \implies p = \frac{1}{\alpha}\ln M_S(\alpha)$$

Resultado idéntico al principio exponencial: ambos son **el mismo principio**, formulado desde perspectivas distintas (función de utilidad cóncava vs. función de valor convexa).

---

### 3.6 Principio del porcentaje (pérdida máxima)

Sea $\epsilon > 0$ una tolerancia al riesgo. La prima se define como:

$$\boxed{p = \inf\{x > 0 : P(S > x) \leq \epsilon\}}$$

Geométricamente, $p$ es el cuantil $(1-\epsilon)$ de la distribución de $S$: la probabilidad de que el riesgo supere la prima es a lo sumo $\epsilon$.

**Observación crítica.** Este principio **no garantiza** la condición de ganancia neta. Por ejemplo, si $S \sim \text{Exp}(\lambda)$, entonces $P(S > x) = e^{-\lambda x}$, de modo que $p = -\frac{1}{\lambda}\ln\epsilon$. La condición $p > E(S) = \frac{1}{\lambda}$ se cumple si y solo si $-\ln\epsilon > 1$, es decir, $\epsilon < e^{-1} \approx 0.368$. Para valores grandes de $\epsilon$ (tolerancia alta), la prima puede ser insuficiente.

---

### 3.7 Principio de Esscher

Este principio transforma la distribución del riesgo antes de calcular la prima. La **transformada de Esscher** con parámetro $h \geq 0$ de la densidad $f(x)$ es:

$$\boxed{g(x) = \frac{e^{hx} f(x)}{M_S(h)}}$$

Es inmediato verificar que $g$ es una función de densidad válida (integra 1). La transformada pondera la distribución original por $e^{hx}$, asignando mayor probabilidad a valores grandes del riesgo: es una **distorsión conservadora**.

La prima de Esscher es la esperanza bajo esta nueva distribución:

$$\boxed{p(h) = \frac{E(S e^{hS})}{E(e^{hS})}}$$

**Propiedades:** $p(0) = E(S)$ y $p(h)$ es función creciente de $h$ (a mayor $h$, mayor distorsión y mayor prima). Por tanto $p(h) \geq p(0) = E(S)$, y siempre se cumple la condición de ganancia neta.

La función generadora de momentos de la variable transformada $\tilde{S}$ es:

$$M_{\tilde{S}}(t) = \frac{M_S(t + h)}{M_S(h)}$$

---

### 3.8 Principio del riesgo ajustado

Para un riesgo $S$ con función de distribución $F(x)$, se define una nueva distribución:

$$\boxed{G(x) = 1 - (1 - F(x))^{1/\rho}, \quad \rho \geq 1}$$

El parámetro $\rho$ se llama **índice del riesgo**. Como $1 - F(x) \in [0,1]$ y $\rho \geq 1$, se cumple:

$$1 - G(x) = (1-F(x))^{1/\rho} \geq 1 - F(x)$$

La cola de $G$ es mayor que la de $F$: la nueva distribución **sobreestima sistemáticamente** la probabilidad de siniestros grandes. La prima es la esperanza bajo esta distribución distorsionada:

$$\boxed{p = \int_0^\infty (1 - G(x))\, dx = \int_0^\infty (1-F(x))^{1/\rho}\, dx}$$

La condición de ganancia neta se cumple porque $(1-F(x))^{1/\rho} \geq 1 - F(x)$ para todo $x \geq 0$, de modo que:

$$p = \int_0^\infty (1-F(x))^{1/\rho}\, dx \geq \int_0^\infty (1-F(x))\, dx = E(S)$$

---

## 4. Tabla comparativa de principios

| Principio | Fórmula | Cota inf. $p \geq E(S)$ | Aditividad | Inv. escala | Consistencia |
|---|---|:---:|:---:|:---:|:---:|
| Valor esperado | $(1+\theta)E(S)$ | ✓ | ✓ | ✓ | ✓ |
| Varianza | $E(S) + \theta\,\text{Var}(S)$ | ✓ | ✓ | ✗ | ✓ |
| Desviación estándar | $E(S) + \theta\sqrt{\text{Var}(S)}$ | ✓ | ✗ | ✓ | ✓ |
| Exponencial | $\frac{1}{\alpha}\ln M_S(\alpha)$ | ✓ | ✓ | ✗ | ✓ |
| Porcentaje | $\inf\{x: P(S>x)\leq\epsilon\}$ | ✗ en general | ✗ | ✓ | ✓ |
| Esscher | $E(Se^{hS})/E(e^{hS})$ | ✓ | — | — | — |
| Riesgo ajustado | $\int_0^\infty(1-F(x))^{1/\rho}dx$ | ✓ | — | — | — |

---

## 5. Primas y funciones de utilidad: la decisión de asegurar

### Marco de negociación

El principio de utilidad cero permite modelar la decisión de **ambas partes** del contrato:

**Desde la aseguradora** (capital inicial $u_1$, utilidad $v_1$): la prima mínima aceptable $p^-$ satisface:
$$v_1(u_1) = E[v_1(u_1 + p^- - S)]$$

La aseguradora acepta cualquier prima $p \geq p^-$.

**Desde el asegurado** (riqueza $u_2$, utilidad $v_2$): la prima máxima aceptable $p^+$ satisface:
$$v_2(u_2 - p^+) = E[v_2(u_2 - S)]$$

El asegurado acepta cualquier prima $p \leq p^+$.

### Condición de asegurabilidad

> El riesgo $S$ es **asegurable** bajo estas condiciones si y solo si:
> $$\boxed{p^- \leq p \leq p^+}$$
> Es decir, existe un rango de primas que satisface simultáneamente a ambas partes.

### Ejemplo con utilidades exponenciales

Aseguradora: $v_1(x) = 1 - e^{-\alpha_1 x}$ $\;\Rightarrow\;$ $p^- = \dfrac{1}{\alpha_1}\ln M_S(\alpha_1)$

Asegurado: $v_2(x) = 1 - e^{-\alpha_2 x}$ $\;\Rightarrow\;$ $p^+ = \dfrac{1}{\alpha_2}\ln M_S(\alpha_2)$

El riesgo es asegurable si y solo si:

$$\frac{1}{\alpha_1}\ln M_S(\alpha_1) \leq \frac{1}{\alpha_2}\ln M_S(\alpha_2)$$

Esto conecta el parámetro de aversión al riesgo de cada parte con la viabilidad del mercado de seguros.

---

## 6. Síntesis conceptual

El cálculo de primas no tiene una solución única. Cada principio refleja una **filosofía distinta** sobre cómo compensar la incertidumbre:

- Los principios de **valor esperado, varianza y desviación estándar** son paramétricos y transparentes, pero ignoran la forma completa de la distribución.
- El principio **exponencial** (utilidad cero o valor medio) utiliza toda la información vía la FGM y tiene propiedades de aditividad, pero no es invariante de escala.
- El principio del **porcentaje** es intuitivo y muy usado en regulación, pero no garantiza ganancia neta.
- Los principios de **Esscher y riesgo ajustado** distorsionan la distribución para ser conservadores, y son los más usados en finanzas de seguros modernas (pricing de riesgo de cola).

En la práctica actuarial se combina el principio matemático con criterios regulatorios, competencia de mercado y apetito de riesgo de la compañía.
