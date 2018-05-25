## Resumen: Filtrado y limpieza de datos y ensamblaje de transcriptoma de Novo.

En este trabajo repliqué parte del trabajo de Meslin
et al (2015), quienes ensamblaron el transcriptoma de la mariposa *Pieris rapae* utilizando RNA del órgano genital femenino, llamado Corpus bursae, y otros tejidos de esta especie con el fin de entender la evolución del Corpus bursae, el cual actúa de manera similar a un estómago. Para mi este estudio es interesante ya que el obtener este tipo de información permite identificar genes expresados en los órganos de estas especies y compararla con la información de genes expresados en otros órganos y especies, para estudiar su función, su orígen y evolución, e identificar las presiones selectivas que han llevado a esto.

###Obtención de archivos y muestra

Aunque las secuencias que utilizaron los autores no se encontraron en un repositorio, les escribí para pedirles estos datos. Los autores me envaron las secuencias de RNA que utilizaron, las cuales se encuentran en archivos, cada uno de RNA de distintos órganos y algunos de distintas condiciones del Corpus bursae. Estos archivos en total pesan alrededor de 60 GB, por lo que, aunque realicé los análisis con los datos completos, obtuve una muestra muy pequeña de estos datos para enviar estos resultados.

Para obtener la muestra utilicé únicamente las secuencias del Corpus bursae (cuyos archivos inician con BC) y con estas obtuve una muestra con las primeras 400 líneas. Para esto me ubiqué en la carpeta con los archivos y realicé un for loop para aplicar el comando a todas estas secuencias. Al utilizar archivos comprimidos en formato gz, primero especifiqué en cada comando redireccionando para manejarlo sin descomprimirlo. De esta forma utilicé el comando:

for i in B*; do

zcat $i | head -400  > muestra$i;

done

Con lo que obtuve los archivos muestraBC<string>.fastq.gz que se encuentran en la carpeta `/SecuenciasPierisrapae`

## Análisis

Realicé los análisis  con los datos completos y con las muestras. Para realizar los análisis leí los manuales y los requerimientos de los programas que utilicé y ví que se necesita una buena cantidad de memoria RAM para correr trinity (~1G of RAM por ~1M pairs de lecturas de Illumina), por lo que utilicé la mejor computadora que conseguí que me prestaran y traté de instalar los programas que utilicé y los programas anexos que estos necesitarían, sin embargo tuve varias dificultades, por lo que preferí utilizar docker para trabajar en contenedores con los programas que utilicé.

###Filtrado de secuencias

Utilicé FASTX Toolkit para realizar el filtrado de secuencias por calidad. Utilicé una imagen de docker y monté un volúmen para filtrar las secuencias con calidad menor a 30. Corrí este comando on un loop para aplicarlo a todos los archivos de la carpeta `/SecuenciasPierisrapae`.

MIentras que al redireccionar archivos comprimidos en formato gz utilizando zcat funcionó con los archivos grandes, al hacerlo con los archivos de muestra reducidos, no reconoce se formato g.zip, por lo que en estos cambié el comando zcat por cat.

Los comandos utilizados en parte del análisis se encuentra en el script  `Trim`  y los archivos de salida se encuentran en la carpeta `/FilteredSec`.

###Ensamblado de transcriptoma

Para realizar el ensamblado del transcriptoma bajé también una imagen de docker, monté un volúmen en dode corremos el biocontenedor con trinity. El comando para hacer esto en trinity es un poco distinto a los otros programas, por lo que presentó dificultades. 

Al correr el contenedor corrí el comando de ensamblado del transcriptoma de trinity con mis opciones,indicando que utilizamos muestras paired end. a su vez el comando de trinity es un poco confuso, ya que lo intenté correr de varias formas que no funcionaron antes de lograrlo, Obtuve el resultado de la muestra, el cual se encuentra en la carpeta `/trinity_out_dir`, mientras que los comandos se encuentran en el script  `Assambly`.

##Discusión

 Debido a que mis estudios se enfocan en evolución de la conducta de artrópodos, mi interés principal en este curso y este proyecto es el poder manejar datos genómicos, transcriptómicos o proteómicos para poder identificar expresión de genes en distintas especies  y bajo diversas condiciones, para lo cual necesito primero poder construir los genomas completos (ya que no hay muchos datos más alla de las especies modelo) y comparar su expresión diferencial.
 
 En el estudio en el que me basé para este proyecto, además del ensamblado del transcriptoma, los autores realizaron una serie de análisis prácticos que me parecen muy útiles para futuros estudios que puedo realizar, pero para los cuales es necesario primero contar con un transcriptoma de la especie que se estudia.

 Los autores estimaron abundancia del transcrito e identificaron transcritos diferencilmente expresados utilizando el programa EdgeR de bioconductor en R. Este programa podría permitirme posteriormente obtener muestras de RNA expresado bajo diferentes condiciones e identificar su expresión diferencial. El artículo presenta algunos heatmaps donde presenta el nivel de expresión en el Corpus bursae en comparación con otros órganos, así como el nivel de expresión de los genes en distintos órganos que realiza con datos con los que no conté. Debido a que no conté con datos para graficar y a el límite de tiempo, no me dió tiempo de buscar otros datos para graficar, no pude realizar esta parte del proyecto
 


En el artículo se identificó la función anotada de los genes, con lo que se identifiacaron las funciones principales del órgano estudiado. Personalmente este tipo de análisis me puede ayudar a identificar funciones de los órganos y presiones selectivas que pueden intervenir en esto. En este artículo también se buscaron genes homólogos utilizando un BLASTP de fragmentos del transcriptoma contra proteinas en bases de datos de las especies de lepidópteros más estudiadas, lo que personalmente me puede ayudar a detectar relaciones filogenéticas entre los insectos que estudio.

###Referencias
Revisé los artículos: Next generation transcriptome assembly (Martin & Wang, 2011), y The simple fool’s guide to population genomics via
RNA-Seq: an introduction to high-throughput sequencing
data analysis (Wit P, Pespeni H et al., 2012).

-          Meslin, C., Plakke, M.S., Deutsch, A.B., Small, B.S., Morehouse, N.I., and Clark, N.L., 2015. Digestive Organ in the Female Reproductive Tract Borrows Genes from Multiple Organ Systems to Adopt Critical Functions. Molecular Biology and Evolution 32, 1567-1580.
