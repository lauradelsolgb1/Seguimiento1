# Seguimiento1
Laura del Sol González 

1. DESCRIPCION DEL ARCHIVO 

Describa los campos que se encontrará en este tipo de archivos. ¿Qué información está allí contenida? Proporcione ejemplos.

El formato GFF3 (Generic Feature Format Version 3) es un estándar ampliamente usado en bioinformática para describir características genómicas (genes, exones, intrones, regiones codificantes, etc.) en archivos de texto plano tabulados.
Los campos que se encuentran en este tipo de archivos son:

| Columna | Nombre       | Descripción                                                          | Ejemplo                          |
| ------- | ------------ | -------------------------------------------------------------------- | -------------------------------- |
| 1       | `seqid`      | ID de la secuencia o cromosoma donde ocurre la característica        | `chr1` o `NC_000001.11`          |
| 2       | `source`     | Herramienta o base de datos que generó la anotación                  | `GenBank`, `Augustus`, `Ensembl` |
| 3       | `type`       | Tipo de característica. Debe provenir del **Sequence Ontology (SO)** | `gene`, `exon`, `CDS`, `mRNA`    |
| 4       | `start`      | Posición inicial (1-based) de la característica en la secuencia      | `1000`                           |
| 5       | `end`        | Posición final de la característica                                  | `2000`                           |
| 6       | `score`      | Puntaje o valor de confianza (puede ser `.` si no aplica)            | `0.95`, `.`                      |
| 7       | `strand`     | Hebra en la que se encuentra: `+`, `-`, `.`, o `?`                   | `+`                              |
| 8       | `phase`      | Para CDS, indica dónde empieza el próximo codón (0, 1, 2)            | `0`, `1`, `2`, o `.`             |
| 9       | `attributes` | Lista de atributos clave-valor separados por `;`                     | `ID=gene00001;Name=BRCA1`        |

Ejemplo del archivo readme de Index of /pub/current_gff3/sorex_araneus. 

* Estructura:
- Archivo general: contiene todos los genes del genoma.

- Archivos por cromosoma: dividen la información por cromosoma.

- Archivo "abinitio": contiene genes predichos computacionalmente

* Tipos de features en GFF3

a. Genes (type en columna 3):

- "gene" → gen codificante de proteínas.

- "ncRNA_gene" → gen de ARN no codificante (ej. miRNA, snoRNA).

- "pseudogene" → pseudogen.

b. Transcritos:

- "mRNA" → transcrito codificante.

- "snoRNA", "lnc_RNA" → transcritos no codificantes.

- "pseudogenic_transcript" → transcritos de pseudogenes.

* Todos los transcritos están asociados a exones, y si son codificantes, también a:

- CDS (regiones codificantes)

- five_prime_UTR y three_prime_UTR (regiones no traducidas)

* Ejm
GL476399  ensembl gene       2596494 2601138 . + . ID=gene:ENSPMAG00000009070;Name=TRYPA3;biotype=protein_coding
GL476399  ensembl transcript 2596494 2601138 . + . ID=transcript:ENSPMAT00000010026;Parent=gene:ENSPMAG00000009070
GL476399  ensembl exon       2596494 2596538 . + . Parent=transcript:ENSPMAT00000010026;rank=1
GL476399  ensembl CDS        2596499 2596538 . + 0 ID=CDS:ENSPMAP00000009982;Parent=transcript:ENSPMAT00000010026

Significa que:
En la secuencia GL476399 (un contig del genoma de Petromyzon marinus), hay un gen codificante (TRYPA3).

Ese gen tiene un transcrito asociado, y ese transcrito tiene exones.

También hay regiones CDS, UTR, etc., relacionadas con el transcrito.

2. DESCARGA DE ARCHIVOS DE TRABAJO 
En el archivo seguimiento.txt

3. ANÁLISIS DEL ARCHIVO 

* Proporcione una breve descripción del organismo asignado en el archivo
README.

Sorex araneus

El Sorex araneus, conocido como musaraña bicolor o musaraña común, es un pequeño mamífero perteneciente al orden Soricomorpha, familia Soricidae y subfamilia Soricinae. 
Esta especie se caracteriza por un tamaño mediano dentro del grupo de los sorícidos. Su morfología incluye un hocico alargado y móvil, terminado en una pequeña trompa con vibrisas sensoriales. Los ojos son diminutos y poco visibles, y las orejas que también son pequeñas se encuentran ocultas por el pelaje, que es bicolor y varía según la estación: en verano tiende a ser café y en invierno se vuelve más largo y oscuro.

