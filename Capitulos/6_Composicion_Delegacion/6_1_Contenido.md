![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 6. Composición - Delegación

## 6.1 Contenido

Puede llegar a haber métodos iguales o muy parecidos en una clase que estamos programando y en otra ya existente. Esto puede llegar a ocurrir en clases que no tienen relación, pero es muy común en todas las clases que implementan un mismo tipo. Debería ser posible usar los métodos existentes en las clases ya programadas, para no hacer el trabajo de nuevo. ¿Cómo uso los métodos de una clase que implementa un tipo en otra clase que implementa el mismo tipo?

Agrego a los objetos de la clase que debe implementar el tipo, objetos de una segunda clase que ya tenga el tipo implementado, o creo una segunda clase sólo a efectos de contener esos métodos; esto es llamado **composición**.

<details open>
<summary>🗒 Tarjeta: Composición ±</summary>

| Composición |
| ---- |
| La composición es una asociación fuerte entre una clase compuesta y una clase componente en la que instancias de la clase componente no suelen existir independiente de instancias de la clase compuesta. |

</details>
<br/>

Cada método de la primera clase envía un mensaje al objeto de la segunda; esto es conocido como **delegación**.

<details open>
<summary>🗒 Tarjeta: Delegación ±</summary>

| Delegación |
| ---- |
| La delegación es un mecanismo en programación por el cual cuando un objeto recibe un mensaje para realizar una operación, no la realiza él mismo, si no que la encarga a otro objeto. |
| El objeto que recibe el mensaje es un objeto compuesto, y el otro objeto al que se le delega el mensaje es un objeto componente |

</details>
<br/>

La clase compuesta sólo necesita conocer el tipo de la clase componente para poder hacer la delegación. La composición y delegación permiten reutilizar código basándose exclusivamente en los tipos de los objetos.

<details open>
<summary>🗒 Tarjeta: Composición y reutilización ±</summary>

| Composición y reutilización |
| ---- |
| La composición y delegación es una forma de reutilización de código pues permite crear nuevas clases a partir de clases existentes. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Composición vs. herencia ±</summary>

| Composición vs. herencia |
| ---- |
| La composición y delegación es una alternativa a la herencia. |
| En el contexto de la reutilización toda implementación que use herencia se puede cambiar por una equivalente que use composición y delegación. |

</details>
<br/>

Es posible cambiar el comportamiento de una clase compuesta, cambiando la clase componente. Esto se puede lograr aún dinámicamente.

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
| La composición y delegación permiten reutilizar estado y comportamiento en forma selectiva | La herencia reutiliza todo, no puede elegir qué heredar y qué no<sup>1</sup>. |
| La composición y delegación puede reutilizar más de una clase simultáneamente | La herencia simple solo permite reutilizar una clase a la vez; la herencia múltiple permite reutilizar más de una clase simultáneamente<sup>2</sup> |
| La composición y delegación no impone ninguna relación entre los tipos de la clase compuesta y  las clases componentes; la clase compuesta puede  implementar varios tipos | La herencia impone una relación de supertipo/subtipo entre la clase base y la clase sucesora; ésta tiene  al menos el tipo de aquélla, aunque puede implementar varios tipos más |

La composición reduce las dependencias indeseables, porque require sólo de la interfaz pública dada por los tipos de los objetos y no depende de detaller internos de implementación. La composición también favorece el diseño clases encapsuladas y enfocadas a una sola tarea; como consecuencia, las jerarquías de clases se mantienen razonablemente reducidas y manejables.

Por estas razones decimos que la composición favorece la modularidad. La modularidad es la propiedad de un sistema que ha sido descompuesto en un conjunto de módulos altamente cohesivos y poco acoplados.

<details open>
<summary>🗒 Tarjeta: Cohesión ±</summary>

| Cohesión |
| ---- |
| La cohesión es la forma y el grado en el que las responsabilidades de una clase o de las clases contenidas en un paquete están relacionas unas con otras. |
| Cuando la cohesión es alta es mejor. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Acoplamiento ±</summary>

| Acoplamiento |
| ---- |
| El acoplamiento es la forma y el grado de interdependencia entre clases y entre paquetes. |
| Cuando el acoplamiento es bajo es mejor. |

</details>
<br/>

<details open>
<summary>🗒 Tarjeta: Modularidad ±</summary>

| Modularidad |
| ---- |
| La modularidad es una propiedad de las clases y paquetes cuando son altamente cohesivos y están poco acoplados. |

</details>
<br/>


<br/>

> [6.2 Presentación »](./6_2_Presentacion.md)

<br/>

****

_<sup>1</sup> En algunos lenguajes de programación, la herencia es selectiva, la clase sucesora puede decidir no heredar algunos atributos o métodos. Esto implica que la relación entre clase y subclase no necesariamente es también una relación entre tipo y subtipo, lo que conduce a la violación del principio de sustitución. Lo consideramos una anomalía y no debe ser tenido en cuenta._

_<sup>2</sup> Lamentablemente son pocos los lenguajes que implementan herencia múltiple._
