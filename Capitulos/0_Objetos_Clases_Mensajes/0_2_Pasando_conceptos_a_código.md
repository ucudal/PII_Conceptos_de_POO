![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 0. Objetos, Clases y Mensajes

## 0.2 Pasando conceptos a código

Ahora vamos a ver cómo se implementan en un lenguaje de programación los conceptos que definimos en el capítulo anterior. En este curso vamos a utilizar un lenguaje llamado C# -que se pronuncia "si yarp"-.

Repasamos la definición de clase:

> **Clase**
>
> Una **clase** es una **plantilla** o **molde** para un conjunto de objetos que comparten los mismos atributos, métodos, relaciones y semántica.

Veamos por ejemplo cómo declaramos una [clase](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/class) `Person` en C# para representar personas:

```c#
public class Person
{
}
```

La palabra clave `public` indica que la clase es visible para todas las demás clases y objetos. Le llamamos "palabra clave" a una palabra que tiene un significado especial en el lenguaje. La palabra clave `class` indica que a continuación aparece la definición de una clase, que es el código que está entre los corchetes `{`  y `}```. La palabra `Person` es el nombre de la clase. No es una palabra clave, sino un indentificador.

Como los objetos representan "cosas", los nombres de las clases son siempre sustantivos. Por [convención](https://docs.microsoft.com/es-es/dotnet/standard/design-guidelines/capitalization-conventions), utilizamos para los nombres de clases una combinación de mayúsculas y minúsculas llamada "Pascal case": una combinación de palabras sin espacios en la que la letra inicial de cada palabra va en mayúsculas; por ejemplo, `PascalCase```.

Aunque no lo hayamos programado, en C# todas las clases tienen un [constructor](https://docs.microsoft.com/es-es/dotnet/csharp/programming-guide/classes-and-structs/using-constructors) predeterminado. Recordemos qué es el constructor:

> **Constructor**
>
> El **constructor** es un método de clase para **crear** e **inicializar** nuevas instancias de esa clase.

Como la clase se llama `Person`, en C# el constructor predeterminado es `Person()`, un método de clase especial cuyo nombre coincide con el nombre de la clase, y no tiene argumentos.

Recordemos qué es un objeto:

> **Objeto**
>
> Un **objeto** es una instancia de una clase.

En C# creamos objetos de la siguiente forma:

```c#
new Person();
```

La palabra clave `new` seguida del constructor de la clase [crea un objeto](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/operators/new-operator) de esa clase.

Tal como está definida, por ahora, esta clase `Person` no tiene estado ni comportamiento.

Recordemos las definiciones de atributo y de estado:

> **Atributo**
>
> Cada dato que un objeto conoce es un **atributo**.

> **Estado**
>
> El **estado** de un objeto son los **datos** y los **valores** de los datos que el objeto **conoce** y están almacenados en los **atributos**.

Digamos que nuestra clase `Person` tiene la responsabilidad de conocer el nombre y el apellido de una persona. La clase debería tener un atributo para representar el nombre -en inglés "name"- y otro para representar el apellido -en inglés "family name"-:

```c#
public class Person
{
    private string name;
    private string familyName;
}
```

Ahora la clase `Person` tiene un atributo `name` y otro `familyName`; ambos son de tipo `string`, es decir, [cadenas de caracteres](https://docs.microsoft.com/es-es/dotnet/csharp/programming-guide/strings/) -en C# es necesario declarar el tipo de los atributos-. Los atributos se declaran dentro de la definición de la clase, es decir, dentro de los corchetes `{`  y `}` que siguen a `class Person`.

Esta forma de declarar atributos en C# es mediante [variables de instancia](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/language-specification/variables#instance-variables). En la declaración anterior de la clase `Person`, `name` es una variable de instancia [privada](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/private) de tipo `string`, y `familyName` es otra variable de instancia también privada del mismo tipo. Veremos más adelante que esta no es la única forma de declarar atributos en C#.

Repasemos la definición de encapsulacion:

> **Encapsulación**
>
> La **encapsulación** es el resultado de ocultar todos los detalles acerca de la implementación de las responsabilidades.

La palabra clave `private` en la declaración de los atributos indica que el atributo es accesible sólo por objetos de la propia clase en la que se define; es decir, el detalle de cómo se implementan las responsabilidades de conocer el nombre y el apellido de las personas está oculto.

La clase `Person` debería permitir obtener y cambiar el nombre y el apellido de la personas. Estas son responsabilidades de hacer o comportamiento. Recordemos la definición de comportamiento:

> **Comportamiento**
>
> Cada objeto puede tener la **responsabilidad** de hacer una parte de la lógica del programa. El **comportamiento** de un objeto son las cosas que el objeto **hace** y está implementado en los **métodos**.

Agreguemos entonces a la clase `Person` los [métodos](https://docs.microsoft.com/es-es/dotnet/csharp/programming-guide/classes-and-structs/methods) que implementan las responsabilidades de obtener el nombre -en inglés "get name"-, cambiar el nombre -en inglés "set name"-, etc.:

```c#
public class Person
{
    private string name;
    private string familyName;
    public string GetName()
    {
        return this.name;
    }
    public void SetName(string value)
    {
        this.name = value;
    }
    public string GetFamilyName()
    {
        return this.familyName;
    }
    public void SetFamilyName(string value)
    {
        this.familyName = value;
    }

}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v1/src/Library/Person.cs)

La palabra clave `string` en `string GetName()` indica que el método retorna una cadena de caracteres -en C# es necesario declarar el tipo del resultado de los métodos-; el identificador `GetName` es el nombre del método; los paréntesis `()` indican que el método no tiene parámetros. Llamaremos "firma de un método" al conjunto de resultado, nombre y parámetros de un método.

La implementación del método es el código que está entre los corchetes `{`  y `}` que siguen a la firma del método. En este caso la implementación consiste de la palabra clave `return`, que indica que el método [retorna](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/return) la expresión que está a continuación, seguida de `this.name` que indica la variable de instancia `name` del objeto que está ejecutando el método; nos referimos al [objeto que está ejecutando](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/keywords/this) el método con la palabra clave `this```.

Como los métodos representan "acciones", los nombres de las métodos son siempre verbos.

La palabra clave `void` en `void SetName(string value)` indica que el método no retorna [ningún valor](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/builtin-types/void); el identificador `SetName` es el nombre del método; este método tiene un [parámetro](https://docs.microsoft.com/es-es/dotnet/csharp/programming-guide/classes-and-structs/passing-parameters) llamado `value` de tipo `string` que va entre los paréntesis `()` que siguen al nombre del método -en C# es necesario declarar el tipo de los parámetros-. En este caso la implementación de ese método consiste en asignar el valor a la variable de instancia `name` del objeto que está ejecutando el método, con el valor del parámetro `value`, mediante el operador `=```.

> :warning: El operador `=` no se lee "igual" sino "asignar", y sirve para "copiar" el valor que está a la derecha a la variable que está a la izquierda.

Vean que todos los métodos en este ejemplo están declarados con la palabra clave `public```, para que otros objetos puedan conocer y cambiar el nombre y apellido de las personas. La forma como eso sucede, es decir, el detalle de implementación de los métodos, es desconocido para los objetos de otras clases, por eso decimos que está encapsulado.

Cuando otro objeto desea crear una persona y darle un nombre, debe hacer lo siguiente:

```c#
Person tota;
tota = new Person();
tota.SetName("Diego");
tota.SetFamilyName("Lugano");
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v1/src/Program/Program.cs)


En este fragmento de código declaramos primero una [variable](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/language-specification/variables#local-variables) llamada `tota` de tipo `Person` -en C# es necesario declarar el tipo de las variables-. A esa variable le asignamos con el operador `=` el objeto creado con `new Person()```.

Luego enviamos al objeto en la variable `tota` el mensaje con selector `SetName("Diego")`. Esto hace que se ejecute el método de nombre `SetName` que vimos antes, pasándole como parámetro el valor `"Diego"`. Como resultado de la ejecución de ese método, según analizamos antes, se asignará a la variable de instancia `name` del objeto contenido en `tota` el valor `"Diego"`. Algo análogo ocurre con el mensaje con selector `SetFamilyName("Lugano")`, que hará que se ejecute el método de nombre `SetFamilyName` con el parámetro `"Lugano"`, que como resultado asignará el valor del parámetro a la variable de instancia `FamilyName` del objeto contenido en `tota```.

Como es muy común que los atributos se implementen mediante variables de instancia privadas y métodos públicos que permiten obtener y cambiar sus valores, C# provee un mecanismo simplificado llamado [propiedades](https://docs.microsoft.com/es-es/dotnet/csharp/programming-guide/classes-and-structs/properties). Veamos como queda la declaración de la clase `Person` utilizando propiedades en lugar de variables de instancia:

```c#
public class Person
{
    private string name;
    private string familyName;
    public string Name { get { return this.name; } set { this.name = value; } }
    public string FamilyName { get { return this.familyName; }  set { this.familyName = value; } }

}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v2/src/Library/Person.cs)

Esta forma de declarar los atributos para el nombre y el apellido de la persona es equivalente a la anterior. Ahora la clase `Person` tiene un atributo `Name` y otro `FamilyName`; ambos son de tipo `string`. Las propiedades se declaran dentro de la definición de la clase, es decir, dentro de los corchetes `{`  y `}` que siguen a `class Person```, al igual que las variables de instancia.

La diferencia es que la implementación de los métodos de la versión anterior ahora está contenida dentro de los corchetes `{`  y `}` que siguen a las palabras claves `get` y `set`. A estas implementaciones les llamamos coloquialmente "getters" y "setters", aunque esas palabras no existen en español.

El fragmento de código que vimos antes para crear una persona y darle un nombre quedaría ahora así:

```c#
Person tota;
tota = new Person();
tota.Name = "Diego";
tota.FamilyName = "Lugano";
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v2/src/Program/Program.cs)

De esta forma podemos utilizar las propiedades de forma equivalente a como usaríamos variables de instancia, pero la forma como se obtiene o modifica el valor de esas variables de instancia, está encapsulada en los "getters" y los "setters" cuya implementación es interna a la clase, al igual que la variables de instancia privada, sólo se puede obtener y cambiar su valor a través esas implementaciones.

Como es muy frecuente que los "getters" y los "setters" obtengan y modifiquen valores de variables de instancias privadas, en versiones más recientes de C# apareció una forma resumida de declarar las propiedades, donde se omite la variable de instancia, y también la implementación de los "getters" y los "setters":

```c#
public class Person
{
    public string Name { get; set; }
    public string FamilyName { get; set; }

}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v3/src/Library/Person.cs)

Desde el punto de vista del código que usa la clase `Person`, esta versión es exactamente igual a la anterior.

El constructor predeterminado permite crear instancias de personas sin nombre ni apellido. Para evitar eso tenemos que crear un constructor que nos pida como parámetro el valor inicial del nombre y del apellido. Cuando creamos un constructor, el constructor predeterminado deja de existir, y sólo podemos utilizar el que definimos, con lo que logramos que todas las personas tengan un nombre y un apellido.

Definimos el constructor así:

```c#
public class Person
{
    ...
    public Person(string name, string familyName)
    {
        this.Name = name;
        this.FamilyName = familyName;
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v4/src/Library/Person.cs)

Para crear objetos de la clase `Person` ahora tenemos que usar el nuevo constructor, el fragmento de código que veníamos usando quedaría así:

```c#
Person tota;
tota = new Person("Diego", "Lugano");
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v4/src/Program/Program.cs)

<br/>

> [0.3 Desarrollo »](./0_3_Desarrollo.md)

<br/>
