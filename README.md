## proyecto-analisis-ecommerce
# Análisis de rentabilidad y conversión de usuarios de E-commerce

### Rol: Junior Data Analyst

## 📊 Dashboard Interactivo
🔗[Ver Dashboard en Looker Studio](https://datastudio.google.com/embed/u/0/reporting/4837de8f-0806-4f11-837f-1b2204bc17ce/page/MfQzF)

## Resumen:
En este proyecto de análisis de datos se explorará la eficiencia y rentabilidad de un negocio de e-commerce de artículos para el hogar y estilo de vida a nivel global. Se enfocará principalmente en conocer la eficiencia de la página web, rentabilidad de los productos, conversión de usuarios, franjas horarias con mayor tráfico y zona geográfica predominante en ventas. Se utilizará datasets de ventas y transacciones de la página web, que se limpiará, transformará y ordenará para ser procesada y analizada. Los resultados de este análisis serán usadas para conocer como se está moviendo el negocio en los tres últimos años de operación y establecer los insights que más impactan al negocio.

## Fase de consulta: 
La empresa de e-commerce de productos de regalo y estilo de vida enfrenta una falta de visibilidad sobre la eficiencia real de su plataforma digital durante el último año. 
Actualmente, existen las siguientes incertidumbres:
- Posible pérdida de clientes potenciales durante el proceso de compra.
- Alto tráfico de clientes pero que no se refleja proporcionalmente en el revenue.
- No se identifican los productos con mayor y menor venta.
- No se puede determinar que productos necesitan un ajuste de precios, promoción o inversión en marketing.
- La empresa no conoce sus horas o días de mayor tráfico en su página web.
- La empresa no puede medir la conversión de las interacciones de los usuarios en ingresos generados.

Para eso se establecieron 3 principales preguntas a responder:
1. ¿Qué categoría de productos generan la rentabilidad más alta en el último año (2010-2011)?  
2. ¿Qué porcentaje de conversion rate existe por categoría y qué horario tiene el mayor tráfico pero bajo conversion rate? 
3. ¿Cuál es el LTV promedio de nuestros clientes y qué porcentaje del revenue depende de compradores recurrentes?

## Objetivo del análisis:
Analizar datos de ventas de productos de regalo y estilo de vida para identificar la rentabilidad global y aumentar la conversión de los usuarios.

## Proceso de análisis:
### 1. Preparación:
### Datos utilizados:
Para este análisis se usaron dos datasets abiertos del sitio web kaggle.com. El primer dataset corresponde al registro de ventas y el segundo a transacciones en el sitio web de dos diferentes ventas retail en la modalidad de e-commerce.

### Información de los datasets:
El primer dataset registró ventas retail online de una tienda localizada en Reino Unido de dos periodos (2009-2010 y 2010-2011) sobre productos de regalo y estilo de vida, entre los cuales están artículos para el hogar, estacionales y de regalo, que se han vendido en diferentes regiones. La tabla contiene filas con información como: invoice, stockcode, descripción, cantidad, fecha, precio, customer ID y país.
El segundo dataset registró las transacciones de ventas e-commerce a través de un sitio web recolectados en un período de 4.5 meses. La tabla contiene filas con información como: todos los eventos registrados de cada usuario que visitó el sitio web, otra de las propiedades sobre los productos de venta y las categorías de los mismos.

### Organización de los datasets:
El dataset que registra las ventas contiene una tabla llamada online_retail y el dataset que registra las transacciones de las ventas tiene 3 tablas, una de ellas se llama category_tree, events y propiedades.
Estos datasets se relacionan mediante el ID de la llaves (primaria y foránea), lo cual usé para unir las tablas.

### 2. Staging:
### Análisis exploratorio de los datasets en Excel/Power Query:
1. Estandarización de encabezados y verificación de tipos de datos.
2. Migración del procesamiento de datos a Power Query para optimizar la carga de memoria del conjunto de datos.
3. Transformación de la fecha mediante la fórmula:
                   #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [timestamp] / 1000)
   
### Diagrama entidad-relación:





