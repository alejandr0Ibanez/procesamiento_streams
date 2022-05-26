# 1 - ¿Qué pasaría si cancelamos (utilizando Ctrl+C) uno de los consumidores (quedando 2) y seguimos
> El texto que debería de mostrar ese consumidor no se pierde, se distribuye entre los otros que quedan

# 2 - ¿Qué pasaría si cancelamos otro de los consumidores (quedando yasolo 1) y seguimos enviando
> El texto no se perdería, lo consumiría todo el único consumidor que queda

# 3 - ¿Qué sucede si lanzamos otro consumidor pero está vez de un grupo llamado my-second-application leyendo el topic desde el principio (--from-beginning)?
>Sí, muestra todos los mensajes desde el principio

# 4 - Cancela el consumidor y ¿Qué sucede si lanzamos de nuevo elconsumidor pero formando parte delgrupo my-second-application?¿Aparecen los mensajes desde el principio?
>No lee ningún mensaje porque los del principio ya los ha leido otro consumidor. El consumidor mostrará los mensajes dedsde el principio si no han sido consumidos por consumidores del mismo grupo

# 5 - Cancela el consumer, a su vez aprovecha de enviar más mensajes utilizando el producer y de nuevo lanza el consumidor formando parte del grupo my-second_application ¿Cuál fue el resultado?
>Si paramos el consumer que lee desde el principio, escribimos y lo volvemos a iniciar podemos ver como hemos dicho antes que no se veran los mensajes ya que había otro consumer del mismo grupo que los estaba leyendo

# Ejercicio final 

* Crear topic llamado "topic_app" con 3 particiones y replication-factor = 1
> kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic topic_app --create --partitions 3 --replication-factor 1 

* Crear un producir que inserte mensajes en el topic recien creado (topic_app).
>kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic topic_app

* Crear 2 consumer que forman parte de un grupo de consumo llamado "my_app"
>kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic topic_app --group my_app

* Aplicar los comandos necesarios para listar los topics, grupos de consumo, asícomo describir cada uno de estos.
>Listar topics
>>kafka-topics.sh --zookeeper 127.0.0.1:2181 --list 

> Listar grupos de consumo
>> kafka-consumer-groups.sh --bootstrap-server 127.0.0.1:9092 --list 

>Describir topics
>>kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic topic_app --describe

> Describir grupos de consumo
>> kafka-consumer-groups.sh --bootstrap-server 127.0.0.1:9092 --describe --group my_app