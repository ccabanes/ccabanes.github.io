---
layout: post
disqus: "true"
title:  "Servicios WMTS"
date:   2016-02-12 09:33:00
categories: "WMTS webservices webmapping GIS OGC"
urlshort: http://bit.ly/1Qy0Zib 
mathjax: true
---

El uso de servicios de cartografía base está ampliamente extendido en el uso de aplicaciones de webmapping. Los más conocidos por ser los primeros en llegar fueron los servicios WMS. Esta popularidad está fuertemente ligada a la cantidad de usuarios que hacen uso del mismo, lo que genera una nueva necesidad: **eficiencia en cuanto a la agilidad de la respuesta de los servicios, costes computacionales, etc.**

Bajo estas premisas nace el servicio OGC WMTS que lo que pretende es mejorar la calidad del servicio haciéndolo más rápido y eficaz. La pregunta es:

Y como mejoramos el servicio WMS? 

Básicamente la mejora que implementa el servicio WMTS respecto a WMS es que las imágenes o teselas que responde de cada petición **ya están generadas en el servidor**, es decir, ya están cacheadas. Esto hace que se reduzca al mínimo los costes computacionales a nivel de servidor y mejore los tiempos de respuesta.
En cuanto a las alternativas para implementar el servicio, los más populares son **[Mapproxy](http://mapproxy.org/)**, **[GeoWebCache](http://geowebcache.org/)**, **[TileCache](http://tilecache.org/)**.


##Principios del servicio WMTS
Es muy recomendable que antes de continuar, se lea la documentación de la especificación del servicio ([aquí](http://www.opengeospatial.org/standards/wmts#downloads)). Como en este post nos vamos a centrar en el consumo del servicio, te recomiendo que leas el capítulo _6 WMTS overview_ y el _anexo E_.

####Fundamento general
El fundamento general en el que se basa el servicio WMTS es que la información del servicio se encuentra previamente generada (cacheada) y la gestión de la información se realiza mediante matrices y conjuntos de matrices que, según el CRS y las características de la matriz , el usuario es capaz de identificar, pedir y obtener la información requerida.

####TileMatrixSet

El TileaMatrixSet es un conjunto de matrices de imágenes (teselas), cada una de ellas con una resolución determinada (zoom o escala), que es la base de la ordenación de las teselas del servicio.

>El servicio WMTS normaliza los dpi de las teselas para que el pixel tenga una dimesión de 0.28 mm

![TileMatrixSet]({{ site.url }}/assets/images/uploads/wmts/tilematrixset.png)

Para cada resolución de dicho TileMatrixSet está definido un TileMatrix.

####TileMatrix
Las TileMatrix son cada una de las matrices de imágenes que forman el TileMatrixSet. Para cada resolución existe una matriz de teselas o TileMatrix, que tiene como orgigen la esquina superior izquierda, y que contiene las correspondientes filas y columnas (_TileCol_ y _TileRow_), que son parámetros necesarios en las peticiones del servicio.

>Por defecto, si no se indica lo contrario, las dimensiones de cada una de las teselas es de 256x256 

![TileMatrix]({{ site.url }}/assets/images/uploads/wmts/tileMatrix.png)

En función de estos componentes, el usuario puede calcular la posición de cualquier telesa en dicha matriz teniendo en cuenta la resolución y la posición sobre eje X e Y respecto a la posición origen de la matriz.

Por ejemplo:

$$pixelSpan = {scaleDenominator * 0.0028\over metersPerUnit(CRS)}$$

$$tileSpan = tileWidth * pixelSpan$$

$$tileSpanY = tileHeight * pixelSpan$$

$$tileMatrixMaxX = tileMatrixMinX + tileSpanX * matrixWidth$$

$${tileMatrixMinY = tileMatrixMaxY - tileSpanY * matrixHeight}$$


###Escalas conocidas (Well-known scale sets)
Dentro de la especificación en el **Anexo E** indica el comportamiento que deben de tener algunos sistemas de referencia globales como el **WGS84** o **GoogleMapsCompatible**.
Para estos sistemas globales la especificación indica cuales tienen que ser los denominadores de escalas para los diferentes niveles de zoom así como las resoluciones para cada una de ellas.

En las imágenes siguientes se puede comprobar como los datos del GetCapbilities del servicio coindicen con la tabla del anexo E de la definición de la especificación.
![GoogleMapsCompatible Scales]({{ site.url }}/assets/images/uploads/wmts/googleMapsCompatible_Escales.png)

![WMTS Capabilities 3857 Scales]({{ site.url }}/assets/images/uploads/wmts/capabilities_3857_TileMatrix.png)