Importante saber que su número de cromosomas puede variar entre 18 y 30 debido a fusiones Robertsonianas, y los machos presentan una configuración sexual especial XY₁Y₂.

Geográficamente, el Sorex araneus ocupa gran parte de Europa central y oriental, incluyendo el Reino Unido, Escandinavia y hasta Siberia. El hábitat preferido de esta especie incluye zonas con vegetación boreoalpina y eurosiberiana, como bosques de coníferas, mixtos o caducifolios, y prefiere territorios con precipitaciones anuales superiores a los 800 mm. 

Su reproducción ocurre entre marzo y octubre, con una gestación de 19 a 24 días y una lactancia de tres semanas. Puede tener de dos a tres camadas anuales, con una media de seis crías por parto. La madurez sexual se alcanza al año, aunque la esperanza de vida rara vez supera los 18 meses.

El Sorex araneus es un depredador carnívoro de invertebrados terrestres, principalmente insectos y anélidos, aunque ocasionalmente incluye semillas en su dieta. Debido a su elevado metabolismo, necesita ingerir diariamente hasta el 90% de su peso corporal. 

En su entorno natural, es presa habitual de aves rapaces nocturnas y de mamíferos carnívoros.

tomado de: https://secem.es/sites/default/files/mamiferos/sorex-araneus/files/07.%20Sorex%20araneus.pdf

*  Investigue, usando las herramientas de la línea de comandos de Unix, y conteste las siguientes preguntas:

# 1. ¿Cuantos features (líneas de anotación) contiene el archivo? excluí las que empiezan con #
cat Sorex_araneus.COMMON_SHREW1.114.gff3 | grep -v "^#" | wc -l
- El comando cuenta el número de líneas con información real en el archivo GFF3. Con cat se muestra el contenido completo del archivo. Y con el pipe | la información se envía a grep -v "^#", que elimina todas las líneas que comienzan con el símbolo #, ya que estas corresponden a comentarios dentro del archivo y wc -l cuenta el total de líneas restantes.

# 2. ¿Cuantas regiones de la secuencia (cromosomas) contiene el archivo? Indicadas con sequence-region, ya que biological region es una parte pequeña de la biological region  
cat Sorex_araneus.COMMON_SHREW1.114.gff3 | grep "##sequence-region"| wc -l
- El comando cuenta cuántas líneas del archivo GFF3 contienen "##sequence-region". El grep busca únicamente las líneas que incluyen ese texto específico, el cual indica las regiones de secuencia dentro del archivo, wc -l, cuenta el número de líneas encontradas. 

# 3. ¿Cuántos genes están listados en el organismo? - Columna 3 tipo freature = gen o ID=gen- Osea que con la de gene no pseudo - de cada uno de los tipos de genes y contarlo
cat Sorex_araneus.COMMON_SHREW1.114.gff3 | grep -v "^#" | cut -f3 | grep -wi "gene" | wc -l
- El comando inicia con cat que muestra el contenido completo del archivo. Grep -v "^#", elimina todas las líneas que comienzan con el símbolo #. Después, el resultado filtrado sigue cut -f3, que extrae únicamente la tercera columna de cada línea, la cual indica el tipo de feature. Grep -wi "gene" busca exclusivamente las líneas donde aparece la palabra gene de forma exacta (-w) e ignorando mayúsculas o minúsculas (-i). Wc -l, que cuenta el número de líneas encontradas. 


# 4. ¿Cuál es el top 10 de tipo de features (columna 3) más anotados en el
genoma?
cat Sorex_araneus.COMMON_SHREW1.114.gff3| grep -v "^#"| awk '{print $3}'| sort | uniq -c | sort -nr | head -10
- Este comando inicia con cat que muestra el contenido completo del archivo. Grep -v "^#" elimina todas las líneas que comienzan con el símbolo #. Awk '{print $3}', imprime únicamente la tercera columna de cada línea, donde se indica el feature (por ejemplo, gene, exon, CDS, etc.). Sort organiza alfabéticamente todos los valores de esa columna, y uniq -c agrupa los valores repetidos mostrando al lado la cantidad de veces que aparecen. Sort -nr ordena esos resultados de forma numérica (-n) y en orden descendente (-r), de mayor a menor. Head -10 muestra solo los 10 primeros resultados. 

