---
layout: post
title:  "Conceptos de Cartografía I"
date:   2015-11-15 10:18:00
categories: Cartography
---
Este es el primero de una serie de post con los que quiero empezar este blog, el cual pretendo iniciarlo desde hace ya un tiempo. 
Hoy en día topógrafos, cartógrafos, geógrafos, agrónomos, forestales, informáticos, telecos, son solo una muestra de los profesionales que nos dedicamos al mundo del GIS. En definitiva, hacer mapas.

Cada uno de estos profesionales suelen intervenir en diferentes partes del proceso de elaboración de mapas. Toma y generación de datos, procesado y depuración de datos, gestión y administración de la información, implementación  y desarrollo de aplicaciones, etcétera.

Sin embargo, todos ellos (o al menos la mayoría de ellos) deberían de conocer con suficiente claridad los conceptos relacionados con la cartografía, la geodesia y su representación. Por ello, con esta serie de post voy a intentar aclarar algunos conceptos que se aplican constantemente y muchas veces no lo tenemos lo suficientemente claro.

Este primer post pretende aclarar un poco los diferentes términos y las definiciones de los conceptos necesarios para aclarar una pregunta. Qué son los sistemas de referencia cartográficos o CRS?

### EPSG

Las siglas provienen de _European Petrolium Survey Group_. Esta fué la primera organización que trabajó en el cálculo un conjunto de datos geodésicos como elipsoides, datums, proyecciones, etc. Hoy en día utilizamos su identificador **SRID** para identificar Sistemas de Referencia.

### SRID

Identificador numérico único con el que podemos identificar los diferentes sistemas de referencia.

### Sistemas de coordenadas

Según la wikipedia, un sistema de coordenas geométrico es: 
>un sistema que utiliza uno o más números (coordenadas) para determinar unívocamente la posición de un punto o de otro objeto geométrico 

[Ver definición completa en la Wikipedia](https://es.wikipedia.org/wiki/Sistema_de_coordenadas). 

Estos sistemas los podemos clasificar en fución de:


#####1. Situación del origen

***Sistemas de coordenadas topocéntricos*** : Su origen está situado sobre la superficie terrestre.

***Sistemas de coordenadas geocéntricos*** : su centro de coordenadas está situado en el centro de masa terrestre. Se corresponde con sistemas de referencia globales.

#####2. Tipo de coordenadas

* ***Coordenadas cartesianas***: 

    - _Geocéntricas_.

    - _Locales_.

* ***Curvilíneas***: 

    - _Geodésicas o geográficas (φ, λ, h/H)_.

    - _Polares_.

    - _Otras_.


* ***Coordenadas sobre una proyección***:

    - _Cartesianas_.

    - _Curvilíneas_.


###Sistema de Referencia Espacial 

Un sistema de referencia define la estructura geométrica para poder definir puntos en el espacio. Queda definido por el origen, el eje, la escala, así como con los algoritmos necesarios para realizar sus transdormaciones espaciales.

###Datum

Es el conjunto mínimo de parámetros que permiten definir de forma única el orige, la orientacioón y la escala de un sistema de coordenadas. Normalmente se aplica a la realizad mediante la obtención de un conjunto de coordenadas o bien mediante unos parámtros de transformación.

###Marco de Referencia

El Marco de Referencia es un cojunto de puntos con ubicación espacial respecto a un sistema de referencia. Normalmente este marco es la forma más común de definir un Datum. 


###Sistema de Referencia de Coordenadas (CRS)

De una forma sencilla, un Sistema de Referencia de coordenadas es la suma de un sistema de coordenadas y un datum, de forma que se pueda definir de forma única un punto sobre dicho sistema. 
Aplicado al ámbito geográfico, la posición de un punto sobre la Tierra se describe por un conjutno de coordenadas y éstas son inequívocas solo cuando el sistema de referencia de coordenadas al cual están referidas se ha definido completamente. Esto pasa por definir el Datum para dicho sistema. 


[Atributos para describir un CRS (ISO 19111)](http://redgeomatica.rediris.es/traducciones/ISO_19111_Sistema_de_Referencia_MABP.pdf)

[Consulta la descripción completa en Wikipedia](https://en.wikipedia.org/wiki/Spatial_reference_system) 

######Sistemas de Referencia más comunes:
- **4326 (WGS84)**
- **3857 (WGS84 Web Mercator)** 
- **25830 (ETRS89 / UTM zone 30N)**

Las imágenes siguientes muestran la definición según [spatialreference.org](http://spatialreference.org/) de los sistemas de referencia más comunes en España. Hoy en día es muy común trabajar con datos y servicios que nos brindan la información en dichos sistemas y una pregunta muy común es saber como transformar entre ellos. 

Pues bien, entre los sistemas 4326 y 3857 es muy sencillo. Ambos sistemas tienen mismo Datum y Elipsoide de referencia (WGS84). Lo que les hace diferentes es la proyección. Mientras que el sistema 4326 utiliza un sistema de coordenadas geográfico, el 3857 lo hace con un sistema proyectado.



[![SRID: 4326][2]][1]

  [1]: http://spatialreference.org/ref/epsg/4326/
  [2]: {{ site.url }}/assets/images/uploads/4326.png (Ver descripción completa en  spatialreference.org)


[![SRID: 3857][4]][3]

  [3]: http://spatialreference.org/ref/sr-org/7483/
  [4]: {{ site.url }}/assets/images/uploads/3857.png (Ver descripción completa en  spatialreference.org)

[![SRID: 25830][6]][5]

  [5]: http://spatialreference.org/ref/epsg/25830/
  [6]: {{ site.url }}/assets/images/uploads/25830.png (Ver descripción completa en  spatialreference.org)



###Sistemas de Proyección Cartográficos

[Según la definición de la WikiPedia](https://es.wikipedia.org/wiki/Proyecci%C3%B3n_cartogr%C3%A1fica)
>Los sistemas de proyección cartográficos son sistemas de representación gráficos que establecen una relación entre la supercicie curva de la Tierra y los de una superficie plana.


