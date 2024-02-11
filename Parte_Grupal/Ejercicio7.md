## 7. Explicad en qué consiste el sharding en MongoDB. Intentad montarlo.

Para poder montarlo tenemos que entender en que consiste el sharding en MongoDB.

El **sharding** en MongoDB es una técnica para dividir los datos de una base de datos en fragmentos más pequeños, lo que mejora su rendimiento y capacidad de escalabilidad. Esto se logra distribuyendo los datos en distintos servidores o máquinas, creando fragmentos llamados **shard**. 

Esta distribución equitativa de datos entre máquinas ayuda a gestionar grandes volúmenes de datos y mejora la respuesta del sistema al permitir la lectura y escritura simultánea entre varios servidores.

Por ello, las principales ventajas que puede tener el **sharding** son:

- Mejora la escabilidad y el rendimiento en los datos de nuestra base de datos.

- Podemos dividir el trabajo en una base de datos de varias máquinas.

- Mejoramos el rendimiento de respuesta y permitimos que los datos se lean y se escriban en paralelo en varios servidores.


### Como montar el Sharding en MongoDB

Para preparar la configuración de sharding en MongoDB, debemos tener tres componentes clave: