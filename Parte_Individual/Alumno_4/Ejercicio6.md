## 6. Realiza un procedimiento llamado MostrarInfoTabla que reciba el nombre de una tabla y muestre la siguiente información sobre la misma: propietario, usuarios que pueden leer sus datos, usuarios que pueden cambiar (insertar, modificar o eliminar) sus datos, usuarios que pueden modificar su estructura, usuarios que pueden eliminarla, lista de extensiones y en qué fichero de datos se encuentran.

El procedimiento principal "MostrarInfoTabla" recibe el nombre de una tabla como parámetro. Con este nombre de tabla, el procedimiento principal llamará a una serie de subprocedimientos para mostrar diferentes tipos de información sobre la tabla.

1. **mostrar_propietario**: muestra el propietario de la tabla.
2. **mostrar_usuarios_leer**: muestra los usuarios que tienen permisos de lectura en la tabla.
3. **mostrar_usuarios_mod**: muestra los usuarios que tienen permisos para modificar los datos de la tabla.
4. **mostrar_usuarios_alt**: muestra los usuarios que tienen permisos para alterar la estructura de la tabla.
5. **mostrar_usuarios_del**: muestra los usuarios que tienen permisos para eliminar la tabla.
6. **mostrar_extensiones_fichero**: muestra las extensiones de la tabla, su ID y el fichero.

Cada subprocedimiento usa consultas SQL para consultar la información relevante de las vistas de diccionario de datos de Oracle, como "all_tables", "dba_tab_privs", "dba_role_privs", "dba_sys_privs", "dba_users", "dba_extents", y "dba_data_files".

```sql
create or replace procedure MostrarInfoTabla (p_tabla in varchar2)
as
    v_propietario varchar2(50);
    v_tabla varchar2(50);
begin
    v_tabla := upper(p_tabla);
    dbms_output.put_line('Informacion sobre la tabla '|| upper(v_tabla));
    mostrar_propietario(v_tabla, v_propietario);
    dbms_output.put_line(chr(1));
    mostrar_usuarios_leer(v_tabla);
    dbms_output.put_line(chr(1));
    mostrar_usuarios_mod(v_tabla);
    dbms_output.put_line(chr(1));
    mostrar_usuarios_alt(v_tabla);
    dbms_output.put_line(chr(1));
    mostrar_usuarios_del;
    dbms_output.put_line(chr(1));
    mostrar_extensiones_fichero(v_tabla, v_propietario);
end;
/
```


El procedimiento `mostrar_propietario` es un subprocedimiento que muestra el propietario de la tabla especificada. Recibe el nombre de la tabla como un parámetro de entrada (`p_tabla`) y devuelve el nombre del propietario de la tabla como un parámetro de salida (`p_propietario`).

Este procedimiento utiliza una consulta SQL para buscar el propietario de la tabla en la vista de diccionario de datos `all_tables` de Oracle, que contiene información sobre todas las tablas en la base de datos de Oracle. La consulta selecciona el valor de la columna `owner` de la fila donde el nombre de la tabla coincide con el parámetro de entrada.

El nombre del propietario de la tabla se almacena en una variable local (`v_propietario`), que se imprime en la salida. Finalmente, el valor de `v_propietario` se asigna al parámetro de salida `p_propietario` para que los demás procedimientos puedan usarlos si fuera necesario.

```sql
create or replace procedure mostrar_propietario(p_tabla in varchar2, p_propietario out varchar2)
as
    v_propietario varchar2(50);
begin
    select owner into v_propietario
    from all_tables
    where table_name = p_tabla;
    dbms_output.put_line('El propietario de la tabla es '|| v_propietario);
    p_propietario := v_propietario;
end;
/
```

El procedimiento `comprobar_privilegios_select` comprueba si un usuario específico tiene privilegios de `SELECT` en una tabla. Este procedimiento lo usamos para determinar qué usuarios tienen acceso de lectura a los datos de una tabla. El procedimiento toma dos parámetros de entrada: el nombre del usuario (`p_usuario`) y el nombre de la tabla (`p_tabla`).

En el procedimiento, se declara un indicador (`v_indicador`) a 0. Luego se ejecuta una consulta SQL que cuenta el número de registros en las vistas de diccionario de datos `dba_tab_privs` y `dba_tables` de Oracle, donde el `p_usuario` coincide con el nombre del receptor del privilegio, y el nombre de la tabla coincide con el nombre de la tabla del parámetro recibido (`p_tabla`). Si este número es mayor que 0, significa que el usuario tiene privilegios de `SELECT` en la tabla, y el indicador se establece en 1.
De lo contrario, el indicador permanece en 0.
El valor del indicador se devuelve como un parámetro de salida (`p_indicador`), que puede ser usado por otros procedimientos para determinar si un usuario tiene privilegios de `SELECT` en una tabla.

