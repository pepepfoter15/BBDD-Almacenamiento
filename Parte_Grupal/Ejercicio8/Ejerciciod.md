## d) Pepe decide que los programadores tengan acceso a la tabla Prueba antes creada y puedan ceder ese derecho y el de conectarse a la base de datos a los usuarios que ellos quieran.

```sql
GRANT SELECT ON PEPE.PRUEBA TO JUAN WITH GRANT OPTION;
GRANT SELECT ON PEPE.PRUEBA TO CLARA WITH GRANT OPTION;
GRANT CONNECT TO CLARA WITH ADMIN OPTION;
GRANT CONNECT TO JUAN WITH ADMIN OPTION;
```