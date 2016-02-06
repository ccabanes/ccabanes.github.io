---
layout: mapPost
disqus: "true"
title:  "Visualizar WMTS en Leaflet"
date:   2016-01-17 20:36:00
categories: "WMTS, webservices, webmapping, GIS, leaflet, OGC"
urlshort: 
mathjax: false
mapa: "true"
urlmapa : cantabria.html
mapTitle : Demo WMTS con leaflet.js en CRS:25830
---

El uso de servicios de cartografía base está ampliamente extendido en el uso de aplicaciones de webmapping. Los más conocidos por ser los primeros en llegar fueron los servicios WMS. Esta popularidad está fuertemente ligada a la cantidad de usuarios que hacen uso del mismo, lo que genera una nueva necesidad: **eficiencia en cuanto a la agilidad de la respuesta de los servicios, costes computacionales, etc.**
Bajo estas premisas nace el servicio OGC WMTS que lo que pretende es mejorar la calidad del servicio haciéndolo más rápido y eficaz. La pregunta es:

Y como mejoramos el servicio WMS? 

Básicamente la mejora que implementa el servicio WMTS respecto a WMS es que las imágenes o teselas que responde de cada petición **ya están generadas en el servidor**, es decir, ya están cacheadas. Esto hace que se reduzca al mínimo los costes computacionales a nivel de servidor y mejore los tiempos de respuesta.
En cuanto a las alternativas para implementar el servicio, los más populares son **[Mapproxy](http://mapproxy.org/)**, **[GeoWebCache](http://geowebcache.org/)**, **[TileCache](http://tilecache.org/)**.


###Principios del servicio WMTS
Es muy recomendable que antes de continuar, se lea la documentación de la especificación del servicio ([aquí](http://www.opengeospatial.org/standards/wmts#downloads)). Como en este post nos vamos a centrar en el consumo del servicio, te recomiendo que leas el capítulo _6 WMTS overview_ y el _anexo E_.

####Fundamento general
El fundamento general en el que se basa el servicio WMTS es el almacenamiento de las imágenes ya generadas y cacheadas. La gestión de la información se hace teniendo en cuenta la escala o zoom a la que se encuentra el usuario y en función de una matriz, que divide las diferentes teselas.

![TileMatrixSet]({{ site.url }}/assets/images/uploads/wmts/tilematrixset.png)

De esta forma, para cada nivel de zoom se tendrá almacenado las teselas. A menor denominador de escala, mayor número de teselas y viceversa.

####TileMatrixSet



####TileMatrix

![TileMatrix]({{ site.url }}/assets/images/uploads/wmts/tileMatrix.png)