El procedimiento `mostrar_usuarios_leer` muestra los nombres de los usuarios que tienen privilegios de `SELECT` en una tabla. Este procedimiento utiliza el procedimiento `comprobar_privilegios_select` para determinar qué usuarios tienen acceso de lectura a los datos de una tabla.

Toma un parámetro de entrada, el nombre de una tabla (`p_tabla`). Se define un cursor (`c_usuarios`) que selecciona todos los nombres de usuario de la vista de diccionario de datos `dba_users` de Oracle. Luego, en un bucle FOR, el procedimiento recorre todos los nombres de usuario y para cada nombre de usuario, el procedimiento llama a `comprobar_privilegios_select`, pasando el nombre de usuario y el nombre de la tabla como parámetros.
Si `comprobar_privilegios_select` devuelve 1, indica que el usuario tiene privilegios de `SELECT` en la tabla, entonces el nombre del usuario se imprime.

```sql
create or replace procedure comprobar_privilegios_select(p_usuario in varchar2, p_tabla in varchar2, p_indicador out number)
as
    v_indicador number := 0;
begin
    select count(*) into v_indicador
    from dba_tab_privs tp join dba_tables t on tp.table_name = t.table_name and tp.owner = t.owner
    where (tp.grantee = p_usuario or tp.grantee in (select granted_role from dba_role_privs where grantee = p_usuario))
    and tp.privilege = 'SELECT' and t.table_name = p_tabla;
    if v_indicador > 0 then
        p_indicador := 1;
    else
        p_indicador := 0;
    end if;
end;
/



create or replace procedure mostrar_usuarios_leer(p_tabla in varchar2) as
    cursor c_usuarios is select username from dba_users;
    v_indicador number;
begin
    dbms_output.put_line('Usuarios con permisos de lectura:');
    for usuario in c_usuarios loop
        comprobar_privilegios_select(usuario.username, p_tabla, v_indicador);
        if v_indicador > 0 then
            dbms_output.put_line(usuario.username);
        end if;
    end loop;
end;
/
```

(Es similar al anterior)
El procedimiento `comprobar_privilegios_mod` se encarga de verificar si un usuario específico tiene privilegios para modificar una tabla, es decir, si tiene permisos de `INSERT`, `UPDATE` o `DELETE` sobre la tabla indicada. Este procedimiento toma dos parámetros de entrada: el nombre del usuario (`p_usuario`) y el nombre de la tabla (`p_tabla`).

Dentro del procedimiento, se inicializa un indicador (`v_indicador`) en 0. Luego, se ejecuta una consulta SQL que cuenta el número de registros en las vistas de diccionario de datos `dba_tab_privs` y `dba_tables` de Oracle. Esta consulta busca coincidencias donde el `p_usuario` tenga permisos de modificación (INSERT, UPDATE, DELETE) sobre la tabla especificada (`p_tabla`). Si el recuento es mayor que 0, significa que el usuario tiene estos privilegios sobre la tabla, por lo que el indicador se establece en 1. En caso contrario, el indicador permanece en 0.
El valor del indicador se devuelve como un parámetro de salida (`p_indicador`), que puede ser utilizado por otros procedimientos para determinar si un usuario tiene permisos de modificación sobre una tabla.

Por otro lado, el procedimiento `mostrar_usuarios_mod` muestra los nombres de los usuarios que tienen privilegios de modificación sobre una tabla específica. Este procedimiento utiliza el procedimiento `comprobar_privilegios_mod` para determinar qué usuarios tienen permisos de modificación sobre la tabla.

El procedimiento toma un parámetro de entrada que es el nombre de la tabla (`p_tabla`). Se define un cursor (`c_usuarios`) que selecciona todos los nombres de usuario de la vista de diccionario de datos `dba_users` de Oracle. Luego, en un bucle FOR, el procedimiento recorre todos los nombres de usuario y para cada nombre de usuario, llama a `comprobar_privilegios_mod`, pasando el nombre de usuario y el nombre de la tabla como parámetros. Si `comprobar_privilegios_mod` devuelve 1, indica que el usuario tiene privilegios de modificación sobre la tabla, por lo que se imprime el nombre del usuario.

