## f) Lidia y Jaime dejan la empresa, borra los usuarios y el espacio de tablas correspondiente, detalla los pasos necesarios para que no quede rastro del espacio de tablas.

```
DROP USER Lidia CASCADE;
DROP USER Jaime CASCADE;
DROP TABLESPACE T_PRODUCCION INCLUDING CONTENTS AND DATAFILES CASCADE CONSTRAINTS;
```

Después de eliminar los usuarios y sus objetos, se puede ejecutar lo siguiente para liberar el espacio físico del disco:
```
ALTER SYSTEM FLUSH BUFFER_CACHE;
ALTER SYSTEM FLUSH SHARED_POOL;
```