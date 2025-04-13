# Implementación de un Pipeline para la Gestión de Datos en Mercadona

Mercadona, líder en el sector de supermercados en España, desea mejorar la integración y el procesamiento de los datos generados en sus almacenes y puntos de venta. La compañía enfrenta el desafío de consolidar grandes volúmenes de datos provenientes de sus almacenes regionales en un Data Warehouse (DW) centralizado, para habilitar análisis avanzados y tomar decisiones basadas en datos.
Para ello, se ha elegido una solución on-premise que permita el procesamiento de datos de inventarios y ventas. Se debe desarrollar un pipeline capaz de ingerir estos datos desde archivos CSV hacia una base de datos PostgreSQL, alojada en el Data Warehouse.

---

## Estructura del Proyecto
```
pipeline_mercadona/
├── dags/
| ├── dag_inventario.py
| ├── dag_ventas.py
| └── dag_reporte.py
├── data/
| ├── inventario.csv
| ├── ventas.csv
├── imagenes/
| ├── airflow_ui.png 
│ ├── inventario.png
│ ├── reporte_stock.png
│ ├── ventas.png
├── README.md 
├── docker-compose.yml
```
---

## Tecnologías Utilizadas

- Apache Airflow
- PostgreSQL
- Docker y Docker Compose
- Python 3
- DBeaver (opcional, para visualización de datos)

---

## Cómo levantar el entorno

1. Clona el repositorio:
   ```bash
   git clone https://github.com/tuusuario/airflow_mercadona.git
   cd airflow_mercadona
   ```
2. Levantar los contenedores con docker-compose:
  ```bash
   sudo docker-compose up -d
  ```
3. Inicializar la base de datos (solo se hace 1 vez):
  ```bash
   docker exec -it airflow-webserver-1 airflow db init
   docker exec -it airflow-webserver-1 airflow users create --username admin --firstname Admin  --lastname Airflow --role Admin --email admin@example.com --password admin

  ```
4. Acceder a la interfaz web de Airflow:
   http://localhost:8081
    Usuario: admin
    Contraseña: admin

## Funcionamiento de los DAGs
· dag_inventario.py
  Carga los datos del archivo inventario.csv a la tabla inventario.

· dag_ventas.py
  Carga los datos del archivo ventas.csv a la tabla ventas.

· dag_reporte.py
  Genera la tabla reporte_stock combinando inventario y ventas para calcular el stock final.
## Esquema de la Base de Datos
```sql
CREATE TABLE reporte_stock (
    producto VARCHAR(100),
    almacen VARCHAR(100),
    cantidad_original INTEGER,
    unidades_vendidas INTEGER,
    stock_actual INTEGER
);
CREATE TABLE ventas (
    id SERIAL PRIMARY KEY,
    producto VARCHAR(255) NOT NULL,
    unidades INT NOT NULL,
    fecha DATE NOT NULL,
    CONSTRAINT fk_producto FOREIGN KEY (producto) REFERENCES inventario(producto)
);
CREATE TABLE inventario (
    id SERIAL PRIMARY KEY,
    producto VARCHAR(255) NOT NULL,
    cantidad INT NOT NULL,
    almacen VARCHAR(255) NOT NULL,
    CONSTRAINT unique_producto UNIQUE (producto)
);
```

