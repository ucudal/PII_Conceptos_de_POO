![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 1. Ciclo de Vida de los Objetos

## 1.2 Desarrollo

Veamos ahora con ejemplos de código lo que presentamos sobre el ciclo de vida de los objeots.

Mira el siguiente ejemplo. Definine una clase `Person` con nombre y apellido. El nombre de la persona se obtiene y se cambia a través de la propiedad `Name` y el apellido a través de la propiedad `FamilyName`. El nombre completo, que consiste en concaternar el nombre y el apello, se obtiene a través de una propiedad de sólo lectura `FullName`.

``` c#
public class Person
{
    private string name;
    private string familyName;

    public Person(string name, string familyName)
    {
        this.Name = name;
        this.familyName = familyName;
    }

    public string Name {
        get
        {
            return this.name;
        }

        set
        {
            this.name = value;
        }
    }

    public string FamilyName
    {
        get
        {
            return this.familyName;
        }

        set
        {
            this.familyName = value;
        }
    }        

    public string FullName
    {   
        get
        {
            return String.Format($"{this.name} {this.familyName}");
        }
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Object_Lifecycle/blob/master/src/Library/Person.cs)

Veamos ahora un ejemplo que usa la clase `Person`.


``` c#
public static void AssignVariableWithObject()
{
    Person lucho;
    lucho = new Person("Luis", "Suárez");
    Console.WriteLine(lucho.FullName);
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Object_Lifecycle/blob/master/src/Program/Program.cs#L29)


La sentencia `Person lucho` define una variable llamada `lucho`capaz que contener instancias de la clase ``Person``.

La variable en este momento está vacía, no tiene ningún objeto y su valor es `null`. Como vimos antes, `null` es una palabra clave en C#, que se usa para indicar que no hay ningún objeto asignado a una variable.

La sentencia que está a la derecha del signo `=` en la sentencia `lucho = new Person("Luis", "Suárez")` crea una nueva instancia de la clase Word mediante el constructor `Person(string,string)`. Cuando usamos este constructor los textos `"Luis"` y `"Suárez"` que se pasan como parámetro son asignados a las propiedades `Name` y `FamilyName` del nuevo objeto creado. Ahora tenemos un objeto de la clase `Person` cuya propiedad `FullName` tiene el valor `"Luis Suárez"`.

La variable `lucho` se define cuando se comienza a ejecutar el método `AssignVariableWithObject` y ocupa espacio suficiente en el **stack** de la memoria como para contener la dirección de memoria que fue asignada al objeto `Person`. Por su lado ese objeto ocupa espacio en la memoria **heap** de tamaño suficiente como para contener las variables de instancia `name` y `familyName`.

Ese objeto recién creado se asigna a la variable `lucho` que está a la izquierda del signo `=`. El signo `=` no se lee como “igual”, sino como “asignar”. Para comparar si dos variables apuntan al mismo objeto el signo es `==`, como veremos más adelante.

La variable `lucho` referencia o apunta al objeto creado. Para acceder al valor de la propiedad `FullName` usamos `lucho.FullName`. Por ejemplo, `Console.WriteLine(lucho.FullName)` imprimirá en la consola `"Luis Suárez"`, que es el resultado de concatenar los valores con los que inicializamos las propiedades `Name` y `FamilyName` al crear el objeto.

Como la variable `lucho` es una variable local definida dentro del método de clase `AssignVariableWithObject` de la clase `Program`, tan pronto como finalice la ejecución de ese método la variable `lucho` deja de ser accesible, es decir, no va a ser posible enviar mensajes o acceder a las propiedades del objeto contenido o referenciado en esa variable. En ese momento, el objeto se destruye<sup>1</sup>.

Es posible destruir el objeto antes. Por ejemplo, si hacemos `lucho = null`, estamos asignando nuevamente el valor `null` a la variable `lucho`. Tampoco vamos a poder enviar mensajes o acceder a las propiedades del objeto que habíamos creado a partir de ese momento, por lo tanto, el objeto se destruye.

``` c#
public static void AssignTwoVariablesWithObject()
{
    Person lavandina;
    lavandina = new Person("Gonzalo", "Bergessio");
    Console.WriteLine(lavandina.FullName);
    
    Person goleador;
    goleador = lavandina;
    Console.WriteLine(goleador.FullName);
    
    Console.WriteLine($"¿Las dos variables apuntan al mismo objeto? {lavandina==goleador}");
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Object_Lifecycle/blob/master/src/Program/Program.cs#L39)

El código es similar al anterior. Además de la variable `lavandina` de tipo `Person` se define una variable `goleador` también de tipo `Person`. En la sentencia `goleador = lavandina` a esa variable `goleador` se le asigna el objeto referenciado en `lavandina`. Hay un solo objeto, el que se creó con `new Person("Gonzalo", "Bergessio")`, pero dos variables apuntan a él.

Esto puede verse claramente cuando accedemos a la propiedad `FullName` del objeto referenciado en la variable `lavandina` con `lavandina.FullName` y luego accedemos a la misma propiedad del objeto referenciado en la variable `goleador`: cuando imprimimos el valor de esa propiedad con `Console.WriteLine(goleador.FullName)` aparece `"Gonzalo Bergessio"` en la consola, a pesar de que enviamos el mensaje al objeto en la variable `goleador` y accedemos a la propiedad del objeto creado en la variable `lavandina`. Ambas variables apuntan al mismo objeto.

Una forma de comprobar esto es comparando los objetos referenciados por esas variables con el operador `==`. Por ejemplo, al ejecutar `Console.WriteLine($"¿Las dos variables apuntan al mismo objeto? {lavandina==goleador}")` se imprimirá `True` en la consola.

Mira ahora este otro ejemplo.

``` c#
public static void CompareEqualObjects()
{
    Person chino;
    chino = new Person("Sergio", "Rochet");
    Console.WriteLine(chino.FullName);

    Person arquero;
    arquero = new Person("Sergio", "Rochet");
    Console.WriteLine(arquero.FullName);

    Console.WriteLine($"¿Las dos variables apuntan al mismo objeto? {chino == arquero}");
    Console.WriteLine($"¿Los dos objetos son iguales? {chino.Equals(arquero)}");
}  
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Object_Lifecycle/blob/master/src/Program/Program.cs#L71)

El ejemplo crea dos objetos `new Person("Sergio", "Rochet")` y los asigna a las variables `chino` y `arquero` respectivamente. Aunque ambos objetos son iguales, porque en ambos casos la propiedad `Name` contiene el valor `"Sergio"` y la propiedad `"FamilyName"` contiene el valor `"Rochet"`, al ejecutar `Console.WriteLine($"¿Las dos variables apuntan al mismo objeto? {chino == arquero}")` se imprimirá `False` en la consola. Esto es porque los objetos referenciados por ambas variables son diferentes, son objetos distintos, cada uno ocupando un espacio diferente en el **heap** de la memoria.

También se imprimirá `False` en la consola al ejecutar `Console.WriteLine($"¿Los dos objetos son iguales? {chino.Equals(arquero)}")`. Aunque conceptualmente estemos hablado de la misma persona, para C# son dos objetos diferentes.

> ⚠️  **Nota**: <br/>
> Si quisiéramos que el método `Equals(object?)` y el operador `==` retornen `True` cuando las propiedades de dos objetos diferentes son iguales, es necesario sobrescribir el método `Equals(object?)` y el operador `==` en la clase de ese objeto. Vamos a ver qué es sobrescribir un método más adelante.
> 
> El código de la clase `Person` en el [repo](https://github.com/ucudal/PII_Object_Lifecycle/blob/master/src/Library/Person.cs) tiene implementados esos métodos pero están comentados. Quita los comentarios y ejecuta el programa para que puedas ver las diferencias.

Veamos ahora que pasa con los parámetros de los métodos. Los parámetros de los métodos se comportan en forma análoga a las variables que declaramos dentro de los métodos: sólo pueden ser usados dentro del método.

Cuando pasamos un objeto como parámetro a un método, en realidad estamos pasando la dirección de memoria del objeto. Vean el siguiente ejemplo:

``` c#

public static void CreateObject()
{
    Person artillero = new Person("Luis", "Artime");
    Console.WriteLine($"Antes de cambiar el nombre: {artillero.FullName}");
    UseObject(artillero);
    Console.WriteLine($"Después de cambiar el nombre: {artillero.FullName}");
}

public static void UseObject(Person person)
{
    person.FamilyName = "Ganador de Copas";
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Object_Lifecycle/blob/master/src/Program/Program.cs#L89)

En el método `CreateObjects()` se crea una instancia de la clase `Person`, que se pasa como parámetro al método `UseObject(Person)`. En el método `UseObject(person)` no se puede acceder a la variable `artillero` declarada en el méotodo `CreateObjects()`, pero sí se puede acceder al objeto que se asignó a esa variable, a través del parámetro `person`. Por ejemplo, se puede cambiar la propiedad `FamilyName` asignado el valor `"Ganador de Copas"`. La variable `artillero` y el parámetro `person` apuntan al mismo objeto. Esto puede comprobarse al imprimir en la consola `Console.WriteLine($"Después de cambiar el nombre: {artillero.FullName}")`: aparece `"Luis Ganador de Copas"` que fue el valor asignado en `UseObject(person)` con `person.FamilyName = "Ganador de Copas"`.

El espacio en la memoria del **stack** para el parámetro `person` se asigna cuando se llama al método `UseObject(person)` y se libera cuando el método termina.

<br/>

> [1.3 Lecturas Sugeridas »](../1_Ciclo_de_vida_de_los_objetos/1_3_Lecturas_Sugeridas.md)

<br/>

*****

_<sup>1</sup> Esto es parcialmente cierto. El proceso de **garbage collection** no se ejecuta inmediatamente sino cada cierto tiempo cuando se cumplen ciertas condiciones_
