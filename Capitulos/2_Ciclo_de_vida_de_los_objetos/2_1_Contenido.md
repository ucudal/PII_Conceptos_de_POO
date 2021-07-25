![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 2. Ciclo de vida de los objetos

## 2.1 Contenido



Ya sabes qué son las clases de objetos y cómo definir clases de objetos. También sabes crear objetos que son instancias de esas clases. Ahora veremos qué pasa desde que creas un objeto hasta que desaparece. Probablemente te preguntes, ¿cómo, los objetos desaparecen? 🤔

Correcto, los objetos se crean y en algún momento desaparecen, a eso le llamamos el ciclo de vida de un objeto.

<details>
<summary>🗒 Tarjeta: Ciclo de vida »</summary>

| Tipo |
| ---- |
| El ciclo de vida de un objeto va desde que se asigna un bloque de memoria a este objeto durante algún proceso de ejecución y hasta que ese bloque de memoria se libera cuando el proceso finaliza. |

</details>
<br/>


Recuerda que la clase de un objeto es como una plantilla o molde que describe las propiedades de los objetos de esa clase. Cuando sea crea un objeto con la palabra clave ```new``` suceden tres cosas:

1.	Se crea un bloque de memoria de tamaño suficiente como para almacenar los valores de todas las propiedades de ese objeto

2.	Se invoca al constructor de la clase de ese objeto

3.	Se retorna la dirección del bloque de memoria creado, que típicamente se almacena en una variable de un método, o en una propiedad de otro objeto o clase


Mientras que los objetos creados ocupan una parte de la memoria llamada **heap** o **montículo**, las variables ocupan otra parte de la memoria llamada **stack** o **pila**.

Las variables que se definen para contener o referenciar objetos dentro de un método existen solamente mientras se ejecuta ese método. El espacio de memoria ocupado por esas variables -suficiente como para contener una dirección de memoria por cada variable- es reservado en el **stack** o **pila** cuando se declaran esas variables. Cuando el método termina, el espacio ocupado por las variables se libera, porque las variables definidas dentro un método no pueden ser accedidas fuera de ese método.

<details>
<summary>🗒 Tarjeta: Pila y variables »</summary>

| Stack o pila |
| ---- |
| El **stack** o **pila** es el espacio de memoria donde se almacenan las variables definidas dentro un método y los parámetros de ese método |

</details>
<br/>

<details>
<summary>🗒 Tarjeta: Montículo y objetos »</summary>

| Heap o montículo |
| ---- |
| El **heap** o **montículo** es el espacio de memoria donde se almacenan los objetos creados |

</details>
<br/>

> Por simplicidad estamos asumiendo que todas las variables son refrencias a objetos. En realidad, también hay variables que pueden contener valores, típicamente de tipos de datos simples como valores lógicos, números enteros, caracteres, etc. Para obtener más información sobre la diferencia entre ambos consulta [tipos de datos por referencia](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/reference-types) y [tipos de datos por valor](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/builtin-types/value-types).

La memoria no es infinita; cada vez que se asigna memoria a un objeto, es necesario devolverla cuando ese objeto ya no pueda ser utilizado. De lo contrario, se producirían **pérdidas de memoria** o **memory leaks**.

El ciclo de vida de los objetos es manejado por el **runtime** o **ambiente de ejecución** y depende del lenguaje de programación.

> En el caso de C# ese ambiente de ejecución es el **CLR** o **Common Language Runtime**, en el caso de Python es el **intérprete de Python**, en el caso de Java es la **JVM** o **Java Virtual Machine**, etc.

El **runtime** es una máquina virtual donde se ejecuta el programa. Esta máquina virtual convierte las sentencias de tu programa en instrucciones de código de máquina que pueden ser ejecutadas por el procesador. Además, gestiona el ciclo de vida de los objetos: cuando creas un objeto, el **runtime** utiliza servicios del sistema operativo para asignar un espacio de memoria en el **heap**, y la dirección de ese espacio de memoria se guarda en una variable que está en otro espacio de memoria en el **stack**.


<details>
<summary>🗒 Tarjeta: Asignación de variables »</summary>

| Asignación de variables |
| ---- |
| Cuando se asigna una variable con el valor de otra variable que referencia un objeto se copia la dirección de memoria en el **heap** de ese objeto. Luego de la asignación las dos variables que apuntan a la misma dirección de memoria. |

</details>
<br/>

Cuando un método termina, se libera el espacio de memoria en el **stack** ocupado por las variables definidas en ese método. Cuando todas las variables que referencian a un objeto son liberadas, ese objeto no podrá ser accedido -no es posible enviarle mensajes o acceder a sus propiedades-, y el espacio de memoria en el **heap** ocupado por el objeto puede ser liberado. También puede ser liberado el espacio en el **heap** cuando todas las variables que referencian a un objeto tienen el valor ```null```.

<details>
<summary>🗒 Tarjeta: Nulos »</summary>

| Nulos |
| ---- |
| ```null``` es una palabra clave en C# utilizada para indicar que el valor de una referencia a un objeto es nulo. Es equivalente a la palabra clave ```None``` de Python y al literal ```null``` de Java. |

</details>
<br/>

El proceso de liberar la memoria en el **heap** cuando los objetos desaparecen es realizado automáticamente por el **runtime** y se llama **garbage collection**. En la mayoría de los casos es transparente para el programador.

Cuando se destruye un objeto suceden dos cosas:

1.	Se invoca un método especial llamado finalizador o destructor. Así como todos los objetos tienen un método constructor definido en la clase, aunque nosotros no lo programemos, todos los objetos tienen también un método finalizador o destructor.

2.	Se libera la memoria ocupada por el objeto, es decir, se retorna para que pueda ser utilizada más adelante cuando se creen otros objetos.

> En C# método constructor tiene el mismo nombre que la clase, mientras que el método finalizador o destructor tiene el nombre de la clase precedido por el símbolo ```~```. Vean más información [aquí](https://docs.microsoft.com/es-es/dotnet/csharp/programming-guide/classes-and-structs/destructors).

Todos los recursos que un objeto consuma en el constructor -abrir archivos, conexiones de red, conexiones a bases de datos, etc.- deben ser liberados en el destructor -cerrar archivos, conexiones, etc.-.

Más adelante, cuando hablemos de [excepciones](https://github.com/ucudal/PII_Conceptos_de_POO/blob/master/Capitulos/4_Programar_Contra_Especificaciones/4_2_Excepciones.md), veremos que es importante asegurar que todos los recursos consumidos sean liberados, usando la cláusula [try…finally](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/try-finally), o la cláusula [using](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/using-statement).  
