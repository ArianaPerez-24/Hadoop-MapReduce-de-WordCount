**LEVANTAR NODO HADOOP*

Se debe tener intalado Doker desktop y descargar del siguiente repositorio o de el mio para poder ser ejecutado 
Referencia: https://github.com/big-data-europe/docker-hadoop 
(Esta carpeta debe ser guardada en una nueva y renombrarla),una vez creada tu cuenta en el programa doker es momento de abrir el cmd para ejecutar lo siguiente:
docker-compose up -d #Este comando iniciará los cinco contenedores requeridos, pero debe estar abierto doker para que sea posible
docker exec -it namenode bash #Este comando es para entrar al nodo maestro y poder empezar a realizar 3 ejercicios.

1.Contar Palabras
Use un archivo .jar que contiene las clases necesarias para ejecutar el algoritmo MapReduce
https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/ descargar el archivo llamado 'hadoop-mapreduce-examples-2.7.1-sources.jar'
movi el archivo a la misma carpeta llamada 'doker-hadoop-master'

Descargar o utilizar un arcivo txt. y moverlo a la misma carpeta, en este caso mi archivo se llama 'Nomina'

Para mover el archivo hadoop-mapreduce-examples-2.7.1-sources.jar ejecute
docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp

Hice lo mismo para el archivo Nomina.txt
docker cp Nomina.txt namenode:/tmp 

Ingrese al contenedor namenode ejecutando 
docker exec -it namenode bash
despues hdfs dfs -mkdir /user/root/input_contador

Copiamos el archivo de texto a HDFS
cd /tmp despues hdfs dfs -put el_quijote.txt /user/root/input_contador

Ejecutamos el MAPREDUCE
hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador

Mostramos resultados
hdfs dfs -cat /user/root/output_contador/*
Comprobamos resultados accediendo a la carpeta de salida
hdfs dfs -ls /user/root/output_contador

Exportamos los resultados, colocándolo en un archivo .txt y moverlo a nuestra carpeta base
hdfs dfs -cat /user/root/output_contador/part-r-00000 > /tmp/resultado_nomina.txt
 exit
 docker cp namenode:/tmp/resultado_nomina.txt .
Ahora el archivo de texto esta en la carpeta del  repositorio docker-hadoop nombrado 'resultado_nomina'

2.Ordenar números de Menor a Mayor (Temperaturas)
Dsescargue el archivo txt de temperaturas https://github.com/Rkrahul04/Maximum_Temp_Calculation_mapreduce/blob/master/Dataset%20-%20Calculate%20Maximum%20Temperature/Temperature
Mover a la misma carpeta de hadoop
como ya tenemos este archivo hadoop-mapreduce-examples-2.7.1-sources.jar
movemos el archivo de temperatura 
docker cp Temperature.txt namenode:/tmp

Creamos la carpeta de entrada
Ingrese=amos al contenedor namenode ejecutando 
docker exec -it namenode bash
Luego 
hdfs dfs -mkdir /user/root/input_temperaturas

Copiamos el archivo .txt a HDFS
Ejecutamos: 
cd /tmp 
Después: 
hdfs dfs -put Temperature.txt /user/root/input_temperaturas

Ejecutamos el MAPREDUCE
hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.SecondarySort input_temperaturas output_temperaturas

Resultados
hdfs dfs -cat /user/root/output_temperaturas/*
Comprobar resultados accesiento a la carpeta de salida:
hdfs dfs -ls /user/root/output_temperaturas

Exportamos nuestro archivo en nuestra carpeta base
hdfs dfs -cat /user/root/output_temperaturas/part-r-00000 > /tmp/temperaturas_ordenadas.txt 
Después ejecutamos:
exit
Por último: 
docker cp namenode:/tmp/temperaturas_ordenadas.txt .
Ahora el archivo de texto esta en la carpeta del  repositorio docker-hadoop.

3.Resolver Sudoku
Usaremos un archivo .jar que contiene las clases necesarias para ejecutar el algoritmo MapReduce
https://drive.google.com/file/d/1m6uSyzNCQV1617fmhQyW59sMQ2DYef2E/view

Descargamos el archivo .txt puzzle1.dta y lo movemos a la misma carpeta donde esta el repositorio docker-hadoop.
Para mover el archivo hadoop-examples-0.20.205.0.jar ejecute
docker cp hadoop-examples-0.20.205.0.jar namenode:/tmp 
Hacemos lo mismo para el archivo puzzle1.dta
docker cp puzzle1.dta namenode:/tmp 

Ingrese al contenedor namenode ejecutando 
docker exec -it namenode bash
Ejecute: 
cd /tmp 
Ejecutar
hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.dta

Resultados:
Ejecutar: 
hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.dta > solucion_puzzle1.txt 
Después ejecutamos:exit
Por último: 
docker cp namenode:/tmp/solucion_puzzle1.txt .
El archivo ya se descargo en nuestra carpeta base.

APAGAR EL NODO DE HADOOP:
Como estamos utilizando doker ejecutamos en el cmd:
docker stop namenode
También puedes detener todos los contenedores relacionados ejecutando;
docker ps   # para ver todos
docker stop namenode datanode resourcemanager nodemanager historyserver

Otra opción es detener desde la aplicación doker desktop en la parte del cuadrito de STOP.
