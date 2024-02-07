# BBDD-Almacenamiento
## Proyecto grupal de Base de Datos en relación al almacenamiento

- Alumno 1 (Organizador):   Pepe
- Alumno 2 (Facilitator):   Fran
- Alumno 3 (Portavoz):      Fabio
- Alumno 4 (Secretario):    Alex

### Alumno 1:
#### ORACLE:

1. [Muestra los espacios de tablas existentes en tu base de datos y la ruta de los ficheros que los componen. ¿Están las extensiones gestionadas localmente o por diccionario?](/Parte_Individual/Alumno_1/Ejercicio1.md)

2. [Usa la vista del diccionario de datos v$datafile para mirar cuando fue la última vez que se ejecutó el proceso CKPT en tu base de datos.](/Parte_Individual/Alumno_1/Ejercicio2.md)

3. [Intenta crear el tablespace TS1 con un fichero de 2M en tu disco que crezca automáticamente cuando sea necesario. ¿Puedes hacer que la gestión de extensiones sea por diccionario? Averigua la razón.](/Parte_Individual/Alumno_1/Ejercicio3.md)

4. [Averigua el tamaño de un bloque de datos en tu base de datos. Cámbialo al doble del valor que tenga.](/Parte_Individual/Alumno_1/Ejercicio4.md)

5. [Realiza un procedimiento MostrarObjetosdeUsuarioenTS que reciba el nombre de un tablespace y el de un usuario y muestre qué objetos tiene el usuario en dicho tablespace y qué tamaño tiene cada uno de ellos.](/Parte_Individual/Alumno_1/Ejercicio5.md)

6. [Realiza un procedimiento llamado MostrarUsrsCuotaIlimitada que muestre los usuarios que puedan escribir de forma ilimitada en más de uno de los tablespaces que cuentan con ficheros en la unidad C:](/Parte_Individual/Alumno_1/Ejercicio6.md)
       
#### Postgres:
       
7. [Averigua si existe el concepto de tablespace en Postgres, en qué consiste y las diferencias con los tablespaces de ORACLE.](/Parte_Individual/Alumno_1/Ejercicio7.md)
       
#### MySQL:

8. [Averigua si pueden establecerse claúsulas de almacenamiento para las tablas o los espacios de tablas en MySQL.](/Parte_Individual/Alumno_1/Ejercicio8.md)

#### MongoDB:

9. [Averigua si existe el concepto de índice en MongoDB y las diferencias con los índices de ORACLE. Explica los distintos tipos de índice que ofrece MongoDB.](/Parte_Individual/Alumno_1/Ejercicio9.md)


### Alumno 2:
#### ORACLE:

1. [Establece que los objetos que se creen en el TS1 (creado por Alumno 1) tengan un tamaño inicial de 200K, y que cada extensión sea del doble del tamaño que la anterior. El número máximo de extensiones debe ser de 3.](/Parte_Individual/Alumno_2/Ejercicio1.md)
       
2. [Crea dos tablas en el tablespace recién creado e inserta un registro en cada una de ellas. Comprueba el espacio libre existente en el tablespace. Borra una de las tablas y comprueba si ha aumentado el espacio disponible en el tablespace. Explica la razón.](/Parte_Individual/Alumno_2/Ejercicio2.md)
       
3. [Convierte a TS1 en un tablespace de sólo lectura. Intenta insertar registros en la tabla existente. ¿Qué ocurre?. Intenta ahora borrar la tabla. ¿Qué ocurre? ¿Porqué crees que pasa eso?](/Parte_Individual/Alumno_2/Ejercicio3.md)
       
4. [Crea un espacio de tablas TS2 con dos ficheros en rutas diferentes de 1M cada uno no autoextensibles. Crea en el citado tablespace una tabla con la clausula de almacenamiento que quieras. Inserta registros hasta que se llene el tablespace. ¿Qué ocurre?](/Parte_Individual/Alumno_2/Ejercicio4.md)

5. [Hacer un procedimiento llamado MostrarUsuariosporTablespace que muestre por pantalla un listado de los tablespaces existentes con la lista de usuarios que tienen asignado cada uno de ellos por defecto y el número de los mismos, así:](/Parte_Individual/Alumno_2/Ejercicio5.md)

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


No olvides incluir los tablespaces temporales y de undo.

6.  [Realiza un procedimiento llamado MostrarDetallesIndices que reciba el nombre de una tabla y muestre los detalles sobre los índices que hay definidos sobre las columnas de la misma.](/Parte_Individual/Alumno_2/Ejercicio6.md)
       
#### Postgres:
       
7. [Averigua si existe el concepto de segmento y el de extensión en Postgres, en qué consiste y las diferencias con los conceptos correspondientes de ORACLE.](/Parte_Individual/Alumno_2/Ejercicio7.md)
       
#### MySQL:

8. [Averigua si existe el concepto de espacio de tablas en MySQL y las diferencias con los tablespaces de ORACLE.](/Parte_Individual/Alumno_2/Ejercicio8.md)

