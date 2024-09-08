[Conceptos de Programación Orientada a Objetos](../../)

# 2. Tipos

## 2.1 Contenido

Los objetos colaboran pidiendo y prestando servicios mediante el envío mensajes. El objeto que envía el mensaje quiere consultar o cambiar el estado, o quiereactivar cierto comportamiento, del objeto que recibe
el mensaje.

Para que esta colaboración sea eficaz, el objeto emisor sólo debe enviar mensajes para consultar el estado o
activar un comportamiento que el objeto que los recibe tenga la responsabilidad de conocer o hacer, pero no
otros. A su vez, el objeto que recibe los mensajes debe informar o cambiar el estado que tiene la responsabilidad de conocer, o exhibir el comportamiento que tiene la responsabilidad de hacer, pero no menos.

En cada colaboración se establece un contrato entre el objeto que envía el mensaje y el objeto que lo recibe. Los contratos implican derechos y obligaciones para los participantes: el objeto que envía el mensaje está obligado a enviar mensajes solicitando solamente aquello que está en el contrato y el que recibe el mensaje está obligado a hacer lo que le pidan que esté en el contrato; a su vez el objeto que envía el mensaje tiene todo el derecho de solicitar cualquier cosa que esté en el contrato y el que lo recibe tiene el derecho de hacer
sólo lo que establece en el contrato y nada más.

Los **tipos** son la realización o implementación de los contratos entre objetos. Un tipo es un conjunto de
operaciones. Cada operación tiene una **firma** que la caracteriza, que incluye el nombre de la operación, los argumentos, y el resultado.

<details open>
<summary>🗒 Tarjeta: Tipo ±</summary>

| Tipo |
| ---- |
| Un **tipo** es un conjunto de **operaciones** que determina los mensajes que pueden ser enviados a los objetos de ese tipo. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Firma ±</summary>

| Firma |
| ---- |
| La **firma** de una operación en un tipo o de un método en una clase está compuesta por el **nombre**, los **parámetros** y **tipo de los parámetros**, y el **tipo del resultado** de la operación o método. |
| No hay firmas duplicadas. |

</details>
<br/>

El uso de tipos permite a los objetos que envían mensajes saber qué operaciones pueden solicitar a los objetos
que los reciben, cómo solicitarlas, y qué ocurre cuando lo hacen <sup>1</sup>.

La clase de un objeto que tiene un tipo cuenta con un método para todas y cada una de las operaciones contenidas en el tipo. La firma de cada método coincide exactamente con la de la operación correspondiente. En tal caso decimos que la clase **implementa** el tipo.

<details open>
<summary>🗒 Tarjeta: Clases implementan tipos ±</summary>

| Clases implementan tipos |
| ---- |
| La **clase** de un objeto **implementa** un **tipo** si tiene un método para cada una de las operaciones incluidas en el tipo. |
| La firma de cada método coincide exactamente con la de la operación correspondiente. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Mensajes y métodos ±</summary>

| Mensajes y métodos |
| ---- |
| El método que se ejecuta como consecuencia de la recepción de un mensaje tiene el mismo nombre que el selector del mensaje, los mismos parámetros que el mensaje, y los parámetros son del mismo tipo que los del mensaje. |

</details>
<br/>


Cuando un objeto envía un mensaje a otro, el mensaje dice quién es el objeto **receptor**, cuál es el nombre de la operación, o **selector**, y cuáles son los valores de los argumentos, si los hubiere.

<details open>
<summary>🗒 Tarjeta: Emisores y tipos ±</summary>

| Emisores y tipos |
| ---- |
| El emisor de un mensaje está obligado a usar las operaciones incluidas en el tipo del receptor y tiene el derecho de usar cualquier de las operaciones incluidas en el tipo. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Receptores y tipos ±</summary>

