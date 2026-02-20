# 📊 Modelo de Profit Bridge Avanzado en DAX (Descomposición Normativa)

Bienvenido a este repositorio. Aquí encontrarás un modelo analítico desarrollado en **Excel (Power Pivot) con DAX**, diseñado para auditar y explicar las variaciones de rentabilidad de una empresa con precisión matemática exacta.



## 🎯 ¿De qué trata este proyecto?
Cuando las ganancias de una empresa suben o bajan entre dos periodos, los directivos necesitan saber exactamente *por qué*. Tradicionalmente, los analistas construyen "Puentes de Ganancias" (Profit Bridges) que suelen dejar márgenes de error o "términos cruzados" sin explicar.

Este modelo aplica la **Descomposición Normativa** postulada por el Dr. Tim J. Smith en su publicación científica *"Normative decomposition of the profit bridge into the impact of changes in marketing variables" (2021)*. 

La magia de este enfoque radica en utilizar promedios inter-periodo para aislar perfectamente el impacto de cuatro variables clave, sin dejar un solo centavo sin explicar:

* **📈 Impacto de Volumen (QI):** Cuánto dinero ganamos/perdimos por vender más o menos unidades.
* **🏷️ Impacto de Precio (PI):** El efecto puro de nuestras subidas o bajadas de precios.
* **⚙️ Impacto de Costo Variable (VI):** La eficiencia operativa al reducir o aumentar costos.
* **🔀 Efecto Mix (MI):** Cómo impactó el cambio en la proporción de productos vendidos (vender más de los productos más rentables).

## 🛠️ Herramientas Utilizadas
* **Microsoft Excel** (Base de datos relacional).
* **Lenguaje DAX** (Medidas iterativas avanzadas usando `SUMX`, `CALCULATE` y transiciones de contexto).

## 💡 ¿Por qué es diferente a un análisis tradicional?
La mayoría de los sistemas fallan al calcular el **Efecto Mix** porque usan un solo periodo como base. Este modelo en DAX itera producto por producto, comparando las diferencias de márgenes y pesos relativos frente a un estado neutral (promedio inter-periodo). **El resultado: La suma de los 4 impactos es exactamente igual a la variación de la ganancia en la cuenta bancaria.**

## 🧮 La Matemática detrás del Modelo (El problema del "Término Cruzado")

La mayoría de los analistas que construyen un Puente de Ganancias se topan con un problema matemático fundamental: **El Término Cruzado** (Cross-Term).

Imagina que tus ingresos son un rectángulo donde la base es la **Cantidad (Q)** y la altura es el **Precio (P)**. Si de un mes a otro aumentas el Precio y también logras vender más Cantidad, tu nuevo rectángulo de ingresos es más grande. Ese crecimiento total se divide en tres partes:
1. El aumento por el nuevo precio.
2. El aumento por la nueva cantidad.
3. **El término cruzado ($\Delta P \times \Delta Q$):** Una pequeña área generada por la interacción de ambas variables al mismo tiempo.

**El dilema tradicional:** ¿A quién le damos el crédito por el dinero de ese término cruzado? ¿Al equipo de Ventas (volumen) o al equipo de Pricing (precio)? Los sistemas clásicos lo asignan arbitrariamente a uno de los dos, o lo dejan como un "Residual / Variación no explicada", lo cual ensucia el análisis.

### La Solución Normativa
El Dr. Tim J. Smith demostró que la única forma matemáticamente justa y neutral de repartir este término cruzado (y eliminar los residuales) es utilizando **promedios inter-periodo**. 

Al multiplicar las variaciones por el *estado promedio* de la otra variable, el modelo asume una postura neutral que colapsa las ecuaciones algebraicas perfectamente. Denotando la Variación con el símbolo **$\Delta$** y el Promedio con una **barra superior** ($\overline{X}$), así es como este modelo DAX calcula los 4 impactos:

* **🏷️ Impacto de Precio (PI):** Aísla el cambio en precios asumiendo que el volumen se mantuvo en su estado promedio.
  > **$\Delta P \times \overline{Q}$**

* **⚙️ Impacto de Costo (VI):** Aísla el cambio en la eficiencia de costos (negativo porque un aumento de costo reduce la ganancia).
  > **$-\Delta V \times \overline{Q}$**

* **📦 Impacto de Volumen (QI):** Mide el efecto de la expansión/contracción de las unidades totales vendidas, manteniendo la mezcla de productos y los márgenes en su estado promedio.
  > **$\Delta Q_{total} \times (\overline{P} - \overline{V}) \times \overline{Mix}$**

* **🔀 Efecto Mix (MI):** Mide la ganancia/pérdida obtenida por vender una mayor o menor proporción de productos de alto margen.
  > **$\overline{Q}_{total} \times (\overline{P} - \overline{V}) \times \Delta Mix$**

### ¿Cómo se traduce esto a DAX?
En el código fuente de este modelo de Power Pivot, notarás la creación sistemática de medidas que suman el Periodo 1 y el Periodo 2, dividiéndolas entre 2 (Ej. `DIVIDE([PrecioP1] + [PrecioP2], 2)`). 

Estas medidas representan las barras de promedio ($\overline{P}$, $\overline{Q}$, $\overline{Mix}$). Al usar la función iteradora `SUMX` para evaluar estas ecuaciones fila por fila (producto por producto), todos los términos cruzados se anulan mutuamente, logrando que la suma de los 4 impactos explique el **100% de la variación real de la ganancia ($\Delta \Pi$)** sin errores de redondeo.
---
*Si llegaste aquí desde LinkedIn y el análisis de datos para la toma de decisiones te apasiona tanto como a mí, ¡no dudes en mandarme un mensaje o conectar!*
