---
layout: post
disqus: "true"
title:  "10 funciones Postgis que debes conocer"
date:   2016-01-13 20:36:00
categories: "Cartography, postgis, bbdd"
urlshort: 
mathjax: false
---
El uso de un sistema de gestión de base de datos con soporte geospacial es fundamental para cualquier entorno GIS tanto a nivel de desarrollo de aplicaciones como a nivel de técnico. Este post tiene como propósito recopilar las funciones que bajo mi criterio son más utilizadas y por lo tanto, es altamente recomendable que cualquier gestor de información geográfica deba saber.

Si por contra no sabes que es esto de Postgis entra [aquí](http://postgis.net) y échale un vistazo antes de seguir leyendo.

Aquí dejo el enlace al manual completo de Postgis 2.2 en [PDF](http://postgis.net/stuff/postgis-2.2.pdf) y un enlace a un [curso] (http://workshops.boundlessgeo.com/postgis-intro/) de inciación a Postgis.

####St_AsText - St_AsEWKT 
Ambas funciones obtienen la geometría en formato WKT. La diferencia es que la segunda incluye el SRID de la misma.

```java
text ST_AsText ( geometry g1)
text ST_AsEWKT ( geometry g1)
```

```sql
select St_AsText(the_geom) from  myTable;
select St_AsTEWKT(the_geom) from  myTable;
```

####St_GeomFromText - St_GeomFromEWKT
En este caso estas funciones generan una geometría en función a las cadenas en formato WKT o EWKT respectivamente.

```java
geometry ST_GeomFromText(text WKT);
geometry ST_GeomFromText(text WKT, integer srid);
geometry ST_GeomFromEWKT(text EWKT);
```

```sql
select ST_GeomFromText('POINT (0 0)') from  myTable;
select ST_GeomFromText('POINT (0 0)', 4326) from  myTable;
select ST_GeomFromEWKT('SRID=4326;POINT(0 0)') from  myTable;
```

####St_Buffer
Una de las funciones más utilizadas en los sistemas de información geográfica. Esta función genera un polígono alrededor de la geometría pasada por parámetro. Dicho polígono puede definirse según los parámetros adicionales de la función

```java
geometry ST_Buffer(geometry g1, float radio buffer);
geometry ST_Buffer(geometry g1, float radio buffer, integer num_seg_quarter);
geometry ST_Buffer(geometry g1, float radio buffer, text params);
geography ST_Buffer(geography g1, float radio buffer);
```

```sql
SELECT ST_Buffer( ST_GeomFromText('POINT(0 0)'), 100, 'quad_segs=8');
SELECT ST_Buffer( ST_GeomFromText('LINESTRING(50 50,150 150,150 50)' ), 10, 'endcap=round join=round');
SELECT ST_Buffer( ST_GeomFromText('LINESTRING(50 50,150 150,150 50)' ), 10, 'join=bevel');
```

####St_Distance
Calcula la distancia existente entre las dos geometrías pasadas por parámetro

```java
float ST_Distance(geometry g1, geometry g2);
float ST_Distance(geography gg1, geography gg2, boolean use_spheroid);
```

```sql
SELECT ST_Distance(ST_GeomFromText('POINT(0 -1)',4326),ST_GeomFromText('LINESTRING(0 -3, -2 0)', 4326));
```

####St_Intersects
Calcula si dos geometrías intersectan espacialmente. Devuelve 'TRUE' si intersecta y 'FALSE' en caso contrario.

```java
boolean ST_Intersects( geometry geomA , geometry geomB );
```

```sql
SELECT ST_Intersects('POINT(0 0)', 'LINESTRING ( 2 0, 0 2 )');
```

####St_Union
Si estás familiarizado con un GIS de escritorio lo estarás con la función 'merge'. St_Union realiza esta operación de unión de objetos espaciales.

```java
geometry ST_Union(geometry set g1field);
geometry ST_Union(geometry g1, geometry g2);
geometry ST_Union(geometry[] g1_array)
```


####St_ClosestPoint
Una de mis favoritas. Existen multitud de situaciones en las que se necesita 'proyectar' un punto sobre una línea o algo similar. Esta función obtiene el punto más cercano en la geometría g2 a la geometría g1.

-- codigo de demo


####St_Transform
Función que transforma entre datos entre diferentes sistemas de referencia. 
-- codigo de demo

####St_Interpolate

-- codigo de demo