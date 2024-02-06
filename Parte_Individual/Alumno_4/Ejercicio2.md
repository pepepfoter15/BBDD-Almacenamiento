## 2. Crea un tablespace temporal TEMP2 y escribe una sentencia SQL que genere un script que haga usar TEMP2 a todos los usuarios que tienen USERS como tablespace por defecto.

El siguiente comando crea un nuevo tablespace temporal llamado `TEMP2`.
Este tablespace temporal se utiliza para almacenar datos temporales generados durante la ejecución de consultas SQL. Se crea con un archivo de datos llamado 'temp2.dbf' y se le asigna un tamaño inicial de 20MB. La opción `AUTOEXTEND ON` indica que cuando el archivo de datos se llena, se expandirá automáticamente según sea necesario.

```sql
SQL> CREATE TEMPORARY TABLESPACE TEMP2
  2  TEMPFILE 'temp2.dbf' SIZE 20M AUTOEXTEND ON;

Tablespace created.
```

La consulta selecciona todos los usuarios de la base de datos cuyo tablespace por defecto es 'USERS'. Para cada uno de estos usuarios, genera un comando SQL que cambia su tablespace temporal a 'TEMP2'.

```sql
SQL> select 'ALTER USER ' || username || ' TEMPORARY TABLESPACE TEMP2;' as script_temp2
  2  from dba_users
  3  where default_tablespace = 'USERS';

SCRIPT_TEMP2
--------------------------------------------------------------------------------
ALTER USER C##USRPRACTICA TEMPORARY TABLESPACE TEMP2;
ALTER USER SYSRAC TEMPORARY TABLESPACE TEMP2;
ALTER USER USUARIOEXAMEN TEMPORARY TABLESPACE TEMP2;
ALTER USER C##ADMIN TEMPORARY TABLESPACE TEMP2;
ALTER USER C##ALEX TEMPORARY TABLESPACE TEMP2;
ALTER USER C##RAUL TEMPORARY TABLESPACE TEMP2;
ALTER USER C##PEPE TEMPORARY TABLESPACE TEMP2;
ALTER USER C##CARLOS TEMPORARY TABLESPACE TEMP2;
ALTER USER C##JUANMA TEMPORARY TABLESPACE TEMP2;
ALTER USER C##JAVIER TEMPORARY TABLESPACE TEMP2;
ALTER USER EXAMENALEX TEMPORARY TABLESPACE TEMP2;

SCRIPT_TEMP2
--------------------------------------------------------------------------------
ALTER USER GSMCATUSER TEMPORARY TABLESPACE TEMP2;
ALTER USER MDDATA TEMPORARY TABLESPACE TEMP2;
ALTER USER REMOTE_SCHEDULER_AGENT TEMPORARY TABLESPACE TEMP2;
ALTER USER SYSBACKUP TEMPORARY TABLESPACE TEMP2;
ALTER USER GSMUSER TEMPORARY TABLESPACE TEMP2;
ALTER USER GSMROOTUSER TEMPORARY TABLESPACE TEMP2;
ALTER USER DIP TEMPORARY TABLESPACE TEMP2;
ALTER USER DGPDB_INT TEMPORARY TABLESPACE TEMP2;
ALTER USER SYSKM TEMPORARY TABLESPACE TEMP2;
ALTER USER SYS$UMF TEMPORARY TABLESPACE TEMP2;
ALTER USER SYSDG TEMPORARY TABLESPACE TEMP2;

22 rows selected.
```