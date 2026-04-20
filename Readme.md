REQUISITOS:
- Asegurarse de tener python 3.11
- Asegurarse de tener Java11 (la versión correcta en realidad) instalado y configurado en la PC
- Tener instalado Maven
- Tener instalado apache flink

mock_data_generator.py:
El mock_data_generator.py es un archivo python que nos ayuda a generar los datos para simular el escenario. 
    - Se generan impresiones realizando elecciones aleatorias y retornando un json con dichas elecciones. 
    - Lo mismo con los clicks, con la única distinción que consideramos un margen de 30 segundos despues del momento que se generó una impresión, puede ser que el click se "genere" 1, 2 , ... 10, 11, ... o 30 sec después de la impresión. 
    - Ahora la función para pushear el dato (json) generado ya sea de las impresiones o de los clicks, pues hace eso nomás, publicar dicho dato generado al stream que se le indique en la función main, que naturalmente será el correcto stream (digamos que un stream llamado impressions corresponde con los datos generados de la función que genera las impresiones, y digamos que el stream llamado clicks corresponde con los datos generados de la función que genera los clicks).
    - En la función main como tal se invoca a la función generate_impression() y se publica inmediatamente en su respectivo stream, ahora para darle más realismo lo que se consideró es generar un número aleatorio entre 0 y 1 y validar si este es mayor a 0.4, si es así entonces se generad el click y se publica inmediatamente. Esto se hizo así para decir que existe un 60% de probabilidad que un click se genere después de cada impresión. 



