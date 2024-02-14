## a) Pepe es el administrador de la base de datos. Juan y Clara son los programadores de la base de datos, que trabajan tanto en la aplicación que usa el departamento de Ventas como en la usada por el departamento de Producción. Ana y Eva tienen permisos para insertar, modificar y borrar registros en las tablas de la aplicación de Ventas que tienes que crear, y se llaman Productos y Ventas, siendo propiedad de Ana. Jaime y Lidia pueden leer la información de esas tablas pero no pueden modificar la información. Crea los usuarios y dale los roles y permisos que creas conveniente.

*Pepe es el administrador de la base de datos.*
```sql
CREATE USER Pepe IDENTIFIED BY Pepe;
GRANT DBA TO Pepe;
```

*Juan y Clara son los programadores de la base de datos, que trabajan tanto en la aplicación que usa el departamento de Ventas como en la usada por el departamento de Producción.*
```sql
CREATE USER c##Juan IDENTIFIED BY Juan;
CREATE USER c##Clara IDENTIFIED BY Clara;
CREATE ROLE informatica;
GRANT RESOURCE TO informatica;
GRANT informatica TO Juan;
GRANT informatica TO Clara;
```

*Ana y Eva tienen permisos para insertar, modificar y borrar registros en las tablas de la aplicación de Ventas que tienes que crear, y se llaman Productos y Ventas, siendo propiedad de Ana.*
```sql
CREATE USER c##Ana IDENTIFIED BY Ana;
CREATE USER c##Eva IDENTIFIED BY Eva;
CREATE ROLE ventas;
GRANT SELECT, INSERT, UPDATE, DELETE ON Ana.Ventas to produccion;
GRANT SELECT, INSERT, UPDATE, DELETE ON Ana.Productos to produccion;
GRANT produccion TO Ana;
GRANT produccion TO Eva;
```

*Jaime y Lidia pueden leer la información de esas tablas pero no pueden modificar la información. Crea los usuarios y dale los roles y permisos que creas conveniente.*
```sql
CREATE USER c##Jaime IDENTIFIED BY Jaime;
CREATE USER c##Lidia IDENTIFIED BY Lidia;
CREATE ROLE produccion
GRANT SELECT on Ana.Ventas to produccion;
GRANT SELECT on Ana.Productos to produccion;
GRANT produccion TO Jaime;
GRANT produccion TO lidia;
```