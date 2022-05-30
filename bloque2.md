# Como ejercicio crea un nuevo topic que maneje datos con formato JSON con variedad de atributos (que tengan cadenas de caracteres, enteros, y tipos de datos reales/flotantes) inserta varios registros (al menos unos 6-10 registros) y crea una tabla en presto asociada a ese topic y lanza consultas sobre el topic (que incluso podría estar en una continua evolución, asumiendo que sigan insertando elementos) con condiciones (cláusulas WHERE) basadas en algunos de los atributos que contienen dichos JSON.NOTA: Recuerda que todo el documento json debe estar en una linea, como sugerencia utilizar un editor de texto como por ejemplo atom, utilizar el plugin Pretty JSON y aplicar minify (plasma todo un documento json en una única línea)

>Primero creamos el topic
>>kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic json_topic_2 --create --partitions 3 --replication-factor 1

>Ahora modificamos el archivo de configuración de Kafka para añadirle otra tabla
>> gedit etc/catalog/kafka.properties

>Paramos Presto
>> bin/launcher stop

>Ahora vamos a crear la estructura de la tabla, para ello crearemos un json
>>gedit etc/kafka/json_topic.json  

>Dentro del json
>>{  
 "tableName": "json_topic_2",  
 "topicName": "json_topic_2",  
 "schemaName": "default",  
 "message": {  
 "dataFormat":"json",  
 "fields": [  
 {  
 "name": "nombre",  
 "mapping": "nombre",  
 "type": "VARCHAR"  
 },  
{  
 "name": "precio",  
 "mapping": "precio",  
 "type": "INT"  
 },  
 {  
 "name": "peso",  
 "mapping": "peso",  
 "type": "DOUBLE"  
 },  
 {  
 "name": "descripcion",  
 "mapping": "descripcion",  
 "type": "VARCHAR"  
 }  
 ]  
 }  
}  

>Vamos a crear registros
>> {"nombre":"Mesa","precio":78,"peso":35.5,"descripcion":"Color marron"}  
{"nombre":"Puzle","precio":8,"peso":0.3,"descripcion":"Más de 1000 piezas"}  
{"nombre":"Cartas","precio":2,"peso":0.1,"descripcion":"Baraja española"}  
{"nombre":"Jarrón","precio":15,"peso":1.2,"descripcion":"Color blanco"}  
{"nombre":"Silla","precio":20,"peso":2.5,"descripcion":"Pequeña"}  
{"nombre":"Florero","precio":20,"peso":1.0,"descripcion":"Cristal y metal"}

> Arrancamos Presto
>> bin/launcher start  
>> cd $HOME  
>> ./presto --catalog kafka --schema default  

> Lnazamos una consulta a la tabla
>> SELECT * FROM json_topic_2;
>> select * from json_topic_2 where peso = 0.1;