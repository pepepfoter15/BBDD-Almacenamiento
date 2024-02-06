# **ASGBD - Práctica Grupal 4: Almacenamiento**

## **Alumno 3 - Fabio Gonzalez del Valle**

**Índice:**

  - [**Oracle**](#oracle)
    - [**Ejercicio 1**](#ejercicio-1)
    - [**Ejercicio 2**](#ejercicio-2)
    - [**Ejercicio 3**](#ejercicio-3)
    - [**Ejercicio 4**](#ejercicio-4)
    - [**Ejercicio 5**](#ejercicio-5)
    - [**Ejercicio 6**](#ejercicio-6)
  - [**PostgreSQL**](#postgresql)
    - [**Ejercicio 7**](#ejercicio-7)
  - [**MySQL**](#mysql)
    - [**Ejercicio 8**](#ejercicio-8)
  - [**MongoDB**](#mongodb)
    - [**Ejercicio 9**](#ejercicio-9)

---

## **Oracle**

### **Ejercicio 1**

> **1. Muestra los objetos a los que pertenecen las extensiones del tablespace TS2 (creado por Alumno 2) y el tamaño de cada una de ellas.**

> **2. Borra la tabla que está llenando TS2 consiguiendo que vuelvan a existir extensiones libres. Añade después otro fichero de datos a TS2.**

> **3. Crea el tablespace TS3 gestionado localmente con un tamaño de extension uniforme de 128K y un fichero de datos asociado. Cambia la ubicación del fichero de datos y modifica la base de datos para que pueda acceder al mismo. Crea en TS3 dos tablas e inserta registros en las mismas. Comprueba que segmentos tiene TS3, qué extensiones tiene cada uno de ellos y en qué ficheros se encuentran.**

> **4. Redimensiona los ficheros asociados a los tres tablespaces que has creado de forma que ocupen el mínimo espacio posible para alojar sus objetos.**

> **5. Realiza un procedimiento llamado InformeRestricciones que reciba el nombre de una tabla y muestre los nombres de las restricciones que tiene, a qué columna o columnas afectan y en qué consisten exactamente.**

> **6. Realiza un procedimiento llamado MostrarAlmacenamientoUsuario que reciba el nombre de un usuario y devuelva el espacio que ocupan sus objetos agrupando por dispositivos y archivos:**

## **PostgreSQL**

### **Ejercicio 7**

> **7. Averigua si es posible establecer cuotas de uso sobre los tablespaces en Postgres.**

En PostgreSQL, no hay una función integrada para establecer cuotas de uso directamente en tablespaces a nivel de usuario. Sin embargo, hay algunas estrategias que puedes implementar para lograr un control de uso de espacio por usuario o por base de datos. Una de ellas es establecer límites de tamaño directamente en la base de datos.

```sql
CREATE DATABASE ejemplo WITH OWNER = usuario TABLESPACE = ejemplo CONNECTION LIMIT = -1 TEMPLATE template0 MAXSIZE 100MB;
```

## **MySQL**

> **8. Averigua si existe el concepto de extensión en MySQL y si coincide con el existente en ORACLE.**

En MySQL, el término "extensión" se utiliza para referirse a algo diferente en comparación con el concepto de "extensión" en Oracle. Estas son las diferencias:

En MySQL, una "extensión" se refiere a un complemento opcional que agrega funcionalidades adicionales al servidor MySQL. Estas extensiones pueden ser funciones, almacenamientos de motores, plugins de autenticación, plugins de cifrado, y más.

En Oracle, el término "extensión" se usa en un contexto diferente. En Oracle, una "extensión" se refiere a la capacidad de agregar nuevos métodos y atributos a un tipo de objeto definido por el usuario.

En resumen en MySQL, una extensión agrega funcionalidades adicionales al servidor MySQL, mientras que en Oracle Database, una extensión permite la herencia de atributos y métodos en tipos de objetos definidos por el usuario.

## **MongoDB:**

> **9. Averigua si en MongoDB puede saberse el espacio disponible para almacenar nuevos documentos.**

En MongoDB, podemos utilizar el método **db.stats()** en la shell de MongoDB para obtener estadísticas sobre una base de datos específica. Esto incluye información sobre el tamaño total de la base de datos, el espacio utilizado y el espacio libre.

![Mongo](img/mongo.png)

También podemos usar el comando **db.nombrecoleccion.stats()** para obtener información sobre una colección específica de nuestra base de datos. Este comando nos otorgará una información detallada sobre la colección.

![Mongo](img/mongo2.png)

