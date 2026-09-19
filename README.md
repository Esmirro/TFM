# 📦 Análisis de E-Commerce Brasileño — Olist (2016-2018)

**Proyecto Final de Máster (TFM) — Data Analytics**

Análisis integral de más de 112.000 registros de pedidos del marketplace brasileño **Olist**, cubriendo limpieza y transformación de datos, análisis estadístico, segmentación de clientes (RFM) y un dashboard operativo en Power BI para la toma de decisiones comerciales y logísticas.

---

## 📑 Tabla de contenidos

1. [Resumen ejecutivo](#-resumen-ejecutivo)
2. [Objetivos del proyecto](#-objetivos-del-proyecto)
3. [Fuente de los datos](#-fuente-de-los-datos)
4. [Estructura del repositorio](#-estructura-del-repositorio)
5. [Metodología](#-metodología)
6. [Análisis estadístico realizado](#-análisis-estadístico-realizado)
7. [Segmentación de clientes (RFM)](#-segmentación-de-clientes-rfm)
8. [Dashboard operativo](#-dashboard-operativo)
9. [Resultados y hallazgos clave](#-resultados-y-hallazgos-clave)
10. [Tecnologías utilizadas](#-tecnologías-utilizadas)
11. [Cómo reproducir el análisis](#-cómo-reproducir-el-análisis)
12. [Autor](#-autor)

---

## 📊 Resumen ejecutivo

| Métrica | Valor |
|---|---|
| Periodo analizado | Sep 2016 – Sep 2018 (~2 años) |
| Registros del dataset final | 112.650 filas × 50 columnas |
| Pedidos únicos | 98.666 |
| Clientes únicos | 98.666 |
| Vendedores (sellers) | 3.095 |
| Ingresos totales | R$ 13.591.643,70 |
| Rating medio de cliente | 4,03 / 5 |
| % de clientes satisfechos (rating 4-5) | 75,5 % |
| % de entregas a tiempo | 90,6 % |
| Tiempo medio de entrega | 12,0 días |
| Estados cubiertos | 27 (todo Brasil) |
| Categorías de producto | 71 |

---

## 🎯 Objetivos del proyecto

Este TFM tiene como finalidad demostrar el ciclo completo de un proyecto de análisis de datos, cubriendo:

- **Transformación y limpieza profunda de los datos** de múltiples tablas relacionales.
- **Análisis descriptivo** del comportamiento de compra, entrega y satisfacción del cliente.
- **Análisis estadístico** (correlaciones, pruebas de normalidad, segmentación RFM).
- **Visualización de datos** mediante gráficos exploratorios en Python.
- **Dashboard operativo** en Power BI que aporte valor accionable al negocio.
- **Informe explicativo** con los hallazgos y recomendaciones del análisis.

---

## 🗂 Fuente de los datos

Los datos provienen del dataset público **[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)** (Kaggle), que documenta ~100.000 pedidos realizados entre 2016 y 2018 en el marketplace Olist.

El dataset se distribuye en **8 tablas relacionales** que fue necesario limpiar, transformar y unir mediante claves primarias/foráneas (`order_id`, `customer_id`, `product_id`, `seller_id`, código postal):

| Tabla original | Contenido | Filas |
|---|---|---|
| `olist_orders_dataset.csv` | Pedidos y sus fechas de ciclo de vida | 99.442 |
| `olist_order_items_dataset.csv` | Líneas de producto por pedido, precio y flete | 112.650 |
| `olist_order_payments_dataset.csv` | Pagos y métodos de pago | 103.886 |
| `olist_order_reviews_dataset.csv` | Reseñas y puntuación del cliente | 104.719 |
| `olist_products_dataset.csv` | Catálogo de productos y dimensiones físicas | 32.951 |
| `olist_customers_dataset.csv` | Datos e identificadores de clientes | 99.441 |
| `olist_sellers_dataset.csv` | Datos de vendedores | 3.095 |
| `olist_geolocation_dataset.csv` | Coordenadas por código postal brasileño | 1.000.163 |
| `product_category_name_translation.csv` | Traducción ES/PT → EN de categorías | 71 |

> ⚠️ **Nota sobre el requisito de "dos fuentes distintas":** estas 8 tablas pertenecen a un único dataset de Kaggle (relacional, no un CSV plano). Si tu convocatoria exige explícitamente dos **fuentes** independientes (no solo dos archivos), conviene confirmarlo con el equipo docente o complementar el análisis con una fuente externa (p. ej. inflación/tipo de cambio del Banco Central de Brasil, o datos poblacionales del IBGE) para reforzar el cumplimiento formal del requisito.

### Dataset final (tras la unión y transformación)

El archivo `olist_dataset_final.csv` es el resultado de unir las 8 tablas anteriores y aplicar *feature engineering*:

- **112.650 filas × 50 columnas** → cumple sobradamente el mínimo exigido (50.000 filas / 20 columnas).
- Incluye variables derivadas: `delivery_days`, `delay_days`, `on_time_delivery`, `order_value`, `rating_category`, `order_quarter`, coordenadas de geolocalización, etc.

---

## 🗂 Estructura del repositorio

Estructura recomendada para organizar el repositorio de GitHub a partir de los archivos entregados:

```
tfm-olist-brazil/
│
├── README.md                          # Este archivo
│
├── data/
│   ├── raw/                           # Datos originales de Kaggle (sin modificar)
│   │   ├── olist_customers_dataset.csv
│   │   ├── olist_geolocation_dataset.csv
│   │   ├── olist_orders_dataset.csv
│   │   ├── olist_order_items_dataset.csv
│   │   ├── olist_order_payments_dataset.csv
│   │   ├── olist_order_reviews_dataset.csv
│   │   ├── olist_products_dataset.csv
│   │   ├── olist_sellers_dataset.csv
│   │   └── product_category_name_translation.csv
│   │
│   └── processed/                     # Datos transformados/finales
│       ├── olist_dataset_final.csv    # Dataset unificado (112.650 x 50)
│       ├── rfm_analysis.csv           # Segmentación RFM por cliente
│       ├── correlation_matrix.csv     # Matriz de correlaciones
│       ├── category_analysis.csv      # Agregados por categoría de producto
│       └── state_analysis.csv         # Agregados por estado
│
├── notebooks/
│   └── 01_Analisis_Olist_Completo.ipynb   # EDA, limpieza, transformación y análisis estadístico
│
├── dashboard/
│   └── TFM_Brazil.pbix                # Dashboard operativo en Power BI
│
└── reports/
    └── informe_analisis.pdf           # Informe explicativo del análisis (a redactar/exportar)
```

---

## 🔧 Metodología

El proceso completo está documentado paso a paso en `notebooks/01_Analisis_Olist_Completo.ipynb`, estructurado en las siguientes fases:

### 1. Exploración inicial (EDA)
Revisión de dimensiones, tipos de dato, nulos y duplicados de cada una de las 8 tablas originales antes de tocarlas.

### 2. Limpieza de datos
- **Fechas:** conversión de las 5 columnas de fechas de `orders` (y las de `reviews`) a formato `datetime`.
- **Valores nulos:** los comentarios de reseña vacíos se etiquetan como `"Sin comentario"`; las dimensiones físicas de producto (peso, alto, ancho, largo) se imputan con la **mediana** de cada columna.
- **Categorías de producto:** traducción de las categorías (originalmente en portugués) al inglés mediante `product_category_name_translation`; las categorías sin traducción se agrupan como `"Otros"`.
- **Geolocalización:** eliminación de duplicados por código postal (`geolocation_zip_code_prefix`), quedándose con el primer registro de cada zona.

### 3. Feature engineering sobre pedidos
A partir de las fechas del ciclo de vida del pedido se calculan:
- `delivery_days`: días reales de entrega (compra → entrega al cliente).
- `estimated_days`: días estimados de entrega (compra → fecha estimada).
- `delay_days`: diferencia entre entrega real y estimada (negativo = entregado antes de lo previsto).
- `on_time_delivery`: indicador binario de entrega a tiempo.
- `order_month`, `order_year`, `order_quarter`: variables temporales para análisis de estacionalidad.

### 4. Unión de los datasets (joins secuenciales)
```
orders ⟶ order_items ⟶ products ⟶ sellers ⟶ customers ⟶ payments (agregados) ⟶ reviews (agregados) ⟶ geolocation
```
Los pagos se agregan por pedido (suma del importe, método de pago más frecuente) y las reseñas se agregan por pedido (primera puntuación y comentario) antes de unirse, para mantener una fila por línea de producto sin duplicar información de cabecera del pedido.

### 5. Transformación final
- Cálculo de `order_value` (precio + flete), `revenue`, `freight`, `total_value`.
- Imputación de nulos remanentes (rating → 0 si no hay reseña, estado → `"Unknown"`).
- Eliminación de filas sin `order_id`, `product_id` o `seller_id`.
- Creación de `rating_category` (Sin calificación / Bajo 1-2 / Medio 3 / Alto 4-5) para facilitar la segmentación en el dashboard.

---

## 📈 Análisis estadístico realizado

- **Matriz de correlaciones (Pearson)** entre precio, flete, valor del pedido, días de entrega, retraso y rating del cliente (`correlation_matrix.csv`).
- **Test de normalidad (Shapiro-Wilk)** sobre las variables numéricas clave para validar los supuestos antes de aplicar pruebas paramétricas.
- **Análisis RFM** (Recency, Frequency, Monetary) y segmentación de clientes en 6 grupos de valor.
- **Análisis temporal**: evolución mensual/trimestral de pedidos e ingresos.
- **Análisis por categoría de producto**: ingresos, ticket medio, rating y % de entregas a tiempo por categoría (`category_analysis.csv`).
- **Análisis geográfico**: ingresos, ticket medio y satisfacción por estado brasileño (`state_analysis.csv`).
- **Impacto de la entrega en la satisfacción**: relación entre retraso/días de entrega y rating del cliente.
- **Top sellers**: identificación de los vendedores con mayor volumen e ingresos.

---

## 👥 Segmentación de clientes (RFM)

A partir de `rfm_analysis.csv` (98.667 clientes), se calcularon los scores de Recencia, Frecuencia y Monetario (cuartiles) y se clasificó a cada cliente en uno de estos segmentos:

| Segmento | Clientes | % |
|---|---|---|
| VIP | 24.961 | 25,3 % |
| Recurrentes | 24.787 | 25,1 % |
| Potencial | 24.510 | 24,8 % |
| Leales | 24.408 | 24,7 % |

> La frecuencia media es de **1,14 compras por cliente**, lo que evidencia una base de clientes mayoritariamente de compra única — un hallazgo relevante para estrategias de retención y fidelización.

---

## 📊 Dashboard operativo

El dashboard operativo (`TFM_Brazil.pbix`, Power BI) permite explorar de forma interactiva:

- Evolución de pedidos e ingresos en el tiempo.
- Desempeño logístico (entregas a tiempo, retraso medio) por estado y categoría.
- Ranking de categorías y vendedores.
- Distribución de la satisfacción del cliente (rating) y su relación con la entrega.
- Segmentos RFM y su aportación al ingreso total.

*(Recomendación para el repositorio: exportar también 2-3 capturas de pantalla del dashboard a `dashboard/screenshots/` para que se pueda valorar sin necesidad de abrir Power BI.)*

---

## 💡 Resultados y hallazgos clave

- **Concentración geográfica:** el estado de **São Paulo (SP)** concentra el 48,1 % de los pedidos (47.449 de 98.666), muy por delante de Río de Janeiro (14.579) y Minas Gerais (13.129).
- **Categoría líder:** `bed_bath_table` (hogar/baño) es la categoría con más pedidos (11.115), seguida de `health_beauty` y `sports_leisure`.
- **La entrega es el principal driver de satisfacción:** el `delivery_days` (días de entrega) tiene la correlación negativa más fuerte con el rating del cliente (**r ≈ -0,30**), seguido del retraso (`delay_days`, r ≈ -0,23). El precio y el valor del pedido apenas correlacionan con la satisfacción (r ≈ 0).
- **Buen desempeño logístico general:** 90,6 % de los pedidos se entregan a tiempo o antes, con una media de 12 días de entrega.
- **Satisfacción alta pero mejorable:** rating medio de 4,03/5, con un 75,5 % de clientes en la banda "satisfecho" (4-5), dejando un ~24 % de margen de mejora ligado sobre todo a la experiencia de entrega.
- **Base de clientes poco recurrente:** con una frecuencia media de 1,14 compras/cliente, la retención es un área de oportunidad clara para el negocio.

---

## 🛠 Tecnologías utilizadas

**Procesamiento y análisis de datos**
- Python 3
- Pandas / NumPy
- SciPy (`stats`, `pearsonr`, `spearmanr`, `kruskal`, Shapiro-Wilk)
- Matplotlib / Seaborn
- Visual Studio Code / Jupyter Notebook

**Dashboard y visualización**
- Power BI (`TFM_Brazil.pbix`)

---

## ▶️ Cómo reproducir el análisis

1. Clonar el repositorio y situarse en la raíz del proyecto.
2. Crear un entorno virtual e instalar dependencias:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy jupyter
   ```
3. Colocar los CSV originales en `data/raw/`.
4. Abrir y ejecutar `notebooks/01_Analisis_Olist_Completo.ipynb` de principio a fin. El notebook genera automáticamente en `data/processed/`:
   - `olist_dataset_final.csv`
   - `rfm_analysis.csv`
   - `correlation_matrix.csv`
   - `category_analysis.csv`
   - `state_analysis.csv`
5. Abrir `dashboard/TFM_Brazil.pbix` con Power BI Desktop y actualizar el origen de datos si es necesario (`data/processed/olist_dataset_final.csv`).

---

## ✍️ Autor

**Analitica** — Proyecto Final de Máster, Data Analytics.
