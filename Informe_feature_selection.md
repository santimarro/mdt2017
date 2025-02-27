
# Informe Feature Selection


## Introducción

En este informe se explicará lo realizado para seleccionar features de dos formas diferentes.

Tipos de selección de features:
  - Selección de features supervisado (con corpus anotado).
  - Selección de features no supervisado.
 

### Sampleo del corpus anotado
  Se utilizó el [Wikicorpus](http://www.cs.upc.edu/~nlp/wikicorpus/) en español, el cual a su vez está particionado en diferentes documentos. Como sampleo se eligió una de estas particiones al azar y se trabajó con ella. En este caso particular se utilizó la partición _"spanishEtiquetado\_480000\_485000"_.
   Dadas caracteristicas del corpus, donde cada línea representa una palabra junto a su _lema_, _PoS_ y _synset_, para su parseo simplemente se leyó el documento y se fue almacenando la información relevante.


### Corpus anotado
  Como corpus se utilizó el del diario la voz, el cual contiene 400000 líneas de texto de noticias.
  Para su preprocesamiento, simplemente se extrajo los caracteres no relevantes para nuestro uso, como
  por ejemplo _'%'_, _'#'_, _'&'_, etc. Se conservaron las letras y las distintas puntuaciones.  
 
  Para el parseo del corpus se utilizó la librería _SpaCy_, donde primero se cargó el modelo de _español_
  y luego se le proveyó el corpus preprocesado.
  Esto nos provee un objeto tipo _Doc_, el cual posee todo el corpus tokenizado, junto a su información
  sintáctica y semántica que el mismo puede proveer.
  A partir de esta información, se procedió a armar el diccionario de features de las palabras.

### Herramientas utilizadas
  - La herramienta principal fue la librería Scikit Learn, la cual nos proveyó los distintos algoritmos para el seleccionado de features que utilizamos.

## Procedimiento

### Selección de features supervisada

#### Técnica de selección de features utilizada
  A fines del práctico se decidió utilizar la técnica de _Univariate Feature Selection_, donde el estimador es la relación entre la feature y la clase.
  Para ello se utilizó la función _SelectKBest_ [source](http://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html#sklearn.feature_selection.SelectKBest) la cual calcula esta relación, en este caso particular, utilizando la distribución de chi cuadrado.
  A esta función se le debe indicar también la cantidad de features que se quieren seleccionar, por ello, según el tamaño del corpus, este número debe variar para que sea representativo. En este caso se decidió seleccionar el 70% de las mejores features.
 
#### Diccionario de features
  Para la creación del diccionario de features se utilizó la información proveída por las anotaciones del corpus.
  Por cada palabra tomamos:
  - Su lema.
  - Su pos tagging.
  - El lema de la palabra anterior en la oración.
  - El pos tagging de la palabra anterior.
  - El lema de la palabra siguiente en la oración.
  - El pos tagging de la siguiente siguiente.
 
#### Clase de pretexto
  Dado el corpus utilizado, se nos presenta la posibilidad de utilizar como clase de pretexto el _PoS_ de las palabras o su _synset_ (sentido). En este caso se decidió utilizar su _synset_.
 

#### Vectorizado de palabras
  Para el vectorizado de las palabras se utilizó _Dictvectorizer_ el cual nos genera la matriz
  de features necesaria para realizar el clusterizado.
  Luego de generar la matriz, se realizó la selección de features utilizando la misma y la lista de synsets para cada palabra.



### Selección de features no supervisada

#### Técnica de selección de features utilizada
  Para aplicar la selección de features se utilizó la técnica de _latent semantic analysis (LSA)_.
  En este caso la función _TruncatedSVD_ [source](http://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) nos proveyó lo que se buscaba. La misma realiza una reducción de dimensionalidad lineal mediante una descomposición en valores singulares (SVD).
  El número de componentes a las cuales se redujo es a _100_, valor el cual esta descomposición se comporta como _LSA_.
 
#### Diccionario de features
  Para la creación del diccionario de features, tomando la información proveída por la librería _SpaCy_
  se tuvieron en consideración distintas características.
  Por cada token se utilizó:
  - Si el token está en minúsculas.
  - Su pos tagging (Coarse grained tagging).
  - Su tag (Fine grained tagging)
  - El token anterior en la oración.
  - El pos tagging del token anterior.
  - El token siguiente en la oración.
  - El pos tagging del token siguiente.
  - Se implementaron triplas de dependencias con formato conll.

#### Vectorizado de palabras
  Para el vectorizado de las palabras se utilizó _Dictvectorizer_ el cual nos genera la matriz
  de features necesaria para realizar el clusterizado.
  Luego de generar la matriz, se procedió a reducir las dimensiones de la misma utilizando LSA.
 

### Clusterización
  Para la clusterización se utilizaron los mismos métodos que en el práctico pasado.
 
# Resultados
## Para selección de features supervisada
  Dadas las features utilizadas para el vectorizado, los clusters obtenidos con y sin feature selection aplicada no fueron muy interesantes. Aún así se distinguieron algunas mejoras luego de aplicar la técnica de feature selection, donde, por ejemplo, se vieron agrupadas nacionalidades en un cluster o profesiones y cargos en otros.
  Lo esperado es ver clusters semánticos, dado que la clase de pretexto eran los sentidos de cada palabra, pero debido a lo mencionado previamente sobre las features, se pueden dar clusters sintácticos más que semánticos, aunque la mejora al aplicar la selección de features si tiende a ser semántica.
 
## Para selección de features no supervisada
  Se obtuvieron resultados muy parecidos al práctico de clusterizado, donde los clusters tienden a ser más sintácticos que semánticos ,dadas las features utilizadas.
    
  # Conclusión
  Mediante distintas técnicas de selección de features se redujo la dimensionalidad de las matrices, donde en el caso de _supervisado_ se inclinó por la selección de las mismas tales que provean una mejor categorización semántica. En el caso de _no supervisadas_ la reducción de dimensionalidad nos sirvió para descartar aquellas que no aportan información útil para el clusterizado.
 
 
 