```sql
create or replace procedure comprobar_privilegios_mod(p_usuario in varchar2, p_tabla in varchar2, p_indicador out number)
as
    v_indicador number := 0;
begin
    select count(*) into v_indicador
    from dba_tab_privs tp join dba_tables t on tp.table_name = t.table_name and tp.owner = t.owner
    where (tp.grantee = p_usuario or tp.grantee in (select granted_role from dba_role_privs where grantee = p_usuario))
    and tp.privilege in ('INSERT', 'UPDATE', 'DELETE') and t.table_name = p_tabla;
    if v_indicador > 0 then
        p_indicador := 1;
    else
        p_indicador := 0;
    end if;
end;
/



create or replace procedure mostrar_usuarios_mod(p_tabla in varchar2) as
    cursor c_usuarios is select username from dba_users;
    v_indicador number;
begin
    dbms_output.put_line('Usuarios con permisos de modificacion sobre la tabla:');
    for usuario in c_usuarios loop
        comprobar_privilegios_mod(usuario.username, p_tabla, v_indicador);
        if v_indicador > 0 then
            dbms_output.put_line(usuario.username);
        end if;
    end loop;
end;
/
```


(Es similar al anterior)
El procedimiento `comprobar_privilegios_alt` verifica si un usuario específico tiene el privilegio de `ALTER` sobre una tabla determinada. Este procedimiento toma dos parámetros de entrada: el nombre del usuario (`p_usuario`) y el nombre de la tabla (`p_tabla`).

Dentro del procedimiento, se declara un indicador (`v_indicador`) en 0. Luego, se ejecuta una consulta SQL que cuenta el número de registros en las vistas de diccionario de datos `dba_tab_privs` y `dba_tables` de Oracle. Esta consulta busca coincidencias donde el `p_usuario` tenga el privilegio de `ALTER` sobre la tabla especificada (`p_tabla`). Si el recuento es mayor que 0, significa que el usuario tiene este privilegio sobre la tabla, por lo que el indicador se establece en 1. En caso contrario, el indicador permanece en 0. El valor del indicador se devuelve como un parámetro de salida (`p_indicador`), que puede ser utilizado por otros procedimientos para determinar si un usuario tiene el privilegio de `ALTER` sobre una tabla.

Por otro lado, el procedimiento `mostrar_usuarios_alt` muestra los nombres de los usuarios que tienen el privilegio de `ALTER` sobre una tabla específica. Este procedimiento utiliza el procedimiento `comprobar_privilegios_alt` para determinar qué usuarios tienen este privilegio sobre la tabla.

El procedimiento toma un parámetro de entrada que es el nombre de la tabla (`p_tabla`). Se define un cursor (`c_usuarios`) que selecciona todos los nombres de usuario de la vista de diccionario de datos `dba_users` de Oracle. Luego, en un bucle FOR, el procedimiento recorre todos los nombres de usuario y para cada nombre de usuario, llama a `comprobar_privilegios_alt`, pasando el nombre de usuario y el nombre de la tabla como parámetros. Si `comprobar_privilegios_alt` devuelve 1, indica que el usuario tiene el privilegio de `ALTER` sobre la tabla, por lo que se imprime el nombre del usuario.

```sql
create or replace procedure comprobar_privilegios_alt(p_usuario in varchar2, p_tabla in varchar2, p_indicador out number)
as
    v_indicador number := 0;
begin
    select count(*) into v_indicador
    from dba_tab_privs tp join dba_tables t on tp.table_name = t.table_name and tp.owner = t.owner
    where (tp.grantee = p_usuario or tp.grantee in (select granted_role from dba_role_privs where grantee = p_usuario))
    and tp.privilege = 'ALTER' and t.table_name = p_tabla;
    if v_indicador > 0 then
        p_indicador := 1;
    else
        p_indicador := 0;
    end if;
end;
/



create or replace procedure mostrar_usuarios_alt(p_tabla in varchar2) as
    cursor c_usuarios is select username from dba_users;
    v_indicador number;
begin
    dbms_output.put_line('Usuarios con permisos de modificacion de estructura de la tabla:');
    for usuario in c_usuarios loop
        comprobar_privilegios_alt(usuario.username, p_tabla, v_indicador);
        if v_indicador > 0 then
            dbms_output.put_line(usuario.username);
        end if;
    end loop;
end;
/
```


(Es similar al anterior)
El procedimiento `comprobar_privilegios_drop` verifica si un usuario específico tiene el privilegio de sistema `DROP ANY TABLE`, lo que le permite eliminar cualquier tabla en la base de datos, incluyendo a la que se hará referencia. Este procedimiento toma un parámetro de entrada: el nombre del usuario (`p_usuario`).

Dentro del procedimiento, se declara un indicador (`v_indicador`) en 0. Luego, se ejecuta una consulta SQL que cuenta el número de registros en la vista de diccionario de datos `dba_sys_privs` de Oracle. Esta consulta busca coincidencias donde el `p_usuario` tenga el privilegio de sistema `DROP ANY TABLE`. Si el recuento es mayor que 0, significa que el usuario tiene este privilegio, por lo que el indicador se establece en 1. En caso contrario, el indicador permanece en 0.
El valor del indicador se devuelve como un parámetro de salida (`p_indicador`), que puede ser utilizado por otros procedimientos para determinar si un usuario tiene el privilegio de eliminar cualquier tabla en la base de datos.

