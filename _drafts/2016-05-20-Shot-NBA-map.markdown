---
layout: post
disqus: "true"
title:  "Datos NBA en un mapa web. Lanzamientos de Stephen Curry"
date:   2016-05-20 11:00:00 -0000
categories: "map Leaflet lodash dataviz nba"
mathjax: false
mapa: "true"
urlmapa : 
mapTitle : 
urlshort: 
---

Coincidiendo con los playoffs 2016, y recordando algún post leído hace unos meses, se me ocurrió la idea de intentar plasmar mediante un mapa en [LeafletJS](http://leafletjs.com/) información a cerca de los partidos o jugadores de la NBA.

Es ampliamente conocido que la NBA es una de las competiciones deportivas que más y mejor datos generan y manejan. Un buen ejemplo son lo que llaman [estadísticas avanzadas](http://stats.nba.com/#!?StatType=Advanced) , que no es más que la aplicación de Bisness Intelligence a los datos producidos por los jugadores y equipos. Con esto se genera información como:

  - **Offensive Rating** *(Ratio ofensivo)*: Número de puntos anotados por cada 100 posesiones por un equipo/jugador. 
  - **Deffensive Rating** *(Ratio defensivo)*: Número de puntos recibidos por cada 100 posesiones de un equipo/jugador.
  - **Player Impact Estimated** *(Impacto de un jugador)*: Refleja la participación que tiene un jugador en las acciones de su equipo, medido en %.

Con el estudio de estos datos se pueden sacar conclusiones que, a veces, no son sencillas de medir dentro de una cancha. Este es el motivo por el cual es cada vez más frecuente el lanzamiento de 3 puntos ya que con los porcentages de tiro actual sale más rentable que lanzar de 2 puntos, sobre todo cuando no se está cerca de la canasta.

### Dataviz: Lanzamientos temporada 2015-2016 Stephen Curry
"Hazlo fácil. Usa un mapa" es el título de mi blog. Creo que para este caso un mapa es la forma más fácil de representar y entender lo que sucede en una cancha. 

En este post se va a plasmar en un mapa los lanzamientos hechos por Stephen Curry durante la temporada regular 2015-2016.

Para ello es necesario saber como pedir la información. A pesar de que la NBA no tiene un API pública de acceso de datos, hoy en día los frameworks JS hacen que cada vez más nos saltemos nuestro servidor de aplicaciones para atacar directametne al proveedor de datos.

Depurando un poco las peticiones que se hacen desde la página [stats.nba.com](http://stats.nba.com/), se puede sacar de forma fácil qué peticiones se están ejecutando para obtener ciertos datos.

Para implementar este ejemplo y poder obtener los lanzamientos, la petición será:

```java
http://stats.nba.com/stats/shotchartdetail?CFID=33&CFPARAMS=2015-16&ContextFilter=&ContextMeasure=FGA&DateFrom=&DateTo=&GameID=&GameSegment=&LastNGames=0&LeagueID=00&Location=&MeasureType=Base&Month=0&OpponentTeamID=0&Outcome=&PaceAdjust=N&PerMode=PerGame&Period=0&PlayerID=201939&PlusMinus=N&Position=&Rank=N&RookieYear=&Season=2015-16&SeasonSegment=&SeasonType=Regular%20Season&TeamID=0&VsConference=&VsDivision=&mode=Advanced&showDetails=0&showShots=1&showZones=0
```

Como no la respuesta no varía (ya ha pasado la temporada regular), me he guardado en un fichero la respuesta para así dejar tranquilos los servidores de la nba ;). Esta es la respuesta del servicio: [CurryData](https://github.com/ccabanes/ccabanes.github.io/blob/master/assets/data/curry.js) en formato JSON.

Ahora solo falta procesar los datos y extrar la información que se quiera para pintarla en un mapa. 
En este ejemplo, se van a visualizar simplemente los lanzamientos y su criterio de pintado será:

* Convertido: **verde**
* Fallado: **rojo**

Para poder establecer los criterios, primero se deben de recorrer los datos y establecer la comparación de la propiedad que se está estudiando. En este caso, la respuesta del servicio tiene la siguiente estructura:

```java
parameters: [Array<requestParams>]
resource: "shotchartdetail"
resultSets: [
  { 
    {
      headers:[Array<DataHeader>] ,
      rowSet:[Array], 
      name:"Shot_Chart_Detail"
    }
   }
]
```

Estos son las cabeceras de los datos:

```java
"GRID_TYPE"
"GAME_ID"
"GAME_EVENT_ID"
"PLAYER_ID"
"PLAYER_NAME"
"TEAM_ID"
"TEAM_NAME"
"PERIOD"
"MINUTES_REMAINING"
"SECONDS_REMAINING"
"EVENT_TYPE"
"ACTION_TYPE"
"SHOT_TYPE"
"SHOT_ZONE_BASIC"
"SHOT_ZONE_AREA"
"SHOT_ZONE_RANGE"
"SHOT_DISTANCE"
"LOC_X"
"LOC_Y"
"SHOT_ATTEMPTED_FLAG"
"SHOT_MADE_FLAG"
```

Para este ejemplo sencillo bastará con evaluar el campo "SHOT_MADE_FLAG" para saber si el lanzamiento ha sido convertido, y los campos "LOC_X" y "LOC_Y" para mostrar desde dónde se ha efectuado el mismo.

Llegado a este punto y en vista del volumen de datos (en torno a los 1600 elementos) decidí probar una librería para optimizar al máximo el manejo de colecciones de datos en JS llamada [lodash](https://lodash.com/). 

Con **lodash** se puede incluir iteraciones e índices como en el siguiente ejemplo, donde se procesan todos los datos y se obtiene su localización para pintarlo en el mapa:

```java
   _.forEach(mapOptions.shots.shotsData, function(value, key) {
        if(mapOptions.shots.location.x == null || mapOptions.shots.location.y == null){
            var x =_.indexOf(mapOptions.shots.headerShots, "LOC_X");
            var y =_.indexOf(mapOptions.shots.headerShots, "LOC_Y");
            if(x >=0 && y >= 0){
                mapOptions.shots.location.x = x;
                mapOptions.shots.location.y = y;    
            }else{
                return;
            }
            
        }
            locateShotOnMap(value);          
        });   
```


#### Y que CRS se aplica al mapa?
Esta es una pregunta fundamental. Como definir el sistema de referencia para el mapa? En este caso, puesto que los datos vienen en un sistema cartesiano normal y corriente (eje X, Y), se ha definido un **CRS Simple** con el centro en **(0,0)** _([Otro ejemplo de CRS Simple aquí](http://ccabanes.github.io/leaflet/2016/03/05/Visor-de-imagenes-con-leaflet/))_


Aquí hay una **[DEMO](http://ccabanes.github.io/map-demos/nba/nba.html)** para ver el resultado final.