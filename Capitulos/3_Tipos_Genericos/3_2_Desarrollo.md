![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 3. Tipos genéricos

## 3.2 Desarrollo

Una colección puede contener objetos de cualquier tipo. A veces es bueno que así sea, pero a veces no. Por ejemplo, si quisiera tener una lista con canciones, en la que todas las canciones son cadenas de la clase `string`, nada impide que también pueda agregar enteros de la clase int a esa lista. 

Por otro lado, al recuperar las `string` de la lista, debo usar _typescast_<sup>16</sup> de tipos para convertir el tipo object retornado por la lista al tipo `string`. Esta coerción es propensa a errores, pues si la lista contiene otros tipos además de cadenas de tipo `string`, ocurrirá un error en tiempo de ejecución. 

La coerción es como una indicación que hacemos al compilador para que no haga el control de tipos de la forma habitual. Siempre que sea posible debemos dejar que el compilador haga los controles de tipos, pues si los controles los hacemos nosotros, nos podemos equivocar. Además, los controles que hace el compilador, previenen problemas temprano, antes que el programa ejecute. Los errores que ocurren cuando usamos incorrectamente la coerción de tipos, son errores en tiempo de ejecución. 

Vean el siguiente ejemplo, que tiene un tipo `Media` definido con una interfaz, que representa obras artísticas; la interfaz es implementada por las clases `Movie` y `Song`, que representan películas y canciones, respectivamente. 

```c#
public interface Media 
{ 
    string Name { get; } 

    string Genre { get; } 

    bool IsFavorite(); 
} 
```
> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v1/src/Library/Media.cs )

<br>

```c#
public class Movie : Media 
{ 
    private string[] actors; 

    public Movie(string name, string genre, string[] actors) 
    { 
        this.Name = name; 
        this.Genre = genre; 
        this.actors = actors; 
    } 

    public string Name { get; } 

    public string Genre { get; } 
 
    public Boolean Acts(string actor) 
    { 
        return Array.IndexOf(this.actors, actor) >= 0; 
    } 

    public bool IsFavorite() 
    { 
        return Array.IndexOf(this.actors, "Uma Thurman") >= 0; 
    } 
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v1/src/Library/Movie.cs)

<br>

```c#
public class Song : Media 
{ 
    public Song(string name, string genre, string artist) 
    { 
        this.Name = name; 
        this.Genre = genre; 
        this.Artist = artist; 
    } 

    public string Name { get; } 

    public string Genre { get; } 

    public string Artist { get; } 

    public Boolean IsFavorite() 
    { 
        return this.Genre.Equals("Rock"); 
    } 
} 
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v1/src/Library/Song.cs)

<br/>

La clase `Library` es una biblioteca que puede contener obras artísticas. Como el tipo definido por la interfaz Media tiene declarada la propiedad Genre, puedo buscar en la biblioteca las películas y las canciones de un género dado; vean el método `GetMediaByGenre()` a continuación. Como Media también tiene el método `IsFavorite()`, en la biblioteca puedo buscar tanto las películas como las canciones favoritas; vean el método `GetFavorites()` a continuación. Sin embargo, como sólo las películas tienen actores, si quiero buscar en la biblioteca todas las películas en las que actúa cierto actor, no tengo más remedio que preguntar a cada objeto de la biblioteca de qué tipo es, y sólo si es una película, preguntar por los actores; vean el método `GetMoviesWith_UsingGetType()` y noten el uso del operador typeof para averiguar de qué tipo es un objeto. Por el contrario, el método `GetMoviesWith_WithoutGetType()` no averigua el tipo de objeto en la biblioteca, y da un error en tiempo de ejecución la invocación está comentada en el ejemplo del programa que aparece más adelante. 

```c#
public class Library : IEnumerable 
{ 
    private IList items = new ArrayList(); 
 
    public void Add(Media item) 
    { 
        this.items.Add(item); 
    } 
 
    public IList GetMediaByGenre(string genre) 
    { 
        IList result = new ArrayList(); 
 
        foreach (Media item in this.items) 
        { 
            if (item.Genre.Equals(genre)) 
            { 
                result.Add(item); 
            } 
        } 

        return result; 
    } 

    public IList GetFavorites() 
    { 
        IList result = new ArrayList(); 

        foreach (Media item in this.items) 
        { 
            if (item.IsFavorite()) 
            { 
                result.Add(item); 
            } 
        } 

        return result; 
    } 

    public IList GetMoviesWith_WithoutGetType(string actor) 
    { 
        IList result = new ArrayList(); 

        foreach (Media item in this.items) 
        { 
            if (((Movie)item).Acts(actor)) 
            { 
                result.Add(item); 
            } 
        } 

        return result; 
    } 

    public IList GetMoviesWith_UsingGetType(string actor) 
    { 
        IList result = new ArrayList(); 

        foreach (Media item in this.items) 
        { 
            if (item.GetType().Equals(typeof(Movie)) && 
                ((Movie)item).Acts(actor)) 
            { 
                result.Add(item); 
            } 
        } 

        return result; 
    } 

    [ExcludeFromCodeCoverage] 
    public IEnumerator GetEnumerator() 
    { 
        return this.items.GetEnumerator(); 
    } 

}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v1/src/Library/Library.cs)

<br/>

Puedo recorrer la biblioteca `Library` en un bucle foreach porque implementa la interfaz IEnumerable; vean el método de clase `PrintMediaItems()` a continuación. El programa crea varias películas y canciones, las agrega a la biblioteca, y las imprime el código de los métodos de clase `AddMovies()` y `AddSongs()` no se muestra por simplicidad y en su lugar aparecen …, pero pueden verlo en el repositorio: 


```c#
public class Program 
{ 
    private static Library library = new Library(); 
 
    public static void Main(string[] args) 
    { 
        AddMovies(); 
        AddSongs(); 
 
        PrintMediaItems("La biblioteca contiene:", library); 
 
        Console.WriteLine("\n"); 
        IList moviesWithUma; 
 
        // [1] moviesWithUma = library.GetMoviesWith_WithoutGetType("Uma Thurman"); 
        moviesWithUma = library.GetMoviesWith_UsingGetType("Uma Thurman"); 
        PrintMediaItems("Las películas con Uma Thurman son:", moviesWithUma); 
    } 
 
    private static void AddMovies() 
    { 
        … 
    } 
 
    private static void AddSongs() 
    { 
        … 
    } 
 
    private static void PrintMediaItems(string title, IEnumerable items) 
    { 
        string result = title; 
 
        foreach (Media item in items) 
        { 
            result = string.Format("{0}\n{1}", result, item.Name); 
        } 
 
        Console.WriteLine(result); 
    } 
} 
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v1/src/Program/Program.cs)

<br/>

El problema con el typecast o con la excepción si no usamos typeof- es la que biblioteca puede contener cualquier tipo de objeto porque utiliza ArrayList para almacenar las canciones y las películas. Puedo crear contenedores que sólo pueda almacenar un tipo de objetos. Vean las clases MoviesContainer y SongsContainer a continuación. La clase Library ahora guarda los objetos de la clase Movie en la instancia de MoviesContainer y las de Song en la de SongsContainer. Como el estado de Library está encapsulado, y la representación interna de la biblioteca no se conoce fuera de la clase Library, el cambio no tiene impacto en los objetos que usan Library, por ejemplo, el programa principal. 

```c#
public class MoviesContainer : IEnumerable 
{ 
    private IList movies = new ArrayList(); 
 
    public void Add(Movie movie) 
    { 
        this.movies.Add(movie); 
    } 
 
    public IEnumerator GetEnumerator() 
    { 
        return this.movies.GetEnumerator(); 
    } 
} 
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v2/src/Library/MoviesContainer.cs)

<br/>

```c#
public class SongsContainer : IEnumerable 
{ 
    private IList songs = new ArrayList(); 
 
    public void Add(Song song) 
    { 
        this.songs.Add(song); 
    } 
 
    public IEnumerator GetEnumerator() 
    { 
        return this.songs.GetEnumerator(); 
    } 
} 
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v2/src/Library/SongsContainer.cs)

<br/>

Una ventaja de este nuevo diseño es que el método `GetMoviesWith()` de la clase `Library` ahora no tiene que preguntar por el tipo de los objetos, porque los objetos `MoviesContainer` sólo puede contener instancias de `Movie`. Sin embargo, una desventaja es que en el método `GetFavorites()` ahora tiene que recorrer dos colecciones. A continuación, aparece la nueva versión de la clase `Library`, los puntos … representan el código que ya apareció antes. 

