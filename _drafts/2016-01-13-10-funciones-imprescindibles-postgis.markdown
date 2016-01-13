---
layout: post
disqus: "true"
title:  "10 funciones Postgis que debes conocer"
date:   2016-01-13 20:36:00
categories: Cartography, postgis, bbdd
urlshort: 
mathjax: false
---
El uso de un sistema de gestión de base de datos con soporte geospacial es fundamental para cualquier entorno GIS tanto a nivel de desarrollo de aplicaciones como a nivel de técnico. Este post tiene como propósito recopilar las funciones que bajo mi criterio son más utilizadas y por lo tanto, es altamente recomendable que cualquier gestor de información geográfica deba saber.

Si por contra no sabes que es esto de Postgis entra (aquí)[http://postgis.net] y échale un vistazo antes de seguir leyendo.

####St_AsText
Esta función devuelve de en formato WKT la geometría que se pasa por parámetro.
-- codigo de demo

####St_AsEWKT
Similar a la función St_AsText pero esta incluye también el SRID de la geometría pasada por parámetro.
-- codigo de demo

####St_Buffer
Una de las funciones más utilizadas en los sistemas de información geográfica. Esta función genera un polígono alrededor de la geometría pasada por parámetro. Dicho polígono puede definirse según los parámetros adicionales de la función
-- codigo de demo

####St_Distance
Calcula la distancia existente entre las dos geometrías pasadas por parámetro
-- codigo de demo


####ST_Intersects

-- codigo de demo


####ST_Union
Si estás familiarizado con un GIS de escritorio lo estarás con la función 'merge'. St_Union realiza esta operación de unión de objetos espaciales.
-- codigo de demo

####St_ClosestPoint
Una de mis favoritas. Existen multitud de situaciones en las que se necesita 'proyectar' un punto sobre una línea o algo similar. Esta función obtiene el punto más cercano en la geometría g2 a la geometría g1.

-- codigo de demo


####St_Transform
Función que transforma entre datos entre diferentes sistemas de referencia. 
-- codigo de demo

####St_Interpolate

-- codigo de demo