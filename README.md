# D3js Workshop
Documentación utilizada para workshops/charlas sobre D3js

## Introducción
[D3js](https://d3js.org/) es una pequeña librería JavaScript para modificar el DOM basándonos en datos (D3 = Data Driven Documents). Es un projecto de código abierto iniciado y dirigido por [Mike Bostock](https://github.com/mbostock) que se ha convertido en el estándar para la confección de gráficos y visualizaciones interactivas en entornos web.

Muchas librería de dibujo de gráficas de alto nivel se basan en D3, y es el rey indiscutible cuando nos adentramos en visualizaciones más complejas que van más allá de los gráficos estándar. D3 es usado por CartoDB, Github, The New York TImes y otros muchos conocidos proyectos.

D3, además de la librería, tiene un pequeño ecosistema de aplicaciones web para facilitar su desarrollo y su difusión. La más usada es [bl.ocks.org](http://bl.ocks.org/), que simplemente es una visualización de archivos de [Gist](https://gist.github.com) (funciona con git). Para más información, el señor Mike Bostock tiene una breve explicación sobre [cómo usar bl.ocks.org](https://bost.ocks.org/mike/block/). 

Mike Bostock tiene la [colección más grande de ejemplos de D3](https://bl.ocks.org/mbostock), con el código justo debajo de cada gráfico.

----------
## Versiones

## Selectores
Los selectores de D3 se parecen mucho a jQuery:

```
//Selector simple
d3.select('h1').style('color', '#BF30B3');

//Selector múltiple
//d3.selectAll('p').attr('class', 'text');
```
Podemos seleccionar de la misma forma que usábamos jQuery, es decir, usando elementos `d3.select('div')`, clases e IDs `d3.selectAll('.container')`, o estructuras más complejas `d3.selectAll('#wrap div > p')`.

Los selectores también pueden anidarse y guardarse:
```
var body = d3.select('body');
body.selectAll('p').style('color', 'red');
```
## Añadir y eliminar elementos
D3 tiene también funciones parecidas a jQuery para estas operaciones:
```
//Añadir elementos
d3.select('.chart').append('ul').append('li').text('hola');

//Eliminar elementos
d3.select('.chart').select('h1').remove();
```
 **Atención:** Hay algunas operaciones, como `append()`, que cambian el selector (en este caso hacia el elemento añadido), por lo que hay que tenerlo en cuenta a la hora de guardar dichos selectores:
```
// Crea un elemento <ul>, añade dos <li>, y cambia el texto de cado <li>
var lista = d3.select('.chart')
    .append('ul');
    
lista.append('li')
    .text('elemento 1');
    
lista.append('li')
    .text('elemento 2');

// Crea un elemento <ul>, añade un <li>, y a este <li> se le añade otro <li>
var lista = d3.select('.chart')
					.append('ul')
					.append('li')
					.text('elemento 1');

lista.append('li').text('elemento 2');
```
## Bindado de datos
La parte interesante de D3 es que podemos hacer las operaciones anteriores basándonos en arrays o matrices de de datos, y hacer que cada elemento de nuestra página dependa de ellos

```
var my_data = [4,15,16,23,42]; 

d3.selectAll("li")
    .data(my_data);
    .style("font-size", function(d) { return d + "px"; });
```

### Enter y Exit
D3js fue concebido para manipular gráficos basándose en actualizaciones de datos (el array anterior). Existen dos selectores especiales, **enter()** y **exit()**, que nos ayudan a hacer dichas actualizaciones.

**enter()** selecciona los elementos de nuestro array de datos que no están bindados a ningún elemento del DOM. **exit()** selecciona los elementos del array de datos que han sido eliminados (y pueden estar bindados en el DOM).

Dicho de otro modo. Empezamos con nuestro simplificado html:
```
<html>
	<body>
		<ul class="list">
		<ul>
	</body>
	<script>
		var data = [4, 8, 15, 16, 23, 42]
	</script>
</htm>
```

Queremos añadir tantos `<li>` como elementos del array `data`, y escribir el número de cada uno en él. Como los elementos no existen en el DOM, utilizamos **enter()**:  

```
var items = d3.select(".list").selectAll("li")
    .data(data);
    .enter().append("li")
    .text(function(d) {
	    return "I’m number " + d ;
	});
```
Ahora nuestros datos cambian, se añade un elemento (12) y se elimina otro (8):
```
var data = [4, 15, 16, 23, 42, 12]
```
Eliminamos elementos:
```
items.exit().remove();
```
Y creamos nuevos:
```
items.enter().append("li")
    .text(function(d) {
	    return "I’m number " + d ;
	});
```
## SVG
Mientras HTML está limitado mayormente a formas rectangulares, SVG soporta muchas más trazados, como curvas Bézier, degradados, recortes y máscaras. Para hacer un gráfico de barras no es necesario conocer todas las funciones que ofrece SVG, pero conocerlo es un plus importante a la hora de diseñar visualizaciones, ya que nos permite realizar muchas más representaciones.

SVG es un lenguaje estandarizado de representación de imágenes vectoriales. Puede crearse tanto a partir de programas de diseño como Illustrator o Photoshop o mediante técnicas de programación. Son elementos que se interpretan por los navegadores (pueden inspeccionarse y manipularse!), pueden ser estilizados mediante CSS y pueden modificarse con javascript.

La [tabla de especificaciones](https://www.w3.org/TR/SVG/) parece un poco engorrosa, pero para la mayoría de formas son sencillas.

```
var data = [4, 8, 15, 16, 23, 42];
 
var width = 420,
	barHeight = 20;
    height = 600;
 
var chart = d3.select("body")
  .append('svg')
    .attr("width", width)
    .attr("height", height);
 
var rects = chart.selectAll("rect")
    .data(data)
  .enter().append("rect")
	.attr("x", 0)
    .attr("y", function(d, i) {
	    return i * barHeight;
    })
    .attr("width", function(d) {
	    return d;
    })
    .attr("height", barHeight - 1);
```

## Carga de datos externos
Podemos parsear datos desde un archivo. Se aceptan diferentes formatos, entre ellos json, txt, csv, tsv, xml.

D3 se descarga los arhivos **asíncronamente**, por lo que hay no vale con poner el código en cualquier lado:
```
// 1º. El código de aquí se ejecuta primero, antes de que empiece la descarga.

d3.tsv("http://www.whatever.com/data.tsv", function (error, data) {

	// 3º. Este código se ejecuta al final, cuando la descarga ha terminado.

});

// 2º. Este código se ejecuta el segundo, mientras se descarga el archivo.
```

## Interacciones con el DOM
Tenemos onclick, mouseover, mouseout y otros cuantos, y los bindamos tan ricamente:
```
d3.select("p")
 .on("click", function() {
	alert('Que pasa bro");
 });
```

## Transiciones
Esto es tan sorprendentemente fácil que seguro que tiene alguna maldición encerrada:

```
d3.selectAll("p")
 .data(data)
 .transition()
 .style("font-size", function(d) { return d * 10 + "px"; });
```

Y podemos controlar la duración, mi wey:
```
.transition()
.duration(1000)
```
Y también los tipos de transiciones, a saber:
```
.transition()
.duration(1000)
.ease("elastic")
```
Este código deberemos situarlo debajo de .duration(), siendo los tipos principales son los siguientes:

 - cubic-in-out – cúbica, empieza lento, acelera, y frena
 - circle – exponencial, empieza lento y acaba a toda leche
 - elastic – elástica, con sus rebotes y todo
 - bounce – con un bote al final, cual pelota

Y también hay delays, no nos olvidemos:
```
.transition()
.delay( function(d,i){ return i / data.length * 1000; })
.duration(1000)
```

## Siguientes pasos
 - [Convención de margenes de dibujo](http://bl.ocks.org/mbostock/3019563)
 - [Convención para el update](http://bl.ocks.org/mbostock/3808218)
 - [Gráfico de fiebre](http://bl.ocks.org/mbostock/3883245)
 - [Gráfico simple de tarta](http://bl.ocks.org/mbostock/3887235)
 - [Reusabilidad del código](http://www.saigesp.es/d3js-graficos-reusables/)
 - [Gráficos de barras reusable](http://www.saigesp.es/d3js-graficos-de-barras/)
 - [Mapas con google maps](http://www.saigesp.es/d3js-mapas-con-google-maps/)
 - [D3 como directiva de Angular](http://www.saigesp.es/d3js-y-angularjs-primera-directiva/)