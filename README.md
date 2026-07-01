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

### 2. Información de los datasets:
### Organización de los datasets:
El primer dataset registró ventas retail online de una tienda localizada en Reino Unido de dos periodos (2009-2010 y 2010-2011) sobre productos de regalo y estilo de vida, entre los cuales están artículos para el hogar, estacionales y de regalo, que se han vendido en diferentes regiones. La tabla contiene filas con información como: invoice, stockcode, descripción, cantidad, fecha, precio, customer ID y país.

![Diagrama de Relación de Base de Datos - E-commerce](diagrama-er.png)

El segundo dataset registró las transacciones de ventas e-commerce a través de un sitio web recolectados en un período de 4.5 meses. La tabla contiene filas con información como: todos los eventos registrados de cada usuario que visitó el sitio web, otra de las propiedades sobre los productos de venta y las categorías de los mismos.

### Relación y estructura de las tablas:
El dataset que registra las ventas contiene una tabla llamada online_retail y el dataset que registra las transacciones de las ventas tiene 3 tablas, una de ellas se llama category_tree, events y propiedades.
Estos datasets se relacionan mediante el ID de la llaves (primaria y foránea), lo cual usé para unir las tablas.

### 3. Staging:
### Análisis exploratorio de los datasets en Excel/Power Query:
1. Estandarización de encabezados y verificación de tipos de datos.
2. Migración del procesamiento de datos a Power Query para optimizar la carga de memoria del conjunto de datos.
3. Transformación de la fecha mediante la fórmula:
                   #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [timestamp] / 1000)
   
### 4. Intermediate:

### Consulta de datos con SQL

1. Comencé por la creación de una tabla unificada de los dos períodos de ventas (2009-2010 / 2010-2011) y otra para las registrar las transacciones, categorías y propiedades.
   
```sql
CREATE TABLE online_retail_2009_2010 (
    invoice_id VARCHAR(50),
    item_id VARCHAR(50),   
    description VARCHAR(200),
    quantity INT,
    invoice_date TIMESTAMP,
    unit_price VARCHAR(50), 
    customer_id INT,
    country VARCHAR(100)
);
#######
----- Cambié las comas por puntos en los precios -----
UPDATE online_retail_2009_2010
SET unit_price = REPLACE(unit_price, ',', '.');
----- Convertí la columna de unit_price a tipo númerico real -----
ALTER TABLE online_retail_2009_2010
ALTER COLUMN unit_price TYPE NUMERIC(10,2) USING unit_price::NUMERIC;
#######
CREATE TABLE online_retail_2010_2011 (
    invoice_id VARCHAR(50),
    item_id VARCHAR(50),   
    description VARCHAR(200),
    quantity INT,
    invoice_date TIMESTAMP,
    unit_price VARCHAR(50), 
    customer_id INT,
    country VARCHAR(100)
);
#######
----- Cambié las comas por puntos en los precios -----
UPDATE online_retail_2010_2011
SET unit_price = REPLACE(unit_price, ',', '.');
----- Convertí la columna de unit_price a tipo númerico real -----
ALTER TABLE online_retail_2010_2011
ALTER COLUMN unit_price TYPE NUMERIC(10,2) USING unit_price::NUMERIC;
----- Uní las tablas de los dos períodos -----
CREATE TABLE ventas_totales AS
SELECT * FROM online_retail_2009_2010
UNION ALL
SELECT * FROM online_retail_2010_2011;
----- Confirmé que el número total de filas sea la sumatoria de las dos tablas-----
SELECT COUNT(*) AS total_filas_proyecto FROM ventas_totales;
#######
CREATE TABLE category_trans (
category_id VARCHAR(50),
parent_id VARCHAR(50)
);
#######
CREATE TABLE events_trans (
timestamp_raw VARCHAR(100),
visitor_id VARCHAR(50),
event_type VARCHAR(50),
item_id VARCHAR(50),
transaction_id VARCHAR(50),
time_real TIMESTAMP
);
#######
CREATE TABLE proper1_trans (
timestamp_raw VARCHAR(100),
item_id VARCHAR(50),
property_name VARCHAR(100),
property_value TEXT,
prop_time VARCHAR(100)
);
----- Transformé los datos correspondientes a el tiempo -----
SELECT * FROM proper1_trans LIMIT 10;
UPDATE proper1_trans
SET proper1_trans = REPLACE(proper_time, ',', '.');
#######
CREATE TABLE proper2_trans  (
timestamp_raw VARCHAR(100),
item_id VARCHAR(50),
property_name VARCHAR(100),
property_value TEXT,
prop_time VARCHAR(100)
);
----- Transformé los datos correspondientes a el tiempo -----
SELECT * FROM proper2_trans LIMIT 10;
UPDATE proper2_trans
SET proper2_trans = REPLACE(proper_time, ',', '.');
----- Uní las tablas de los dos períodos -----
CREATE TABLE properties_totales AS
SELECT * FROM proper1_trans 
UNION ALL
SELECT * FROM proper2_trans
----- Confirmé que el número total de filas sea la sumatoria de las dos tablas-----
SELECT COUNT(*) AS properties_total FROM properties_totales;
```
2. Transformé el formato de los archivos de Excel de .xsl a .csv. para introducir el conjunto de datos a SQL, sin embargo algunos contenían una gran cantidad de datos que no se pueden transformar a ese formato directamente, para esos archivos usé DAX studio para transformar algunos de ellos (eventos y propiedades).
   
3. Realicé las consultas que me permitirán encontrar las respuestas a las preguntas de análisis planteadas.

### ¿Que producto fue el más rentable en el último período (2010-2011)?
```sql
CREATE VIEW mas_rentabilidad AS
SELECT 
    item_id,
    SUM(unit_price * quantity) AS rentabilidad_total
FROM ventas_totales
WHERE invoice_date BETWEEN '2010-01-01' AND '2011-12-31'
GROUP BY item_id
ORDER BY rentabilidad_total DESC
LIMIT 10;
```
**Resultado:** 

El item_id 22423 registró la mayor rentabilidad con un valor de $327813.65 generados.

**Observación:**

El segundo item_id con mayor rentabilidad está registrado como DOT y no con un valor numérico, por lo tanto se debe definir si es un código de algún item u otro concepto.

### ¿Que producto fue el menor rentable en el último período (2010-2011)?
```sql
CREATE VIEW menor_rentabilidad AS
SELECT 
    item_id,
SUM(unit_price * quantity) AS rentabilidad_total
FROM ventas_totales
WHERE invoice_date BETWEEN '2010-01-01' AND '2011-12-31'
GROUP BY item_id
ORDER BY rentabilidad_total ASC
LIMIT 10;
```
**Resultado:** 

El item_id registrado como AMAZONFEE representa una rentabilidad de               - $260763.58

**Observación:**

El segundo item_id con menos rentabilidad está registrado como “B” y no con un valor numérico, por lo tanto se debe definir si es un código de algún item u otro concepto.

### % Crecimiento mensual (MoM) Nov 2011 - Dic 2011

```sql
CREATE VIEW nov11dic11_mom AS
SELECT 
EXTRACT(MONTH FROM invoice_date),
SUM(unit_price*quantity) AS rentabilidad_mensual
FROM ventas_totales
#######
SELECT *,
LAG(rentabilidad_mensual) OVER (ORDER BY extract) AS rentabilidad_anterior,
((rentabilidad_mensual - LAG(rentabilidad_mensual) OVER (ORDER BY extract))*100.0)/LAG(rentabilidad_mensual) OVER (ORDER BY extract) AS growth_mensual
FROM nov11dic11_mom
```
