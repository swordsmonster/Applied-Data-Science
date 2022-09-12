# **Universidad de los Andes**
## **Ciencia de datos aplicada**

---

## **Taller 1**

---

### **Punto 1**
Describa el conjunto de datos, tipos de variables y destaque cualquier problema de calidad
de datos y procesos de limpieza que deba implementar.

#### Lectura de los datos
El conjunto de datos tiene carácteres de doble comilla que dificulta la lectura de los mismos, puesto que las dobles commilas se usan para indicar que adentro hay varios "objetos" separados por coma, por lo que no los deberíamos tener en cuenta al momento de interpretar el archivo CSV (archivo separado por comas).
Debido a lo anterior, se definió una lógica que permita reconocer las frases o palabras contenidas entre las dobles comillas y remover las posibles comas que se encuentren dentro de ellas. Asimismo, teniendo en cuenta que tras realizar esta limpieza las dobles comillas no generan valor, se decide remover cada dobles comillas usadas para especificar lo anteriormente descrito.

Se definieron dos funciones para el procesamiento:
* find_quotes: encuentra todas las dobles comillas ("") en cada una de las líneas del texto usando una expresión regular.
* remove_commas: remueve todas las posibles comas que es encuentran dentro de las comillas. Hace uso de los índices para verificar sub-strings y remover la información deseada.

Teniendo en cuenta la limpieza realizada a los datos, se crea un nuevo archivo del conjunto de datos con toda la información limpia.

La lógica consiste en examinar los registros que empiezan y terminan con comillas, puesto que esto indica que el registro puede contener varias comillas.

Adicionalmente, se encontro un problema de lectura con la fila 1977, que se debe a la estructura del nombre del artista, ya que su nombre tiene una coma:
* Nombre del artista: Tyler, The Creator
* Canción: EARFQUAKE

La coma en el nombre es un inconveniente dado que la estructura del conjunto de datos usa las comas para separar cada campo enla fila correspondiente a una columna. De esta forma, la coma adicional indicaría que existen 19 columnas en lugar de 18 (todos los regitros deben tener 18 columnas).

Una vez solucionados los inconvenientes anteriores se logra una correcta lectura de los datos y se procede a analizar particularidades de los datos:
* Registros duplicados: son eliminados
* Valores nulos: se imputa un vavlor promedio para los valores nulos en el conjunto de datos, siempre y cuando el campo sea numérico. Para aquellos registros que contienen nulos en un campo no númerico se elimina el registro completo.

Adicionalmente, se realiza un análisis de los datos usando la librería Pandas Profiling, lo que permite ve con mayor detalle una descripción de cada columna del conjunto de datos.

Tipos de datos:
|       Columna     | Tipo  |
|-------------------|-------|
|artist             |object |
|song               |object |
|duration_ms        |float64|
|explicit           |object |
|year               |int64  |
|popularity         |float64|
|danceability       |float64|
|energy             |float64|
|key                |int64  |
|loudness           |float64|
|mode               |int64  |
|speechiness        |float64|
|acousticness       |float64|
|instrumentalness   |float64|
|liveness           |float64|
|valence            |float64|
|tempo              |float64|
|genre              |object |

Teniendo en cuenta que en la casilla de género existen diferentes valores válidos, es de nuestro interés encontrar los valores únicos:
|   Géneros únicos    |
|---------------------|
| 'Folk/Acoustic'     |
| 'latin'             |
| 'metal'             |
| 'set()'             |
| 'hip hop'           |
| 'blues'             |
| 'country'           |
| 'Dance/Electronic'  |
| 'easy listening'    |
| 'World/Traditional' |
| 'classical'         |
| 'jazz'              |
| 'R&B'               |
| 'rock'              |
| 'pop'               |

Se hace una análisis de correlación entre las columnas para entender el comportamiento de los datos:
Se puede observar que hay algunas columnas correlacionadas entre si, como por ejemplo: 
* valence con danceability
* loudness con energy
* energy con acousticness
entre otras.

Lo ideal es remover columnas algunas de las columnas correlacionadas, ya que no aportan información a un modelo. Sin embargo, dado que el taller está enfocado al analísis de los datos, se mantendrán estas columnas.

---
### **Punto 1**
¿Cuál es el top 10 de artistas más activos de los últimos 10 años?

Se define actividad de los artistas como aquellos que presentan mayor número de apariciones a través de los años. De esta forma se realizará un conteo de las apariciones de los artistas (teniendo en cuenta que aparece una vez por cada canción), durante los últimos 10 años para definir cuales de ellos están en el Top 10.

Se acotó nuestro conjunto de datos de acuerdo a la condición de los últimos 10 años. Se asume como año actual el 2022 y se toman 10 años hacía atrás a partir de este. Se realiza el conteo de las aparciones a través de los últimos años de los artistas y finalmente se ordena el conteo realizado. De esta forma se toma únicamente los 10 artistas con mayor número de apariciones.

El top 10 de artistas encontrados para los últimos 10 años es:

|        artist | count |
|--------------:|------:|
| Calvin Harris |    18 |
|         Drake |    16 |
| Ariana Grande |    13 |
|  David Guetta |    13 |
|  Taylor Swift |    13 |
|    Katy Perry |    11 |
|        Avicii |     9 |
|      Maroon 5 |     9 |
|    Ed Sheeran |     9 |
| One Direction |     9 |

---
### **Punto 2**
¿Cómo se diferencian las canciones de los géneros de Latin y Folk/Acoustic en relación con
su duración? Halle la diferencia del tiempo promedio de ambos géneros.

Para este análisis se construyen dos dataframes: uno por cada género, y se saca el promedio total de la duración.
En este caso se encontró que los promedios de tiempos de duración son:

|        género | promedio |
|--------------:|---------:|
| Latin         |  227.5   |
| Folk/Acoustic |  220.18  |

La diferencia del tiempo promedio de ambos géneros es:
7.32

---
### **Punto 3**
Halle el top 5 de los géneros del 2019 según la cantidad de canciones. ¿Cómo ha variado la
cantidad de canciones del Top de géneros en los años 2000, 2005, 2010, 2015, 2019?

Teniendo en cuenta los géneros del conjunto de datos, se revisan las canciones por año y género para cada año, de esta forma podemos contar apariciones de canciones para un género y para cada año específico.

Se encuentran los top 5 de gènero para los años 2000, 2005, 2010, 2015, 2019.

Para poder ver la progresión a través de los años de los géneros, se construye un dataframe que cada fila corresponde a un año y cada columna a un género, lo que nos permite realizar una representación gráfica del mismo.

Top 5 de los años:
| Top 5 Genres -  2000  |
Genre: metal, Count: 7 
Genre: rock, Count: 12 
Genre: hip hop, Count: 20 
Genre: R&B, Count: 24 
Genre: pop, Count: 55 

| Top 5 Genres -  2005  |
Genre: metal, Count: 5 
Genre: rock, Count: 18 
Genre: R&B, Count: 30 
Genre: hip hop, Count: 50 
Genre: pop, Count: 78 

| Top 5 Genres -  2010  |
Genre: rock, Count: 6 
Genre: R&B, Count: 22 
Genre: Dance/Electronic, Count: 28 
Genre: hip hop, Count: 52 
Genre: pop, Count: 94 

| Top 5 Genres -  2015  |
Genre: rock, Count: 13 
Genre: R&B, Count: 19 
Genre: Dance/Electronic, Count: 28 
Genre: hip hop, Count: 29 
Genre: pop, Count: 83 

| Top 5 Genres -  2019  |
Genre: latin, Count: 8 
Genre: R&B, Count: 9 
Genre: Dance/Electronic, Count: 21 
Genre: hip hop, Count: 38 
Genre: pop, Count: 63 

---
### **Punto 4**
¿Cómo ha sido la progresión de nuevos artistas? Asuma que un artista nuevo es aquel del
cual no se tiene registros pasados y solo es nuevo durante el primer año de aparición.

Se considera como nuevo artista quien un año realice una aparición y en ningún año anterior halla aparecido.
Para este análisis se analizan los artistas de cada año (actual) con respecto a los años anteriores. Si se observa una aparición en años anteriores el artista ya no es considerado como nuevo. Si no se encuentra ninguna aparición previa, el artista se define como nuevo.

Se define el año 1998 como inicial y se empezarán a considerar nuevos artistas a partir del año 1999. El conjunto de datos tiene únicamente un registro para el año 1998, por lo que para el año 1999 todos los artistas serán nuevos (excepto por el artista presente en el año 1998).

A continuación se puede ver una tabla con el número de artistas encontrados por cada año.
| año  |número de artistas |
|-----:|------------------:|
| 1999 |                30 |
| 2000 |                55 |
| 2001 |                60 |
| 2002 |                39 |
| 2003 |                42 |
| 2004 |                44 |
| 2005 |                40 |
| 2006 |                38 |
| 2007 |                37 |
| 2008 |                33 |
| 2009 |                31 |
| 2010 |                35 |
| 2011 |                34 |
| 2012 |                34 |
| 2013 |                33 |
| 2014 |                41 |
| 2015 |                39 |
| 2016 |                40 |
| 2017 |                44 |
| 2018 |                39 |
| 2019 |                41 |
| 2020 |                 3 |

