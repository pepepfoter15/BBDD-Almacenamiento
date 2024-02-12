## 5. Meted las tablas EMP y DEPT de SCOTT en un cluster.

Creamos un cluster llamado `cluster_sct` con la columna `DEPTNO` de tipo `NUMBER(2)` y un tamaño inicial de 1024 bloques.
Un cluster en Oracle es una estructura que almacena datos de tablas relacionadas físicamente juntas en el disco al crear un cluster en Oracle, se indica que las filas de una o más tablas estarán almacenadas físicamente juntas, utilizando un índice clusterizado en una columna común. Esto puede proporcionar ventajas en términos de rendimiento ya que las filas relacionadas pueden estar almacenadas en la misma área del disco y pueden ser recuperadas más eficientemente durante las consultas que acceden a esos datos.

```sql
SQL> CREATE CLUSTER cluster_sct (DEPTNO NUMBER(2)) SIZE 1024;

Cluster created.
```


Creamos un índice llamado `idx_cluster_sct` en el cluster `cluster_sct`. Un índice en un cluster funciona de manera similar a un índice en una tabla normal, pero en lugar de indexar directamente las filas de una tabla, indexa las filas dentro del cluster.
Al crearse un índice en un cluster, se crea una estructura que permite acceder de manera más eficiente a los datos almacenados en ese cluster. Este, puede utilizar el orden físico del cluster para acelerar la recuperación de datos durante las consultas que utilicen el índice.

```sql
SQL> CREATE INDEX idx_cluster_sct ON CLUSTER cluster_sct;

Index created.
```

Creamos las tablas especificando que las filas de la tabla DEPT se almacenarán en el cluster indicado, entonces se guardarán junto a las demás tablas que también usen el mismo cluster.

```sql
SQL> CREATE TABLE DEPT (
  2      DEPTNO NUMBER(2) PRIMARY KEY,
  3      DNAME VARCHAR2(14),
  4      LOC VARCHAR2(13))
  5  CLUSTER cluster_sct(DEPTNO);

Table created.
```

```sql
SQL> CREATE TABLE EMP (
  2      EMPNO NUMBER(4) PRIMARY KEY,
  3      ENAME VARCHAR2(10),
  4      JOB VARCHAR2(9),
  5      MGR NUMBER(4),
  6      HIREDATE DATE,
  7      SAL NUMBER(7, 2),
    COMM NUMBER(7, 2),
  9      DEPTNO NUMBER(2),
CONSTRAINT FK_DEPTNO FOREIGN KEY (DEPTNO) REFERENCES DEPT(DEPTNO))
 11  CLUSTER cluster_sct(DEPTNO);

Table created.
```

Le insertaremos datos:

```sql
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (20, 'RESEARCH', 'DALLAS');
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (30, 'SALES', 'CHICAGO');
INSERT INTO DEPT (DEPTNO, DNAME, LOC) VALUES (40, 'OPERATIONS', 'BOSTON');
```

```sql
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7839, 'KING', 'PRESIDENT', NULL, TO_DATE('17-11-1981', 'DD-MM-YYYY'), 5000, NULL, 10);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7566, 'JONES', 'MANAGER', 7839, TO_DATE('2-4-1981', 'DD-MM-YYYY'), 2975, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7698, 'BLAKE', 'MANAGER', 7839, TO_DATE('1-5-1981', 'DD-MM-YYYY'), 2850, NULL, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7782, 'CLARK', 'MANAGER', 7839, TO_DATE('9-6-1981', 'DD-MM-YYYY'), 2450, NULL, 10);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7788, 'SCOTT', 'ANALYST', 7566, TO_DATE('19-4-1987', 'DD-MM-YYYY'), 3000, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7902, 'FORD', 'ANALYST', 7566, TO_DATE('3-12-1981', 'DD-MM-YYYY'), 3000, NULL, 20);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7844, 'TURNER', 'SALESMAN', 7698, TO_DATE('8-9-1981', 'DD-MM-YYYY'), 1500, 0, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7900, 'JAMES', 'CLERK', 7698, TO_DATE('3-12-1981', 'DD-MM-YYYY'), 950, NULL, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7654, 'MARTIN', 'SALESMAN', 7698, TO_DATE('28-9-1981', 'DD-MM-YYYY'), 1250, 1400, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7499, 'ALLEN', 'SALESMAN', 7698, TO_DATE('20-2-1981', 'DD-MM-YYYY'), 1600, 300, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7521, 'WARD', 'SALESMAN', 7698, TO_DATE('22-2-1981', 'DD-MM-YYYY'), 1250, 500, 30);
INSERT INTO EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO) VALUES (7934, 'MILLER', 'CLERK', 7782, TO_DATE('23-1-1982', 'DD-MM-YYYY'), 1300, NULL, 10);
```


Comprobamos con la siguiente consulta veremos el cluster al que están asociadas las tablas.

```sql
SQL> SELECT table_name, cluster_name FROM user_tables WHERE table_name IN ('DEPT', 'EMP');

TABLE_NAME															 CLUSTER_NAME
-------------------------------------------------------------------------------------------------------------------------------- -----------------------------------------------------------------------
DEPT																 CLUSTER_SCT
EMP																 CLUSTER_SCT
```
