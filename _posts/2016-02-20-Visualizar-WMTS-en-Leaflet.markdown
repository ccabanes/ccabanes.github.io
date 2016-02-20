---
layout: mapPost
disqus: "true"
title:  "Visualizar WMTS en Leaflet"
date:   2016-02-20 09:33:00
categories: "WMTS webservices webmapping GIS leaflet OGC"
mathjax: false
mapa: "true"
urlmapa : cantabria.html
mapTitle : Demo WMTS Cantabria
urlshort: 
---

Este es un segundo post sobre los servicios WMTS. Si quieres saber que se dijo en el primero, [aquí](http://ccabanes.github.io/wmts/webservices/webmapping/gis/ogc/2016/02/12/Servicios-WMTS/) tienes el enlace.

La librería javascript que se va a usar para visualizar este tipo de servicio es [Leaflet.js](http://leafletjs.com). Esta librería es bastante reciente comparada con otras más clásicas como [OpenLayers](http://openlayers.org) y aunque tengan el mismo objetivo que es pintar mapas y datos sobre estos, lo hacen mediante conceptos bastante distintos.

Si se está familiarizado con las librerías javascript para visualización de mapas en la web, sabrás que incluir un servicio de mapas base como WMTS es relativametne sencillo. Sin embargo, para cartogramadores inexpertos el consumo de servicios sobre CRS locales pueden dar algún quebradero de cabeza ya que es necesario tener bien afianzados ciertos concetos.

Lo primero que se debe de conocer al usar leaflet.js es que por si solo no tiene soporte para manejar sistemas de proyecciones (exceptuando lso CRS 3857 y 4326). Para poder incluir este soporte es necesario apoyarse en el plugin [Proj4Leaflet](http://kartena.github.io/Proj4Leaflet/). A grandes rasgos este plugin extiende la funcionalidad básica de los CRS de leaflet.js y se apoya en proj4.js para implementar el soporte para proyecciones.

>"Proj4Leaflet makes it possible to use projections and CRS not built into Leaflet. All major projections are supported by using the popular and well-tested Proj4js library."

Antes de empezar, es importante recalcar que a diferencia de los servicios WMTS publicados sobre los CRS: 4326 y 3857, no existe ninguna normativa en cuanto a las resoluciones y denominadores de escala para los diferentes niveles de zoom para el resto de sistemas. Esto implica que es necesario modificar el comportamiento habitual de la implementación para adaptarla a las particularidades de cada capa.

####Ejemplo de servicio WMTS
Voy a usar los servicios cántabros de ortofotos para realizar un pequeño ejemplo. Si quieres probar con cualquier otro, en la idee tienen una lista con los [servicios públicos WMTS](http://idee.es/web/guest/directorio-de-servicios?p_p_id=DIRSRVIDEE_WAR_DIRSRVIDEEportlet_INSTANCE_q4BW&p_p_lifecycle=1&p_p_state=normal&p_p_mode=view&p_p_col_id=column-1&p_p_col_count=1&_DIRSRVIDEE_WAR_DIRSRVIDEEportlet_INSTANCE_q4BW_descSrv=VISUALIZACION&_DIRSRVIDEE_WAR_DIRSRVIDEEportlet_INSTANCE_q4BW_supertipo=OGC&_DIRSRVIDEE_WAR_DIRSRVIDEEportlet_INSTANCE_q4BW_tipoServicio=WMTS). 

**Propiedades de la capa de ejemplo:**

* __URL del servicio__: http://geoservicios.cantabria.es/inspire/rest/services/Ortofoto_2014/MapServer/WMTS?
* __Capabilities__ : [link](http://geoservicios.cantabria.es/inspire/rest/services/Ortofoto_2014/MapServer/WMTS?service=WMTS&request=getCapabilities)
* __Layer__: Ortofoto_2014
* __CRS__: EPSG:25830

Este servicio a diferencia de otros de este tipo, solo tiene un sistema de referencia sobre el que se sirve la capa *Ortofoto_2014*. Es habitual que para un mismo servicio WMTS existan diferentes CRS sobre los que consumir la información.

Los parámetros más importantes a tener en cuenta para el consumo de este tipo de servicios son el  *ScaleDenominator* y *TopLeftCorner*. 
El parámetro *ScaleDenominator* permite calcular la resolución de cada nivel de zoom ([ver explicación en post anterior](http://bit.ly/1Qy0Zib)) y el parámetro *TopLeftCorner* establece el punto inicial sobre el que contar el desplazamiento en los ejes X,Y para obtener las posiciones de los tiles y montar así la petición.

Ahora sí, vamos con el código:

```javascript

var cantabria_resol = [541.8677504021,270.9338752011,203.2004064008,135.4669376005,101.6002032004,67.7334688003,33.8667344001,16.9333672001,8.4666836,4.2333418,2.1166709,1.05833545,0.529167725,0.2645838625,0.1322919313];
var ETRS89_Cantabria = new L.Proj.CRS('EPSG:25830',
'+proj=utm +zone=30 +ellps=GRS80 +units=m +no_defs',
{
resolutions: cantabria_resol,
origin: [-5120900.0 , 9998100.0]
});
var mapCantabria = L.map('mapCantabria', {
        crs: ETRS89_Cantabria,
        continuousWorld: true,
        worldCopyJump: false, 
        scale: cantabria_scale
    }).setView([43.421936, -3.781941], 7);

var can_orto_Url_WMTS = 'http://geoservicios.cantabria.es/inspire/rest/services/Ortofoto_2014/MapServer/WMTS?service=WMTS&request=GetTile&version=1.0.0&layer=Ortofoto_2014&style=default&tilematrixset=default028mm&TileMatrix={z}&TileRow={y}&TileCol={x}',
attrib = 'Ortofoto 2014 OGC WMTS',
Orto2014_Layer = new L.TileLayer(can_orto_Url_WMTS, {
    scheme: 'xyz',
    maxZoom: cantabria_resol.length - 1,
    minZoom: 0,
    opacity: 0.5,
    continuousWorld: true,
    attribution: attrib
}); 
var can_base_Url_WMTS = 'http://geoservicios.cantabria.es/inspire/rest/services/Cartografia_Basica_Topografica/MapServer/WMTS?service=WMTS&request=GetTile&version=1.0.0&layer=Cartografia_Basica_Topografica&style=default&tilematrixset=default028mm&TileMatrix={z}&TileRow={y}&TileCol={x}',
attrib = 'Ortofoto 2014 OGC WMTS',
Base_Layer = new L.TileLayer(can_base_Url_WMTS, {
    scheme: 'xyz',
    maxZoom: cantabria_resol.length - 1,
    minZoom: 0,
    opacity: 0.5,
    continuousWorld: true,
    attribution: attrib
});

var carto_base ={
    "Ortofoto 2014": Orto2014_Layer ,   
    "Cartografía Basica" : Base_Layer
    
};


mapCantabria.addLayer(Orto2014_Layer);
mapCantabria.addLayer(Base_Layer);
L.control.layers(carto_base).addTo(mapCantabria);
L.control.mousePosition().addTo(mapCantabria);
L.control.scale({'position':'bottomleft','metric':true,'imperial':false}).addTo(mapCantabria);

```