| Receptores y tipos |
| ---- |
| El receptor de un mensaje está obligado a que su clase tenga un método para cada operación incluida en su tipo y tiene el derecho de que su clase tenga métodos sólo para esas operaciones y ninguna más. |

</details>
<br/>

Un objeto puede colaborar con muchos otros en diferentes oportunidades. Cada una de estas colaboraciones puede requerir un contrato diferente. Por eso un objeto puede tener más de un tipo. Al mismo tiempo varios objetos pueden colaborar exactamente de la misma manera cuando prestan exactamente los mismos servicios. Por eso varios objetos aún de diferentes clases pueden tener el mismo tipo.

<details open>
<summary>🗒 Tarjeta: Un objeto, más de un tipo ±</summary>

| Un objeto, más de un tipo |
| ---- |
| Un objeto puede tener más de un tipo si la clase de ese objeto implementa más de un tipo. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Un tipo, más de un objeto ±</summary>

| Un tipo, más de un objeto |
| ---- |
| Objetos de clases diferentes pueden tener el mismo tipo si las clases de esos objetos implementan el mismo tipo. |

</details>
<br/>

Algunos lenguajes de programación tienen construcciones sintácticas particulares para la declaración explícita de tipos, es decir, independientemente de la declaración de clases. De todas formas, una clase siempre declara implícitamente un tipo, pues a todos los efectos puedo ver el conjunto de operaciones declarados en la clase, cuando veo solamente la forma de los métodos e ignoro su contenido.

<details open>
<summary>🗒 Tarjeta: Declaración de tipos ±</summary>

| Declaración de tipos |
| ---- |
| Los tipos pueden ser declarados explícitamente usando **interfaces** e implícitamente usando **clases**. |

</details>
<br/>

Un tipo se construye alrededor de una abstracción. Una abstracción expresa las características esenciales de un objeto, que lo distinguen de todos los demás tipos de objetos, y que provee límites conceptuales claramente definidos, relativos a la perspectiva del usuario<sup>2</sup>. La abstracción es una de las formas más importantes de enfrentar la complejidad que tenemos las personas.

<details open>
<summary>🗒 Tarjeta: Abstracción ±</summary>

| Abstracción |
| ---- |
| Una **abstracción** expresa las características **esenciales** de un objeto, que lo distinguen de todos los demás tipos de objetos, y que provee límites conceptuales claramente definidos, relativos a la perspectiva del usuario. |

</details>
<br/>


El objeto que recibe un mensaje no tiene porqué saber quién es el objeto que envía el mensaje. Al objeto que envía el mensaje, sólo le interesa que el receptor haga lo que su tipo especifica que tiene responsabilidad de hacer, pero no necesita saber mucho más acerca del receptor. Entonces, la posibilidad de enviar un mensaje depende solamente del tipo del objeto que lo recibe<sup>3</sup>. Esto es muy bueno, porque implica que para que un objeto pida colaboración a otro, necesita saber muy pocas cosas acerca del otro, apenas qué tipo tiene, es decir, es posible enviar mensajes a un objeto con cierto tipo, sabiendo que cualquier otro objeto que tenga el mismo tipo, podrá eventualmente aceptar la solicitud.

De hecho, no es necesario que este otro objeto tenga exactamente el mismo tipo, alcanza con que tenga las mismas operaciones, pero podría tener más. Un tipo es **subtipo** de otro si el conjunto de operaciones contiene el conjunto de las del otro. Un tipo es **supertipo** de otro si el conjunto de operaciones está incluido en el del otro.

<details open>
<summary>🗒 Tarjeta: Subtipo ±</summary>

| Subtipo |
| ---- |
| Un tipo A es **subtipo** de otro B si el conjunto de operaciones del tipo A **contiene** el conjunto de las del tipo B. |
| El **subtipo** A suele ser **menos abstracto** o **más concreto** que el tipo B. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Supertipo ±</summary>

