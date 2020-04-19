![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 5. Herencia-Agregación

## 5.1 Contenido

En ciertas oportunidades es posible reconocer una relación de clasificación taxonómica entre diversas clases. En esa relación es posible afirmar que una clase «es un tipo de» o «es un» otra clase. Un puma «es un» mamífero y un mamífero «es un» animal. Un tero «es un» ave y un ave «es un» animal pero un tero no es un mamífero.

Entre las clases así clasificadas es posible reconocer características compartidas por varias de ellas y carac- terísticas exclusivas de cada una. Es posible extraer esas características compartidas de todas las clases que las comparten a una nueva clase, más general o más abstracta, y dejar en las demás clases solamente las características exclusivas de cada una.

En otras oportunidades es posible reconocer que objetos de una clase están siempre formados por objetos de otras clases. En esa relación es posible afirmar que un objeto «está compuesto por» otro u otros objetos o que «es parte de» otro objeto.

Cuando decimos “una clase «es un» otra clase”, estamos diciendo “una clase más concreta «es un» caso particular de una clase más abstracta”. O dicho de otra forma “una clase más concreta hereda de otra más abstracta sus características”. Esto define entre las clases una relación de **herencia** o **generalización-especialización**.

> [🗒 Tarjeta: Generalización »](../../Tarjetas/Herencia_Agregacion/Generalizacion.md)

> [🗒 Tarjeta: Relaciones "es tipo de" »](../../Tarjetas/Herencia_Agregacion/Es_Tipo_De.md)

La herencia genera ilusión de simplicidad: no es necesario repetir en la clase concreta los detalles de la clase abstracta; basta dar los detalles de la clase concreta y decir de cual hereda. Las características de las que hemos venido hablando son atributos y métodos. Una subclase es una clase que hereda atributos y métodos de otra clase y habitualmente agrega nuevos atributos y nuevos métodos. Una **superclase** es la clase a partir de la cual se hereda. Una **subclase** es la clase heredada.

> [🗒 Tarjeta: Clase base »](../../Tarjetas/Herencia_Agregacion/Clase_Base.md)

> [🗒 Tarjeta: Clase sucesora »](../../Tarjetas/Herencia_Agregacion/Clase_Sucesora.md)

Además de implementar la relación taxonómica de generalización-especialización, la herencia es una forma de reutilizar código, pues permite definir clases con atributos y métodos que no fue necesario escribir. No es correcto usar la herencia sólo por la reutilización, las clases también deben estar en relación taxonómica de generalización-especialización.

> [🗒 Tarjeta: Herencia »](../../Tarjetas/Herencia_Agregacion/Herencia.md)

> [🗒 Tarjeta: Herencia y reutilización »](../../Tarjetas/Herencia_Agregacion/Herencia_y_Reutilizacion.md)

> [🗒 Tarjeta: Herencia múltiple »](../../Tarjetas/Herencia_Agregacion/Herencia_Multiple.md)

Cuando decimos “un objeto «es parte de» otro objeto”, estamos diciendo “el todo es la suma de las partes”. O dicho de otra forma “un objeto agrega otros objetos”. Esto define una relación de **agregación** entre los objetos. También aquí hay una ilusión de simplicidad: podemos usar el todo haciendo caso omiso de las partes.

> [🗒 Tarjeta: Agregación »](../../Tarjetas/Herencia_Agregacion/Agregacion.md)

> [🗒 Tarjeta: Relaciones "es parte de" »](../../Tarjetas/Herencia_Agregacion/Es_Parte_De.md)

Herencia y agregación son formas de jerarquía u ordenación clases y objetos.

La posibilidad de enviar un mensaje depende solamente del tipo del objeto que lo recibe. Lo que ocurra como resultado de procesar el mensaje, depende del método ejecutado, que a su vez depende tanto del selector del mensaje como del receptor<sup>24</sup>. La asociación entre selectores y métodos en tiempo de ejecución se llama **encadenamiento dinámico** o **encadenamiento tardío**.

Lo contrario a encadenamiento dinámico es **encadenamiento estático** o **encadenamiento temprano**. En éste caso, el método a ejecutar se decide en tiempo de compilación en base al tipo de la variable a la cual se está enviando el mensaje.

> [🗒 Tarjeta: Encadenamiento dinámico o tardío »](../../Tarjetas/Herencia_Agregacion/Encadenamiento_Dinamico.md)

> [🗒 Tarjeta: Encadenamiento estático o temprano »](../../Tarjetas/Herencia_Agregacion/Encadenamiento_Estatico.md)

El encadenamiento dinámico es necesario para soportar polimorfismo, debido a que en otro caso la operación siempre se ejecutaría de una forma, pues el método a ejecutar se define una única vez al momento de compilar. En algunos lenguajes, es posible indicar al compilador qué tipo de encadenamiento queremos utilizar en cada caso<sup>25</sup>; en los lenguajes donde eso no es posible, el encadenamiento es siempre dinámico.

<br/>

> [5.2 Desarrollo »](./5_2_Desarrollo.md)

<br/>


********

<sup>24</sup> _La dependencia del selector se debe a que selectores diferentes provocarán la ejecución de métodos diferentes. La dependencia del receptor se debe a que receptores de diferentes clases responderán a la solicitud con métodos diferentes._

<sup>25</sup> _Aunque existen casos donde no hay opción. Por ejemplo, si la única información que tengo de una variable es que su tipo es el tipo de una interfaz, no va a ser posible encadenar el código estáticamente ya que no hay un método para encadenar. Algo similar sucede en lenguajes donde no se define el tipo de las variables._