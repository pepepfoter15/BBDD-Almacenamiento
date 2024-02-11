## 8. Averigua si existe el concepto de índice en MySQL y si coincide con el existente en ORACLE. Explica los distintos tipos de índices existentes.

Un índice es útil para una búsqueda rápida de filas. En lugar de que MySQL escanee toda la tabla, el índice ayuda a encontrar las posiciones correctas de las filas que desea.
Se puede agregar un índice cuando crea una tabla o añadir más a las tablas existentes al modificarlas. En la última sección de este artículo, verá ejemplos de estos.

MySQL utiliza un optimizador de consultas para decidir el mejor índice a utilizar en una consulta. O si es más rápido utilizar el escaneo de tablas. Por eso, es importante elegir las columnas correctas para un índice.

### Tipos de índices en MySQL
Los índices en MySQL son estructuras de datos que aceleran las consultas al organizar los datos de una tabla de forma eficiente. Al usar índices, el servidor MySQL puede encontrar rápidamente los datos que necesita sin tener que escanear toda la tabla.

#### 1. Índice PRIMARY:

- No permite valores nulos.
- Se utiliza para identificar de forma única cada fila de la tabla.
Se crea automáticamente al definir una columna como PRIMARY KEY.

```sql
CREATE TABLE ej1 (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(255) NOT NULL,
  edad INT NOT NULL
);
```

#### 2. Índice UNIQUE:

- Permite valores nulos en una sola fila.
- No permite valores duplicados en las columnas que forman el índice.
- Se puede crear con la palabra clave UNIQUE.

```sql
CREATE TABLE ej2 (
  id INT PRIMARY KEY AUTO_INCREMENT,
  correo VARCHAR(255) UNIQUE NOT NULL,
  contrasena VARCHAR(255) NOT NULL
);
```

#### 3. Índice INDEX (No único):

- Permite valores duplicados en las columnas que forman el índice.
- Se puede crear con la palabra clave INDEX.

```sql
CREATE TABLE ej3 (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(255) NOT NULL,
  categoria VARCHAR(255) NOT NULL,
  precio DECIMAL(10,2) NOT NULL,
  INDEX (categoria)
);
```

#### 4. Índice FULLTEXT:

- Se utiliza para realizar búsquedas de texto completo.
- Solo se puede crear en columnas de tipo CHAR, VARCHAR o TEXT.
- Se crea con la palabra clave FULLTEXT.

```sql
CREATE TABLE ej4 (
  id INT PRIMARY KEY AUTO_INCREMENT,
  titulo VARCHAR(255) NOT NULL,
  contenido TEXT NOT NULL,
  FULLTEXT (titulo, contenido)
);
```


#### Comandos
##### Crear un índice
```sql
CREATE INDEX nombre_idx ON tabla (nombre);
```

```sql
MariaDB [prac]> CREATE INDEX ix_ej1 ON ej1 (edad);
Query OK, 0 rows affected (0,046 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
##### Modificar un índice
```sql
ALTER TABLE tabla DROP INDEX nombre_idx;

ALTER TABLE tabla ADD INDEX nombre_idx (columna);
```

```sql
MariaDB [prac]> ALTER TABLE ej2 DROP INDEX correo;
Query OK, 0 rows affected (0,033 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [prac]> ALTER TABLE ej2 ADD INDEX idx_passwd (contrasena);
Query OK, 0 rows affected (0,037 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
##### Eliminar un índice
```sql
DROP INDEX nombre_idx ON tabla;
```

```sql
MariaDB [prac]> DROP INDEX categoria ON ej3;
Query OK, 0 rows affected (0,027 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
##### Mostrar información sobre los índices
```sql
SHOW INDEXES FROM tabla;
```

```sql
MariaDB [prac]> show indexes from ej1\G;
*************************** 1. row ***************************
        Table: ej1
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: id
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
      Ignored: NO
*************************** 2. row ***************************
        Table: ej1
   Non_unique: 1
     Key_name: ix_ej1
 Seq_in_index: 1
  Column_name: edad
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
      Ignored: NO
2 rows in set (0,003 sec)



MariaDB [prac]> show indexes from ej2\G;
*************************** 1. row ***************************
        Table: ej2
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: id
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
      Ignored: NO
*************************** 2. row ***************************
        Table: ej2
   Non_unique: 1
     Key_name: idx_passwd
 Seq_in_index: 1
  Column_name: contrasena
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
      Ignored: NO
2 rows in set (0,002 sec)



MariaDB [prac]> show indexes from ej3\G;
*************************** 1. row ***************************
        Table: ej3
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: id
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
      Ignored: NO
1 row in set (0,002 sec)



MariaDB [prac]> show indexes from ej4\G;
*************************** 1. row ***************************
        Table: ej4
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: id
    Collation: A
  Cardinality: 0
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
      Ignored: NO
*************************** 2. row ***************************
        Table: ej4
   Non_unique: 1
     Key_name: titulo
 Seq_in_index: 1
  Column_name: titulo
    Collation: NULL
  Cardinality: NULL
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: FULLTEXT
      Comment: 
Index_comment: 
      Ignored: NO
*************************** 3. row ***************************
        Table: ej4
   Non_unique: 1
     Key_name: titulo
 Seq_in_index: 2
  Column_name: contenido
    Collation: NULL
  Cardinality: NULL
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: FULLTEXT
      Comment: 
Index_comment: 
      Ignored: NO
3 rows in set (0,001 sec)
```
#### Conclusión
Los índices pueden mejorar el rendimiento de las consultas, pero también pueden aumentar el espacio que ocupa la tabla en disco. Es importante crear índices solo en las columnas que se utilizan con frecuencia en las consultas. Se debe monitorizar el rendimiento de las consultas para verificar si los índices están funcionando correctamente.
