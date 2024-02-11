## 5. Hacer un procedimiento llamado MostrarUsuariosporTablespace que muestre por pantalla un listado de los tablespaces existentes con la lista de usuarios que tienen asignado cada uno de ellos por defecto y el número de los mismos, así:

```
Tablespace xxxx:

	Usr1
	Usr2
	...

Total Usuarios Tablespace xxxx: n1

Tablespace yyyy:

	Usr1
	Usr2
	...

Total Usuarios Tablespace yyyy: n2
....
Total Usuarios BD: nn
```

### No olvides incluir los tablespaces temporales y de undo.
```
CREATE OR REPLACE PROCEDURE MostrarUsuariosporTablespace AS
   CURSOR c_tablespaces IS
      SELECT DISTINCT tablespace_name
      FROM DBA_TABLESPACES
      UNION
      SELECT DISTINCT tablespace_name 
      FROM DBA_UNDO_EXTENTS;
    v_total_ts NUMBER;
    v_total NUMBER :=0;
BEGIN
    FOR i IN c_tablespaces LOOP
        dbms_output.put_line('Tablespace ' || i.tablespace_name || ':' || chr(10));
        mostrar_usuarios(i.tablespace_name, v_total_ts);
        dbms_output.put_line(chr(9));
        dbms_output.put_line('Total Usuarios Tablespace: ' || v_total_ts || chr(10));
        v_total := v_total + v_total_ts;
    END LOOP;
        mostrar_temp();
        mostrar_undo();
    dbms_output.put_line ('Total Usuarios BD: ' || v_total);
END;
/

CREATE OR REPLACE PROCEDURE mostrar_usuarios (p_tablespace VARCHAR2, p_total OUT NUMBER) AS
   CURSOR c_usuarios IS
      SELECT username
      FROM DBA_USERS
      WHERE default_tablespace = p_tablespace;
    v_total_ts NUMBER:=0;
BEGIN
    FOR i IN c_usuarios LOOP
        dbms_output.put_line(chr(9) || i.username );
        v_total_ts:= v_total_ts+1;
    END LOOP;
    p_total:=v_total_ts;
END;
/

CREATE OR REPLACE PROCEDURE mostrar_temp AS
    v_temp VARCHAR2;
BEGIN
    SELECT PROPERTY_VALUE INTO v_temp
    FROM DATABASE_PROPERTIES
    WHERE PROPERTY_NAME = 'DEFAULT_TEMP_TABLESPACE';

    dbms_output.put_line('Tablespace temporal por defecto: ' || v_temp);
END;
/

CREATE OR REPLACE PROCEDURE mostrar_undo AS
    v_undo VARCHAR2;
BEGIN
    SELECT DISTINCT tablespace_name INTO v_undo
    FROM DBA_UNDO_EXTENTS;

    dbms_output.put_line('Tablespace undo por defecto: ' || v_undo);
END;
/
```

```
EXEC MostrarUsuariosporTablespace
```
