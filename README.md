# Proyecto de ETL con Apache Airflow y PostgreSQL - Pipeline Mercadona

Este proyecto simula un pipeline de datos on-premise para una cadena de supermercados (Mercadona), usando **Apache Airflow** para orquestación de tareas y **PostgreSQL** como base de datos. Se procesan archivos CSV de inventario y ventas, se transforman los datos y se generan reportes consolidados.

---

## Estructura del Proyecto

pipeline_mercadona/
├── README.md 
├── docker-compose.yaml
├── dags/
| ├── dag_inventario.py
| ├── dag_ventas.py
| └── dag_reporte.py
├── imagenes/
| ├── airflow_ui.png 
│ ├── dbeaver.png

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
  ```
4. Acceder a la interfaz web de Airflow:
   http://localhost:8080
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

