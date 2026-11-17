# Análisis funcional de la respuesta a dexametasona en células de músculo liso de vía aérea

Interpretación funcional de un experimento de RNA-seq en el que se tratan células de músculo liso de vía aérea humana con **dexametasona** (un glucocorticoide) y se comparan con las mismas células sin tratar. Primero hemos hecho un análisis de sobrerrepresentación (**ORA**) y después lo hemos contrastado con un análisis sobre la lista ordenada completa (**GSEA preranked**), usando en los dos la colección **Hallmark** de MSigDB.

*Functional interpretation of the dexamethasone response in human airway smooth muscle cells (airway dataset, GSE52778): ORA vs GSEA preranked with MSigDB Hallmark gene sets.*

Lo hemos hecho en la asignatura **Herramientas y Algoritmos en Bioinformática** del Grado en Ingeniería de la Salud (Universidad de Málaga, curso 2026/27), en dos entregas: E1 (interpretación inicial) y E2 (interpretación contrastada y revisión).

## Datos

Son los datos públicos del conjunto *airway* ([GSE52778](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE52778), Himes et al., 2014): cuatro líneas celulares, cada una con una muestra control y otra tratada con dexametasona. El diseño es **pareado**, porque cada línea celular aporta las dos condiciones, y la unidad experimental es la línea celular.

En clase los recibimos en un paquete preparado por el profesor (`airway_26-27_v1.1.zip`), con los resultados de expresión diferencial ya calculados. Ese paquete no se incluye en el repositorio porque es material de la asignatura; los cuadernos explican cómo se carga.

## Qué hemos hecho

| Entrega | Pregunta | Método | Carpeta |
|---|---|---|---|
| E1 | ¿Qué procesos biológicos cambian con la dexametasona? | ORA por separado para los genes que suben y los que bajan, con el universo de genes elegibles y corrección de Benjamini-Hochberg | [`E1_ORA`](E1_ORA) |
| E2 | ¿Se sostiene la conclusión de E1 si se usa toda la lista ordenada y no solo los genes seleccionados? | GSEA preranked con gseapy sobre el estadístico de cada gen, comparación conjunto a conjunto con el ORA y revisión del *leading edge* | [`E2_ORA_frente_a_GSEA`](E2_ORA_frente_a_GSEA) |

## Resultados principales

- En el ORA hemos partido de 16 122 genes en el universo, de los que 493 suben y 464 bajan con el tratamiento, y han salido 25 conjuntos Hallmark significativos.
- El conjunto más significativo entre los que suben es **TNF-alpha Signaling via NF-kB** (26 genes, como *PER1*, *DUSP1*, *KLF9* o *TSC22D1*, varios de ellos dianas clásicas de los glucocorticoides). Entre los que bajan destaca **p53 Pathway**.
- La GSEA confirma TNF-alpha como el segundo conjunto con el NES más alto (NES = 1.87, FDR = 0.0026). Su *leading edge* son 60 genes, entre ellos inhibidores de la propia vía NF-kB como *NFKBIA* y *TNFAIP3*, que el ORA no veía porque cambian menos del doble. Al revisar la curva completa hemos visto que también hay 35 genes del conjunto en el extremo de los que bajan (mediadores como *LIF*, *PTGS2* o *VEGFA*), así que no concluimos que todo el conjunto suba. Lo que planteamos, como hipótesis y no como resultado, es que la dexametasona induce reguladores negativos de NF-kB a la vez que reprime algunos de sus productos.
- Hemos hecho dos comprobaciones: al ordenar por log2FC en lugar del estadístico, TNF-alpha mantiene el signo y casi todo el *leading edge*, pero su FDR sube a 0.069, así que la dirección es robusta y la significación no tanto. Además, hemos recalculado la puntuación de enriquecimiento y el *leading edge* con código propio para los 50 conjuntos, y el ORA de E2 reproduce exactamente los números de E1.

## Estructura del repositorio

```
E1_ORA/
  E1_analisis_ORA.ipynb                 cuaderno con explicación, código, salidas e interpretación
  resultados/
    E1_universo_y_seleccion.csv         universo de genes y genes seleccionados
    E1_ORA_hallmark_completa.csv        ORA completo de los 50 conjuntos Hallmark
    E1_figura_ORA.png                   figura resumen del ORA
E2_ORA_frente_a_GSEA/
  E2_ORA_frente_a_GSEA.ipynb            cuaderno de la segunda entrega
  resultados/
    E2_ranking_stat.csv                 lista ordenada usada en la GSEA
    E2_GSEA_hallmark_completa.csv       GSEA completa de los 50 conjuntos
    E2_comparacion_ORA_GSEA.csv         comparación conjunto a conjunto
    E2_TNF_leading_edge_y_cola.csv      genes del leading edge y de la cola de TNF-alpha
    E2_figura1_TNF.png                  curva de enriquecimiento de TNF-alpha
    E2_figura2_NES_vs_ORA.png           NES frente a resultado del ORA
```

## Cómo reproducirlo

1. Abrir el cuaderno en Google Colab o en Jupyter con Python 3.
2. Subir el paquete de datos de la asignatura y ajustar la ruta como se indica en la sección *Cargar los datos* del cuaderno.
3. Ejecutar todas las celdas en orden. El cuaderno de E2 instala `gseapy==1.3.2`, que no viene en Colab.

## Autor

Antonio Sánchez Doña. Los cuadernos parten de la plantilla de la asignatura, que aporta la carga de datos y los controles del paquete; el análisis funcional, las comprobaciones y la interpretación son nuestros.
