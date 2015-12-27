---
layout: post
disqus: "true"
title:  "Conceptos de Cartografía II"
date:   2015-12-27 20:36:00
categories: Cartography
urlshort: 
---
Este es el segundo de la serie de post sobre conceptos de cartografía. En este caso voy a hablar de qué son los sistemas de proyección cartográficos y como afectan a la representación de los elementos sobre mapas.

Lo primero, qué es una proyección? Sigún la [WikiPedia](https://es.wikipedia.org/wiki/Proyecci%C3%B3n_cartogr%C3%A1fica) es un sistema de representación gráfico que establece una relación ordenada de puntos entre la superficie de la Tierra (esférico) y un mapa (plano). Es el paso del 3D al 2D. Este paso a las dos dimensiones siempre conlleva unas distorsiones asociadas que pueden estar asociadas con las distancias, los ángulos o las áreas medidas sobre el mapa resultante. En función de estas distorsiones se clasifican los diferentes sistemas de proyección según sus propiedades

###Propiedades de las Proyecciones Cartográficas

##### Proyecciones conformes: 
Conservan los ángulos medidos entre dos puntos en la superficie de referencia y el mapa
##### Proyecciones equidistantes
Conserva las distancias medidas en la superficie de referencia y el mapa
##### Proyecciones equivalentes
Conservan las superficies medidas en la superficie de referencia y el mapa
##### Proyecciones afilácticas
No conserva ninguna de las propiedades anteriores

También se pueden clasificar los diferentes sistemas de proyección según su tipo:
### Tipos de Proyecciones Cartográficas
#### 1. Por desarrollo. Proyección sobre superficie desenrollable
#####[Proyección cilíndrica][1] 
>"Una proyección cilíndrica es una proyección cartográfica que usa un cilindro tangente.
El cilindro es una figura geométrica que puede desarrollarse en un plano y matemáticamente es muy utilizada. La más famosa es una proyección cilíndrica modificada conocida por proyección de Mercator."
![Proyección Cilíndrica][5]  
  [5]: https://upload.wikimedia.org/wikipedia/commons/thumb/6/62/Usgs_map_mercator.svg/413px-Usgs_map_mercator.svg.png

<iframe width="100%" height="520" frameborder="0" src="https://carcaas.cartodb.com/viz/54227eb4-ac90-11e5-aeab-0ea31932ec1d/embed_map" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>

#####[Proyección cónica][2]
>"Es una proyección tangente, que toca una linea con otra, que proyecta los elementos de la superficie esferica terrestre sobre una superficie "geometrica(cono)". Tomando el vertice, punto en el que coinciden los dos angulos de un poligono,en el eje de los dos polos."

![Proyección Cónica][6]  
  [6]: https://upload.wikimedia.org/wikipedia/commons/d/da/USGS_map_Albers_conic_tall.gif


<iframe width="100%" height="520" frameborder="0" src="https://carcaas.cartodb.com/viz/5e3ba2ec-a7d4-11e5-82fd-0ecfd53eb7d3/embed_map" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>


#### 2. Azimutales. Proyección sobre un plano
#####[Proyección azimutal][3]
>"La proyección acimutal o proyección cenital, es la que se consigue proyectando una porción de la Tierra sobre un plano tangente a la esfera en un punto seleccionado, obteniéndose la visión que se lograría ya sea desde el centro de la Tierra o desde un punto del espacio exterior."

![Proyección Azimutal][7]  
  [7]: https://upload.wikimedia.org/wikipedia/commons/6/64/Usgs_map_azimuthal_equidistant.PNG


<iframe width="100%" height="520" frameborder="0" src="https://carcaas.cartodb.com/viz/2547dc16-ac8f-11e5-b9ed-0e31c9be1b51/embed_map" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>

[1]: https://es.wikipedia.org/wiki/Proyecci%C3%B3n_cil%C3%ADndrica "Wikipedia"
[2]: https://es.wikipedia.org/wiki/Proyecci%C3%B3n_c%C3%B3nica_cartogr%C3%A1fica "Wikipedia"
[3]: https://es.wikipedia.org/wiki/Proyecci%C3%B3n_acimutal "Wikipedia"
