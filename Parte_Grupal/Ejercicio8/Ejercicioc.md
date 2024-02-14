## c) Pepe quiere crear una tabla Prueba que ocupe inicialmente 256K en el tablespace Ventas.

```sql
CREATE TABLE pepe.Prueba (
    id number,
) TABLESPACE Ventas
STORAGE (
    INITIAL 256K);
```