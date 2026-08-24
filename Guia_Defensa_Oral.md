# Guía de defensa oral — Propuesta A (Olist)
Máx. 3 minutos, sin apuntes. Todas las cifras salen de `proyecto_olist_completo.ipynb`.

---

## Pregunta 1
**¿Por qué eligió el Teorema de Bayes —y no solo la probabilidad condicional— para responder la pregunta sobre pedidos tardíos y reseñas negativas? ¿Qué le aportó invertir la probabilidad que no tenía antes?**

**Idea central para responder:**
La probabilidad condicional (Concepto 1) responde "¿qué tan probable es una mala reseña *dado* que el pedido llegó lento?" — es decir, mide el efecto de la logística hacia adelante. Pero desde el punto de vista operativo, Olist normalmente observa primero la reseña (el cliente se queja) y quiere saber hacia atrás: *dado que ya hay una reseña negativa, ¿qué tan probable es que la causa fue un retraso?* Eso es exactamente lo que invierte Bayes.

**Cifras propias a citar:**
- Prior: P(pedido tarde) = **8.0%**
- Posterior: P(pedido tarde | reseña negativa) = **33.7%**
- Eso es un incremento de **4.2 veces** sobre el prior.

**Qué aportó invertir la probabilidad:** sin Bayes, Olist solo sabría que el retraso *empeora* la reseña (correlación hacia adelante). Con Bayes, Olist puede usar una reseña negativa como *señal de alerta* de que probablemente hubo un problema logístico — sin monitorear cada pedido en tiempo real. Es una herramienta de priorización: cuando llega una reseña de 1-2 estrellas, hay 1 de cada 3 probabilidades de que la causa raíz sea logística, así que ese es el primer lugar donde investigar.

---

## Pregunta 2
**Al ajustar una distribución paramétrica al tiempo de entrega o al valor del flete, ¿qué evidencia numérica usó para descartar la distribución alternativa que no eligió?**

**Idea central para responder:**
Ajusté por Máxima Verosimilitud dos distribuciones candidatas —Log-Normal y Gamma— al tiempo de entrega (`delivery_days`), y las comparé con AIC (Criterio de Información de Akaike), que penaliza la complejidad del modelo además de mirar el ajuste.

**Cifras propias a citar:**
- AIC Log-Normal = **646,576.0**
- AIC Gamma = **647,287.6**
- Diferencia de AIC ≈ **711.6** a favor de Log-Normal (una diferencia de AIC >10 ya se considera decisiva en la literatura; aquí es mucho mayor).

**Por qué se descartó Gamma:** con el mismo número de parámetros libres (2 cada una, con `loc` fijado en 0), Gamma tiene una log-verosimilitud menor (-323,641.8 vs -323,286.0 de Log-Normal) — ajusta peor a los datos observados. Interpretación de negocio: el tiempo de entrega tiene una cola derecha muy pesada (algunos pedidos se demoran desproporcionadamente), lo cual es más consistente con un proceso multiplicativo de varios cuellos de botella en cadena (transporte → aduana → última milla), que es justamente lo que la distribución Log-Normal captura mejor que la Gamma.

---

## Pregunta 3
**En su análisis de independencia entre método de pago y categoría de producto, ¿qué significaría para Olist, en términos de negocio, que esas dos variables NO fueran independientes?**

**Idea central para responder:**
El test de independencia (chi-cuadrado sobre tabla de contingencia, método de pago × top-10 categorías) **rechazó la independencia**.

**Cifras propias a citar:**
- Chi² = **405.24**, p-value ≈ **4.3×10⁻⁶⁹** (grados de libertad = 27)
- Con ese p-value, se rechaza con altísima confianza la hipótesis nula de independencia.

**Qué significa en términos de negocio:** que el método de pago que un cliente elige no es aleatorio respecto a qué está comprando — está sistemáticamente asociado a la categoría de producto (por ejemplo, categorías de ticket más alto como `computers` muestran más uso de cuotas/financiamiento que categorías de bajo ticket). Si Olist tratara las condiciones de pago de forma uniforme en todo el catálogo, estaría ignorando un patrón de comportamiento real y perdiendo oportunidad de: (a) ofrecer planes de financiamiento diferenciados por categoría, (b) negociar comisiones de pasarela de pago distintas según el mix de categorías de cada vendedor, y (c) anticipar fricción de checkout en categorías donde el método de pago preferido no está bien soportado.

---

### Notas de preparación
- Practica decir las cifras de memoria (%, no solo la interpretación) — el PDF exige citar valores numéricos concretos.
- Si preguntan por cualquier otro concepto del notebook, todos los resultados están también en `data/resultados_finales_notebook.json` dentro del notebook ejecutado.
