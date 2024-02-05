## 6. Realiza un procedimiento llamado MostrarDetallesIndices que reciba el nombre de una tabla y muestre los detalles sobre los índices que hay definidos sobre las columnas de la misma.

```
CREATE OR REPLACE PROCEDURE MostrarDetallesIndices (p_tabla VARCHAR2) AS
    CURSOR c_indexes IS
        SELECT index_name, owner
        FROM DBA_INDEXES
        WHERE table_name = p_tabla
    v_len_barra NUMBER;
BEGIN
    FOR i IN c_indexes LOOP
        v_len_barra:= length(i.index_name)

        dbms_output.put_line('Índice ' || c_indexes%ROWCOUNT || ': '  || i.index_name)
        dbms_output.put_line(rpad('----------', v_len_barra, '-'))
        dbms_output.put_line(char(9) ||'Dueño: ' || i.owner)

        --Ya pondre mas cosas cuando avance mas la práctica
    END LOOP;
END;
/
```

```
MostrarDetallesIndices('emp');
```