## 5. Queremos cambiar de ubicación un tablespace, pero antes debemos avisar a los usuarios que tienen acceso de lectura o escritura a cualquiera de los objetos almacenados en el mismo. Escribe un procedimiento llamado MostrarUsuariosAccesoTS que obtenga un listado con los nombres de dichos usuarios.


Primero en este procedimiento llamado `comprobar_privilegios` comprobaremos los privilegios de los usuarios para conocer realmente que tienen acceso de lectura y escritura a los objetos del tablespace que reciba desde el otro procedimiento, recibirá un parametro de usuarion de tablespace, y devolverá un parametro indicador, que nos servirá para verificar que realmente la consulta ha devuelto más de un solo usuario que realmente tiene los permisos sobre el tablespace que se le ha pasado.

Entonces la variable del indicador empieza siendo 0, en la consulta devolverá un numero que introducirá en la variable `v_indicador` el número de usuarios que tienen los permisos indicados en el where sobre algun objeto del tablespace (`p_tablespace`), se utiliza una unión entre `dba_tab_privs` y `dba_tables` basada en el nombre de la tabla y el propietario para obtener esta información.
Mostrará el usuario que tenga directamente el privilegio o que lo reciba por un rol, además se limita a solo los privilegios de lectura y escritura dobre el tablespace que reciba con el parámetro.

```sql
create or replace procedure comprobar_privilegios(p_usuario in dba_users.username%type, p_tablespace in dba_tables.tablespace_name%type, p_indicador out number)
as
    v_indicador number := 0;
begin
    select count(*) into v_indicador
    from dba_tab_privs tp join dba_tables t on tp.table_name = t.table_name and tp.owner = t.owner
    where (tp.grantee = p_usuario 
        or tp.grantee in (select granted_role 
                            from dba_role_privs 
                            where grantee = p_usuario))
    and tp.privilege in ('SELECT', 'INSERT', 'UPDATE', 'DELETE') 
    and t.tablespace_name = p_tablespace;
    if v_indicador > 0 then
        p_indicador := 1;
    else
        p_indicador := 0;
    end if;
end;
/
```

El siguiente procedimiento, utiliza el procedimiento `comprobar_privilegios` para determinar qué usuarios tienen acceso a un tablespace específico, identificado por `p_nombre_tablespace`.
Primero, el procedimiento declara un cursor `c_usuarios` que selecciona todos los nombres de usuario de la tabla `dba_users`. Luego, en un bucle, se recorren todos los usuarios y para cada uno, se llama al procedimiento `comprobar_privilegios` para comprobar los privilegios y cumplir con la recursividad de roles anidados.
Si el indicador `v_indicador` es mayor que 0, lo que indica que el usuario tiene privilegios de lectura o escritura, se imprime el nombre de usuario.
Por lo tanto, este procedimiento imprime en pantalla la lista de usuarios que tienen acceso al tablespace especificado y que deben ser notificados si se planea cambiar la ubicación de ese tablespace.

```sql
create or replace procedure MostrarUsuariosAccesoTS(p_nombre_tablespace in varchar2) as
    cursor c_usuarios is select username from dba_users;
    v_indicador number;
begin
    dbms_output.put_line('Estos son los usuarios que debes avisar si cambias de ubicacion el tablespace ' || p_nombre_tablespace || ': ');
    for usuario in c_usuarios loop
        comprobar_privilegios(usuario.username, p_nombre_tablespace, v_indicador);
        if v_indicador > 0 then
            dbms_output.put_line(usuario.username);
        end if;
    end loop;
end;
/
```

```sql
SQL> exec MostrarUsuariosAccesoTS('USERS');
Estos son los usuarios que debes avisar si cambias de ubicacion el tablespace
USERS:
SYS
C##JUAN
C##RAUL

PL/SQL procedure successfully completed.
```