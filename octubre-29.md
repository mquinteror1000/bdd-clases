## Octubre 29

[t-04.pdf](notas_prof/t-04-parte-1.pdf)

### Index unique scan

### Index Range Scan

Es escaneo 

- Un operador SQL que pudiera devolver mas de un registro -> Resuelve en un index Range Scan 

- También pude funcionar con la igualdad
  
  ```sql
  select * from paciente where
      email = 'x@x.com';
  ```
  
  Sí el indice es del tipo **non unique** -> el optimizado empleará **index range scan**
  
  ¿por que no usaría el índice?
  
  Ejemplo
  
  ```sql
  explain plan for
  select email from paciente where email like 'bob@%' order by email desc;
  select plan_table_output from table(dbms_xplan.display);
  ```
  
  -> range scan

- sdss

### Index Full Scan

Hace una consulta del $100\%$ de los rowid que contiene el índice

Cuando una BD quiere todo los datos de una columna y se requieren ordenados

```sql
select email
from paciente where email is not nul -- para agesurarse que no intente graesrse nulos y asi si usar el indice
order by email asc;
```

```sql
explain plan for
select nombre from paciente order by nombre;
select plan_table_output from table(dbms_xplan.display);
```

![](images/2026-10-29-09-54-27-image.png)

### Index Fast Full Scan

Cuando se requieren todos los rowid, pero no importa el ordenamiento

##### ¿Cómo se guarda un índice de manera física?

<img src="images/2026-10-29-10-08-58-IMG_20261029_100418399.jpg" title="" alt="" width="705">

```sql
sql
select email
from paciente where email is not nul; -- para agesurarse que no intente graesrse nulos y asi si usar el indice
```

![](images/2026-10-29-10-07-03-image.png)

¿Que sucede acá?

```sql
select 1+1 from paciente; -- que pasa aca ?
sect 1+1 from dual; -- uso de la tabla dummy
select 1+1; -- apartir de 23ai
```

Por cada renglon que ecuentres en la table, ejecuta esto

```sql
select [] from tabla; -- por cada renglon de la table ejecuta la consulta []
select upper(nombre) from tabla ; -- por cada renglon de la table, ejecuta la funcion
select fecha_nac +1 from tabla; -- por cada renglon de la tabla ejecuta la expresi
```

#### Ejemplo 2

![](images/2026-10-29-10-18-43-image.png)

```shellsession
sys@mqrbda_s1> desc dual;
 Name					   Null?    Type
 ----------------------------------------- -------- ----------------------------
 DUMMY						    VARCHAR2(1)

sys@mqrbda_s1> select 1+1 from dual;

       1+1
----------
	 2

sys@mqrbda_s1> select 1+1;

       1+1
----------
	 2

sys@mqrbda_s1> 
```
