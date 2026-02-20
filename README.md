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

---
*Si llegaste aquí desde LinkedIn y el análisis de datos para la toma de decisiones te apasiona tanto como a mí, ¡no dudes en mandarme un mensaje o conectar!*
