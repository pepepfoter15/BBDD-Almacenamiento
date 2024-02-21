## d) Pepe decide que los programadores tengan acceso a la tabla Prueba antes creada y puedan ceder ese derecho y el de conectarse a la base de datos a los usuarios que ellos quieran.

```sql
GRANT SELECT ON pepe.Prueba TO juan WITH GRANT OPTION;
GRANT SELECT ON pepe.Prueba TO clara WITH GRANT OPTION;
GRANT CONNECT TO clara WITH ADMIN OPTION;
GRANT CONNECT TO juan WITH ADMIN OPTION;
```