TEMPLATE STANDAR CUANDO TRABAJAMOS CON FLINK Y AWS:
    ▼ pom.xml: el archivo que le dice a Maven qué descargar y cómo generar tu aplicación (por ejemplo, un JAR de Apache Flink). Después vamos a hacer mvn clean package para generar el zip. Y maven buscará el pom.xml por defecto.
    ▼ application_properties.json: Todo ese JSON define el contexto completo del pipeline en tiempo real (durante ejecución). Primero indica qué archivo debe ejecutar Flink, luego de dónde leer los datos (dos streams) y finalmente a dónde enviar el resultado. 
    ▼ main.py: Dónde está la lógica de la app. En este caso se leen los datos de cada stream mediante las tablas API/SQL nativas de flink y luego se hace la unión para depositarlo en una tabla resultante.
    ▼ assembly.xml: En pom.xml definimos como se construye la app y que descargar, pero en assembly que forma parte o participa dentro de pom.xml, lo que se hace es algo más específico. En realidad assembly es quien genera el zip final, pero pom se encarga de construir las demás dependencias y estructura como tal del zip final.

REQUISITOS:
- Asegurarse de tener python 3.11 (la lógica está hecha en python )
- Asegurarse de tener Java11 (la versión correcta en realidad) instalado y configurado en la PC (si corremos la app en forma local pero también se requiere dado que maven trabaja con Java para poder generar el zip)
- Tener instalado Maven (porque queremos el zip con las dependencias y todo para correrlo en aws)
- Tener instalado apache flink (pyflink)

mock_data_generator.py:
El mock_data_generator.py es un archivo python que nos ayuda a generar los datos para simular el escenario. 
    - Se generan impresiones realizando elecciones aleatorias y retornando un json con dichas elecciones. 
    - Lo mismo con los clicks, con la única distinción que consideramos un margen de 30 segundos despues del momento que se generó una impresión, puede ser que el click se "genere" 1, 2 , ... 10, 11, ... o 30 sec después de la impresión. 
    - Ahora la función para pushear el dato (json) generado ya sea de las impresiones o de los clicks, pues hace eso nomás, publicar dicho dato generado al stream que se le indique en la función main, que naturalmente será el correcto stream (digamos que un stream llamado impressions corresponde con los datos generados de la función que genera las impresiones, y digamos que el stream llamado clicks corresponde con los datos generados de la función que genera los clicks).
    - En la función main como tal se invoca a la función generate_impression() y se publica inmediatamente en su respectivo stream, ahora para darle más realismo lo que se consideró es generar un número aleatorio entre 0 y 1 y validar si este es mayor a 0.4, si es así entonces se generad el click y se publica inmediatamente. Esto se hizo así para decir que existe un 60% de probabilidad que un click se genere después de cada impresión. 



