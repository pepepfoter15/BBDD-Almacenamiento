## 7. Averigua si pueden establecerse claúsulas de almacenamiento para las tablas o los espacios de tablas en Postgres.


En PostgreSQL, aunque no se utilizan cláusulas de almacenamiento como en Oracle, existen otras formas de controlar el espacio, gestionar tablespaces y crear índices. A continuación, ejemplos de comandos para cada uno de estos aspectos:

**Controlar el espacio**:
- Monitorear el tamaño de una tabla específica:
```sql
postgres=# select pg_size_pretty(pg_total_relation_size('camion'));
pg_size_pretty 
----------------
 8192 bytes
(1 row)
```
- Monitorear el tamaño de todas las tablas en una base de datos:
```sql
SELECT table_name, pg_size_pretty(pg_total_relation_size(table_name))
FROM information_schema.tables
WHERE table_schema = 'public';
```

**Gestionar tablespaces**:
- Crear un nuevo tablespace:
```sql
postgres=# CREATE TABLESPACE datos LOCATION '/var/lib/postgresql/datos';
CREATE TABLESPACE
```
- Asignar una tabla a un tablespace específico:
```sql
postgres=# CREATE TABLE clientes (
    id DECIMAL PRIMARY KEY,
    nombre VARCHAR(100),
    apellido VARCHAR(100),
    email VARCHAR(100),
    telefono VARCHAR(20)) TABLESPACE datos;
CREATE TABLE
```
- Cambiar el tablespace de una tabla existente:
```sql
postgres=# ALTER TABLE clientes SET TABLESPACE datos;
ALTER TABLE
```

**Crear índices**:
- Crear un índice simple en una columna:
```sql
postgres=# CREATE INDEX idx_clientes ON clientes (nombre);
CREATE INDEX
```

Estos ejemplos de comandos en PostgreSQL nos permiten controlar el espacio, gestionar tablespaces y crear índices de manera efectiva dentro de la base de datos. Aunque la sintaxis es diferente a la de Oracle, los conceptos son similares y almenos nos ofrecen un control sobre la gestión del almacenamiento y la optimización del rendimiento en PostgreSQL.