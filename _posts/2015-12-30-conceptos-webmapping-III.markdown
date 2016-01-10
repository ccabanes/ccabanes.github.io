---
layout: post
disqus: "true"
title:  "Conceptos de Cartografía III"
date:   2015-12-30 20:36:00
modified: 2016-01-10
categories: Cartography
urlshort: http://bit.ly/1TNTmHi 
mathjax: true
---
Este es el tercer post a cerca de fundamentos cartográficos. En esta entrada voy a tratar los diferentes sistemas de coordenadas.
Es muy común que se manejen diferentes tipos de coordenadas, sobre diferentes sistemas y en todos los casos se deben de saber manejar y usar correctamente en su respectivo contexto.

El fundamento inicial de cualquier sistema de coordenadas es permitir la definición de un objeto en el espacio. Desde un punto de vista matemático cualquier sistema sería admisible para ello. Sin embargo, para usos cartográficos utilizamos los que describen de forma más sencilla y práctica un objeto. 

Los sitemas se pueden clasificar en función de:

+ **La situación de su origen**
    * Sistemas **topocéntricos**: origen en un punto en la superficie terrestre.
    * Sistemas **geocéntricos**: origen en centro de masas terrestre.
+ **El tipo de coordenadas**
    * Coordenadas **carterianas**:
        - Geocéntricas ($X,Y,Z$)
        - Locales ($x,y,z$)
    * Coordenadas **curvilíneas**:
        - Polares 
        - Esféricas
        - _Geodésicas o geográficas_
        - Astronómicas
    * Coordenadas en **proyección**:
        - Sobre superficie _desarrollable_ (UTM, Lambert): **_cartesianas_**
        - Sobre superficie no desarrollable : curvilíneas

Muchos de estos sitemas son utilizados en topografía, geodésia y astronomía. Sin embargo son muy poco frecuentes dentro del mundo del GIS. En este contexto los sistemas más comúnmente usados los sistemas curvilíneos geodésicos y los sistemas proyectados sobre superficies desarrollables (UTM). Por ello voy a entrar más en detalle con estos:

#### Coordenadas Geodésicas (geográficas)
Como se ha comentado en entradas anteriores de esta serie, la inmensa mayoría de sistemas de referencia tienen como base un elipsoide de revolución. El utilizar un elipsoide de referencia implica que para un punto P sobre la superficie se corresponde una proyección P' sobre el elipsoide, y a su vez la posición de ese punto mediante dos coordenadas correspondientes a la parametrización de la superficie del elipsoide ($φ,$λ).

+ *Latitud ($φ$*): Ángulo medido en el plano meridiano que forman la normal al elipsoide en el punto P y el plano del ecuador. Los valores se encuentran entre $π/2 < φ < π/2$

+ *Longitud ($λ$*): Ángulo medido en el plano del ecuador que forman el plano meridiano que contiene al punto P y el plano meridiano de Greenwich. Los valores se encuentran entre $-π < φ < π$

![Elipsoide de Referencia]({{ site.url }}/assets/images/uploads/conceptos-webmapping-III/elipsoide.jpg)

#### Coordenadas proyectadas (UTM)
En cartografía se emplean las proyecciones para representar en un plano (2D) la situación de puntos sobre la Tierra (3D). Para poder realizar esta labor se utilizan las proyecciones cartográficas ([+info]({{site.url}}/cartography/2015/12/27/conceptos-webmapping-II/)). Una de las más utilizadas en la proyección UTM. Esta proyeción lo que hace es asignar a cada punto de coordenadas ($φ,$λ) unas coordenadas carterianas ($x,$y) sobre un plano, siguiendo unas determinadas condiciones de conformidad. Ver definición completa en la [Wikipedia](https://es.wikipedia.org/wiki/Sistema_de_coordenadas_universal_transversal_de_Mercator)

![Proyección UTM](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Utm-zones.jpg/700px-Utm-zones.jpg)

+ **Características de la proyección UTM**

    La división de la tierra  por sus meridianos y pararlelos hace que se genere una "malla" o "rejilla". Esta malla sienta la base para definir el concepto de *Huso*, que son todas aquellas posiciones geográficas que ocupan los puntos entre dos meridianos. Cada uno de ellos será de $6^0$.
    Además esto nos lleva a otro concepto que es el *meridiano central del huso*. Este es el punto donde el cilindro es tangente a la esfera con lo que los puntos que se sitúen sobre este meridiano no tendrán deformación alguna al proyectarse. Sobre esta línea, el módulo de deformación lineal $K$ es 1, aumentando linealmente conforme nos alejamos del mismo.
