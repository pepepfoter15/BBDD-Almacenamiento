## 2. Crea dos tablas en el tablespace recién creado e inserta un registro en cada una de ellas. Comprueba el espacio libre existente en el tablespace. Borra una de las tablas y comprueba si ha aumentado el espacio disponible en el tablespace. Explica la razón.


Utilizaré la siguiente consulta para ver el espacio según vaya avanzando en el ejercicio:
```
SELECT (bytes / 1024) || 'KB' AS kilobytes
FROM DBA_FREE_SPACE
WHERE tablespace_name = 'TS1';
```
![ ](img/o201.png)

Para empezar, vemos que el tamaño disponible en TS1 es de 128kb. Esto sucede por que el tamaño de segmento mínimo (el que usa en estos casos) es 64kb (tamaño de bloque de 8kb, *8 bloques por segmento.), y oracle redondea el tamaño, en este caso da para 3 bloques 3x64 = 192kb. Uno de los bloques está siendo usado para guardar metadatos internamente, así que nos quedamos con 2, dando el valor total de 128kb

```
select distinct (bytes / 1024) || 'KB' AS kilobytes from dba_segments where tablespace_name='TS1';
select distinct blocks from dba_segments where tablespace_name='TS1';
select (block_size / 1024) || 'KB' AS kilobytes from dba_tablespaces where tablespace_name='TS1'; 
```

![ ](img/o203.png)

Creamos las tablas y volvemos a comprobar el espacio libre:
```
CREATE TABLE Tabla1 (
    id NUMBER(5)
) TABLESPACE TS1;

CREATE TABLE Tabla2 (
    id NUMBER(5)
) TABLESPACE TS1;

INSERT INTO Tabla1 VALUES (1);
INSERT INTO Tabla2 VALUES (1);
```

![ ](img/o202.png)

Vemos que el tamaño libre ha ascendido a 1024KB (1MB). Esto se debe a que oracle agrupa extensiones de bloques en grupos de 8 por defecto, ya que nuestra instalación gestiona localmente, en vez de por diccionario. En el momento que se introduce cualquier dato, el espacio mínimo pase a ser de 1mb, aun que esos datos ocupen 1 byte, como es en este caso.

*buscar archivo de config

