---
layout: mapPost
disqus: "true"
title:  "Leaflet como visor de imágenes"
date:   2016-03-05
categories: "leaflet"
mathjax: false
mapa: "true"
urlmapa : imagenLeaflet.html
mapTitle : Visualicación de imagen en Leaflet.js 
urlshort: http://bit.ly/24HANvH
---

Aunque no es la finalidad principal de esta librería, el poder visualizar imágenes sueltas, sin ser nada relacionado con mapas, también es posible con [Leaflet.js](http://leafletjs.com/).

Este hecho que parece sin importancia a mi personalmente me parece que es realmente úitil en algunos casos. Yo personalmente estoy muy acostumbrado a usar la rueda del ratón para acercarme y alejarme usando mapas. De esta forma puedo identificar elementos pequeños o bien alejarme y tener una perspectiva más amplia.
Es precisamente por esto por lo que cuando interactúo con los visores de imágenes clásicos en la web, simpre me da la sensación de que me falta algo. El hecho de poder interactuar con la imágen acercándote o alejándote creo que da un aspecto mucho más interactivo y en ciertos casos, puede que sea el elemento distintivo necesario para impulsar una idea.

Se me ocurren algunos casos en los que el uso de este tipo de librerías y tratamiento de las imágenes serían realmente interesantes, como por ejemplo en imágenes de alta resolución o obras de arte en el cuál el usuario podría 'navegar' por el cuadro investigando los más pequeños y precisos detalles.

En cuanto a la parte técnica, esta se vuelve aún más sencilla dado que no aparecen los conceptos de cartografía, de los que tanto se ha hablado en este blog.

Ahora sí, vamos con el código:

```javascript
var map = L.map('map', {
        crs: L.CRS.Simple
    });

    var imageBounds = [[0,0], [500,700]];
    var image = L.imageOverlay('https://unsplash.it/700/500/?random', imageBounds).addTo(map);
    map.fitBounds(imageBounds);
    L.control.mousePosition().addTo(map);
    L.control.centerImage().addTo(map); 

```

Como proveedor de imágenes he usado [unsplash](https://unsplash.it/) que en el caso del ejemplo sirve imágenes aleatorias de 700x500. En cuanto al CRS aplicado sobre el mapa es un CRS simple, es decir, un sistema de coordenadas carteriano X-Y.

EL resultado se puede a continuación:
