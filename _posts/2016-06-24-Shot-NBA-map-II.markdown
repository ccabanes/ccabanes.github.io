---
layout: post
disqus: "true"
title:  "Manejando colecciones de datos con lodash.js"
date:   2016-06-24 01:30:00
categories: "map Leaflet lodash dataviz nba"
mathjax: false
mapa: "true"
urlmapa :  
mapTitle : 
urlshort: "http://bit.ly/28SzwZY"
---

Este es el segundo post relacionado con *Dataviz* de datos JSON y representación sobre mapas. Si quieres puedes ver el primero pincha [aquí.](http://ccabanes.github.io/map/leaflet/lodash/dataviz/nba/2016/05/20/Shot-NBA-map/)

El concepto *Dataviz* o visualización de datos no solo conlleva la propia visualización de los mismos, si no el manejarlos, agruparlos, y componerlos de diferentes formas para poder mostrar toda la información que nos aportan.
Para este post, la manipulación y gestión de los datos se ha hecho usando la librería **[lodash.js](https://lodash.com/)**. 

Esta librería nos ofrece una capacidad muy grande a la hora de trabajar con arrays y colecciones, además de ofrecer una serie de funciones adicionales relamente útiles que, en muchos casos, acabamos implementando sucesivamente en liberías de utilidades en diferentes proyectos.

Continuando con el contexto del primer post este segundo pretende que el usuario, de forma fácil, pueda manipular los datos y establecer los criterios de visualización que se desee. Para ello se ha planteado que se carguen en un primer ```select``` los tipos de datos a mostrar y que, de forma dinámica, se cree un segundo ```select``` relacionado con este que contenga los diferentes criteros de representación. Finalmente, la visualización del criterio será al igual que en el primer post, según si el lanzamiento está convertido (verde) o fallado (rojo).

### Filtrando datos con lodash.js
El punto de partida es la respuesta del servicio sobre las estadísticas de tiro de la temporada de Stephen Curry.

El primer ```select``` se compone a partir de los ```header``` de las columnas que contiene la respuesta del servicio. Existen muchas posibilidades de realizar este proceso que no necesariemente implique usar lodash, pero en este caso se ha utilizado la función  **[\_.forEach](https://lodash.com/docs#forEach)** para realizar la iteración y **[\_.sortBy](https://lodash.com/docs#sortBy)** usada para ordenar los diferentes elementos del array.

```java
_.forEach(_.sortBy(map), function(value, key) {
        mapOptions.shots.criteriaOptions.push(value);
         if(value == defaultSelection || object.find("option").length == 0 ){
            object.append('<option value="'+key+'" selected="selected">'+value+'</option>');
            mapOptions.map.selectedValue = value;
         }else{
            object.append('<option value="'+key+'">'+value+'</option>');
         }
     });

```

La composición de los criterios de búsqueda para el segundo ```select``` es un poco más laboriosa. Rellenar este ```select``` implica conocer en primer lugar cuales son las diferentes posibilidades existentes dentro de los más de 1500 registros de la respuesta del servicio de estadísticas. Una opción clásica sería iterar e ir comparando y almacenando los distintos valores para el parámetro seleccionado. Sin embargo, en este caso, mediante tres funciones fáciles de usar es posible sacar los elemetos distintos para cada propiedad: 
**[\_.indexOf](https://lodash.com/docs#indexOf)**, **[\_.map](https://lodash.com/docs#map)** y **[\_.uniq](https://lodash.com/docs#uniq)**.

```java
var location =_.indexOf(mapOptions.shots.headerShots, mapOptions.shots.criteria);
var newMapValues = _.map(mapOptions.shots.shotsData.fulldata, location);
var uniqueValues = _.uniq(newMapValues);
```

El cambio de criterio seleccionado provoca que los datos representados cambien. Para obtener los "candidatos" se ha utilizado la función **[\_.filter](https://lodash.com/docs#filter)**. Con esta función se consigue obtener como resultado otra colección que cumplan los criterios pasados por parámetro. Para este ejemplo será que cumplan que el nombre de la propiedad sea el valor del primer ```select```   y el valor sea igual a la seleccionada por en el segundo.

```java
jQuery("#"+id).on("change", function(){
    var index = _.findIndex(mapOptions.shots.criteriaOptions, function(o) {
        var selectedOption  = jQuery("#shotCriteria").find("option:selected").text()
        if(o == selectedOption ){
            return true;
        }              
    });
    var candidates = _.filter(mapOptions.shots.shotsData.fulldata, [mapOptions.shots.criteriaIndex, mapOptions.shots.criteriaOptions[index]]);
    mapOptions.shots.shotsData.criteria = candidates;
    printShots();
});
```

###Visualizando lanzamientos con leaflet.js
Finalmente el método empleado para visualizar estos datos es, como no, un mapa. La metodología para realizar el pintado es la misma que en el primer blog, así que no voy a entrar más en detalle en este tema. El único cambio ha sido la gestión de las capas de forma dinámica. Es este caso, antes de volver a representar la nueva condición, siempre se evalúa si existe una capa anterior que es borrada e instanciada de nuevo para almacenar los nuevos datos.

```java
function cleanLayers(){
    //Clean layer values from last criteria selection
    if(mapOptions.map.hasLayer(mapOptions.layers.shots)){
        mapOptions.map.removeLayer(mapOptions.layers.shots);
    }
    var layer = L.featureGroup();
    mapOptions.layers.shots = layer;
    mapOptions.map.addLayer(layer);
}
```

En este enlace dejo la **[DEMO](http://ccabanes.github.io/map-demos/nba/nba_lodash.html)** para ver el resultado final y el [repositorio.](https://github.com/ccabanes/map-demos/tree/master/nba)

###Conclusiones
Esta ha sido mi primera experiencia con el uso de esta librería. Personalmente creo que abarca un área en el que no habían prácticamente soluciones disponibles y que muchos desarrolladores echávamos en falta a la hora de procesar colecciones de datos en javascript. Sobre todo cuando este proceso lo ejecuta el navegador.

Es impresionante la rapidez con la que ejecuta los procesos. Hay que decir que los datos de la demo tienen más 20 columnas por registro con 1598 registros totales y los mueve con absoluta tranquilidad.

Sin ninguna duda es más que recomendable su uso, sobre todo en el caso de volumenes grandes de datos.
