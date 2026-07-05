# 📦 RappiPlus: De Datos a Decisiones de Negocio

## 🎯 Objetivo / Pregunta de Negocio
¿El servicio RappiPlus es rentable? ¿En qué etapa del proceso se pierden los usuarios y qué tan bien se retienen?  
Este proyecto evalúa el desempeño general del negocio para apoyar decisiones estratégicas basadas en datos.

---

## 📂 Datos
| Dataset | Fuente | Contenido |
|---|---|---|
| `rappiplus_orders_raw.csv` | TripleTen / Practicum S3 | 25,100 pedidos con precios, descuentos y revenue |
| `rappiplus_catalog.csv` | TripleTen / Practicum S3 | Costos de productos, categorías y proveedores |
| `rappiplus_marketing_spend.csv` | TripleTen / Practicum S3 | Inversión en marketing por canal y país |
| `events` (SQL) | Base de datos PostgreSQL | Eventos de comportamiento de usuario en el funnel |
| `users` / `user_activity` (SQL) | Base de datos PostgreSQL | Registro de usuarios y actividad semanal |
| `experiment_checkout_ui.csv` | TripleTen / Practicum S3 | Resultados del test A/B en el checkout |

---

## ⚙️ Proceso

### 1. Limpieza y Calidad de Datos (Python - Pandas)
- Conversión de columnas de fecha a formato datetime
- Eliminación de filas con nulos en columnas críticas (< 2% del total, sin pérdida de representatividad)
- Relleno de valores nulos en canal de marketing con categoría `unknown`
- Corrección de inconsistencias de redondeo en `monto_total` (diferencias de $0.01 por punto flotante)
- Eliminación de 100 pedidos duplicados (IDs repetidos idénticos)
- **Detección y eliminación de outlier crítico:** `Laptop-Gaming-16GB` registraba pedidos de hasta 20,000 unidades, distorsionando todas las métricas de rentabilidad

### 2. Análisis de Rentabilidad (Python - Pandas)
- Merge entre `orders` y `catalog` por `nombre_producto`
- Cálculo de `costo_total`, `margen_bruto` y `profit_real` por pedido
- Análisis por categoría de producto, país y canal de marketing
- Cálculo de ROI de marketing por país

### 3. Funnel de Conversión (SQL - PostgreSQL)
- Construcción del funnel secuencial con CTEs para evitar conteo doble de usuarios
- Cálculo de tasa de conversión entre etapas y vs. inicio del funnel

### 4. Retención por Cohortes (SQL - PostgreSQL)
- Agrupación de usuarios por mes de registro
- Cálculo de retención semanal (W1, W2, W3) usando `FLOOR(dias_despues_registro / 7)`

### 5. Test A/B (Python - statsmodels)
- Z-test de proporciones para comparar tasa de conversión entre grupo control y tratamiento
- Nivel de significancia: α = 0.05

### 6. Dashboard (Power BI)
- Visualización de KPIs de revenue, costos, margen y marketing
- Filtros por país, categoría y canal

---

## 📊 Insights Clave

1. **El negocio es rentable:** Revenue total de $9.49M, Profit de $2.84M (margen global del 29.88%). Las 3 categorías (Hogar, Moda, Electrónica) tienen márgenes brutos muy parejos (~59–63%), sin una categoría débil.

2. **El mayor abandono ocurre al final del funnel:** La conversión total (first_visit → purchase) es del 49.47%. Los mayores puntos de fuga están en `begin_checkout → add_payment_info` (78.05%) y `add_payment_info → purchase` (77.65%), sugiriendo fricción en el proceso de pago.

3. **La retención semanal es estable pero hay oportunidad a largo plazo:** La retención W1–W3 se mantiene entre 40–44% en todas las cohortes — los usuarios que se enganchan se quedan. Sin embargo, análisis previos muestran caída fuerte a D28, lo que indica oportunidad para campañas de reactivación.

4. **El test A/B no fue concluyente:** La nueva UI del checkout mostró una mejora de +0.60 pp (15.69% → 16.29%), pero el p-valor (0.4161) supera α = 0.05, por lo que no hay evidencia estadística suficiente para implementar el cambio.

---

## 💡 Recomendación / Siguiente Paso
Si esto fuera una tarea laboral real:
- **Optimizar el checkout:** Simplificar los pasos de pago para reducir el abandono en `add_payment_info → purchase`, que es el cuello de botella más crítico.
- **Campañas de reactivación a D14–D21:** Dado que la retención cae fuertemente después de la semana 3, implementar notificaciones o descuentos personalizados antes de ese punto.
- **Repetir el test A/B con mayor muestra:** El tamaño actual no tiene suficiente poder estadístico para detectar diferencias pequeñas. Se recomienda calcular el tamaño de muestra necesario antes de una nueva iteración.

---

## 🔗 Enlaces
- 📓 [Notebook del proyecto](./S12%20Proyecto%20RappiPlus.ipynb)
- 📊 [Dashboard en Power BI](https://drive.google.com/drive/folders/1kWFhY3nmmINmuXVKG3naqJ-tK3VqrOtl?usp=drive_link)

---

## 🛠️ Herramientas Utilizadas
`Python` · `Pandas` · `NumPy` · `SQL` · `PostgreSQL` · `statsmodels` · `Matplotlib` · `Seaborn` · `Power BI`

---

## 👩‍💻 Sobre mí
Soy Yetlanezzi Robles, Data Analyst con más de 7 años de experiencia en operaciones retail. Actualmente completando mi certificación en Análisis de Datos en TripleTen.

📩 yetlanezziroblescano@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/yetlanezzi-analyst)
