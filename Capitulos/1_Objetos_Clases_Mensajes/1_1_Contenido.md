![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 1. Objetos, Clases y Mensajes

## 1.1 Contenido

Los programas manejan ciertos datos usando cierta lógica o algoritmo. En un programa orientado a objetos hay un conjunto de **objetos** que colaboran pidiendo y prestando servicios.

No hay solo un lugar donde se concentren los datos y la lógica del programa, sino que los datos y la lógica están distribuidas en forma razonablemente equilibrada entre todos y cada uno de los objetos.

<details>
<summary>🗒 Tarjeta: Programa Orientado a Objetos »</summary>

| Programa Orientado a Objetos |
| ---- |
| En un programa orientado a objetos los **datos** y la **lógica** del programa están **distribuidas** en forma razonablemente **equilibrada** entre un conjunto de objetos que **colaboran** solicitándose y prestándose servicios mediante el envío de **mensajes**. |

</details>

<br/>

Esto significa que cada objeto tiene una parte de los datos y una parte de la lógica del programa. Cada objeto tiene así la responsabilidad de conocer la parte de los datos que le corresponde y de hacer la parte de la lógica que le corresponde. Cada dato que un objeto conoce es un **atributo**. El **estado** del objeto son los datos que conoce, es decir, es el conjunto de atributos y de valores de esos atributos<sup>1</sup>. El **comportamiento** del objeto las cosas que hace. Los **métodos** son la realización o implementación del comportamiento de los objetos.

<details>
<summary>🗒 Tarjeta: Estado »</summary>

| Estado |
| ---- |
| Cada objeto puede tener la **responsabilidad de conocer** una parte de los **datos** del programa. |
| El **estado** de un objeto son los **datos** y los **valores** de los datos que el objeto **conoce** y están almacenados en los **atributos**. |

</details>

<br/>

<details>
<summary>🗒 Tarjeta: Comportamiento »</summary>

| Comportamiento |
| --- |
| Cada objeto puede tener la **responsabilidad** de hacer una parte de la lógica del programa. |
| El **comportamiento** de un objeto son las cosas que el objeto **hace** y está implementado en los **métodos**. |

</details>

<br/>

Los objetos colaboran pidiendo y prestando servicios. Los servicios se piden únicamente mediante el envío **mensajes**. El objeto que envía el mensaje quiere consultar o cambiar el estado o quiere activar cierto com- portamiento del objeto que recibe el mensaje. El receptor responde a la solicitud ejecutando un método. El **selector** de un mensaje es el nombre del método que el emisor desea que el receptor ejecute cuando reciba el mensaje.

<details>
<summary>🗒 Tarjeta: Mensaje »</summary>

| Mensaje |
| ---- |
| Los objetos se comunican mediante el envío **mensajes**. |
| El objeto que **emisor** del mensaje quiere consultar o cambiar el **estado** o quiere ejecutar cierto **comportamiento** del objeto **receptor** del mensaje |

</details>

<br/>

<details>
<summary>🗒 Tarjeta: Selector »</summary>

| Selector |
| ---- |
| El **selector** de un mensaje es el nombre del **método** que el emisor desea que el receptor ejecute cuando reciba el mensaje. |

</details>

<br/>

El estado de un objeto no es accesible directamente a otros objetos. Cuando un objeto necesita conocer el valor de un atributo de otro objeto, le envía a este último un mensaje preguntándoselo. El emisor no tiene porqué conocer cómo se representa internamente un atributo; el receptor podría retornar un valor que tiene guardado o podría calcularlo cada vez que fuera necesario. De la misma forma, cuando un objeto quiere cambiar el valor de un atributo de otro objeto, le envían un mensaje a este último con el nuevo valor; el receptor podrá guardar el nuevo valor o procesar el mensaje de alguna forma para que el estado cambie tal como lo solicitó el emisor.

El hecho de que otros objetos no conozcan la representación interna de los atributos, ni la forma en la cual están implementadas los métodos de los demás objetos, se conoce como encapsulación. La encapsulación permite independizarse de los detalles de implementación de un objeto y concentrarse únicamente en los aspectos esenciales del mismo.

La encapsulación es el mecanismo que permite integrar en una misma unidad -el objeto- comportamiento y estado, haciéndolos solo accesibles mediante el envío de mensajes.

<details>
<summary>🗒 Tarjeta: Encapsulación »</summary>

| Encapsulación |
| ---- |
| La **encapsulación** es el resultado de ocultar todos los detalles acerca de la implementación de las responsabilidades. |
| Es sinónimo de **escondimiento de información**. |

</details>

<br/>

<details>
<summary>🗒 Tarjeta: Público/Privado »</summary>

| Público/Privado |
| ---- |
| Un método o atributo **público** es accesible a cualquier objeto de cualquier clase. |
| Un método o atributo **privado** es accesible sólo a los objetos de la clase en la que se define ese método o atributo. |

</details>

<br/>

Los objetos con los mismos atributos y métodos son producidos con el mismo molde. La **clase** del objeto es ese molde. Es objeto es una **instancia** de esa clase. Los objetos no pueden pertenecer a más de una clase. La clase de un objeto habitualmente no cambia durante la vida del objeto<sup>2</sup>.

<details>
<summary>🗒 Tarjeta: Clase »</summary>

| Clase |
| ---- |
| Una **clase** es una **plantilla** o **molde** para un conjunto de objetos que comparten los mismos atributos, métodos, relaciones y semántica. |
| Un objeto es una **instancia** de una clase. |

</details>

<br/>

Siempre es posible reconocer un objeto de otro, aunque luzcan **iguales**, es decir, aunque sean de la misma clase y tengan los mismos valores de los atributos. La **identidad** es el carácter propio y diferenciado de un objeto, que denota una existencia separada de los demás objetos, aunque sus atributos puedan tener los mismos valores que los de otros objetos de la misma clase.

<details>
<summary>🗒 Tarjeta: Igualdad »</summary>

| Igualdad |
| ---- |
| Dos objetos son **iguales** cuando son de la **misma clase** y tienen los **mismos valores** de atributos. |

</details>

<br/>

<details>
<summary>🗒 Tarjeta: Identidad »</summary>

| Identidad |
| ---- |
| La **identidad** es el carácter propio y diferenciado de un objeto que denota una existencia separada de los demás, aunque pueda tener los mismos atributos y valores de atributos que otros objetos. |

</details>

<br/>

La clasificación genera la ilusión de simplicidad, básicamente, porque reduce el número de elementos diferentes a tener en cuenta simultáneamente<sup>3</sup> y porque las características de los objetos no están descritas cada vez en cada uno de los ellos, sino una sola vez en su clase.

Los métodos mencionados hasta el momento son llamados **métodos de instancia**, pues la ejecución se realiza dentro del contexto de la instancia de una clase, es decir, del objeto que ejecuta el método. Existen también los **métodos de clase**, que representan responsabilidades de hacer de las clases propiamente dichas y no de sus instancias. Estos métodos difieren de los primeros en que no están asociados a un objeto específico sino a una clase, por lo cual no pueden acceder directamente al estado ni ejecutar directamente métodos de sus instancias.

Por ejemplo, el mensaje para crear un nuevo objeto no puede ser enviado a un objeto, simplemente porque el objeto todavía no existe. La clase es quien tiene en realidad la responsabilidad de crear sus nuevas instan- cias, por lo que es necesario enviarle un mensaje para crear un nuevo objeto de esa clase. La clase implementa esa responsabilidad en un método de clase, que tiene un nombre especial, el **constructor** de la clase<sup>4</sup>.

<details>
<summary>🗒 Tarjeta: Constructor »</summary>

| Constructor |
| ---- |
| El **constructor** es un método de clase para **crear** e **inicializar** nuevas instancias de esa clase. |

</details>

<br/>

Algo similar ocurre con los **atributos de clase**, que representan responsabilidades de conocer de las clases propiamente dichas<sup>5</sup>.


<br/>

> [1.2 Desarrollo »](./1_2_Desarrollo.md)

<br/>

****

_<sup>1</sup> La forma en la que programamos los atributos es diferente en los distintos lenguajes de programación. Habitualmente las **variables de instancia** son la realización o implementación de los atributos, pero desde que otros objetos deben enviar un mensaje para consultar o cambiar el estado, el receptor es libre de implementar el método correspondiente como quiera, bien retornando o asignando una variable de instancia, o bien de otra forma._

_<sup>2</sup> En algunos lenguajes como Smalltalk o CLOS es posible cambiar la clase de un objeto. Por ejemplo, en Smalltalk, `anObject become: newClass`. También es posible, y fácil, cambiar una clase en tiempo de ejecución y aunque existan instancias de esa clase._

_<sup>3</sup> Se puede describir el mundo como las relaciones entre las clases de objetos, en lugar de las relaciones entre los objetos. Esta visión es útil para la mayoría de los aspectos estructurales y estáticos de relaciones entre clases, pero no para algunos aspectos dinámicos, que se describen mediante los objetos y no mediante sus clases._

_<sup>4</sup> La sintaxis para crear nuevos objetos es diferente en los distintos lenguajes de programación y también puede ser diferente que la sintaxis para enviar un mensaje a un objeto._

_<sup>5</sup> También la forma en la que programamos los atributos de clase es diferente en los distintos lenguajes de programación, pero también es habitual usar las variables de clase con ese propósito._