Por otro lado, el procedimiento `mostrar_usuarios_del` muestra los nombres de los usuarios que tienen el privilegio para eliminar cualquier tabla en la base de datos. Este utiliza el procedimiento `comprobar_privilegios_drop` para determinar qué usuarios tienen este privilegio.

El procedimiento no toma parámetros adicionales y, por lo tanto, no necesita el nombre de la tabla. Se define un cursor (`c_usuarios`) que selecciona todos los nombres de usuario de la vista de diccionario de datos `dba_users` de Oracle. Luego, en un bucle FOR, el procedimiento recorre todos los nombres de usuario y para cada nombre de usuario, llama a `comprobar_privilegios_drop`, pasando el nombre de usuario y un indicador como parámetros. Si `comprobar_privilegios_drop` devuelve 1, indica que el usuario tiene el privilegio de eliminar la tabla, por lo que se imprime el nombre del usuario.

```sql
create or replace procedure comprobar_privilegios_drop(p_usuario in varchar2, p_indicador out number)
as
    v_indicador number := 0;
begin
    select count(*) into v_indicador
    from dba_sys_privs
    where (grantee = p_usuario or grantee in (select granted_role from dba_role_privs where grantee = p_usuario))
    and privilege = 'DROP ANY TABLE';
    if v_indicador > 0 then
        p_indicador := 1;
    else
        p_indicador := 0;
    end if;
end;
/



create or replace procedure mostrar_usuarios_del as
    cursor c_usuarios is select username from dba_users;
    v_indicador number;
begin
    dbms_output.put_line('Usuarios con permisos para eliminar la tabla:');
    for usuario in c_usuarios loop
        comprobar_privilegios_drop(usuario.username, v_indicador);
        if v_indicador > 0 then
            dbms_output.put_line(usuario.username);
        end if;
    end loop;
end;
/
```


El procedimiento `mostrar_extensiones_fichero` recopila información sobre las extensiones de una tabla específica en la base de datos. Este procedimiento toma dos parámetros de entrada: el nombre de la tabla (`p_tabla`) y el propietario de la tabla (`p_propietario`).

Dentro del procedimiento, se declaran dos cursores. El primer cursor (`c_fichero`) selecciona el nombre de archivo de datos asociado a las extensiones de la tabla (`p_tabla`) desde las vistas de diccionario de datos `dba_extents` y `dba_data_files` de Oracle.
El segundo cursor (`c_extensiones`) selecciona el identificador de extensión de las extensiones de la tabla (`p_tabla`) pertenecientes al propietario especificado (`p_propietario`) desde la vista `dba_extents`.

El procedimiento recorre sobre cada extensión encontrada el cursor `c_extensiones`. Para cada extensión, se consulta sobre los archivos de datos encontrados en el cursor `c_fichero` para obtener el nombre del archivo asociado a la extensión actual que se asigna el nombre del archivo a la variable `v_fichero`. Luego, el procedimiento imprime la información de cada extensión, incluyendo su identificador y el nombre del archivo al que pertenece.

```sql
create or replace procedure mostrar_extensiones_fichero(p_tabla in varchar2, p_propietario in varchar2)
as
    v_fichero varchar2(50);
    cursor c_fichero is select f.file_name from dba_extents e
    join dba_data_files f on e.file_id = f.file_id where e.segment_name = p_tabla;
    cursor c_extensiones is select extent_id from dba_extents where owner = p_propietario and segment_name = p_tabla;
begin
    dbms_output.put_line('Informacion sobre extensiones:');
    for extension in c_extensiones loop
        for fichero in c_fichero loop
            v_fichero := fichero.file_name;
        end loop;
        dbms_output.put_line('Id de la extension: '|| extension.extent_id);
        dbms_output.put_line('Fichero de la extension: '|| v_fichero);
    end loop;
end;
/
```


Prueba:

```sql
SQL> exec MostrarInfoTabla('pedido');
Informacion sobre la tabla PEDIDO
El propietario de la tabla es C##PEPE

Usuarios con permisos de lectura:
SYS
EXAMENALEX

Usuarios con permisos de modificacion sobre la tabla:
SYS
C##RAUL
EXAMENALEX

Usuarios con permisos de modificacion de estructura de la tabla:
SYS
C##JUANMA
EXAMENALEX

Usuarios con permisos para eliminar la tabla:
SYS
SYSTEM
C##ADMIN
C##PEPE

Informacion sobre extensiones:
Id de la extension: 0
Fichero de la extension: /opt/oracle/oradata/FREE/users01.dbf

PL/SQL procedure successfully completed.
```