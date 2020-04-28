![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 4. Programar contra especificaciones

## 4.1 Diseño por contrato

Los desarrolladores de software son desafiados con aplicaciones cada vez más complejas y tecnologías más innovadoras. Al mismo tiempo se les exige mayor calidad en las aplicaciones. La calidad de un producto final de software se describe mejor por la combinación de factores internos y externos. Uno de esos factores es la corrección.

Una pieza de software es correcta cuando satisface su especificación. Por lo tanto, la corrección es un concepto relativo, sin especificación no hay corrección. 

Gracias a la encapsulación, necesitamos enviar un mensaje a un objeto para acceder o cambiar su estado, o para activar alguno de sus comportamientos. Los mensajes que podemos mandar están especificados en el tipo de un objeto. Podemos decidir que la implementación provista por los métodos de la clase de un objeto es correcta respecto a las operaciones que están especificadas en el tipo del objeto, si para todas las operaciones declaradas en el tipo, la clase del objeto tiene un método cuya firma coincide con la de la correspondiente operación declarada en el tipo. ¿Alcanza con eso para especificar el estado y comportamiento de un objeto? Por supuesto que no. En ningún lado está especificado lo que hace una operación, es decir, cuál es el resultado de enviar un mensaje, cómo la operación cambia el estado del objeto, cuál tiene que ser el estado para poder enviar el mensaje. Necesitamos introducir un mecanismo que nos permite especificar qué hace una operación, para poder decidir cuando una implementación es correcta. Aquí es donde entran las fórmulas de corrección.

Una fórmula de corrección para una operación A viene dada por la expresión **{P} A {Q}** que se lee “cualquier ejecución de A, que comience en un estado donde P se cumple, termina en un estado donde Q se cumple”. P es llamada **precondición** y Q es llamada **postcondición**.

<details>
<summary>🗒 Tarjeta: Afirmación »</summary>

| Afirmación |
| ---- |
| Una <b>afirmación</b> es un predicado que puede ser <b>verdadero</b> o <b>falso</b> y se usa par a describir las <b>propiedades semánticas</b> de los tipos. |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Precondición »</summary>

| Precondición |
| ---- |
| Una <b>precondición</b> es una <b>afirmación</b> asociada a una operación que debe ser satisfecha por la clase de los objetos que <b>usan</b> esa operación. |
| Es una obligación para el objeto que usa la operación y un derecho para el objeto que la implementa. |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Poscondición »</summary>

| Poscondición |
| ---- |
| Una poscondición es una afirmación asociada a una operación que debe ser satisfecha por la clase de los objetos que implementan esa operación |
| Es un derecho para el objeto que usa la operación y una obligación para el objeto que la implementa |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Fórmula de corrección »</summary>

| Fórmula de corrección |
| ---- |
| **{P} A {Q}** |
| Cualquier ejecución de una operación A que comience en un estado en el que sesatisface la precondición P terminará en un estado en el que se satisface la poscondici ón Q. |

</details>
<br/>

Una afirmación puede ser más fuerte, o más restrictiva que otra. Coloquialmente hablando, decimos que cuesta más trabajo o es más difícil satisfacer una afirmación cuanto más fuerte sea. Formalmente decimos que dadas dos afirmaciones, P<sub>1</sub> y P<sub>2</sub>, se cumple que P<sub>1</sub> es **más fuerte** que otra P<sub>2</sub> [y que P<sub>2</sub> **más débil** que P<sub>1</sub>] si P<sub>1</sub> => P<sub>2</sub> y P<sub>1</sub> ≠ P<sub>2</sub>, es decir, P<sub>1</sub> implica P<sub>2</sub> y son diferentes.

Recuerden que P<sub>1</sub> => P<sub>2</sub> si y sólo si ∼P<sub>1</sub> V P<sub>2</sub>, es decir, P<sub>1</sub> => P<sub>2</sub> es falsa solo si P<sub>2</sub> es falsa.

| P1 | P2 | ∼P1 | ∼P1 V P2 |
|:--:|:--:|:---:|:--------:|
|  V |  V |  F  |     V    |
|  V |  F |  F  |     F    |
|  F |  V |  V  |     V    |
|  F |  F |  V  |     V    |

<details>
<summary>🗒 Tarjeta: Fortaleza/debilidad »</summary>

| Fortaleza/debilidad |
| ---- |
| Se dice que una afirmación P<sub>1</sub> es <b>más fuerte</b> que otra P<sub>2</sub> (y que P<sub>2</sub> más débil que P<sub>1</sub>) si <b>P<sub>1</sub> => P<sub>2</sub> y P<sub>1</sub> ≠ P<sub>2</sub></b> |
| P<sub>1</sub> => P<sub>2</sub> si y sólo si ∼P<sub>1</sub> V P<sub>2</sub> |

</details>
<br/>

El diseño por contrato está relacionado con el principio de sustitución de Liskov que vimos anteriormente. Un subtipo puede redefinir las precondiciones de las operaciones, pero sólo por otras más débiles. De esta forma, cuando en virtud del principio de sustitución, en un lugar donde se espera un objeto de un tipo T aparece un objeto de un subtipo S, un cliente que antes lograba cumplir las precondiciones de las operaciones de T también logrará que se cumplan las de las operaciones de S, porque las precondiciones de las operaciones de S son más débiles o a lo sumo iguales que las de las operaciones de T. En forma análoga, un subtipo S puede redefinir las postcondiciones de las operaciones de un tipo T por otras más fuertes. Un servidor que logre cumplir las postcondiciones de las operaciones de S también lograría que se cumplieras las postcondiciones de las operaciones de T, porque ahora las postcondiciones de las operaciones de S son más fuertes todavía o a lo sumo iguales que las de las operaciones de T

<details>
<summary>🗒 Tarjeta: Precondiciones/ subtipos »</summary>

| Precondiciones/ subtipos |
| ---- |
| Una operación de un subtipo debe tener <b>precondiciones igual es o más débil</b> es que la de su super tipo. |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Poscondiciones/ subtipos »</summary>

| Poscondiciones/ subtipos |
| ---- |
| Una operación de un subtipo debe tener <b>poscondiciones iguales o más fuertes</b> que la de su super tipo. |

</details>
<br/>

Además de precondiciones y postcondiciones para especificar una operación, es necesario introducir invariantes de clase para expresar las  ropiedades globales de una clase.

<details>
<summary>🗒 Tarjeta: Invariante [de tipo | clase] »</summary>

| Invariante [de tipo/clase] |
| ---- |
| Una **invariante** es una **afirmación** asociada a un **tipo** que debe ser satisfecha por los objetos de ese tipo durante toda su existencia. |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Invariante/ subtipos »</summary>

| Invariante/ subtipos |
| ---- |
| La invariante de un subtipo es la conjunción de las invariante de sus supertipos. |

</details>
<br/>

Una invariante puede ser traducida como una precondición y una postcondición en todas las operaciones.

Estamos en condiciones de hablar acerca de la corrección de un programa orientado a objetos. Como en un programa orientado a objetos sólo hay objetos que se envían mensajes unos a otros, y los mensajes que es posible enviar están definidos en el tipo de esos objetos, podemos decir que la especificación de un programa orientado a objetos es la suma o el conjunto de las especificaciones de los tipos de esos objetos. Por su lado, la especificación de un tipo es el conjunto de precondiciones, postcondiciones, e invariantes asociadas a ese tipo.

<details>
<summary>🗒 Tarjeta: Especificación »</summary>

| Especificación |
| ---- |
| La <b>especificación</b> de un tipo es el conjunto de <b>precondiciones</b>, <b>poscondiciones</b> e <b>invariantes</b> asociadas a ese tipo. |

</details>
<br/>

Podemos decir que una clase que implementa uno o más tipos es correcta, cuando satisface la especificación de todos los tipos que implementa. Como la implementación de un programa orientado a objetos está en sus clases, podemos decir que un programa es correcto cuando todas sus clases son correctas.

<details>
<summary>🗒 Tarjeta: Corrección »</summary>

| Corrección |
| ---- |
| Una <b>clase</b> que implementa uno o más tipos es <b>correcta</b> cuando satisface la <b>especificación</b> de todos los tipos que implementa. |
| Un programa es correct o cuando todas sus clases son correctas. |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Violación de afirmación »</summary>

| Violación de afirmación |
| ---- |
| La <b>violación</b> de una afirmación es la situación a la que se llega en un programa <b>incorrecto</b> cuando <b>no se cumple</b> una precondición, poscondición o invariante. |
| Es producto de un error permanente en el código del programa. |

</details>
<br/>

<br/>

> [4.2 Excepciones »](./4_2_Excepciones.md)

<br/>