#### MongoDB:

9. [Averigua si existe la posibilidad en MongoDB de decidir en qué archivo se almacena una colección.](/Parte_Individual/Alumno_2/Ejercicio9.md)


### Alumno 3:

#### ORACLE:

1. [Muestra los objetos a los que pertenecen las extensiones del tablespace TS2 (creado por Alumno 2) y el tamaño de cada una de ellas.](/Parte_Individual/Alumno_3/Alumno3.md)
       
2. [Borra la tabla que está llenando TS2 consiguiendo que vuelvan a existir extensiones libres. Añade después otro fichero de datos a TS2.](/Parte_Individual/Alumno_3/Alumno3.md)
       
3. [Crea el tablespace TS3 gestionado localmente con un tamaño de extension uniforme de 128K y un fichero de datos asociado. Cambia la ubicación del fichero de datos y modifica la base de datos para que pueda acceder al mismo. Crea en TS3 dos tablas e inserta registros en las mismas. Comprueba que segmentos tiene TS3, qué extensiones tiene cada uno de ellos y en qué ficheros se encuentran.](/Parte_Individual/Alumno_3/Alumno3.md)

4. [Redimensiona los ficheros asociados a los tres tablespaces que has creado de forma que ocupen el mínimo espacio posible para alojar sus objetos.](/Parte_Individual/Alumno_3/Alumno3.md)

5. [Realiza un procedimiento llamado InformeRestricciones que reciba el nombre de una tabla y muestre los nombres de las restricciones que tiene, a qué columna o columnas afectan y en qué consisten exactamente.](/Parte_Individual/Alumno_3/Alumno3.md)

6. [Realiza un procedimiento llamado MostrarAlmacenamientoUsuario que reciba el nombre de un usuario y devuelva el espacio que ocupan sus objetos agrupando por dispositivos y archivos:](/Parte_Individual/Alumno_3/Alumno3.md)

```
				Usuario: NombreUsuario

					Dispositivo:xxxx

						Archivo: xxxxxxx.xxx

								Tabla1......nnn K
								…
								TablaN......nnn K
								Indice1.....nnn K
								…
								IndiceN.....nnn K

						Total Espacio en Archivo xxxxxxx.xxx: nnnnn K

						Archivo:...
						…

				
					
					Total Espacio en Dispositivo xxxx: nnnnnn K

					Dispositivo: yyyy
					…

				Total Espacio Usuario en la BD: nnnnnnn K
```
#### Postgres:
       
7. [Averigua si es posible establecer cuotas de uso sobre los tablespaces en Postgres.](/Parte_Individual/Alumno_3/Alumno3.md)

#### MySQL:

8. [Averigua si existe el concepto de extensión en MySQL y si coincide con el existente en ORACLE.](/Parte_Individual/Alumno_3/Alumno3.md)

#### MongoDB:

9. [Averigua si en MongoDB puede saberse el espacio disponible para almacenar nuevos documentos.](/Parte_Individual/Alumno_3/Alumno3.md)


### Alumno 4:
#### ORACLE:

1. [Crea un tablespace de undo e intenta crear una tabla en él.](/Parte_Individual/Alumno_4/Ejercicio1.md)
       
2. [Crea un tablespace temporal TEMP2 y escribe una sentencia SQL que genere un script que haga usar TEMP2 a todos los usuarios que tienen USERS como tablespace por defecto.](/Parte_Individual/Alumno_4/Ejercicio2.md)
       
3. [Borra todos los tablespaces creados para esta práctica sin que quede rastro de ellos. Realiza las acciones previas que sean necesarias.](/Parte_Individual/Alumno_4/Ejercicio3.md)
       
4. [Averigua los segmentos existentes para realizar un ROLLBACK y el tamaño de sus extensiones.](/Parte_Individual/Alumno_4/Ejercicio4.md)

5. [Queremos cambiar de ubicación un tablespace, pero antes debemos avisar a los usuarios que tienen acceso de lectura o escritura a cualquiera de los objetos almacenados en el mismo. Escribe un procedimiento llamado MostrarUsuariosAccesoTS que obtenga un listado con los nombres de dichos usuarios.](/Parte_Individual/Alumno_4/Ejercicio5.md)

6. [Realiza un procedimiento llamado MostrarInfoTabla que reciba el nombre de una tabla y muestre la siguiente información sobre la misma: propietario, usuarios que pueden leer sus datos, usuarios que pueden cambiar (insertar, modificar o eliminar) sus datos, usuarios que pueden modificar su estructura, usuarios que pueden eliminarla, lista de extensiones y en qué fichero de datos se encuentran.](/Parte_Individual/Alumno_4/Ejercicio6.md)

7. [Averigua si pueden establecerse claúsulas de almacenamiento para las tablas o los espacios de tablas en Postgres.](/Parte_Individual/Alumno_4/Ejercicio7.md)

#### MySQL:

8. [Averigua si existe el concepto de índice en MySQL y si coincide con el existente en ORACLE. Explica los distintos tipos de índices existentes.](/Parte_Individual/Alumno_4/Ejercicio8.md)

#### MongoDB:

9. [Explica los distintos motores de almacenamiento que ofrece MongoDB, sus características principales y en qué casos es más recomendable utilizar cada uno de ellos.](/Parte_Individual/Alumno_4/Ejercicio9.md)


### Parte grupal:
#### ORACLE:

1. [Cread un índice para la tabla EMP de SCOTT que agilice las consultas por nombre de empleado en un tablespace creado específicamente para índices. ¿Dónde deberiáis ubicar el fichero de datos asociado? ¿Cómo se os ocurre que podriáis probar si el índice resulta de utilidad?](/Parte_Grupal/Ejercicio1.md)
       
2. [Realizad una consulta al diccionario de datos que muestre qué  índices existen para objetos pertenecientes al esquema de SCOTT y sobre qué columnas están definidos. Averiguad en qué fichero o ficheros de datos se encuentran las extensiones de sus segmentos correspondientes.](/Parte_Grupal/Ejercicio2.md)
       
3. [Cread una secuencia para rellenar el campo deptno de la tabla dept de forma coherente con los datos ya existentes. Insertad al menos dos registros haciendo uso de la secuencia.](/Parte_Grupal/Ejercicio3.md)
       
4. [Queremos limpiar nuestro fichero tnsnames.ora. Averiguad cuales de sus entradas se están usando en algún enlace de la base de datos.](/Parte_Grupal/Ejercicio4.md)
       
5. [Meted las tablas EMP y DEPT de SCOTT en un cluster.](/Parte_Grupal/Ejercicio5.md)

6. [Realizad un procedimiento llamado BalanceoCargaTemp que balancee la carga de usuarios entre los tablespaces temporales existentes. Para ello averiguará cuántos existen y asignará los usuarios entre ellos de forma equilibrada. Si es necesario para comprobar su funcionamiento, crea tablespaces temporales nuevos.](/Parte_Grupal/Ejercicio6.md)

7. [Explicad en qué consiste el sharding en MongoDB. Intentad montarlo.](/Parte_Grupal/Ejercicio7.md)

8. [Resolved el siguiente caso práctico en ORACLE:](/Parte_Grupal/Ejercicio8/)

En nuestra empresa existen tres departamentos: Informática, Ventas y Producción. En Informática trabajan tres personas: Pepe, Juan y Clara. En Ventas trabajan Ana y Eva y en Producción Jaime y Lidia.

[a) Pepe es el administrador de la base de datos. Juan y Clara son los programadores de la base de datos, que trabajan tanto en la aplicación que usa el departamento de Ventas como en la usada por el departamento de Producción. Ana y Eva tienen permisos para insertar, modificar y borrar registros en las tablas de la aplicación de Ventas que tienes que crear, y se llaman Productos y Ventas, siendo propiedad de Ana. Jaime y Lidia pueden leer la información de esas tablas pero no pueden modificar la información. Crea los usuarios y dale los roles y permisos que creas conveniente.](/Parte_Grupal/Ejercicio8/Ejercicioa.md)

[b) Los espacios de tablas son System, Producción (ficheros prod1.dbf y prod2.dbf) y Ventas (fichero vent.dbf). Los programadores del departamento de Informática pueden crear objetos en cualquier tablespace de la base de datos, excepto en System. Los demás usuarios solo podrán crear objetos en su tablespace correspondiente teniendo un límite de espacio de 30 M los del departamento de Ventas y 100K los del de Producción. Pepe tiene cuota ilimitada en todos los espacios, aunque el suyo por defecto es System.](/Parte_Grupal/Ejercicio8/Ejerciciob.md)
	
[c) Pepe quiere crear una tabla Prueba que ocupe inicialmente 256K en el tablespace Ventas.](/Parte_Grupal/Ejercicio8/Ejercicioc.md)

[d) Pepe decide que los programadores tengan acceso a la tabla Prueba antes creada y puedan ceder ese derecho y el de conectarse a la base de datos a los usuarios que ellos quieran.](/Parte_Grupal/Ejercicio8/Ejerciciod.md)
	
[f) Lidia y Jaime dejan la empresa, borra los usuarios y el espacio de tablas correspondiente, detalla los pasos necesarios para que no quede rastro del espacio de tablas.](/Parte_Grupal/Ejercicio8/Ejerciciof.md)

9. Elaboración de un vídeo grupal resumiendo las diferencias de concepto y en la gestión de espacios de tabla, índices, claúsulas  de almacenamiento y segmentos y extensiones entre los cuatro SGBDs estudiados. Cada miembro del grupo hablará de uno de los siguientes temas:

a) Espacios de Tabla y cuotas de uso de los mismos. --> Fabio

b) Índices. Secuencias. --> Fran

c) Claúsulas de Almacenamiento. --> Alex

d) Segmentos y Extensiones. Clusters de tablas. --> Pepe