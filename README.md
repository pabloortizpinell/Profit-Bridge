# 📊 Modelo Avanzado de Profit Bridge en DAX (Descomposición Normativa con Ajuste FX)

Bienvenido a este repositorio. Aquí encontrarás un modelo analítico desarrollado en **Power BI con DAX**, diseñado para auditar y explicar las variaciones de rentabilidad de una empresa multinacional con precisión matemática exacta.

<img width="1066" height="427" alt="image" src="https://github.com/user-attachments/assets/96e753d0-c2a2-427c-9ad6-d67f6703bbed" />


## 🎯 ¿De qué trata este proyecto?
Cuando las ganancias de una empresa multinacional suben o bajan entre dos periodos, los directivos necesitan saber exactamente *por qué*. Tradicionalmente, los analistas construyen "Puentes de Ganancias" (Análisis Price-Volume-Mix) que suelen dejar márgenes de error, "términos cruzados" sin explicar, y fallan al separar el rendimiento operativo real del ruido macroeconómico.

Este modelo aplica el marco de la **Descomposición Normativa** extendido por el Dr. Tim J. Smith, Kyle T. Westra y Nathan L. Phipps en su publicación científica *"Profit bridges that disambiguate impacts of currency fluctuations from other marketing variables" (2023)*. 

La magia de este enfoque radica en utilizar promedios inter-periodo para aislar perfectamente el impacto de cinco variables clave, sin dejar un solo centavo sin explicar:

* **📈 Impacto de Volumen (QI):** Cuánto dinero ganamos/perdimos por vender más o menos unidades.
* **🏷️ Impacto de Precio (PI):** El efecto puro de nuestras subidas o bajadas de precios locales.
* **⚙️ Impacto de Costo Variable (VI):** La eficiencia operativa al reducir o aumentar costos.
* **🔀 Efecto Mix (MI):** Cómo impactó el cambio en la proporción de productos vendidos en la rentabilidad general (ej. vender más de los productos de alto margen).
* **💱 Impacto FX (Fluctuación Cambiaria):** Cómo los cambios en los tipos de cambio inflaron o desinflaron artificialmente nuestras ganancias reportadas, aislando el ruido macroeconómico del desempeño real de la gerencia.

## 🛠️ Herramientas Utilizadas
* **Power BI:** Modelado de datos y visualización interactiva.
* **Lenguaje DAX:** Medidas iterativas avanzadas usando `SUMX`, `CALCULATE` y transiciones de contexto para realizar cálculos algebraicos fila por fila.

## 💡 ¿Por qué es diferente a un análisis tradicional?
La mayoría de los sistemas fallan al calcular el **Efecto Mix** porque usan un solo periodo como base estática, y distorsionan por completo las métricas operativas cuando los **tipos de cambio** son volátiles. Este modelo en DAX itera producto por producto, comparando las diferencias de márgenes, pesos relativos y tasas de cambio frente a un estado neutral (promedio inter-periodo). 

**El resultado: La suma de los 5 impactos es exactamente igual a la variación de la ganancia en la cuenta bancaria reportada.**

## 🧮 La Matemática detrás del Modelo (El problema del "Término Cruzado")

La mayoría de los analistas que construyen un Puente de Ganancias se topan con un problema matemático fundamental: **El Término Cruzado**.

Imagina que tus ingresos son un rectángulo donde la base es la **Cantidad (Q)** y la altura es el **Precio (P)**. Si de un mes a otro aumentas el Precio y también logras vender más Cantidad, tu nuevo rectángulo de ingresos es más grande. Ese crecimiento total se divide en tres partes:
1. El aumento por el nuevo precio.
2. El aumento por la nueva cantidad.
3. **El término cruzado ($\Delta P \times \Delta Q$):** Una pequeña área generada por la interacción de ambas variables al mismo tiempo.

En un contexto multinacional, esto se vuelve exponencialmente más difícil porque introducimos un nuevo multiplicador: el **Tipo de Cambio (FX)**. Ahora tenemos términos cruzados complejos de 3 vías ($\Delta P \times \Delta Q \times \Delta FX$). 

**El dilema tradicional:** ¿A quién le damos el crédito por el dinero de estos términos cruzados? Los sistemas clásicos los asignan arbitrariamente o los dejan como un "Residual / Variación no explicada", lo cual ensucia el análisis.

### La Solución Normativa Ajustada por FX
Smith et al. (2023) demostraron que la única forma matemáticamente justa y neutral de repartir estos términos cruzados (y eliminar los residuales) es utilizando **promedios inter-periodo**. 

Al multiplicar las variaciones por el *estado promedio* de las otras variables, el modelo asume una postura neutral que colapsa las ecuaciones algebraicas perfectamente. Denotando la Variación con el símbolo **$\Delta$** y el Promedio con una **barra superior** ($\overline{X}$), así es como este modelo DAX calcula los 5 impactos en la moneda de reporte:

* **🏷️ Impacto de Precio (PI):** Aísla los cambios de precios locales asumiendo que el volumen y el tipo de cambio se mantuvieron en su estado promedio.
  > **$\Delta P \times \overline{Q} \times \overline{FX}$**

* **⚙️ Impacto de Costo (VI):** Aísla los cambios en la eficiencia de costos (negativo porque un aumento de costo reduce la ganancia).
  > **$-\Delta V \times \overline{Q} \times \overline{FX}$**

* **📦 Impacto de Volumen (QI):** Mide la expansión/contracción de las unidades totales vendidas, manteniendo la mezcla, los márgenes y la moneda en estado neutral.
  > **$\Delta Q_{total} \times (\overline{P} - \overline{V}) \times \overline{Mix} \times \overline{FX}$**

* **🔀 Efecto Mix (MI):** Mide la ganancia/pérdida obtenida por vender una mayor o menor proporción de productos de alto margen.
  > **$\overline{Q}_{total} \times (\overline{P} - \overline{V}) \times \Delta Mix \times \overline{FX}$**

* **💱 Impacto FX (Fluctuación Cambiaria):** Aísla el efecto puro del movimiento del tipo de cambio, asumiendo que todo el negocio operativo (Precio, Costo, Vol, Mix) se mantuvo en su estado promedio.
  > **$\Delta FX \times [\overline{Q}_{total} \times (\overline{P} - \overline{V}) \times \overline{Mix}]$**

### ¿Cómo se traduce esto a DAX?

En el código fuente de este modelo de Power BI, notarás la creación sistemática de medidas que suman el Periodo 1 y el Periodo 2, dividiéndolas entre 2 (Ej. `DIVIDE([PrecioP1] + [PrecioP2], 2)`). 

Estas medidas representan las barras de promedio ($\overline{P}$, $\overline{Q}$, $\overline{Mix}$, $\overline{FX}$). Al usar la función iteradora `SUMX` para evaluar estas ecuaciones fila por fila (producto por producto), todos los términos cruzados se anulan mutuamente, logrando que la suma de los 5 impactos operativos y macroeconómicos explique el **100% de la variación real de la ganancia ($\Delta \Pi$)** sin errores de redondeo.
