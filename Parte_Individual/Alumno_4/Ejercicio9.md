## 9. Explica los distintos motores de almacenamiento que ofrece MongoDB, sus características principales y en qué casos es más recomendable utilizar cada uno de ellos.

El motor de almacenamiento es el componente de la base de datos que se encarga de gestionar cómo se almacenan los datos, tanto en la memoria como en el disco.
MongoDB admite múltiples motores de almacenamiento, ya que diferentes motores funcionan mejor para cargas de trabajo específicas. Elegir el motor de almacenamiento adecuado para su caso de uso puede afectar significativamente el rendimiento de sus aplicaciones.


**WiredTiger**

- Predeterminado desde MongoDB 3.2.
- Ofrece alto rendimiento tanto para lecturas como escrituras.
- Soporta compresión de datos para mejorar la eficiencia del almacenamiento.
- Permite el uso de diferentes niveles de granularidad de bloqueo, lo que mejora la escalabilidad.

Es recomendable utilizar WiredTiger en entornos de producción con cargas de trabajo mixtas, que incluyen tanto operaciones de lectura como de escritura e incluso en aplicaciones que requieren alto rendimiento y escalabilidad en su base de datos.

**MMAPv1**

- Cargas de trabajo con muchas inserciones, lecturas y actualizaciones.
- Requerimientos de alta disponibilidad de escritura duradera.
- Sistemas donde la compatibilidad con arquitecturas big-endian no es un problema.
- Escenarios donde se necesita una gestión dinámica de la memoria.

Es recomendable para aplicaciones que necesiten optimizar el rendimiento en operaciones intensivas de lectura y escritura, asegurando alta disponibilidad y utilizando eficientemente la memoria del sistema. Sin embargo, se debe tener en cuenta que ya no es el motor de almacenamiento predeterminado en versiones más recientes de MongoDB.

**In-Memory**

El motor de almacenamiento en memoria de MongoDB, está disponible desde la versión Enterprise 3.2.6.

- No mantiene ningún dato en el disco, ni índices, configuraciones y credenciales de usuario, a excepción de algunos metadatos y datos de diagnóstico.
- Ofrece una latencia más predecible al evitar las operaciones de entrada/salida de disco.
- Utiliza un control a nivel de documento para operaciones de escritura, permitiendo que múltiples clientes modifiquen diferentes documentos a la vez.

Es recomendable para escenarios donde se requiere una latencia baja y predecible, aplicaciones donde los datos no necesitan ser persistentes más allá de la vida útil de la instancia de MongoDB y además en entornos donde la simultaneidad de escritura es alta y se requiere un control a nivel de documento.
Es importante tener en cuenta que el motor de almacenamiento en memoria requiere que todos los datos encajen en la memoria especificada por la opción `--inMemorySizeGB`. Este motor no conserva los datos después del cierre del proceso y no ofrece la misma durabilidad que otros motores de almacenamiento como WiredTiger mencionado antes.

**RocksDB**
Sistema de almacenamiento clave-valor diseñado para ofrecer alto rendimiento, eficiencia, escalabilidad y flexibilidad.

- Alto rendimiento ideal para aplicaciones que requieren registrar grandes cantidades de datos frecuentemente.
- Eficiencia pues utiliza técnicas de compresión y almacenamiento optimizadas para minimizar el espacio de almacenamiento utilizado.
- Puede escalarse horizontalmente para manejar grandes conjuntos de datos y cargas de trabajo elevadas.
- Compatible con una amplia gama de lenguajes de programación y plataformas, lo que lo hace versátil para diferentes tipos de aplicaciones.

Puede utilizarse para almacenar datos en caché para un acceso rápido y eficiente, mejorando el rendimiento de las aplicaciones así como en bases de datos NoSQL para almacenar grandes cantidades de datos sin estructura o también en aplicaciones de análisis de datos que requieren almacenar y procesar grandes conjuntos de datos para análisis.

**Otros**
MongoDB también ofrece otros motores de almacenamiento menos conocidos, como Ephemereal y WiredTiger Hybrid.