# Proyecto Power BI: Análisis E-Commerce (Modelo Estrella)

Este repositorio contiene el proyecto de Power BI (`brazzilian_report.pbix`) diseñado para el análisis del rendimiento de ventas, logística, clientes y vendedores de un comercio electrónico.

---

## 🛠️ Configuración e Instalación

Para que las consultas dinámicas funcionen correctamente en cualquier entorno de trabajo o equipo, los orígenes de datos están parametrizados mediante una variable de ruta global denominada **`RutaBase`**.

### **Pasos para configurar la ruta de archivos local:**

1. **Clonar/Descargar** este repositorio en tu equipo.
2. Abrir el archivo `.pbix` en **Power BI Desktop**.
3. En la pestaña **Inicio (Home)**, haz clic en **Transformar datos** para abrir el editor de **Power Query**.
4. En el panel lateral izquierdo (*Consultas / Queries*), selecciona el parámetro **`RutaBase`**.
5. Modifica el **Valor actual (Current Value)** asignando la ruta absoluta del directorio local donde se encuentran almacenados tus archivos `.csv` (asegúrate de incluir la barra diagonal final `\`):
   ```text
   C:\TuRutaLocal\datawarehouse\
   ```
6. Haz clic en **Cerrar y aplicar (Close & Apply)**.

> **Nota:** La carga de datos combina dinámicamente `{RutaBase} + dim_products.csv`, `{RutaBase} + dim_customers.csv`, etc., lo que permite una actualización limpia sin romper las conexiones.

---

## 📐 Modelo de Datos

El modelo está estructurado bajo una **Arquitectura en Estrella (Star Schema)** centrada en transacciones de órdenes, optimizada para rendimiento y análisis analítico.

```
                  ┌─────────────────┐
                  │   dim_product   │
                  └────────┬────────┘
                           │ (1)
                           │
                           │ (*)
┌────────────────┐        ┌┴────────────────┐        ┌────────────────┐
│  dim_customer  ├───────►│   fact_orders   │◄───────┤   dim_seller   │
└────────────────┘ (1) (*)└────────────────┘(*)  (1) └────────────────┘
                           │ (*)
                           │
                           │ (1)
                  ┌────────┴────────┐
                  │    dim_date     │
                  └─────────────────┘
```

### **Tablas de Dimensiones (`dim_*`)**

- **`dim_product`**: Información detallada de productos (categorías, dimensiones, peso, fotos).
- **`dim_customer`**: Datos de clientes (nombre, ubicación geográfica, ciudad, estado, identificadores únicos).
- **`dim_seller`**: Datos de vendedores y proveedores (ubicación, estado, identificadores).
- **`dim_date`**: Dimensión de tiempo extendida (año, semestre, trimestre, mes, día de la semana, día del año, fin de mes/semana).
- **`dim_geolocation`**: Tabla auxiliar con coordenadas de latitudes y longitudes para análisis espacial.

### **Tabla de Hechos (`fact_orders`)**

Registra los eventos de compra y transacciones de pedidos. Contiene claves foráneas, fechas de hitos clave de entrega, valores numéricos de artículos, precios, fletes, valores de pago y métricas de reseñas.

---

## 📊 Medidas y Métricas Dax (`Metricas`)

Todas las métricas de negocio están organizadas dentro de la tabla dedicada **`Metricas`**:

| Nombre de la Medida | Descripción |
| :--- | :--- |
| **Ventas Totales** | Suma total del valor de las ventas generadas. |
| **Total Pedidos** | Conteo total de órdenes procesadas. |
| **Total Items Vendidos** | Cantidad total de artículos comercializados. |
| **Ticket Promedio** | Valor promedio consumido por transacción/pedido. |
| **Promedio Cuotas** | Número promedio de cuotas/pagos por compra. |
| **Flete Total** | Importe global acumulado por costos de transporte. |
| **Ingreso Total** | Recaudación total acumulada de transacciones. |
| **Clientes Activos** | Número de clientes únicos con transacciones realizadas. |
| **Vendedores Activos** | Cantidad de vendedores únicos que generaron ventas. |
| **% Entregas a Tiempo** | Porcentaje de pedidos entregados dentro de la fecha estimada. |
| **Dias Promedio Entrega** | Tiempo promedio transcurrido desde la compra hasta la entrega final. |
| **Promedio Dias Retraso** | Días promedio de desfase en entregas fuera de plazo. |
| **Score Promedio** | Calificación media asignada por los clientes en las reseñas. |
| **Total Reseñas** | Cantidad total de evaluaciones de clientes recibidas. |
| **Items por Pedido** | Promedio de artículos contenidos por cada orden realizada. |

---

## 📁 Estructura del Repositorio

```text
.
├── brazzilian_report.pbix     # Archivo principal de Power BI
├── datawarehouse/             # Directorio con los archivos origen (.csv)
│   ├── dim_date.csv
│   ├── dim_products.csv
│   ├── dim_customers.csv
│   ├── dim_sellers.csv
│   ├── dim_geolocation.csv
│   └── fact_orders.csv
└── README.md                  # Documentación del proyecto
```