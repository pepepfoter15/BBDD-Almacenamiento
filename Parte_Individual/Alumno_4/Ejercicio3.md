## 3. Borra todos los tablespaces creados para esta práctica sin que quede rastro de ellos. Realiza las acciones previas que sean necesarias.

Los comandos DROP TABLESPACE se utilizan para eliminar un tablespace. La cláusula INCLUDING CONTENTS AND DATAFILES asegura que se eliminen tanto los datos que contiene el tablespace como los archivos de datos físicos asociados a él.

```sql
drop tablespace (nombre_ts) including contents and datafiles;
```

En este caso, los comandos están eliminando los tablespaces llamados ts_undo_alex y temp2 y sus respectivos datos y archivos de datos.

```sql
SQL> drop tablespace ts_undo_alex including contents and datafiles;

Tablespace dropped.

SQL> drop tablespace temp2 including contents and datafiles;

Tablespace dropped
```