---
### **Punto 5**
Grafique la progresión de la popularidad promedio por género y por año. Concluya sobre
la gráfica, ej: ¿existen tendencias?

Se considera la popularidad del género por año como el promedio total del valor de "popularity" de las canciones de un mismo género.
El análisis se hizo construyendo un dataframe que contiene el promedio de la popularidad por gènero para cada año.

De la gráfica se concluye que existe tendencia respecto a algunos géneros, como por ejemplo el Rock y el Pop que se han mantenido a travès de los años. Por otra parte, el Metal a tendido a desaparecer en los ùltimos años tras haberse mantenido. Géneros como el Jazz y Clásico tienen pocas apariciones a través de los años.

---
### **Punto 6**
Compare los géneros Pop y Rock según sus características de: energy, valence y tempo.
Concluya sobre su análisis.

Para este análisis se extraen los registros que tengan cada tipo de género. Es decir, para el género "pop" se buscan todos los registros que en su campo género contenga la palabra "pop", de esta forma sabemos cada canción de didcho género. De la misma forma se hace para el género "rock" extrayendo todos los registros que contienen dicho género.
Una desventaja es que pueden existir canciones que están clasificadas con género "rock" y "pop", por lo que se encontrarán repetidas a la hora de armar cada dataframe. Una canción que contiene los dos géneros estará presente en los dos dataframes (uno de rock y otro de pop).

Tras analizar algunos datos estadísticos se pudo ver que existen muchas más canciones de género "pop" que de género "rock" en nuestro conjunto de datos, por lo que en este caso nuestros datos se encuentran desbalanceados.
Por otra parte,  se observa que las medidas estadísticas son muy similares para ambos géneros, por lo que tiene sentido encontrar canciones clasificadas con los dos géneros. Para el caso de las canciones del género "rock", los promedios de "energy" y "tempo" son más altos que los correspondientes para "pop", con lo que deducimos que las canciones del género "rock" son más rápidas y con más "energía".

Se pudo observar que los valores para el género "pop" son menores en general que los correspondientes para el género "rock", lo cual está alineado con lo visto en los datos estadísticos.

En general se observó que los datos para "energy", "valence" y "tempo" entre los géneros "pop" y "rock" son similares. 
De acuerdo a la descripción de cada campo del conjunto de datos, se tiene que:
* Energy: Reprecenta un amedida perceptual de intensidad y actividad.
* Valence: Describe el positivismo musical transmitido por una canción.
* Tempo: El tempo general estimado de una pista en pulsaciones por minuto (BPM).

Del análisis anterior podríamos decir para las canciones de nuestro conjunto de datos que el "rock" es un género que tiene una mayor intensidad y actividad, en comparación con el "pop". Asimismo, tiene un tempo mayor, por lo que se dice que las piezas musicales deben ejecutarse a una mayor velocidad.
Por otra parte, el género "pop" tiene un valor medio de "Valence" mayor, por lo que decimos que el positivismo transmitido por una canción del género "pop" es mayor.

---
### **Punto 7**
Plantee una pregunta de negocio de su interés, ya sea por tipo de música, artistas u otra
dimensión, mediante la cual se analicen al menos 3 variables del dataset y concluya.

#### Pregunta de negocio:

Teniendo en cuenta que existen canciones con contenido explicito y que la música está en una continua evolución de ritmos y/o géneros, es de nuestro interés conocer cómo ha evolucionado la popularidad de este tipos de canciones a través de los años. Esta información permitiría saber si es conveniente impulsar canciones de este género, o por el contrario, considerar menos este tipo de canciones, de acuerdo a las tendencias de los usuarios.

¿Deberíamos apoyar más las canciones con contenido explicito?

Para este anàlisis consideramos 3 variables del conjunto de datos: popularidad, año y artista.

Teniendo en cuenta que en el primer año (1998) y en el último año (2020) se cuenta con un bajo número de canciones registradas, se decide omitir la información obtenida para dichos años.

Se observó que existe un comportamiento similar entre el número de artistas por año y el promedio de la popularidad de las canciones. Adicionalmente, se observa que en los últimos años de nuestro conjunto de datos las canciones marcadas con contenido explicito han venido incrementando su popularidad, tanto en la popularidad de cada canción como el número de artistas que generan este tipo de contenido.

De acuerdo a lo anterior, se concluye que si debería promoverse más el apoyo a este tipo de canciones desde una perspectiva de negocio, puesto que su popularidad se ha incrementado en los últimos años y esto podría traer mayores ganancias.