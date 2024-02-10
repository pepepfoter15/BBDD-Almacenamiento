## 8. Averigua si pueden establecerse claúsulas de almacenamiento para las tablas o los espacios de tablas en MySQL.

En MySQL existe el concepto de almacén de tablas para los datos de la base de datos, y como los otros sistemas, nos permite tener los datos en varias discos físicos distintos para mejorar exponencialmente su rendimiento y escalabilidad. Además, los usuarios pueden crear uno o más almacenes de tablas para cada base de datos y asignar uan tabla o espacio de tablas a un almacén de tablas. 

Cuando creamos las tablas, podemos elegir parámetros que nos permiten determinar muchos parámetros, entre otros son la cantidad máxima de registros, el archivo donde se guardará la tabla (con su ruta completa) e incluso, podemos encriptar el contenido de ese almacén de tablas.

Para poder entenderlo, podemos ver la ayuda en MySQL mediante este comando:

```sql
HELP CREATE TABLE
```

Dentro de la devolución de este comando, nos devolverá la sintaxis siguiente:

- Para crear una tabla con todas sus opciones que nos permiten crear o modificar esta tabla a nuestro gusto:

```sql
table_option: 
 [STORAGE] ENGINE [=] engine_name
 | AUTO_INCREMENT [=] value
 | AVG_ROW_LENGTH [=] value
 | [DEFAULT] CHARACTER SET [=] charset_name
 | CHECKSUM [=] {0 | 1}
 | [DEFAULT] COLLATE [=] collation_name
 | COMMENT [=] 'string'
 | CONNECTION [=] 'connect_string'
 | DATA DIRECTORY [=] 'absolute path to directory'
 | DELAY_KEY_WRITE [=] {0 | 1}
 | ENCRYPTED [=] {YES | NO}
 | ENCRYPTION_KEY_ID [=] value
 | IETF_QUOTES [=] {YES | NO}
 | INDEX DIRECTORY [=] 'absolute path to directory'
 | INSERT_METHOD [=] { NO | FIRST | LAST }
 | KEY_BLOCK_SIZE [=] value
 | MAX_ROWS [=] value
 | MIN_ROWS [=] value
 | PACK_KEYS [=] {0 | 1 | DEFAULT}
 | PAGE_CHECKSUM [=] {0 | 1}
 | PAGE_COMPRESSED [=] {0 | 1}
 | PAGE_COMPRESSION_LEVEL [=] {0 .. 9}
 | PASSWORD [=] 'string'
 | ROW_FORMAT [=]
{DEFAULT|DYNAMIC|FIXED|COMPRESSED|REDUNDANT|COMPACT|PAGE}
 | SEQUENCE [=] {0|1}
 | STATS_AUTO_RECALC [=] {DEFAULT|0|1}
 | STATS_PERSISTENT [=] {DEFAULT|0|1}
 | STATS_SAMPLE_PAGES [=] {DEFAULT|value}
 | TABLESPACE tablespace_name
 | TRANSACTIONAL [=] {0 | 1}
 | UNION [=] (tbl_name[,tbl_name]...)
 | WITH SYSTEM VERSIONING
```

- Para definir las opciones generales para definir cómo se va a particionar la tabla en sí misma:

```sql
partition_options:
 PARTITION BY
 { [LINEAR] HASH(expr)
 | [LINEAR] KEY(column_list)
 | RANGE(expr)
 | LIST(expr)
 | SYSTEM_TIME [INTERVAL time_quantity time_unit] [LIMIT
num] }
 [PARTITIONS num]
 [SUBPARTITION BY
 { [LINEAR] HASH(expr)
 | [LINEAR] KEY(column_list) }
 [SUBPARTITIONS num]
 ]
 [(partition_definition [, partition_definition] ...)]
```

- Para definir las características específicas de cada partición en la tabla:

```sql
partition_definition:
 PARTITION partition_name
 [VALUES {LESS THAN {(expr) | MAXVALUE} | IN (value_list)}]
 [[STORAGE] ENGINE [=] engine_name]
 [COMMENT [=] 'comment_text' ]
 [DATA DIRECTORY [=] 'data_dir']
 [INDEX DIRECTORY [=] 'index_dir']
 [MAX_ROWS [=] max_number_of_rows]
 [MIN_ROWS [=] min_number_of_rows]
 [TABLESPACE [=] tablespace_name]
 [NODEGROUP [=] node_group_id]
 [(subpartition_definition [, subpartition_definition] ...)]
```

- Para definir las características específicas de una subpartición dentro de una partición:

```sql
subpartition_definition:
 SUBPARTITION logical_name
 [[STORAGE] ENGINE [=] engine_name]
 [COMMENT [=] 'comment_text' ]
 [DATA DIRECTORY [=] 'data_dir']
 [INDEX DIRECTORY [=] 'index_dir']
 [MAX_ROWS [=] max_number_of_rows]
 [MIN_ROWS [=] min_number_of_rows]
 [TABLESPACE [=] tablespace_name]
 [NODEGROUP [=] node_group_id]
```

Aunque es posible definir tablas en MySQL, la funcionalidad de tablespaces no está disponible de manera estándar. Para acceder a esta característica, se necesita una versión de alta disponibilidad en clúster de MySQL, llamada **MySQL NDB**.

La sintaxis para crear tablespaces en este caso es la siguiente:

```sql
CREATE TABLESPACE nombre_ts

  InnoDB and NDB:
    ADD DATAFILE 'nombre_archivo'

  InnoDB only:
    [FILE_BLOCK_SIZE = value]

  NDB only:
    USE LOGFILE GROUP logfile_group
    [EXTENT_SIZE [=] extent_size]
    [INITIAL_SIZE [=] initial_size]
    [AUTOEXTEND_SIZE [=] autoextend_size]
    [MAX_SIZE [=] max_size]
    [NODEGROUP [=] nodegroup_id]
    [WAIT]
    [COMMENT [=] 'string']

  InnoDB and NDB:
    [ENGINE [=] engine_name]
```

Tras ver las instrucciones para crear un tablespace en MySQL, tenemos que entender cada una de las secciones de este comando.

1. `CREATE TABLESPACE nombre_ts`: Crea un tablespace con un nombre que queramos.

2. `InnoDB and NDB`: Esta sección indica que las siguientes opciones son aplicables tanto a los motores de almacenamiento InnoDB como a NDB.

3. `InnoDB only`: Esta sección indica que las siguientes opciones son específicas del motor de almacenamiento InnoDB y no son aplicables a NDB.

4. `NDB only`: Esta sección indica que las siguientes opciones son específicas de NDB y no son aplicables a InnoDB.

5. `InnoDB and NDB`: Esta sección indica que las siguientes opciones son aplicables tanto a InnoDB como a NDB.


**MySQL NDB** ofrece un almacenamiento en memoria mejor distribuido, muy escalable y de alta disponibilidad, capaz de manejar cargas de trabajo y replicar datos para mejorar la disponibilidad en nuestra base de datos. Es el mejor para entornos de producción críticos. 

Por otro lado, **MariaDB** ha eliminado completamente NDB desde la versión 10.1, ya que no estaba presente en las principales versiones de MySQL. Aunque estaba disponible como descarga separada, ahora no se incluye en MariaDB debido a su menor frecuencia de actualización en comparación con la versión base de MySQL.