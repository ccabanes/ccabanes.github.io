---
layout: post
disqus: "true"
title:  "Datos y gráficos dinámicos on D3.js"
date:   2016-08-30 19:30:00 -0000
categories: "map Leaflet lodash dataviz nba d3.js"
mathjax: false
mapa: "true"
urlmapa :  
mapTitle :
urlshort:
---

Este es el tercer y último post relacionado con *Dataviz* usando la fuente de datos de NBA stats. Las entradas anteriores las puedes ver [aquí.](http://ccabanes.github.io/map/leaflet/lodash/dataviz/nba/2016/05/20/Shot-NBA-map/)

Con este post doy por cerrado el círculo de la visualizacion e interpretación de datos incluyendo la visualiación mediante gráficos.
Para ello he utilizado una de las librerías más potentes para ello: [d3.js](htttp://d3js.org).

No voy a hablar de la potencia y posibilidades que ofrece esta librería. Creo que a día de hoy todo el mundo al menos a oido hablar y seguramente ha visto e interactuado con algún gráfico relizado por esta librería.
De una forma muy rápida, lo que *d3.js* ofrece al desarrollador es la capacidad de manejar e insertar datos en el DOM y crear SVG para visualizar y tranformar estos datos.

Desde mi punto de vista, la fusión de estas metodologías de visualización de datos, **[mapas-gráficos-valores]** es la forma más completa en la que se pueden expresa los mismos y, por consiguiente, la mejor forma para ser capaz de tomar decisiones estratégicas.

D3 ofrece una gran variadad de tipos de representación gráficos. Es importante es este caso saber elegir qué representación es la más adecuada para ofrecer la información de la manera más legible posible, al igual que sucede en las representaciones cartográficas.

Para el caso que nos ocupa, se ha elegido un gráfico circular puesto que esta representación nos muestra de forma clara lo que queremos mostrar, que son los **aciertos vs errores** en los lanzamientos a canasta.

#### Inicializando gráficos en D3.js
Puesto que el proyecto ya consta de funcionalidad y el procesado de datos ya está realizado, tan solo falta inicializar y pasarle los datos para ser representados en los gráficos. La inicizaliación se ha generado partiendo de un  **tag** ```svg``` en el fichero ```html``` y añadiendo de forma dinámica los siguientes elementos:

{% highlight Javascript %}
function initializePie(){
var svg = d3.select("#d3")
    .append("svg").attr("class", "d3")
    .append("g")

svg.append("g").attr("class", "slices")
    .append("text").attr("class", "percent")

d3.select(".slices").append("text").attr("class", "totals");

svg.append("g").attr("class", "labels");
svg.append("g").attr("class", "lines");

mapOptions.graphics.svg = svg;

var pie = d3.layout.pie()
    .sort(null)
    .value(function(d) {
        return d.value;
    });
{% endhighlight %}

La asignación de los datos se realiza una vez son procesados y según los criterios seleccionables. La función ``` redrawPie ``` es la función principal que carga los datos de ```mapOptions.graphics.data```, datos filtrados según criterios y lanza los procesos de creación y cambio del gráfico:

{% highlight Javascript%}

function redrawPie(){
    loadNewCharData(getData());
}

function getData(){
    var labels = mapOptions.graphics.color.domain();    
    var mapData = labels.map(function(label){
		return { label: label, value: mapOptions.graphics.data[label] }
	});
    return mapData;
}

function loadNewCharData(data){
  if(data){
	var slice = mapOptions.graphics.svg.select(".slices").selectAll("path.slice")
		.data(mapOptions.graphics.pie(data), mapOptions.graphics.key);

	slice.enter().insert("path").style("fill", function(d) {
            return mapOptions.graphics.color(d.data.label);
            }).attr("class", "slice");

	slice.transition().duration(1000).attrTween("d", function(d){
			this._current = this._current || d;
			var interpolate = d3.interpolate(this._current, d);
			this._current = interpolate(0);
			return function(t) {
				return mapOptions.graphics.arc(interpolate(t));
			};
		})

	slice.exit().remove();

  var percentage = (100 * data[0].value / mapOptions.shots.shotsData.criteria.length).toPrecision(3);
  var percentageString = percentage + "%";
  if (percentage < 0.1) {
      percentageString = "< 0.1%";
  }
  var totals = data[0].value + " / " + mapOptions.shots.shotsData.criteria.length;

  d3.select("text.percent").attr("dy", ".15em").attr("dx", "-1.30em").text(percentageString);   
  d3.select("text.totals").attr("dy", "1.95em").attr("dx", "-2.30em").text(function(d) {                        
			return totals;
		});  
  }

{% endhighlight %}

Además generar el gráfico, el proceso genera los textos y las líneas con las indicaciones de cada uno de los valores.

En este enlace dejo la **[DEMO](http://ccabanes.github.io/map-demos/nba/nba_d3.html)** para ver el resultado final y el [repositorio.](https://github.com/ccabanes/map-demos/tree/master/nba)

### Conclusiones
Como ya se ha comentado durante los diferentes post de visualización de datos, mi opición personal es que es impresindible la inclusión de mapas junto con los gráficos y viceversa. Hasta ahora estos dos mundos estaban bien separados, centrándose cada uno en sí mismos, y siendo desarrollados por profesionales con rasgos muy marcados. Hoy en día esto ha cambiado y un buen profesional que maneje datos debe de ser capaz de usar estas tecnologías y procedimientos para en análisis y visualización de los mismos.
