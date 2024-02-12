## 6. Realizad un procedimiento llamado BalanceoCargaTemp que balancee la carga de usuarios entre los tablespaces temporales existentes. Para ello averiguará cuántos existen y asignará los usuarios entre ellos de forma equilibrada. Si es necesario para comprobar su funcionamiento, crea tablespaces temporales nuevos.

```
CREATE OR REPLACE PROCEDURE BalanceoCargaTemp AS
    CURSOR c_tablespaces IS
        SELECT tablespace_name
        FROM dba_tablespaces
        WHERE contents = 'TEMPORARY'
        AND status = 'ONLINE';
    CURSOR c_usuarios IS
        SELECT DISTINCT u.username 
        FROM dba_users u, dba_tablespaces t
        WHERE u.account_status = 'OPEN'
        AND u.temporary_tablespace = t.tablespace_name;
    v_usuarios c_usuarios%ROWTYPE;
BEGIN
    open c_usuarios;
    FOR i IN c_tablespaces LOOP
        FETCH c_usuarios into v_usuarios;
        asociar_tablespace(v_usuarios.username, i.tablespace_name);
        IF c_tablespaces%NOTFOUND THEN
            CLOSE c_usuarios;
            OPEN c_usuarios;
        END IF;
    END LOOP;
END;
/

CREATE or REPLACE PROCEDURE asociar_tablespace (p_usuario VARCHAR2, p_tablespace VARCHAR2)
AS
BEGIN
    EXECUTE IMMEDIATE 'alter user ' || p_usuario || ' temporary tablespace ' || p_tablespace;
END;
/
```

```
CREATE TEMPORARY TABLESPACE temp1 TEMPFILE 'temp1' SIZE 30M AUTOEXTEND ON;
CREATE TEMPORARY TABLESPACE temp2 TEMPFILE 'temp2' SIZE 30M AUTOEXTEND ON;
CREATE TEMPORARY TABLESPACE temp3 TEMPFILE 'temp3' SIZE 30M AUTOEXTEND ON;
CREATE TEMPORARY TABLESPACE temp4 TEMPFILE 'temp4' SIZE 30M AUTOEXTEND ON;

EXEC BalanceoCargaTemp;

SELECT temporary_tablespace
FROM dba_users
WHERE username = ' ';
```