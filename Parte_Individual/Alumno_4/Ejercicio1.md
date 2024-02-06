## 1. Crea un tablespace de undo e intenta crear una tabla en él.

Crearemos un tablespace de undo llamado `ts_undo_alex` con un archivo de datos inicial de 100 megabytes, que se expandirá automáticamente según sea necesario.

```sql
SQL> create undo tablespace ts_undo_alex
  2  datafile 'ts_undo_alex.dbf' size 100M autoextend ON;

Tablespace created.
```

Para ver la ruta del archivo de datos realizaremos la siguiente consulta SQL que busca información sobre los archivos de datos asociados con el tablespace que le indiquemos.
```sql
SQL> select tablespace_name, file_name
  2  from dba_data_files
  3  where tablespace_name = 'TS_UNDO_ALEX';

TABLESPACE_NAME
------------------------------
FILE_NAME
--------------------------------------------------------------------------------
TS_UNDO_ALEX
/opt/oracle/product/23c/dbhomeFree/dbs/ts_undo_alex.dbf
```

Al intentar crear una tabla manualmente en el tablespace undo nos saltará un error, pues los tablespaces de undo están reservados exclusivamente para almacenar información de transacciones (rollback).
```sql
SQL> CREATE TABLE tabla_undo (
  2      nombre VARCHAR2(50),
  3      apellido1 VARCHAR2(50),
  4      apellido2 VARCHAR2(50)
  5  ) TABLESPACE ts_undo_alex;
CREATE TABLE tabla_undo (
*
ERROR at line 1:
ORA-30022: Cannot create segments in undo tablespace
Help: https://docs.oracle.com/error-help/db/ora-30022/
```