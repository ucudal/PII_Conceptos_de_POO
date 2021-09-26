![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 5. Herencia-Agregación

## 5.2 Desarrollo

Me gusta la música y las películas y me regalaron un equipo de audio con un reproductor de [discos compactos o CD](https://es.wikipedia.org/wiki/CD_audio) y otro de [discos de video digital o DVD](https://es.wikipedia.org/wiki/DVD-Video). Por eso tengo muchos CD con música y muchos DVD con películas.

> Ustedes probablemente tengan una suscripción a Spotify en lugar de CD de música o una suscripción a Netflix, HBO, Disney+ o Amazon Prime en lugar de DVD. Pero bueno, el ejemplo de los CD y DVD sigue siendo un buen ejemplo 😉.

Los CD y los DVD son discos. Hasta lucen similares. Aunque no puedo ver la película contenida en un DVD en el reproductor de CD y no puedo escuchar la música contenida en un CD en el reproductor de DVD, guardo todos mis discos en una discoteca.

Los discos tienen un nombre. A veces digo “hoy tengo ganas de escuchar tal o cual disco”, me refiero por el nombre a un disco compacto en particular, y cuando digo “hoy voy a ver tal o cual disco”, me refiero a un disco de video digital en particular por su nombre.

También los discos tienen un género. Un género típico es el de los “clásicos”. “Lo que el viento de llevó” y “Casablanca” son DVD “clásicos” y “The Wall” o “Abraxas” son CD clásicos; bueno, clásicos modernos, pero clásicos al fin.

“Lo que el viento se llevó” es del año 1939, “Casablanca” es del año 1943, “Abraxas” es del año 1970 y “The Wall” de 1988. Los discos también tienen un año.

Programemos una clase `Disk` de acuerdo a lo que acabamos de ver.

```c#
public class Disk
{
    protected Disk(string name)
    {
        this.Name = name;
    }

    public string Name { get; set; }

    public string Genre { get; set; }

    public int Year { get; set; }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/Disk.cs)

La discoteca contiene todos los discos. Puedo buscar un disco por el nombre, obtener todos los discos de un género, u obtener los discos más nuevos.

```c#
public class DiskLibrary
{
    private IList discs = new ArrayList();

    public void AddDisc(Disk disc)
    {
        this.discs.Add(disc);
    }

    public IList GetDiscsByGenre(string genre)
    {
        IList result = new ArrayList();
        foreach (Disk disc in this.discs)
        {
            if (disc.Genre.Equals(genre, StringComparison.OrdinalIgnoreCase))
            {
                result.Add(disc);
            }
        }

        return result;
    }

    public IList GetRecentDiscs()
    {
        IList result = new ArrayList();
        int year = 1;
        foreach (Disk disc in this.discs)
        {
            if (disc.Year > year)
            {
                year = disc.Year;
                result.Clear();
                result.Add(disc);
            }
            else if (disc.Year == year)
            {
                result.Add(disc);
            }
        }

        return result;
    }

    public IList GetDiscsByName(string name)
    {
        IList result = new ArrayList();
        foreach (Disk disc in this.discs)
        {
            if (disc.Name.Equals(name, StringComparison.OrdinalIgnoreCase))
            {
                result.Add(disc);
            }
        }

        return result;
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/DiskLibrary.cs)

La relación entre `DiskLibrary` y `Disk`, en la que la primera contiene múltiples instancias de la segunda, se llama **agregación** y se representa en un diagrama UML mediante una línea que comienza con un diamante en blanco del lado del contenedor y termina del lado del contenido. La multiplicad de la relación, esto es, la cantidad de elementos de cada lado, se representa con `0..1` del lado de `DiskLibrary` para indicar que un `Disk` puede estar en ninguna o en una `DiskLibrary`; y con `0..*` del lado de `Disk` para idnicar que una `DiskLibrary` puede contener ningún o muchos `Disk`.

![Disk](../../Assets/DiskLibrary+Disk.svg)

Pero los discos compactos son diferentes de los videodiscos. Ambos son discos, como hemos visto hasta ahora, pero los discos compactos tienen canciones y un intérprete, mientras los videodiscos tienen un director y varios actores. Como los discos compactos son discos, podemos usar la clase `Disk` como punto de partida para crear la clase `CD`; lo mismo con la clase `DVD` , también podemos usar la clase `Disk`. Cuando creamos una nueva clase en base a otra ya existente, decimos que la nueva clase extiende a la otra, que hereda de la otra, o que es una subclase de la otra.

La clase `CD` es una subclase de la clase `Disk`; también la clase `DVD` es una subclase de `Disk`. `Disk` es la superclase de las clases  `CD` y `DVD` . Esta relación de herencia es declarativa. Basta decir que  `CD` extiende a `Disk`, para tener una nueva clase  `CD` con todos los métodos y atributos de `Disk`, sin programar una sola línea de código. Los atributos y métodos de  `CD` se agregan a los existentes en `Disk`, y podemos usar cualquiera de ellos, independientemente que estén implementados en `Disk` o en `CD` , porque en realidad, como un  `CD` es un `Disk`, tiene los atributos y métodos de `Disk`, sin necesidad de programarlos de nuevo.

Para declarar en C# que la clase `CD` hereda de la clase `Disk` escribimos `CD : Disk` en la declaración de la clase `CD`.

```c#
public class CD : Disk
{
    private List<string> songs = new List<string>();

    public CD(string name, string[] songs)
        : base(name)
    {
        this.songs.AddRange(songs);
    }

    public IEnumerable<string> FavoritePlayers
    {
        get
        {
            return this.favoritePlayers;
        }
    }

    public string Player { get; set; }

    public IEnumerable<string> Songs
    {
        get
        {
            return this.songs;
        }
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/CD.cs)

Tengo mis discos favoritos. El criterio para que un disco sea uno de mis favoritos depende si se trata de un disco compacto o de un disco de video digital. En el caso de los CD, es favorito si es de cierto intérprete; en el caso de los DVD, es favorito si es de cierto género, es de ciertos directores, y actúan ciertos actores. Aunque el criterio sea diferente según se trate de un disco compacto o de un disco de video digital, todos los objetos de la clase `Disk` deberían poder decirme si son favoritos o no.

Debo agregar un método `bool IsFavorite()` en la clase `Disk` , pero lo necesario para decidir si un disco es favorito o no, está en las subclases `CD` y `DVD` , por lo que la implementación concreta también debe estar en las subclases `CD` y `DVD` . Al definir el método en la clase `Disk`, es seguro que las clases `CD` y `DVD` tendrán el método sin necesidad de escribirlo, justamente porque son subclases de `Disk` y entonces heredan sus métodos, pero ¿cómo es posible obligar a que cada una lo implemente nuevamente?  Después de todo, el método que hereden las clases `CD` y  `DVD` no será útil si no funciona diferente en cada una, porque el criterio es diferente según se trate de un disco compacto o un `DVD`.

A estos efectos usamos los métodos abstractos. Un **método abstracto** no tiene implementación.

Una clase que tiene un método abstracto es una clase abstracta. No puedo crear objetos de una **clase abstracta**, porque podría enviar un mensaje con el selector del método abstracto, porque ¡ese método no tiene implementación! Los métodos abstractos se crean al solo efecto de obligar a las subclases a tener un método.

Modificamos las clases que teníamos de la siguiente forma, en lugar del código que no cambia aparece `...`:

```diff
-public class Disk
+public abstract class Disk
{

    ...

+    public abstract bool Favorite();
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/Disk.cs)

```diff
public class CD : Disk
{
+    private List<string> favoritePlayers = new List<string>() { "Carlos Santana", "Pink Floyd" };

    ...

+    public override bool IsFavorite()
+    {
+        return this.FavoritePlayers.Contains(this.Player);
+    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/CD.cs)

```diff
public class DVD : Disco
{
+    public string FavoriteActor { get; set; } = "John Travolta";
+    public string FavoriteActress { get; set; } = "Uma Thurman";
+    public string FavoriteDirector { get; set; } = "Quentin Tarantino";
+    public string FavoriteGenre { get; set; } = "Acción";

    ...

+    public override bool IsFavorite()
+    {
+        return (this.IsActorInCast(this.FavoriteActor)
+            || this.IsActorInCast(this.FavoriteActress)
+            || this.Director.Equals(this.FavoriteDirector, StringComparison.OrdinalIgnoreCase))
+            && this.Genre.Equals(this.FavoriteGenre, StringComparison.OrdinalIgnoreCase);
+    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/DVD.cs)

Declaramos el método `bool IsFavorite()` con la palabra clave en `Disk` para indicar que es un método abstract. Usamos la palabra clave `override` para inidicar que el método `bool IsFavorite()` en `CD` y en `DVD` proveen una implementación para el método abstracto declarado antes en `Disk`.

El diagrama UML para representar la clase abstracta `Disk` y sus subclases concretas `CD` y `DVD` es así:

![Disk](../../Assets/Disk+CD+DVD.svg)

El conector entre la subclase `CD` y la superclase `Disk` -o entre la subclase `DVD` y la superclase `Disk`- es similar al que usamos antes para indicar que una clase implementa una interfaz, pero con el texto `extends` en lugar de `implements`. Esto es consistente con el hecho que una sublcase es también un subtipo de una superclase, así como una clase es un subtipo de las interfaces que implementa.

Agreguemos ahora a la discoteca la posibilidad de buscar discos de ciertos intérpretes o discos en los que actúan ciertos actores. Para eso creamos nuevos métodos en la clase `DiskLibrary`: `GetCDsByPlayer(string)` y `GetDVDsByActor(string)`.

```c#
public class DiskLibrary
{
    ...

    public IList GetDVDsByActor(string actor)
    {
        IList result = new ArrayList();
        foreach (Disk disc in this.discs)
        {
            if (disc is DVD && ((DVD)disc).IsActorInCast(actor))
            {
                result.Add(disc);
            }
        }

        return result;
    }

    public IList GetCDsByPlayer(string player)
    {
        IList result = new ArrayList();
        foreach (Disk disc in this.discs)
        {
            if (disc is CD && ((CD)disc).Player.Equals(player, StringComparison.OrdinalIgnoreCase))
            {
                result.Add(disc);
            }
        }

        return result;
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Herencia/blob/master/src/Library/DiskLibrary.cs)

Vean el uso de `disc is DVD` y de `disc is CD` que se debe usar para averiguar la clase de un objeto; luego vean `((CD)disc).Player.Equals(player, StringComparison.OrdinalIgnoreCase)` y  `((DVD)disc).IsActorInCast(actor)` para convertir una referencia a una instancia de `Disk` en una referencia una instancia de `CD` y `DVD` respectivamente. Esto último es llamado **typecast** o **downcast**.

Preguntar a un objeto su clase, generalmente, no es una buena práctica de programación, básicamente porque implica que no sabemos el tipo del objeto, lo que no parece una buena señal. Cuando preguntamos por la clase de un objeto para diferenciar subclases entre sí, como es este caso, generalmente, significa que el objeto de la subclase no es «exactamente» del tipo de la superclase. En general, en estos casos, lo más ventajoso es intentar aprovechar el polimorfismo.

En esta presentación teníamos una clase `Disk` y a partir de ella creamos las clases `CD` y `DVD`. También sería posible lo contario: tener clases `CD` y `DVD` que inicialmente estuvieran separadas, y que hasta fueron creadas en diferentes momentos, pero reconocer en ellas ciertas similitudes; es posible crear una nueva clase y cambiar las existentes para que sean subclases de la primera. Usando las clases del ejemplo, tendríamos las clases `CD` y `DVD`, cada una con los atributos `Name`, `Genre` e `Year` y las propiedades correspondientes, y crearíamos la clase `Disk` y haríamos que `CD` y `DVD` heredaran de está. Deberíamos borrar de las clases `CD` y `DVD` los atributos y los métodos que pasan a estar en `Disk`.

Vean como `DiskLibrary` no necesita saber con qué implementación particular del tipo está trabajando para interactuar con un `Disk`, es decir, no necesita saber si el `Disk` es un `CD` o un `DVD`. Al enviar el mensaje para obtener los favoritos, tampoco sabe cómo va a ser procesado, es decir, qué método va a ser ejecutado, si el de `CD` o el de `DVD`: nuestra operación de obtener favoritos es polimórfica.

Como no lo sabe `DiskLibrary`, tampoco lo sabe el compilador que genera el código de `DiskLibrary`. Por lo tanto es necesario que el código generado por el compilador sea capaz de identificar el método y código que ha de ejecutarse mientras está siendo ejecutado. La capacidad de “encadenar” o “enlazar” una llamada al código correcto en tiempo de ejecución es lo que se conoce como encadenamiento dinámico o tardío (porque se retrasta hasta el último momento posible que es cuando se debe ejecutar el método).

Cuando el compilador ve que se está llamando un método que posiblemente (no necesariamente) está sobreescrito en una subclase, en lugar de generar el código del método a ejecutar, genera código que se encarga de buscar el método en la clase del objeto que recibe el mensaje, y si no lo encuentra, buscar en sus superclases, para luego ejecutarlo. Debe hacerse de esta forma, ya que el compilador no puede saber, en todos los casos, cual es la clase del objeto contenido o referenciado por una variable cualquiera.

Es importante notar que el encadenamiento dinámico no solo se utiliza cuando se hereda de una clase. Cuando se implementan tipos también se usa encadenamiento dinámico, debido a que no se sabe cuál es la clase del objeto contenido o referenciado por la variable a la cual se está mandando el mensaje. En este caso, el encadenamiento dinámico es generalmente la única opción; el código debe encadenarse en tiempo de ejecución ya que no es posible determinar otro código que se pueda encadenar simplemente mirando la declaración de la variable (que normalmente es del tipo de la interface que define el tipo).

Sin el mecanismo de encadenamiento dinámico, el compilador debería determinar el código a ejecutar en tiempo de compilación y se perdería el polimorfismo. Si siempre se ejecuta el mismo código en cada llamada, no importa si variamos la instancia del objeto pasado por parámetro, siempre sucederá lo mismo. Este tipo de sistemas son llamados monomórficos.

Los métodos que se encadenan dinámicamente reciben distintos nombres en distintos lenguajes. En C#, se llaman métodos virtuales y se definen con la palabra clave `virtual`. Los métodos abstractos declarados con la palabra clave `abstract` también son virtuales. En ambos casos, los métodos en las subclases que sobrescriben métodos virtuales o abstractos utilizan la palabra clave `override`.

Lo contrario a encadenamiento dinámico es encadenamiento estático o temprano. En este caso, el código que se va a ejecutar está definido al momento de compilar. Dado que el código se determina al momento de compilación, siempre se va a ejecutar el mismo fragmento de código. Por lo cual, independientemente de si el método encadenado estáticamente es llamado sobre un `CD` o un `DVD`, el código ejecutado es siempre el mismo.

¿Cómo es posible definir de forma estática el código a ejecutar? Solo si éste está definido estrictamente por el tipo con el cual fue declarada la variable receptora y la firma del método. Si éste no se pudiera definir a partir de estos dos componentes -por ejemplo, debido a que el tipo de la variable es una interfaz y por lo tanto no puede haber código asociado a la firma del método-, el compilador no podría determinar el método a ejecutar y por lo tanto no podría existir encadenamiento dinámico. En definitiva, el método a ejecutar queda determinado de forma unívoca por el tipo con el que está declarada la variable a la cual estoy usando para enviar el mensaje.

¿Para que existe entonces el encadenamiento estático? Existen una serie de ventajas el estático con respecto al dinámico. Primero, es más rápido debido a que en el dinámico es necesario resolver que método ejecutar mientras se está ejecutando el programa. Si pensamos en una clase que tiene diez ancestras podríamos tener que realizar una búsqueda muy larga para identificar que método realmente tengo que ejecutar -dependiendo de qué clase de esas diez es la última en definir el método-. Segundo, podemos saber exactamente que método se va a ejecutar, con lo cual no tenemos incertidumbre con respecto a que va a suceder. Esta ventaja pierde peso si seguimos algunas buenas prácticas, como veremos más adelante.

En C#, los métodos son normalmente encadenados estáticamente, excepto que se definan como virtuales.

Es claro que, si para identificar el código de un método definido con encadenamiento estático que se va a ejecutar el compilador sólo toma en cuenta el tipo bajo el cual está definido la variable y el selector del método, no es posible sobreescribir un método estático en otra clase. La sobreescritura en estos casos no se permite o se crea un nuevo método -que no sobrescribiría el método de la clase anterior como sucede en el caso de encadenamiento dinámico-.

<br/>

> [5.3 Lecturas Sugeridas »](./5_3_Lecturas_Sugeridas.md)

<br/>

