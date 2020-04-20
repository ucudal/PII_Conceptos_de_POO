![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 3. Tipos genéricos

## 3.1 Contenido

Las estructuras de propósito general como las colecciones, por ejemplo, usan el tipo `Object` para contener objetos de cualquier tipo. El problema con esto es justamente eso último: “objetos de cualquier tipo”.

A veces es bueno tener colecciones heterogéneas, en las que se mezclen objetos de diferente tipo. Pero en la mayoría de los casos, mezclar objetos de diferente tipo en la misma colección es un error de programación que se comete inadvertidamente; para evitarlo, es necesario restringir el tipo de los objetos contenidos en esos contenedores.

Además, si el contenedor contiene cualquier tipo de objeto -es decir, objetos de tipo `Object`-, aunque nosotros guardemos en ella objetos de cierto tipo diferente de `Object`, recuperaremos objetos de tipo `Object`. ¿Recuerdan que un objeto puede tener más de un tipo? Pues bien, `Object` es uno de los tipos que siempre tienen todos los objetos. Esto nos obliga a hacer un _typecast_, en el que nosotros le decimos al compilador el tipo de los objetos. A diferencia del compilador, nosotros nos podemos equivocar y los _typecast_ pueden fallar y hacen los programas potencialmente menos seguros.

Para evitar este tipo de errores, es posible utilizar **tipos genéricos**. En éstos, es posible declarar sobre qué tipo o tipos va a operar el tipo genérico, de forma de poder realizar los controles de tipos necesarios. Por ejemplo, podría decir que una lista es un tipo genérico y al momento de construirla declarar el tipo de objetos que la lista va a contener. Esto permite que luego el compilador pueda controlar que los objetos agregados en esta lista sean del tipo correcto y evitar hacer un _typecast_ al acceder a uno de ellos.

<details>
  <summary>🗒 Tarjeta: Genéricos »</summary>
<table id="card">
    <tr>
        <td align="center">
            <h3>Genéricos</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Los genéricos son un <b>mecanismo</b> de los lenguajes de programación para implementar <b>declarativamente</b> relaciones de <b>generalización</b> entre un tipo base y uno o más tipos mediante el uso de <b>tipos parámetro</b>.</p>
        </td>
    </tr>
</table>
</details>

<details>
  <summary>🗒 Tarjeta: Tipo genérico y tipo parámetro »</summary>
<table id="card">
    <tr>
        <td align="center">
            <h3>Tipo genérico y tipo parámetro</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Un <b>tipo genérico</b> es un tipo que se define en términos de otro <b>tipo parámetro</b>.</p>
        </td>
    </tr>
</table>
</details>

<details>
  <summary>🗒 Tarjeta: Tipo argumento y tipo construido »</summary>
<table id="card">
    <tr>
        <td align="center">
            <h3>Tipo argumento y tipo construido</h3>
        </td>
    </tr>
    <tr>
        <td>
            <p>Cuando se <b>declara</b> una variable o parámetro de un tipo genérico se provee un <b>tipo argumento</b>.</p>
            <p>Al tipo asi declarado se le llama <b>tipo construido</b>.</p>
        </td>
    </tr>
</table>
</details>

El uso de genéricos provee lo que se conoce como **polimorfismo paramétrico**, al permitir usar una misma declaración de distinta forma en base a un parámetro especificado al momento de utilizarla. Siguiendo con el ejemplo anterior, la declaración de la clase lista puede ser parametrizada con el tipo de objeto contenido al momento de definir una nueva lista. 

Los tipos genéricos son solo una clase de declaraciones genéricas. Muchos lenguajes, incluyendo C#, permiten también definir **métodos genéricos**, los cuales pueden determinar el tipo de dato retornado en base a un parámetro pasado al momento de invocarlo, o imponer restricciones sobre los parámetros recibidos. Por ejemplo, declarar un método que recibe dos objetos de cualquier tipo, siempre y cuando ambos tengan el mismo tipo. 


<br/>

> [3.2 Desarrollo »](./3_2_Desarrollo.md)

<br/>

