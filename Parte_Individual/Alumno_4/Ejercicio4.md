## 4. Averigua los segmentos existentes para realizar un ROLLBACK y el tamaño de sus extensiones.


La consulta SQL presentada aquí muestra los segmentos existentes que pueden ser revertidos y el tamaño de sus extensiones.
Los segmentos de ROLLBACK son una parte crucial de las operaciones de la base de datos que permiten revertir las transacciones en caso de error o fallo.


```sql
select r.segment_name, r.tablespace_name, r.status, e.extent_id, e.bytes
from DBA_ROLLBACK_SEGS r
inner join DBA_EXTENTS e on r.segment_name = e.segment_name;
```

En la consulta, las columnas `segment_name`, `tablespace_name`, `status`, `extent_id` y `bytes` se seleccionan de la unión de las vistas `DBA_ROLLBACK_SEGS` y `DBA_EXTENTS` donde los nombres de los segmentos coinciden.

- `segment_name` es el nombre del segmento de reversión.
- `tablespace_name` es el nombre del tablespace.
- `status` muestra el estado actual del segmento de reversión, que puede ser ONLINE o OFFLINE.
- `extent_id` es el identificador del extent que es una unidad lógica continua de almacenamiento de base de datos (una serie de bloques de datos) que se asigna a un segmento de reversión.
- `bytes` es el tamaño del extent en bytes.

Por ejemplo, para el segmento de ROLLBACK `_SYSSMU1_3312066994$` en el tablespace `UNDOTBS1`, tiene varios extents con identificadores y tamaños diferentes. Cada extent puede tener un tamaño diferente, como puede verse en los resultados de la consulta.

```sql
SQL> select r.segment_name, r.tablespace_name, r.status, e.extent_id, e.bytes
  2  from DBA_ROLLBACK_SEGS r
  3  inner join DBA_EXTENTS e on r.segment_name = e.segment_name;

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
SYSTEM			                  SYSTEM			      ONLINE			0      65536
SYSTEM			                  SYSTEM			      ONLINE			1      65536
SYSTEM			                  SYSTEM			      ONLINE			2      65536
SYSTEM			                  SYSTEM			      ONLINE			3      65536
SYSTEM			                  SYSTEM			      ONLINE			4      65536
SYSTEM			                  SYSTEM			      ONLINE			5      65536
SYSTEM			                  SYSTEM			      ONLINE			6      65536
_SYSSMU1_3312066994$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU1_3312066994$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU1_3312066994$	       UNDOTBS1 		      ONLINE			2    1048576
_SYSSMU1_3312066994$	       UNDOTBS1 		      ONLINE			3    1048576

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
_SYSSMU1_3312066994$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU1_3312066994$	       UNDOTBS1 		      ONLINE			5    1048576
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			2      65536
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			3    1048576
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			5    1048576
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			6    1048576
_SYSSMU2_2822416537$	       UNDOTBS1 		      ONLINE			7    1048576
_SYSSMU3_4114179988$	       UNDOTBS1 		      ONLINE			0      65536

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
_SYSSMU3_4114179988$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU3_4114179988$	       UNDOTBS1 		      ONLINE			2    1048576
_SYSSMU3_4114179988$	       UNDOTBS1 		      ONLINE			3    1048576
_SYSSMU3_4114179988$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU3_4114179988$	       UNDOTBS1 		      ONLINE			5    1048576
_SYSSMU4_3899933493$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU4_3899933493$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU4_3899933493$	       UNDOTBS1 		      ONLINE			2      65536
_SYSSMU4_3899933493$	       UNDOTBS1 		      ONLINE			3    1048576
_SYSSMU4_3899933493$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU4_3899933493$	       UNDOTBS1 		      ONLINE			5    1048576

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			2      65536
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			3    1048576
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			5    1048576
_SYSSMU5_1547618400$	       UNDOTBS1 		      ONLINE			6    1048576
_SYSSMU6_1206432686$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU6_1206432686$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU6_1206432686$	       UNDOTBS1 		      ONLINE			2    1048576
_SYSSMU6_1206432686$	       UNDOTBS1 		      ONLINE			3    1048576

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
_SYSSMU7_3576269398$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU7_3576269398$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU7_3576269398$	       UNDOTBS1 		      ONLINE			2      65536
_SYSSMU7_3576269398$	       UNDOTBS1 		      ONLINE			3      65536
_SYSSMU7_3576269398$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU8_3945328337$	       UNDOTBS1 		      ONLINE			0      65536
_SYSSMU8_3945328337$	       UNDOTBS1 		      ONLINE			1      65536
_SYSSMU8_3945328337$	       UNDOTBS1 		      ONLINE			2    1048576
_SYSSMU8_3945328337$	       UNDOTBS1 		      ONLINE			3    1048576
_SYSSMU8_3945328337$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU8_3945328337$	       UNDOTBS1 		      ONLINE			5    1048576

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			0      65536
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			1      65536
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			2    1048576
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			3    1048576
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			5    1048576
_SYSSMU9_325388135$	        UNDOTBS1 		      ONLINE			6      65536
_SYSSMU10_1642079148$	      UNDOTBS1 		      ONLINE			0      65536
_SYSSMU10_1642079148$	      UNDOTBS1 		      ONLINE			1      65536
_SYSSMU10_1642079148$	      UNDOTBS1 		      ONLINE			2    1048576
_SYSSMU10_1642079148$	      UNDOTBS1 		      ONLINE			3    1048576

SEGMENT_NAME		       TABLESPACE_NAME		      STATUS		EXTENT_ID      BYTES
------------------------------ ------------------------------ ---------------- ---------- ----------
_SYSSMU10_1642079148$	       UNDOTBS1 		      ONLINE			4    1048576
_SYSSMU10_1642079148$	       UNDOTBS1 		      ONLINE			5    1048576
_SYSSMU10_1642079148$	       UNDOTBS1 		      ONLINE			6    1048576

69 rows selected.

```