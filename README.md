# Olist — Diagnóstico probabilístico de experiencia del cliente

Proyecto de primer corte — *Aprendizaje de Máquina No Supervisado*, Universidad de La Sabana (2026-II).
Propuesta A: analista de experiencia del cliente en un marketplace.

## Problema de negocio

Olist es el mayor marketplace de e-commerce de Brasil. La dirección quiere entender, con fundamentos
probabilísticos rigurosos, qué factores explican la satisfacción del cliente y el comportamiento de
los vendedores — para poder priorizar decisiones de inversión (logística, pricing, evaluación de
vendedores nuevos) con evidencia cuantitativa en vez de intuición.

## Dataset

**Brazilian E-Commerce Public Dataset by Olist** — ~99,441 pedidos reales (2016–2018), 9 archivos CSV
relacionados (clientes, pedidos, ítems, pagos, reseñas, productos, vendedores, geolocalización).
Fuente: [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

> Los archivos CSV no están incluidos en este repositorio por su tamaño. Descárgalos del link
> anterior y colócalos en `data/` antes de ejecutar el notebook.

## Estructura del repositorio

```
├── notebooks/
│   └── proyecto_olist_completo.ipynb   # Notebook completo, 18 secciones, ejecutado sin errores
├── data/                                 # (vacío en el repo — colocar aquí los 9 CSV de Kaggle)
├── Informe_Gerencial_Olist.docx          # Informe ejecutivo de 1 página
├── Guia_Defensa_Oral.md                  # Preparación de las 3 preguntas de sustentación
└── README.md
```

## Resumen de los 11 hallazgos

1. **Probabilidad condicional** — un pedido con entrega más lenta que el promedio tiene 21.0% de
   probabilidad de reseña negativa vs 8.2% si llega a tiempo (2.55x).
2. **Teorema de Bayes** — dado que un pedido recibió reseña negativa, la probabilidad de que haya
   llegado tarde sube de un prior de 8.0% a un posterior de 33.7% (4.2x).
3. **Verosimilitud / MLE** — el tiempo de entrega se ajusta mejor a una distribución Log-Normal que
   a una Gamma (AIC 646,576 vs 647,288).
4. **Distribuciones paramétricas** — el peso de los productos también favorece Log-Normal sobre
   Gamma (estadístico KS 0.066 vs 0.139).
5. **Esperanza y varianza** — la categoría `fixed_telephony` tiene la mayor varianza de ticket
   promedio, señal de alta heterogeneidad de precios dentro de la categoría.
6. **Independencia y correlación** — método de pago y categoría de producto NO son independientes
   (chi²=405.2, p<0.001); tiempo de entrega y calificación correlacionan negativamente (r=-0.33).
7. **Prior y posterior** — modelo Beta-Binomial: la confiabilidad estimada de un vendedor nuevo pasa
   de un prior poblacional de 75.5% a un posterior de 83.7% tras 5 ventas bien calificadas.
8. **Entropía** — el catálogo de categorías tiene una entropía de 4.71 bits sobre un máximo teórico
   de 6.15 bits (concentración moderada en pocas categorías líderes).
9. **Entropía cruzada** — un clasificador logístico de reseña negativa logra log-loss de 0.324
   (train) y 0.325 (test), sin sobreajuste; el retraso de entrega es el predictor dominante.
10. **Divergencia KL** — la distribución de calificaciones entre São Paulo y Río de Janeiro difiere
    poco (KL=0.035 bits), aunque RJ tiene más reseñas de 1 estrella.

## Cómo reproducir los resultados

```bash
pip install pandas numpy scipy scikit-learn matplotlib seaborn jupyter
jupyter nbconvert --to notebook --execute notebooks/proyecto_olist_completo.ipynb
```

Todas las cifras del informe gerencial y de la defensa oral se generan directamente en este
notebook — ningún valor fue escrito manualmente.
