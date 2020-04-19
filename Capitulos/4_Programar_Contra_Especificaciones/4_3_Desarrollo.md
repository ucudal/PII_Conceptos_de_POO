![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 4. Programar contra especificaciones

## 4.3 Desarrollo

Recordemos lo que ya hemos visto de tipos. Los tipos permiten a un objeto saber los mensajes que pueden enviar otro; el emisor tiene la seguridad que el receptor procesará el mensaje. Veamos nuevamente el ejemplo de la interfaz **ILikeable** y las clases **Media**, **Movie** y **Song**.

```c#
public interface ILikeable
{
    int Likes { get; }
    void Like();
    void Unlike();
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/ILikeable.cs)

```c#
public abstract class Media : ILikeable
{
    private readonly string name;
    private string genre;
    
    public Media(string name, string genre)
    {
        this.name = name;
        this.genre = genre;
    }

    public string Name 
    { 
        get
        {
            return this.name; 
        } 
    }

    public string Genre 
    { 
        get 
        { 
            return this.genre; 
        } 
    }
    
    public int Likes 
    {
        get;
        protected set 
    } 

    public virtual void Like()
    {
        this.likes = this.likes + 1;
    }

    public virtual void Unlike()
    {
        if (this.likes > 0)
        {
            this.likes = this.likes - 1;
        }
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/Media.cs)

```c#
public class Song : Media
{
    private string artist;

    public Song(string name, string genre, string artist)
    : base(name, genre)
    {
        if (string.Equals(genre, "tecno", StringComparison.OrdinalIgnoreCase))
        {
            throw new LibraryException($"No me gusta {name} porque no me gusta la música tecno");
        }
        this.artist = artist;
    }

    public string Artist 
    { 
        get 
        { 
            return this.artist; 
        } 
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/Song.cs)

Una clase **Library** puede usar una **Movie** o una **Song** sabiendo solamente que es **ILikeable**. Vean por ejemplo el método Play de **Library** que aumenta la cantidad :+1: de de una obra cada vez que se reproduce, enviando un mensaje con selector Like cuya firma está definida en **ILikeable**:

```c#
public class Library : IEnumerable<Media>
{
    …
    public void Play(string name)
    {
        Media mediaToPlay = this.GetFirstByName(name);
        if (mediaToPlay == null)
        {
            throw new LibraryException($"No existe una obra llamada {name}");
        }
        this.playingMedia = mediaToPlay;
        this.playingMedia.Like();
    }
    …
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/Library.cs)

Cuando por ejemplo un objeto de la clase **Song** en la biblioteca es reproducido y recibe un mensaje con selector Like(), como la clase Song es sucesora de la clase **Media** que implementa ese método, aumenta la en uno propiedad Likes de ese objeto. Vean la siguiente **Crazy** que también es sucesora de **Media** como **Song** y **Movie**:

```c#
public class Crazy : Media
{
    private Random likesGenerator = new Random();

    public Crazy(string name, string genre)
    : base(name, genre)
    {
        // Intencionalmente en blanco
    }

    public override void Like()
    {
        this.Likes = this.likesGenerator.Next();
    }

    public override void Unlike()
    {
        this.Likes = this.likesGenerator.Next();
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/Crazy.cs)

¿Puedo agregar instancias de **Crazy** a la biblioteca, y usar todas las operaciones de **Library** como `GetFavorites()`? Claro que sí. Como **Crazy** es sucesora de la clase **Media**, podemos usar un objeto de la clase **Crazy** cada vez que **Library** espere un objeto de la clase **Media**.

¿Funciona igual que una instancia de **Song** o de **Movie**? Definitivamente no. Cuando la biblioteca le envía un mensaje Like() a una instancia de **Crazy**, la cantidad de :+1: no necesariamente aumenta en uno, porque se asigna a la propiedad Likes un valor aleatorio.

¿Puede la clase **Library** asumir que al enviar un mensaje Like() la cantidad de :+1: de un objeto de tipo **Crazy** aumentará en uno? Resulta obvio y evidente que eso debe hacer a partir del nombre del método, pero para ser sinceros, no está dicho en ningún lado. El tipo **Media** dice los mensajes que pueden enviar los objetos y los métodos que deben proveer las clases **Song**, **Movie** y **Crazy**; sin embargo, no dice qué deben hacer esos métodos, solamente cómo deben ser los mensajes.

¿Está correctamente programada la clase **Crazy**? No hace lo que esperamos que haga ¿cierto? Falso. No hemos dicho lo que esperamos que hagan los métodos Like() ni Unlike(), así que pueden hacer cualquier cosa y estará bien. La clase **Crazy** es tan correcta como las clases **Song** o **Media**.

El problema es justamente ese, que no hemos dicho lo que esperamos que hagan esos métodos. Si hubiéramos dicho que el método Like() debía aumentar la cantidad de :+1: en uno, Song y Movie serían correctas y **Crazy** no. Pero si hubiéramos dicho que el método Like() debe cambiar en forma aleatoria la cantidad de :+1:, **Crazy** sería correcta y Song o Movie no. Cuando decimos lo que debe hacer un método, estamos dando la **especificación** del comportamiento del método. La **corrección** es un concepto relativo, relativo a la especificación. Si no hay especificación, no tiene sentido hablar de corrección; cualquier cosa estará bien.

Una fórmula de corrección para una operación A viene dada por la expresión {P} A {Q} que se lee “cualquier ejecución de A, que comience en un estado donde P se cumple, termina en un estado donde Q se cumple”. P y Q son afirmaciones. Las afirmaciones incluyen:

- Precondiciones

- Postcondiciones

- Invariantes de clase

- Invariantes de bucle

Sería una excelente idea agregar el concepto de corrección a los tipos dando, además de la firma de las operaciones, la declaración de sus respectivas precondiciones y postcondiciones, más las invariantes de clase.

Algunos lenguajes soportan en su sintaxis especificar precondiciones, postcondiciones o invariantes en las operaciones de los tipos. El lenguaje de programación Eiffel<sup>1</sup> es uno de los pioneros en esa capacidad. En el caso de lenguajes como C# o Java el soporte es provisto por terceras partes y suele tener algunas limitaciones.

> **Acerca de Contracts en .NET Framework**
>
> El espacio de nombre **System.Diagnostics.Contracts** en .NET Framework provee una clase **Contract** para implementar diseño por contrato. Aunque todavía es posible utilizar ese espacio de nombres, la forma de asociar contratos de tipos declarados en interfaces es compleja, puesto que las interfaces sólo incluyen la declaración de las operaciones y no el código<sup>2</sup>; en el caso de .NET Core, los contratos no son traducidos al código ejecutable, por lo tanto, son análogos a comentarios.

Entre las limitaciones del uso de diseño por contrato en lenguajes como C# o Java encontramos:

- Las precondiciones, postcondiciones, e invariantes son declarativas, es decir, simples enunciados con un valor de verdad. Lo que podemos hacer en C# es siempre imperativo, es decir, ejecutable; esto quiere decir que es posible cambiar inadvertidamente el estado que se evalúa en la propia afirmación, ya que las afirmaciones son instrucciones que se ejecutan al evaluarla.

- El código para controlar las precondiciones, postcondiciones e invariantes debe estar incluido en el cuerpo de los métodos de en una clase; no es posible incluirlas en las operaciones de en una interfaz, como sería deseable.

Sin embargo, aún con esas limitaciones, el uso de afirmaciones mejora la calidad del código y son una buena práctica de programación; a continuación, podemos ver cómo emular el diseño por contrato con las herramientas que conocemos. 

Como la violación de una precondición, postcondición, o invariante es una excepción, es decir, el programa llega a un estado que no es correcto de acuerdo con la especificación dada por esos contratos, podemos utilizar excepciones para señalar la situación.

Por ejemplo, las precondiciones que requieren que el nombre, el género y el artista de una canción no pueden ser **null** ni estar vacíos en el momento de crear una nueva instancia de **Song**, se puede representar con excepciones así:

```c#
public class Song : Media
{
    private string artist;

    public Song(string name, string genre, string artist)
    : base(name, genre)
    {
        if (string.IsNullOrEmpty(name))
        {
            throw new ArgumentNullException(name);
        }

        if (string.IsNullOrEmpty(genre))
        {
            throw new ArgumentNullException(name);
        }

        if (string.IsNullOrEmpty(name))
        {
            throw new ArgumentNullException(name);
        }

        if (string.Equals(genre, "tecno", StringComparison.OrdinalIgnoreCase))
        {
            throw new LibraryException($"No me gusta {name} porque no me gusta la música tecno");
        }
        this.artist = artist;
    }
    …
}
```

Noten que el código relacionado con las precondiciones es poco compacto muy extenso. Introducimos una clase “ayudante” **Check** con operaciones para controlar que se cumplan las afirmaciones asociadas a precondiciones, postcondiciones e invariantes, más clases sucesoras de **Exception** para representar la violación de esas afirmaciones, **PreconditionException**, **PostconditionException** e **InvariantException** respectivamente.

```c#
public class Check
{
    public class PreconditionException : Exception
    {
        public PreconditionException() : base() { }
        public PreconditionException(string message) : base(message) { }
    }

    public class PostconditionException : Exception
    {
        public PostconditionException() : base() { }
        public PostconditionException(string message) : base(message) { }
    }

    public class InvariantException : Exception
    {
        public InvariantException() : base() { }
        public InvariantException(string message) : base(message) { }
    }

    public static void Precondition(bool condition, string message)
    {
        if (!condition)
        {
            throw new PreconditionException(message);
        }
    }

    public static void Precondition(bool condition)
    {
        Precondition(condition, "Precondición insatisfecha");
    }

    public static void Postcondition(bool condition, string message) { … }
    public static void Postcondition(bool condition) { … }
    public static void Invariant(bool condition, string message) { … }
    public static void Invariant(bool condition ) { … }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Check/Check.cs)

El constructor de Song, con esta nueva clase Check, quedaría así:

```c#
public class Song : Media
{
    private string artist;

    public Song(string name, string genre, string artist)
    : base(name, genre)
    {
        Check.Precondition(!string.IsNullOrEmpty(name));
        Check.Precondition(!string.IsNullOrEmpty(genre));
        Check.Precondition(!string.IsNullOrEmpty(artist));
        
        if (string.Equals(genre, "tecno", StringComparison.OrdinalIgnoreCase))
        {
            throw new LibraryException($"No me gusta {name} porque no me gusta la música tecno");
        }

        this.artist = artist;
        Check.Invariant(this.Likes >= 0,"La cantidad de me gusta es siempre mayor o igual que cero");
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/Song.cs)

Esta forma de escribir las precondiciones, postcondiciones e invariantes es más compacta, pero no resuelve varias de las limitaciones que mencionábamos antes: no se pueden usar en interfaces y no se pueden controlar estáticamente. 

Veamos la versión modificada de la clase **Media** y sus métodos Like() y Unlike() a continuación:

```c#
public abstract class Media : ILikeable
{
    public virtual void Like()
    {
        Check.Invariant(this.Likes >= 0,"La cantidad de me gusta es siempre mayor o igual que cero");
        var oldLikes = this.Likes; 
        // Necesario para controlar la postcondición
        this.Likes = this.Likes + 1;
        Check.Postcondition(this.Likes == oldLikes + 1,"Debe haber un me gusta más");
        Check.Invariant(this.Likes >= 0,"La cantidad de me gusta es siempre mayor o igual que cero");
    }

    public virtual void Unlike()
        {
        Check.Invariant(this.Likes >= 0,"La cantidad de me gusta es siempre mayor o igual que cero");
        var oldLikes = this.Likes; 
        // Necesario para controlar la postcondición
        if (this.Likes > 0)
        {
            this.Likes = this.Likes - 1;
        }
        Check.Postcondition(oldLikes > 0 
            ? this.Likes == oldLikes - 1 
            : this.Likes == oldLikes,"Debe haber un me gusta menos si había más de uno");
        Check.Invariant(this.Likes >= 0,"La cantidad de me gusta es siempre mayor o igual que cero");
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Exceptions/blob/master/src/Library/Media.cs)

Simulamos una precondición en C# utilizando el método `Precondition(bool)` de la clase **Check** al comienzo de un método, donde, por un lado, indicamos las condiciones requeridas por el método para ejecutar, y por otro, evaluamos que las condiciones estén dadas. Algo análogo hacemos con las postcondiciones utilizando el método `Postcondition(bool)` de la clase **Check** al final de un método.

Una invariante de clase es una afirmación que debe ser cierta para cada objeto de esa clase, desde que se instancia el objeto y durante toda la existencia del objeto. Una invariante I se pude factorizar en una fórmula de corrección haciendo **{P ∧ I} A {Q ∧ I}**. Simulamos entonces las invariantes en C# utilizando el método `Invariant(bool)` de la clase Check al comienzo y al final de cada método, y al final del constructor.

Si recuerdan cuando hablamos de tipos y hablamos del LSP, vimos que éste afirmaba que en cualquier parte donde usáramos un tipo T apareciera un subtipo de T el programa seguiría siendo correcto. Pero con lo que hemos visto ahora y teniendo en cuenta nuestra definición más estricta de corrección- no es evidente ni necesario que el LSP se cumpla, y de hecho puede ser violado fácilmente. Por ejemplo, si implementamos una operación A de una interfaz X y violamos una postcondición definida para A, tenemos una violación del LSP. ¿Por qué? Porque los clientes que usan el tipo X y su operación A no van a funcionar de la misma manera si uso una implementación que viole una postcondición que una que la cumpla. No resulta lo mismo
de sustituir una implementación por otra.

Además, consideren lo siguiente. Sean las afirmaciones P<sub>1</sub> “la cantidad de :+1: está entre 1 y 10” y P<sub>2</sub> “la cantidad de :+1: es mayor que 0” <sup>3</sup>. ¿Cuál es más fuerte? Podemos ver fácilmente que “la cantidad de :+1: está entre 1 y 10” implica que “la cantidad de :+1: es mayor que 0”, porque si un número está entre 1 y 10 entonces es mayor que 0, pero no al revés; por lo tanto “la cantidad de :+1: está entre 1 y 10” es más fuerte que “la cantidad de :+1: es mayor que 0”.

Supongan una operación en una obra con una precondición “la cantidad de :+1: es mayor que 0” y supongan que una biblioteca usa esa operación en esa obra. ¿Recuerdan el principio de sustitución de Liskov? ¿Será posible reemplazar esa obra por otra en la que la misma operación tiene como precondición “la cantidad de :+1: está entre 1 y 10”? ¿Podrá seguir recibiendo :+1: ?

Seguramente que no. La biblioteca puede satisfacer la precondición original la obra anterior, no necesariamente satisface la nueva precondición de la nueva obra. La biblioteca no tiene el mismo comportamiento que antes, porque ¡ahora no funciona!

Esto nos lleva a la siguiente conclusión: una operación de un subtipo debe tener **precondiciones iguales o más débiles** que la de su supertipo; nunca más fuertes. Por razones análogas, una operación de un subtipo debe tener postcondiciones **iguales o más fuertes** que las de su supertipo; nunca más débiles.

Si el LSP no se cumple, se pierden las ventajas provistas por el polimorfismo, ya que no podríamos realizar la misma operación sobre distintos tipos en forma confiable. Por lo tanto, para poder aprovechar el polimorfismo, es necesario cumplir el LSP. A su vez, para que éste se cumpla, es necesario programar tomando en cuenta las precondiciones, postcondiciones e invariantes definidas

<br>

> [4.4 Desarrollo »](./4_4_Lecturas_Sugeridas.md)

</br>

****

_<sup>1</sup> Vean por ejemplo https://www.eiffel.org/doc/eiffel/Eiffel._

_<sup>2</sup>  Para poder usar una clase como **Contracts** es necesario hacerlo desde el cuerpo de los métodos, pero las interfaces no tienen métodos._

_<sup>3</sup> Los primeros televisores tenían solamente del canal 2 al 13; en aquella época no importaba porque sólo había televisión abierta, eran pocos canales. Luego vino la televisión por cable, con decenas de canales, y más tarde la televisión satelital, con cientos de canales.._