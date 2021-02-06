# D3 Workshop

Documentación utilizada para workshops/charlas sobre D3js

## Introducción
[D3](https://d3js.org/) es una librería JavaScript para generar visualizaciones en base a datos (D3: Data Driven Documents). Es un projecto de código abierto iniciado y dirigido por [Mike Bostock](https://twitter.com/mbostock) que se ha convertido en el estándar de facto para la confección de gráficos y visualizaciones interactivas en entornos web.

Muchas librerías "simples" para hacer gráficos se basan en D3, y es el rey indiscutible cuando nos adentramos en visualizaciones más complejas que van más allá de los gráficos estándar. D3 es usado por CartoDB, Github, The New York TImes y otros muchos conocidos proyectos.

D3, además de la librería, tiene un pequeño ecosistema de aplicaciones web para facilitar su desarrollo y su difusión. Una de las más usadas es [bl.ocks.org](http://bl.ocks.org/), que simplemente es una visualización de archivos de [Gist](https://gist.github.com) (funciona con git). Para más información, el señor Mike Bostock tiene una breve explicación sobre [cómo usar bl.ocks.org](https://bost.ocks.org/mike/block/).

Mike Bostock tiene la [colección más grande de ejemplos de D3](https://bl.ocks.org/mbostock), con el código justo debajo de cada gráfico.

----------
## Versiones

D3 es una librería muy actualizada, lo que conlleva ciertas ventajas e inconvenientes que hay que tener en cuenta. La mayoría de sus actualizaciones incluyen herramientas nuevas; algunas actualizaciones cambian/mejoran la sintaxis, y otras cambian cómo se usa la librería en sí:

D3 nació como una librería "monolítica", que se encargaba de todo: modificar la página para crear los gráficos, hacer los cálculos matemáticos correspondientes, manejar las interacciones de los usuarios, etc. Con el paso del tiempo y la aparición de frameworks JavaScript como Angular, React o Vue, que ya tienen sus propios sistemas para manejar, D3 se ha adaptado para poder ofrecer de forma modular sus funciones: ya no es necesario usar toda la librería, sino que podes usar sólo el módulo específico que necesitemos. Este cambio de paradigma se implementó en la [versión 4](https://github.com/d3/d3/releases/tag/v4.0.0).

## Un vistazo rápido

#### Selectores

Los selectores de D3 se parecen mucho a jQuery, y funcionan con selectores CSS

```javascript
// Selector simple
d3.select('h1')
  .style('color', '#BF30B3');

// Selector múltiple
d3.selectAll('#home .article > h2')
  .attr('class', 'text');

// Selector encadenado
const body = d3.select('body');

body.selectAll('p')
  .style('color', 'red');
```


#### Añadir y eliminar elementos

D3 tiene también funciones parecidas a jQuery para estas operaciones:

```javascript
// Añadir elementos
d3.select('.chart')
  .append('ul')
  .append('li')
  .text('hola');

// Eliminar elementos
d3.select('.chart')
  .select('h1')
  .remove();
```
> Algunos métodos, como `append`, cambian el selector al usarse

#### Bindado de datos

La parte interesante de D3 es que podemos hacer las operaciones anteriores basándonos en arrays o matrices de de datos, y hacer que cada elemento de nuestra página dependa de ellos

```javascript
const colors = ['red', 'blue', 'green']; 

d3.selectAll('li')
  .data(colors);
  .style('color', (d) => d + 'px');
```

##### Enter y Exit

D3js fue concebido para manipular gráficos basándose en actualizaciones de datos (el array anterior). Existen dos selectores especiales, `enter` y `exit`, que nos ayudan a hacer dichas actualizaciones.

`enter` selecciona los elementos de nuestro array de datos que no están bindados a ningún elemento del DOM. `exit` selecciona los elementos del array de datos que han sido eliminados (y pueden estar bindados en el DOM).

Dicho de otro modo. Empezamos con nuestro un html simplificado:

```
<body>
  <ul class="list"><ul>
</body>

<script>
  const data = [4, 8, 15, 16, 23, 42]
</script>
```

Queremos añadir tantos `<li>` como elementos del array `data`, y escribir el número de cada uno en él. Como los elementos no existen en el DOM, utilizamos `enter`:  

```javascript
const items = d3.select(".list").selectAll("li")
    .data(data);
    .enter()
    .append("li")
    .text((d) => "I’m number " + d);
```

Ahora nuestros datos cambian, se añade un elemento (12) y se elimina otro (8):

```javascript
data.push(12) // data = [4, 8, 15, 16, 23, 42, 12]
data.splice(1, 1) // data = [4, 15, 16, 23, 42, 12]
```

Vamos a eliminar los elementos que ya no necesitamos con `exit`:

```javascript
items.exit().remove();
```

Y creamos los elementos nuevos:

```javascript
items.enter()
  .append("li")
  .text((d) => "I’m number " + d);
```

#### SVG

Mientras HTML está limitado mayormente a formas rectangulares, SVG soporta muchas más trazados, como curvas Bézier, degradados, recortes y máscaras. Para hacer un gráfico de barras no es necesario conocer todas las funciones que ofrece SVG, pero conocerlo es un plus importante a la hora de diseñar visualizaciones, ya que nos permite realizar muchas más representaciones.

SVG es un lenguaje estandarizado de representación de imágenes vectoriales. Puede crearse tanto a partir de programas de diseño como Illustrator o Photoshop o mediante técnicas de programación. Son elementos que se interpretan por los navegadores (pueden inspeccionarse y manipularse!), pueden ser estilizados mediante CSS y pueden modificarse con javascript.

La [tabla de especificaciones](https://www.w3.org/TR/SVG/) parece un poco engorrosa, pero para la mayoría de formas son sencillas.

```javascript
const data = [4, 8, 15, 16, 23, 42];
 
const width = 420;
const barHeight = 20;
const height = 600;
 
const chart = d3.select('body')
  .append('svg')
  .attr('width', width)
  .attr('height', height);
 
const rects = chart.selectAll('rect')
  .data(data)
  .enter()
  .append('rect')
  .attr('x', 0)
  .attr('y', (d, i) => i * barHeight)
  .attr('width', (d) => d)
  .attr('height', barHeight - 1);
```

#### Interacciones con el DOM

Tenemos `onclick`, `mouseover`, `mouseout` y otros cuantos, y los bindamos tan ricamente:

```javascript
d3.select("p")
  .on("click", () => {
    alert('Que pasa bro");
  });
```

#### Siguientes pasos
 - [Convención de margenes de dibujo](http://bl.ocks.org/mbostock/3019563)
 - [Convención para el update](http://bl.ocks.org/mbostock/3808218)
 - [Gráfico de fiebre](http://bl.ocks.org/mbostock/3883245)
 - [Gráfico simple de tarta](http://bl.ocks.org/mbostock/3887235)