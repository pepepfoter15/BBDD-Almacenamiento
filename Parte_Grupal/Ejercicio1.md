## 1. Cread un índice para la tabla EMP de SCOTT que agilice las consultas por nombre de empleado en un tablespace creado específicamente para índices. ¿Dónde deberiáis ubicar el fichero de datos asociado? ¿Cómo se os ocurre que podriáis probar si el índice resulta de utilidad?


En este ejercicio, mostramos cómo mejorar el rendimiento de las consultas SQL mediante el uso de índices y tablespaces adecuados en Oracle.

Primero, creamos un procedimiento llamado `inserts_emp`. Este procedimiento utiliza un bucle `FOR` para insertar 100,000 filas en la tabla `SCOTT.emp`. Cada fila representa un empleado con un número de empleado único, un nombre generado dinámicamente...

```sql
create or replace procedure inserts_emp
as
v_nombre scott.emp.ename%type;
begin
    for x in 1..100000 loop
        v_nombre := 'alex-' || x;
        insert into SCOTT.emp values (x, v_nombre, 'manager', 7902, to_date('01/01/1982', 'dd/mm/yyyy'), 1000, 1000 , 20);
    end loop;
end;
/
```


Después, se activa la medición del tiempo de ejecución de las consultas SQL mediante el comando `set timing on;`.

```sql
set timing on;
```


Se ejecuta una consulta sin el uso de un índice, seleccionando el salario de un empleado con un nombre específico ('alex-97384') de la tabla `emp`. 

```sql
SQL> select sal from emp where ename = 'alex-97384';

    SAL
----------
   1000

Elapsed: 00:00:00.08
```


Luego, creamos un nuevo tablespace llamado `TS_IDX`, destinado a almacenar el índice que se creará a continuación. Este tablespace tiene un archivo de datos inicial de `20M`, que se extiende automáticamente según sea necesario.

```sql
SQL> create tablespace ts_idx datafile 'indices.dbf' size 20M autoextend on;

Tablespace created.

Elapsed: 00:00:00.32
```


Se procede a crear un índice llamado `idx_ename` en la columna `ename` de la tabla `emp`, especificando que se almacenará en el tablespace `TS_IDX`. Este índice está diseñado para mejorar el rendimiento de las consultas que involucren la columna `ename`.

```sql
SQL> create index idx_ename on emp(ename) tablespace TS_IDX;

Index created.

Elapsed: 00:00:00.25
```


Después de crear el índice en la columna `ename` de la tabla `emp`, se ejecuta nuevamente la consulta para seleccionar el salario de un empleado con un nombre específico ('alex-97384'). En esta caso, se incluye (`/*+ index(emp idx_ename)*/`) en la consulta para indicar explícitamente al optimizador de consultas que utilice el índice recién creado. Aunque el resultado de la consulta sigue siendo el mismo (un salario de `1000`), se observa una mejora notable en el tiempo de ejecución, que se reduce a `00:00:00.01`. Esto demuestra que el uso del indice ha influido positivamente en el rendimiento de la consulta, al guiar al optimizador para que utilice el índice, lo que resulta en una ejecución más eficiente.

```sql
SQL> select /*+ index(emp idx_ename)*/ sal from SCOTT.emp where ename='alex-97384';

    SAL
----------
    1000

Elapsed: 00:00:00.01
```

En conclusión, es importante del uso de índices y tablespaces adecuados para mejorar el rendimiento de las consultas en bases de datos Oracle. Con la creación de un índice en la columna `ename` de la tabla `emp` y al guardarlo en un tablespace dedicado, se puede conseguir acelerar un poco la ejecución de consultas concretas.

Es importante mencionar que los efectos de estas optimizaciones no son tan evidentes en bases de datos pequeñas o con poca carga de datos como es en este caso, pues se apreciarían mejor en entornos más grandes con múltiples tablas y conjuntos de datos más grandes. En esos casos, la utilización eficiente de índices y tablespaces puede marcar una gran diferencia en el rendimiento general del sistema.

Además, para optimizar aún más el rendimiento, sería mejor almacenar tablespaces con índices en un disco local de alta velocidad. Al hacerlo, se minimizaría el tiempo de acceso a los datos indexados, lo que hará en las consultas que sean rápidas y con una mejor respuesta del sistema en general.