| Supertipo |
| ---- |
| Un tipo A es **supertipo** de otro B si el conjunto de operaciones del tipo A **está contenido** en el conjunto de las del tipo B. |
| El **supertipo** A suele ser **más abstracto** o **menos concreto** que el tipo B. |

</details>
<br/>

Lo anterior se resumen en el **principio de sustitución**: en cualquier contexto en el que sea válido usar un objeto con un cierto tipo, se puede sustituir ese objeto por otro con un subtipo de ese tipo, porque puede recibir y procesar exactamente los mismos mensajes.

<details open>
<summary>🗒 Tarjeta: Principio de sustitución ±</summary>

| Principio de sustitución |
| ---- |
| En cualquier lugar de un programa donde se espera un objeto de un tipo puede aparecer un objeto de un subtipo y el comportamiento del programa no debería cambiar. |

</details>
<br/>

Cuando una misma definición puede ser usada con diferentes tipos, decimos que la definición es **polimórfica**.

<details open>
<summary>🗒 Tarjeta: Polimorfismo ±</summary>

| Polimorfismo |
| ---- |
| El **polimorfismo** permite que una misma definición pueda ser usada con diferentes tipos. |
| En tal caso decimos que la definición es **polimórfica** |

</details>
<br/>

Una operación es polimórfica cuando puede ser usada con diferentes tipos, es decir:

- La operación es polimórfica cuando puede actuar sobre un tipo y todos sus subtipos<sup>4</sup>. Para esto, objetos de diferentes tipos deben tener la capacidad de responder al mismo mensaje, de forma talque en un contexto determinado<sup>5</sup> puedan ser usados indistintamente.
- La operación también es polimórfica cuando los argumentos y el resultado pueden ser usados con diferentes tipos<sup>6</sup>. En este caso se suele hablar de sobrecarga.

<details open>
<summary>🗒 Tarjeta: Operación polimórfica ±</summary>

| Operación polimórfica |
| ---- |
| Una **operación** es **polimórfica** cuando es implementada por dos o más objetos de diferentes tipos. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Sobrecarga ±</summary>

| Sobrecarga |
| ---- |
| Una operación en un tipo o un método en una clase está **sobrecargado** cuando hay más de uno con el mismo nombre, pero con diferentes parámetros, o parámetros de diferente tipo, o diferente tipo de resultado, o una combinación de los anteriores. |

</details>
<br/>


<br>

> [2.2 Desarrollo »](./2_2_Desarrollo.md)

</br>

****

_<sup>1</sup> Cómo solicitar una operación, queda definido por en la declaración de la operación. Qué ocurre cuándo el objeto que recibe el mensaje ejecuta la operación, queda parcialmente definido por el resultado que aparece en la declaración. Pero el cambio de estado que la operación provoca y los efectos colaterales que pudiera tener, requieren especificación adicional; cuando veamos diseño por contrato complementaremos esta idea._

_<sup>2</sup> Usuario en este contexto es el objeto que usa al otro._

_<sup>3</sup> Lo que ocurra como resultado de procesar el mensaje, depende del método ejecutado, que a su vez depende tanto del selector del mensaje como del receptor. La dependencia del selector se debe a que selectores diferentes provocarán la ejecución de métodos diferentes. La dependencia del receptor se debe a que receptores de diferentes clases responderán a la solicitud con métodos diferentes._

_<sup>4</sup> Este tipo de polimorfismo se llama **polimorfismo de subtipo**._

_<sup>5</sup> Un contexto determinado significa que no siempre una clase se puede usar en lugar de la otra y viceversa; sólo es válido intercambiar las dos clases, cuando el cliente espera que el receptor tenga un cierto tipo y ese tipo es uno que ambas clases tienen en común._

_<sup>6</sup> Este tipo de polimorfismo se llama **polimorfismo ad-hoc**. No existe en todos los lenguajes de programación y para algunos no es estrictamente una forma de polimorfismo, pues no es más que una construcción sintáctica para simplificar la lectura y escritura del código._
