![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 5. Herencia-Agregación

## 5.1 Contenido

En ciertas oportunidades es posible reconocer una relación de clasificación taxonómica entre diversas clases. En esa relación es posible afirmar que una clase «es un tipo de» o «es un» otra clase. Un puma «es un» mamífero y un mamífero «es un» animal. Un tero «es un» ave y un ave «es un» animal pero un tero no es un mamífero.

Entre las clases así clasificadas es posible reconocer características compartidas por varias de ellas y características exclusivas de cada una. Es posible extraer esas características compartidas de todas las clases que las comparten a una nueva clase, más general o más abstracta, y dejar en las demás clases solamente las características exclusivas de cada una.

En otras oportunidades es posible reconocer que objetos de una clase están siempre formados por objetos de otras clases. En esa relación es posible afirmar que un objeto «está compuesto por» otro u otros objetos o que «es parte de» otro objeto.

Cuando decimos “una clase «es un» otra clase”, estamos diciendo “una clase más concreta «es un» caso particular de una clase más abstracta”. O dicho de otra forma “una clase más concreta hereda de otra más abstracta sus características”. Esto define entre las clases una relación de **herencia** o **generalización-especialización**.

<details open>
<summary>🗒 Tarjeta: Generalización ±</summary>

| Generalización |
| ---- |
| La <b>generalización</b> es una <b>relación</b> taxonómica entre una <b>clase base</b> más <b>general</b> o <b>abstracta</b> y otra <b>clase sucesora</b> más <b>específica</b> o <b>concreta</b>. |
| La clase base es completamente <b>consistente</b> con la clase sucesora. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Relaciones «es un tipo de» ±</summary>

| Relaciones «es un tipo de» |
| ---- |
| La generalización establece un <b>orden</b> o <b>clasificación</b> de abstracciones que crea una <b>ilusión de simplicidad</b> mediante relaciones «es un tipo de» entre ellas. |

</details>
<br/>

La herencia genera ilusión de simplicidad: no es necesario repetir en la clase concreta los detalles de la clase abstracta; basta dar los detalles de la clase concreta y decir de cual hereda. Las características de las que hemos venido hablando son atributos y métodos. Una subclase es una clase que hereda atributos y métodos de otra clase y habitualmente agrega nuevos atributos y nuevos métodos. Una **superclase** es la clase a partir de la cual se hereda. Una **subclase** es la clase heredada.

<details open>
<summary>🗒 Tarjeta: Clase base ±</summary>

| Clase base |
| ---- |
| En una relación de generalización la <b>clase base</b> es la <b>generalización</b> de otra clase. |
| Tambien <b>clase ancestra</b> o <b>superclase</b> son usados como sinónimo de clase base. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Clase sucesora ±</summary>

| Clase sucesora |
| ---- |
| En una relación de generalización la <b>clase sucesora</b> es la <b>especialización</b> de otra clase. |
| Tambien <b>clase derivada</b> o <b>subclase</b> son usados como sinónimo de clase sucesora. |

</details>
<br/>

Además de implementar la relación taxonómica de generalización-especialización, la herencia es una forma de reutilizar código, pues permite definir clases con atributos y métodos que no fue necesario escribir. No es correcto usar la herencia sólo por la reutilización, las clases también deben estar en relación taxonómica de generalización-especialización.

<details open>
<summary>🗒 Tarjeta: Herencia ±</summary>

| Herencia |
| ---- |
| La <b>herencia</b> es un <b>mecanismo</b> de los lenguajes de programación para implementar <b>declarativamente</b> relaciones de <b>generalización</b> entre una o más clases bases y una o más clases sucesoras. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Herencia y reutilización ±</summary>

| Herencia y reutilización |
| ---- |
| La herencia es una forma de reutilzación de código más utilizadas pues permite crear nuevas clases a partir de clases existentes. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Herencia múltiple ±</summary>

| Herencia múltiple |
| ---- |
| Es una forma de <b>herencia</b> en algunos lenguajes de programación orientada a objetos en la que una clase sucesora puede tener <b>más de una</b> clase base. |

</details>
<br/>

Cuando decimos “un objeto «es parte de» otro objeto”, estamos diciendo “el todo es la suma de las partes”. O dicho de otra forma “un objeto agrega otros objetos”. Esto define una relación de **agregación** entre los objetos. También aquí hay una ilusión de simplicidad: podemos usar el todo haciendo caso omiso de las partes.

<details open>
<summary>🗒 Tarjeta: Agregación ±</summary>

| Agregación |
| ---- |
| La <b>agregación</b> es una relación de <b>todo-parte</b> ente una <b>clase compuesta</b> y una <b>clase componente</b>. |
| Instancias de la clase componente suelen existir independientemente de la existencia de instancias de la otra. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Relaciones «es parte de» ±</summary>

| Relaciones «es parte de» |
| ---- |
| La agregación establece un <b>orden</b> o <b>clasificación</b> de abstracciones que crea una <b>ilusión de simplicidad</b> mediante relaciones «es parte de» entre ellas. |

</details>
<br/>

Herencia y agregación son formas de jerarquía u ordenación clases y objetos.

La posibilidad de enviar un mensaje depende solamente del tipo del objeto que lo recibe. Lo que ocurra como resultado de procesar el mensaje, depende del método ejecutado, que a su vez depende tanto del selector del mensaje como del receptor<sup>24</sup>. La asociación entre selectores y métodos en tiempo de ejecución se llama **encadenamiento dinámico** o **encadenamiento tardío**.

Lo contrario a encadenamiento dinámico es **encadenamiento estático** o **encadenamiento temprano**. En éste caso, el método a ejecutar se decide en tiempo de compilación en base al tipo de la variable a la cual se está enviando el mensaje.

<details open>
<summary>🗒 Tarjeta: Encadenamiento dinámico o tardío ±</summary>

| Encadenamiento dinámico o tardío |
| ---- |
| El método que se ejecuta como consecuencia del envío de un mensaje se determina en <b>tiempo de ejecución</b> buscando en la clase del <b>objeto receptor</b> y en sus ancestras aquél cuyo nombre coincida con el selector del mensaje. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Encadenamiento estático o temprano ±</summary>

| Encadenamiento estático o temprano |
| ---- |
| El método que se ejecuta como consecuencia del envío de un mensaje se determina en <b>tiempo de compilación</b> buscando en la clase de la cual está declarada la variable que referencia al objeto receptor y en sus ancestras aquél cuyo nombre concida con el selector del mensaje. |

</details>
<br/>

El encadenamiento dinámico es necesario para soportar polimorfismo, debido a que en otro caso la operación siempre se ejecutaría de una forma, pues el método a ejecutar se define una única vez al momento de compilar. En algunos lenguajes, es posible indicar al compilador qué tipo de encadenamiento queremos utilizar en cada caso<sup>25</sup>; en los lenguajes donde eso no es posible, el encadenamiento es siempre dinámico.

<br/>

<details open>
<summary>🗒 Tarjeta: Composición vs. herencia ±</summary>

| Composición vs. herencia |
| ---- |
| La composición y delegación es una alternativa a la herencia. |
| En el contexto de la reutilización toda implementación que use herencia se puede cambiar por una equivalente que use composición y delegación. |

</details>
<br/>

Es posible cambiar el comportamiento de una clase compuesta, cambiando la clase
componente. Esto se puede lograr aún dinámicamente.

<details open>
<summary>🗒 Tarjeta: Composición vs Herencia ±</summary>

| Composición | Herencia |
| :----: | :----: |
| Caja negra | Caja blanca |
| Dinámica | Estática |
| Ejecución | Compilación |
| Por código | Declarativa |
| Más código | Menos código |
| Reuso selectivo | Reuso todo o nada |
| 1 o más clases | 1 clase (con herencia simple) |
| Tipos sin relación | Impone subtipo |

</details>
<br/>

| Composición y delegación | Herencia |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| La composición es reutilización de caja negra.  No es necesario conocer los detalles internos de la implementación para lograr la reutilización efectiva. Una clase diseñada para ser reutilizada  por composición y delegación está siempre  encapsulada. | La herencia es reutilización de caja blanca.  Es necesario conocer los detalles internos  de la implementación para lograr la reutilización efectiva. Una clase diseñada para ser reutilizada por herencia está encapsulada para los clientes pero no para los sucesores. |
| La composición es dinámica, puede cambiar en tiempo de ejecución. Composición y delegación pueden lograr que el comportamiento de los objetos así definidos cambie en tiempo de ejecución. | La herencia es estática, se define en tiempo de compilación. El comportamiento de los objetos así definidos es inmutable. |
| La composición y delegación requieren código adicional, agregado a mano por el programador. El tamaño del código aumenta, como así también la posibilidad de introducir errores, los requisitos de testeo, etc. | La herencia es declarativa. Una palabra clave basta para lograrlo. |
| La composición y delegación permiten reutilizar estado y comportamiento en forma selectiva | La herencia reutiliza todo, no puede elegir qué heredar y qué no[^1] |
| La composición y delegación puede reutilizar más de una clase simultáneamente | La herencia simple solo permite reutilizar una clase a la vez; la herencia múltiple permite reutilizar más de una clase simultáneamente[^2] |
| La composición y delegación no impone ninguna relación entre los tipos de la clase compuesta y  las clases componentes; la clase compuesta puede  implementar varios tipos | La herencia impone una relación de supertipo/subtipo entre la clase base y la clase sucesora; ésta tiene  al menos el tipo de aquélla, aunque puede implementar varios tipos más |

[^1]: En algunos lenguajes de programación, la herencia es selectiva, la clase
sucesora puede decidir no heredar algunos atributos o métodos. Esto implica que
la relación entre clase y subclase no necesariamente es también una relación
entre tipo y subtipo, lo que conduce a la violación del principio de
sustitución. Lo consideramos una anomalía y no debe ser tenido en cuenta.
[^2]: Lamentablemente son pocos los lenguajes que implementan herencia múltiple.

> [5.2 Desarrollo »](./5_2_Desarrollo.md)

<br/>


********

<sup>24</sup> _La dependencia del selector se debe a que selectores diferentes provocarán la ejecución de métodos diferentes. La dependencia del receptor se debe a que receptores de diferentes clases responderán a la solicitud con métodos diferentes._

<sup>25</sup> _Aunque existen casos donde no hay opción. Por ejemplo, si la única información que tengo de una variable es que su tipo es el tipo de una interfaz, no va a ser posible encadenar el código estáticamente ya que no hay un método para encadenar. Algo similar sucede en lenguajes donde no se define el tipo de las variables._
