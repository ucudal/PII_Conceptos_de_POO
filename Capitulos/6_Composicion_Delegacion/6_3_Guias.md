[Conceptos de Programación Orientada a Objetos](../../)

# 6. Composición - Delegación

## 6.3 Guías para la asignación de responsabilidades

Vimos antes que es posible componer un objeto con otro para implementar una
responsabilidad. El tipo compuesto implementa un tipo delegando en los métodos
correspondientes a las operaciones en el tipo al objeto componente.

También vimos que el objeto compuesto no necesita conocer la clase del objeto
componente, sino solamente el tipo. Esto permite componer el objeto compuesto
con instancias de diferentes clases del objeto componente, aún en tiempo de
ejecución. Como estos objetos componentes pueden ser polimórficos, al cambiar el
objeto componente, estamos de hecho cambiando tamibén el objeto compuesto. Esto
otorga una gran flexibilidad, manteniendo las demás propiedades deseables de un
programa orientado a objetos como la encapsulación, la cohesión y el
acoplamiento, amén de cumplir con las guías de diseño orientado a objetos que
hemos hasta ahora y otras que veremos más adelante.

A continuación veremos dos nuevas guías para la distribución de
responsabilidades relacionadas con los conceptos de composición y delegación.

### Inversión de dependencias

En principio de inversión de dependencias fue introducido por Robert C. Martin
en [^1].

[^1]: Martin, Robert, The Dependency Inversion Principle, The C++ Report, Junio 1996.

Martin dice que una de las causas de los malos diseños son las interdependencias
indeseadas entre clases[^2]. Eso provoca:

[^2]: En la redacción original de Robert C. Martin, habla de dependencias entre
    módulos en general; aquí lo vemos aplicado a clases.

* Que el diseño sea difícil de cambiar, porque cada cambio afecta muchas partes
  del programa -rigidez-.

* Cuando se hace un cambio, partes inesperadas del programa pueden dejar de
  funcionar -fragilidad-.

* Es más difícil usar clases de un programa en otro, porque no se pueden
  “desenredar” del programa actual -inmovilidad-.

Para evitar esos problemas, propone diseñar en base a **abstracciones**; en este
contexto, abstracciones son tanto **clases abstractas** como **interfaces**.

> Las clases[^2] de alto nivel no deben depender de clases de bajo nivel; ambas
> deben depender de abstracciones.
>
> Las abstracciones no deben depender de detalles; los detalles deben depender
> de abstracciones

Lo que esta guía nos está diciendo es que cuando una clase dependa de otra, por
ejemplo, una clase está compuesta por otra, modifiquemos nuestro diseño para que
no depende de la otra clase, sino del tipo que debe tener la otra clase. En el
contexto de composición y delegación, la guía nos indica que la clase compuesta
debe conocer solamente el tipo del objeto compuesto, no debe saber de qué clase
es el objeto.

> [!IMPORTANT]
> Aunque presentamos esta guía ahora que estamos viendo composición
> y delegación, la guía es aplicable a otros escenarios en los que haya
> dependencias entre clases, no sólo entre compuesto y componente.

> [!TIP]
> Lee más sobre el principio de inversión de dependencias
> [aquí](https://github.com/ucudal/PII_Guias/blob/main/DIP.md).

### Creator

En el contexto de composición y delegación, podemos preguntarnos quién crea el
objeto componente. Típicamente es el objeto compuesto, pero no necesariamente es
así. De hecho, esta guía es útil para determinar qué clases tienen la
responsabilidad de crear objetos de otras clases.

#### Problema

Todos los objetos deben ser creados para poder enviarles mensajes que hagan uso
de sus responsabilidades de hacer y de conocer.

¿Quién debería ser responsable de crear una nueva instancia de cierta clase?

#### Solución

> Asignar a la clase B la responsabilidad de crear una instancia de la clase A
> si una de las siguientes condiciones es verdadera:
>
> * B agrega objetos A
> * B contiene objetos A
> * B guarda instancias de objetos A
> * B usa de forma muy cercana objetos A
> * B tiene los datos que serán provistos al constructor para inicializar
> instancias de A por lo que B es un experto con respecto a crear A.

Entonces B es un creador de objetos A. Cuando se pueda aplicar más de una alternativa, prefieran la clase B que agrega o contiene instancias de A.

En cualquiera de las opciones anteriores la clase B tiene una variable de instancia de A para referenciar objetos de esa clase, o tiene un contener capaz de almacenar instancias de A.

> [!TIP]
> Lee más sobre Creator
> [aquí](https://github.com/ucudal/PII_Guias/blob/main/Creator.md).

<br>

> [6.4 Lecturas sugeridas »](./6_4_Lecturas_Sugeridas.md)

</br>
