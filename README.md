# D3js Workshop
Pequeño workshop desarrollado para formación interna de [Quantion](www.quantion.com)

## Introducción
[D3](https://d3js.org/) es una pequeña librería de JavaScript para modificar el DOM basándonos en datos (D3 = Data Driven Documents). Actualmente es una especie de estándar web para la confección de gráficos y visualizaciones interactivas, ya que muchas librería de dibujo de gráficas de alto nivel se basan en D3.

D3, además de la librería, tiene un pequeño ecosistema de aplicaciones web para facilitar su desarrollo y su difusión. La más usada es [bl.ocks.org](http://bl.ocks.org/), que simplemente es una visualización de archivos de [Gist](https://gist.github.com) (si, de github, luego funciona con git). Si queréis más info, el señor Mike Bostock tiene una breve explicación sobre [cómo usar bl.ocks.org](https://bost.ocks.org/mike/block/). 

Este señor tiene la [colección más grande de ejemplos de D3](https://bl.ocks.org/mbostock), que como podéis ver tienen el código muy accesible

----------
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
##Añadir y eliminar elementos
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
