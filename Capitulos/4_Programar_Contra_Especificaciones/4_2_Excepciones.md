![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 4. Programar contra especificaciones

## 4.2 Excepciones

La lógica de un programa la implementamos creando objetos, enviado mensajes a objetos creados, y utilizando estructuras de control de flujo para determinar qué mensajes enviar y a qué objetos. Al enviar un mensaje esperamos que cambie el estado del programa de forma determinada por el comportamiento del método que se ejecuta como resultado de la recepción del mensaje, que está definida en las postcondiciones de la operación que ese método implementa; si eso no sucede, entonces estamos ante una situación desconocida, que típicamente llamamos error.

Más formalmente, definimos los siguientes términos (basado en IEEE 610.12 1990):
- Una acción humana que produce un resultado incorrecto. El término más preciso para este significado es equivocación *(mistake)*.

- Una definición incorrecta en un paso, un proceso, o un dato; un defecto en un dispositivo o componente. El término más preciso para este significado es falta *(fault)*: la manifestación de una equivocación.

- Un comportamiento o resultado incorrecto. La incapacidad de un sistema o componente de realizar las funciones requeridas con el desempeño especificado. El término más preciso es falla *(failure)*: el resultado de una falta.

- La diferencia entre un valor o condición calculada, observada, o medida, y el valor o condición
verdadera, especificada, o teórica. El término adecuado es error: la cantidad por la que el resultado
es incorrecto.

Una equivocación puede derivar en un error. Cuando aparece un error en nuestro programa y como programadores creemos que la equivocación es de quién interactúa con nuestro programa, por ejemplo, el usuario final u otro programa, podemos tratar de manejar ese error y tratar de subsanar la equivocación.

Piensen, por ejemplo, en el caso que nuestro programa tenga que usar un archivo cuya ubicación ingresa el usuario. El usuario puede cometer una equivocación al ingresar la ubicación del archivo y nuestro programa tendrá una falla al intentar abrirlo<sup>1</sup>. Pero si le damos una nueva oportunidad al usuario, podría ingresar esta vez la ubicación correcta, y nuestro programa sí podría abrirlo. O el usuario desiste de abrir el archivo, y nuestro programa termina como se espera.

También puede aparecer un error en nuestro programa que inadvertidamente es una equivocación nuestra como programadores, o de otro programador que programó una clase que nosotros usamos<sup>2</sup>. En este caso, a diferencia del anterior, no es una situación que se pueda resolver, y nuestro programa no puede continuar: ocurre una excepción.

La decisión de si un error es una excepción o no depende del contexto, la toma el objeto que envía el mensaje. Siguiendo con el ejemplo de un programa que usa un archivo, la clase que usamos para abrirlo va a generar una excepción si el archivo no existe, nuestro programa es el que decide pedir nuevamente su ubicación.

<details>
<summary>🗒 Tarjeta: Excepción »</summary>

| Excepción |
| ---- |
| Una excepción es una situación inesperada en un programa. |
| No necesariamente es un error en el programa. |
| No necesariamente es un error permanente. |

</details>
<br/>

Podríamos decir que todas las excepciones son violaciones de precondiciones, postcondiciones o invariantes. Lo que sucede es que a veces es impráctico especificar absolutamente todas las precondiciones, postcondiciones o invariantes, por eso se controla solamente la corrección de una parte del código con excepciones. Otro aspecto es que a veces es necesario detectar una excepción para poder manejarla, como en el caso del
archivo que no existe del ejemplo anterior.

Las excepciones en C# y en otros lenguajes son instancias de una clase **Exception** o de una clase sucesora de **Exception**. Para crear una excepción se utiliza la palabra clave **new** al igual que para crear una instancia de cualquier otra clase; pero para lograr que esa instancia sea interpretada como una excepción, se utiliza la palabra clave **throw**. Las excepciones se usan con estructuras de control de código llamadas bloques **try** debido a la palabra clave con la que se definen esos bloques.

Un bloque **try** puede ser seguido por un bloque catch, por un bloque finally, o por ambos. Es una estructura de control porque el código que se ejecuta cambia si ocurre una excepción dentro del bloque try:

- El bloque **catch** se ejecuta si y solo si ocurre una excepción; puede haber más de un bloque **catch** para diferentes tipos de excepciones.

- El bloque **finally** se ejecuta siempre, independientemente que haya ocurrido una excepción o no; esto es útil para asegurar que recursos que hayan sido obtenidos en el bloque **try** sean liberados en caso de error.

Vean por ejemplo la siguiente clase **Song** que tiene un atributo **Genre** que representa el género de la canción; si considero que no puede haber canciones del género **"Tecno"** porque no me gusta ese género, puedo levantar una excepción en el constructor de esa clase de esta forma:

```c#
public class Song : Media
{
    …
    public Song(string name, string genre, string artist): base(name, genre)
    {
        if (string.Equals(genre, "tecno", StringComparison.OrdinalIgnoreCase))
        {
            throw new LibraryException($"No me gusta {name} porque no me gusta la música tecno");
        }
        this.artist = artist;
    }
    …
}
```

La clase LibraryException es un sucesor de Exception. Habitualmente se declara un tipo de excepción para las situaciones que no estén contempladas en la jerarquía de clases existente<sup>4</sup>.

En caso de crear una instancia de **Song** con genero **"Tecno"** fuera de un bloque **try**, el programa termina:

```c#
public class Program
{
    public static void Main(string[] args)
    {
        Song song = new Song("Windowlicker", "Tecno", "Alphex Twin");
        …
    }
}
```
El resultado en la consola es el siguiente:
```bash
Unhandled Exception: Exceptions.LibraryException: No me gusta Windowslicker porque no me gusta la música tecno
    at Exceptions.Song..ctor(String name, String genre, String artist) in C:\PII_Exceptions\src\Library\Song.cs:line 34
    at Exceptions.Program.main(String[] args) in C:\PII_Exceptions\src\Program\Program.cs:line 25
```

Para evitar que el programa termine, es necesario manejar la excepción, es decir, incluir el código que puede levantar excepciones en un bloque **try**. En el ejemplo a continuación, el bloque **catch** es usado para mostrar un mensaje de error más amigable:

```c#
public class Program
{
    public static void Main(string[] args)
    {
        try
        {
            Song song = new Song("Windowlicker", "Tecno", "Alphex Twin");
        }
        catch (System.Exception)
        {
            Console.WriteLine("No me gusta la música tecno");
        }
    }
}
```
En este caso el resultado es:
```bash
No me gusta la música tecno
```

Si los datos de la canción fueran ingresados por el usuario, se podría usar el bloque catch para que el usuario ingrese otra canción en el ejemplo el usuario ingresa sólo el género por simplicidad:

```c#
public class Program
{
    …
    public static void Main(string[] args)
    {
        bool keepAsking;
        string genre;
        do
        {
            keepAsking = false;
            Console.Write("Ingrese el género: ");
            genre = Console.ReadLine();
            try
            {
                Song song = new Song("Windowlicker", genre, "Alphex Twin");
            }
            catch (System.Exception)
            {
                Console.WriteLine("No me gusta la música tecno");
                keepAsking = true;
            }
        } while (keepAsking);
    }
    …
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Program/Program.cs)

Ahora debo indicar el género en la consola, si escribo “Tecno” el programa continúa pidiendo el género hasta que ingreso un género diferente:
```bash
Ingrese el genéro: Tecno
No me gusta la música Tecno
Ingrese el genéro: Tecno
No me gusta la música Tecno
Ingrese el genéro: Rock 
```


<br>

> [4.3 Desarrollo »](./4_3_Desarrollo.md)

</br>

****

_<sup>1</sup> Una precondición para abrir un archivo es que el archivo exista._

_<sup>2</sup> No se cumple una postcondición._

_<sup>3</sup> Vean https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/try-catch-finally._

_<sup>4</sup> Vean https://docs.microsoft.com/en-us/dotnet/api/system.exception?view=netframework-4.8&viewFallbackFrom=netcore-3.1._