```c#
public class Library : IEnumerable 
{ 
    // Ahora tenemos una colección para las películas y otra para las 
    // canciones; no podemos mezclar películas con canciones. Tuvimos que 
    // declarar una colección para las películas y otra para las  
    // canciones para evitar hacer typecast. 
    private MoviesContainer movies = new MoviesContainer(); 
 
    private SongsContainer songs = new SongsContainer(); 
 
    public void Add(Movie movie) 
    { 
        this.movies.Add(movie); 
    } 
   
    public void Add(Song song) 
    { 
        this.songs.Add(song); 
    } 
 
    public IList GetMoviesWith(string actor) 
    { 
        IList result = new ArrayList(); 
 
        // Ahora no es necesario usar ningún tipo de typecast porque movies puede 
        // contener solo películas 
        foreach (Movie movie in this.movies) 
        { 
            if (movie.Acts(actor)) 
            { 
                result.Add(movie); 
            } 
        } 
 
        return result; 
    } 
 
    public IList GetFavorites() 
    { 
        IList result = new ArrayList(); 
 
        // Ahora estamos obligados a recorrer dos colecciones 
        foreach (Movie movie in this.movies) 
        { 
            if (movie.IsFavorite()) 
            { 
                result.Add(movie); 
            } 
        } 
 
        foreach (Song song in this.songs) 
        { 
            if (song.IsFavorite()) 
            { 
                result.Add(song); 
            } 
        } 
 
        return result; 
    } 
 
    public IList GetMediaByGenre(string genre) 
    { 
        IList result = new ArrayList(); 
 
        // Ahora estamos forzados a recorrer dos colecciones 
        foreach (Movie movie in this.movies) 
        { 
            if (movie.Genre.Equals(genre)) 
            { 
                result.Add(movie); 
            } 
        } 
 
        foreach (Song song in this.songs) 
        { 
            if (song.Genre.Equals(genre)) 
            { 
                result.Add(song); 
            } 
        } 
 
        return result; 
    } 
    … 
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v2/src/Library/Library.cs)

<br/>

Vean que el método `Add` tiene dos versiones, una que recibe como argumento una instancia de `Movie` y otra que recibe una de Song. Este es un ejemplo de sobrecarga, el mismo nombre de método, con dos firmas diferentes. Noten que los métodos de clase `AddMovies` y `AddSong` de la clase `Program` no cambiaron pueden verlo en el repo. 

Con genéricos se evita el problema de tener que crear un contener para cada tipo de objeto, porque permite diseñar clases y métodos que postergan la especificación de uno o más tipos hasta que la clase o el método sea declarado e instanciado por código de cliente. 

Para definir una lista que sólo pueda contener instancias de `Movie`, uso el tipo genérico `GenericContainer<T>` y al momento de crear ese contenedor, uso `GenericContainer<Movie>`. En ese caso, para poder usar el método `Add(T)` de la clase `GenericContainer<T>`, voy a tener que pasar como argumento una instancia de `Movie`; de lo contrario el compilador me va a dar un problema. 

Vean a continuación la clase `GenericContainer<T>`, que usa tipos genéricos, y la nueva versión de `Library`. Los puntos … representan el código que ya apareció antes, sólo cambia la declaración de las variables de instancia movies y songs. 

```c#
public class GenericContainer<T> 
{ 
    private IList<T> items = new List<T>(); 
 
    public void Add(T item) 
    { 
        this.items.Add(item); 
    } 
 
    public IEnumerator<T> GetEnumerator() 
    { 
        return this.items.GetEnumerator(); 
    } 
} 
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v3/src/Library/GenericContainer.cs)

<br/>

```c#
public class Library : IEnumerable 
{ 
    // Ahora tenemos una colección para las películas y otra para las canciones; no 
    // podemos mezclar películas con canciones. Ahora nos alcanza con crear un 
    // contenedor genérico que nos sirve tanto para películas como para canciones con 
    // tan solo declararlo. 
    private GenericContainer<Movie> movies = new GenericContainer<Movie>(); 
    private GenericContainer<Song> songs = new GenericContainer<Song>(); 
    … 
} 
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Generics/blob/master/v3/src/Library/Library.cs)

<br/>

En esta nueva versión tenemos los mismos beneficios y desventajas que en la anterior, pero con un código mucho más simple. 

<br/>

> [4. Programar contra especificaciones »](../4_Programar_Contra_Especificaciones/4_1_Diseño_Por_Contrato.md)

****

_<sup>16</sup> También llamado coerción de tipos._
