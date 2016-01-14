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

Aquí dejo el enlace al manual completo de Postgis 2.2 en (PDF)[http://postgis.net/stuff/postgis-2.2.pdf] y un enlace a un (curso de inciación a Postgis)[http://workshops.boundlessgeo.com/postgis-intro/]

####St_AsText - St_AsEWKT 
Ambas funciones obtienen la geometría en formato WKT. La diferencia es que la segunda incluye el SRID de la misma.
######Sintaxis
text *ST_AsText* ( *geometry* g1);
text *ST_AsEWKT* ( *geometry* g1);
```sql
select St_AsText(the_geom) from  myTable;
select St_AsTEWKT(the_geom) from  myTable;
```

####ST_GeomFromText - ST_GeomFromEWKT
En este caso estas funciones generan una geometría en función a las cadenas en formato WKT o EWKT respectivamente.
######Sintaxis
geometry *ST_GeomFromText*(text WKT);
geometry *ST_GeomFromText*(text WKT, integer srid);
geometry *ST_GeomFromEWKT*(text EWKT);
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