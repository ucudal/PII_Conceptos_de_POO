![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 5. Herencia-Agregación

## 5.2 Desarrollo

Me gusta la música y las películas y me regalaron un equipo de audio con un reproductor de discos compactos y otro de DVD. Por eso tengo muchos discos compactos con música y muchos videodiscos con películas.

Los discos compactos y los DVD son discos. Hasta lucen similares. Aunque no puedo ver la película contenida en un DVD en el reproductor de discos compactos y no puedo escuchar la música contenida en un disco compacto en el reproductor de DVD, guardo todos mis discos en una discoteca.

Los discos tienen un nombre. A veces digo “hoy tengo ganas de escuchar tal o cual disco”, me refiero por el nombre a un disco compacto en particular, y cuando digo “hoy voy a ver tal o cual disco”, me refiero a un video disco en particular por su nombre.

También los discos tienen un género. Un género típico es el de los “clásicos”. “Lo que el viento de llevó” y “Casablanca” son DVD “clásicos” y “The Wall” o “Abraxas” son discos compactos clásicos; bueno, clásicos modernos, pero clásicos al fin.

“Lo que el viento se llevó” es del año 1939, “Casablanca” es del año 1943, “Abraxas” es del año 1970 y “The Wall” de 1988. Los discos también tienen un año.

Programemos una clase Disco de acuerdo a lo que acabamos de ver.

```c#
// Disco.cs

using System; 

namespace Herencia 
{ 
    public abstract class Disco 
    { 
        private readonly String nombre; 
        
        public String Nombre 
        { 
            get 
            { 
                return nombre; 
            } 
            set 
            { 
                genero nombre = value; 
            } 
        } 
 
        private String genero = ""; 

        public String Genero 
        { 
            get 
            { 
                return genero; 
            } 
            set 
            { 
                genero = value; 
            } 
        } 
 
        private Int32 anio = 0; 

        public Int32 Anio 
        { 
            get 
            { 
                return anio; 
            } 
            set 
            { 
                anio = value; 
            } 
        } 
         
        public Disco(String nombre) 
        { 
            this.nombre = nombre;             
        } 
    } 
} 
```

La discoteca contiene todos los discos. Puedo buscar un disco por el nombre, obtener todos los discos de un género, u obtener los discos más nuevos. 

```c#
// Discoteca.cs

using System; 
using System.Collections; 

namespace Herencia 
{ 
    public class Discoteca 
    { 
        private IList discos = new ArrayList(); 
 
        public void AddDisco(Disco disco) 
        { 
            discos.Add(disco); 
        } 
 
        public IList GetDiscosGenero(String genero) 
        { 
            IList result = new ArrayList(); 
            foreach (Disco disco in discos) 
            { 
                if (disco.Genero.Equals(genero)) 
                { 
                    result.Add(disco); 
                } 
            } 
            return result; 
        } 
 
        public IList getDiscosRecientes() 
        { 
            IList result = new ArrayList(); 
            int anio = -1; 
            foreach (Disco disco in discos) 
            { 
                if (disco.Anio > anio) 
                { 
 
                    anio = disco.Anio; 
                    result.Clear(); 
                    result.Add(disco); 
                }  
                else if (disco.Anio == anio) 
                { 
                    result.Add(disco); 
                } 
            } 
            return result; 
        } 

        public IList getDiscosLlamados(String nombre) 
        { 
            IList result = new ArrayList(); 
            foreach (Disco disco in discos) 
            { 
                if (disco.Nombre.Equals(nombre)) 
                { 
                    result.Add(disco); 
                } 
            } 
            return result; 
        } 
    } 
} 
```

Pero los discos compactos son diferentes de los videodiscos. Ambos son discos, como hemos visto hasta ahora, pero los discos compactos tienen canciones y un intérprete, mientras los videodiscos tienen un director y varios actores. Como los discos compactos son discos, podemos usar la clase `Disco` como punto de partida para crear la clase `CD`; lo mismo con la clase `DVD` , también podemos usar la clase `Disco`. Cuando creamos una nueva clase en base a otra ya existente, decimos que la nueva clase extiende a la otra, que hereda de la otra, o que es una subclase de la otra. 

La clase `CD` es una subclase de la clase `Disco`; también la clase `DVD` es una subclase de `Disco`. `Disco` es la superclase de las clases  `CD` y `DVD` . Esta relación de herencia es declarativa. Basta decir que  `CD` extiende a `Disco`, para tener una nueva clase  `CD` con todos los métodos y atributos de `Disco`, sin programar una sola línea de código. Los atributos y métodos de  `CD` se agregan a los existentes en `Disco`, y podemos usar cualquiera de ellos, independientemente que estén implementados en `Disco` o en `CD` , porque en realidad, como un  `CD` es un `Disco`, tiene los atributos y métodos de `Disco`, sin necesidad de programarlos de nuevo. 

```c#
// CD.cs

using System; 

namespace Herencia 
{ 
    public class CD: Disco 
    { 
        private String interprete; 

        public Strng Interprete 
        { 
            get 
            { 
                return interprete; 
            } 
            set 
            { 
                interprete = value; 
            } 
        } 
 
        private String[] canciones; 

        public String[] Canciones 
        { 
            get 
            { 
                return canciones; 
            } 
            set 
            { 
                canciones = value; 
            } 
        } 
         
        public CD(String nombre) 
            : base(nombre) 
        { 
        }     
    } 
} 
```

```c#
// CD.cs

using System; 

namespace Herencia 
{ 
    public class DVD: Disco 
    { 
        private String director; 

        public String Director 
        { 
            get 
            { 
                return director; 
            } 
            set 
            { 
                director = value; 
            } 
        } 
 
        private String[] actores; 

        public String[] Actores 
        { 
            get 
            { 
                return actores; 
            } 
            set 
            { 
                actores = value; 
            } 
        } 
 
        public DVD(String nombre) 
            : base(nombre) 
        { 
        } 
 
        public Boolean ActuaActor(String unActor) 
        { 
            foreach (String actor in actores) 
            { 
                if (actor.Equals(unActor)) 
                { 
                    return true; 
                } 
            } 
            return false; 
        }     
    } 
} 
```

Tengo mis discos favoritos. El criterio para que un disco sea uno de mis favoritos depende que se trate de un disco compacto o de DVD. En el caso de los discos compactos, es favorito si es de cierto intérprete; en el de los DVD, es favorito si es de cierto género, de ciertos directores, y actúan ciertos actores. Aunque el criterio sea diferente según se trate de un disco compacto o de un videodisco, todos los objetos de la clase Disco deberían poder decirme si son favoritos o no. 

Debo agregar un método `Boolean Favorito()` en la clase `Disco` , pero lo necesario para decidir si un disco es favorito o no, está en las subclases `CD` y `DVD` , por lo que la implementación concreta también debe estar en las subclases `CD` y `DVD` . Al definir el método en la clase `Disco`, es seguro que las clases `CD` y `DVD` tendrán el método sin necesidad de escribirlo, justamente porque son subclases de `Disco` y entonces heredan sus métodos, pero ¿cómo es posible obligar a que cada una lo implemente nuevamente?  Después de todo, si el método que hereden las clases `CD` y  `DVD` no será útil, porque el criterio es diferente según se trate de un disco compacto o un `DVD`. 

A estos efectos usamos los métodos abstractos. Un **método abstracto** no tiene implementación. 

Una clase que tiene un método abstracto es una clase abstracta. No puedo crear objetos de una **clase abstracta**, porque podría enviar un mensaje con el selector del método abstracto, pero ¡ese método no tiene implementación! Los métodos abstractos se crean al solo efecto de obligar a las subclases a tener un método. 

```c#
// Disco.cs

namespace Herencia 
{ 
    public abstract class Disco 
    {
        ... 

        public abstract Boolean Favorito(); 
    } 
} 
```

```c#
// CD.cs

namespace Herencia 
{ 
    public class CD: Disco 
    { 
        ... 

        public override Boolean Favorito() 
        {    
            return (interprete.Equals("Carlos Santana")) || (interprete.Equals("Pink Floyd")); 
        } 
    } 
} 
```

```c#
// DVD.cs

namespace Herencia 
{ 
    public class DVD : Disco 
    { 
        .... 
 
        public override Boolean Favorito() 
        { 
            return (ActuaActor("John Travolta") || ActuaActor("Uma Thurman") || 
                (director.Equals("Quentin Tarantino"))) && (Genero.Equals("Acción"));
        } 
    } 
}
```

Vean que el método `Boolean Favorito()` está declarado abstract en `Disco` y no tiene implementación. Vean también que está declarado nuevamente en `CD` y en `DVD`, sin abstract, donde sí tiene implementación. Cuando un método está definido en una clase y también en una subclase, decimos que el método de la subclase **sobrescribe** al de la superclase. 

Noten que en la clase `Disco` los atributos `nombre`, `genero` y `anio` están declarados `private` y por lo tanto no son accesibles para las subclases `CD` y `DVD`. Sólo cuando están declarados `public` o `protected` las subclases pueden acceder a esos atributos. Las clases `CD` y `DVD` usan las propiedades `Nombre`, `Genero` y `Anio`, que heredaron y por lo tanto también son sus métodos, para acceder a los atributos declarados por la superclase, pero que no son accesibles. 

Agreguemos ahora a la discoteca la posibilidad de buscar discos de ciertos intérpretes o discos en los que actúan ciertos actores. Para eso creamos nuevos métodos en la clase `Discoteca`: `GetCDInterprete(String interprete)` y `GetDVDActor(String actor)`. 

```c#
// Discoteca.cs

using System; 
using System.Collections; 

namespace Herencia 
{ 
    public class Discoteca 
    { 
        ...  

        public IList GetDVDActor(String actor) 
        { 
            IList result = new ArrayList(); 
            foreach (Object disco in discos) 
            { 
                if (disco.GetType().Equals(DVD) && 
                    ((DVD)disco).ActuaActor(actor)) 
                { 
                    result.Add(disco); 
                } 
            } 
            return result; 
        } 

        public IList GetCDInterprete(String interprete) 
        { 
            IList result = new ArrayList(); 
            foreach (Object disco in discos) 
            { 
                if (disco.GetType().Equals(CD) &&  
                    ((CD)disco).Interprete.Equals(interprete)) 
                { 
                    result.Add(disco); 
                } 
            } 
            return result; 
        } 
    }    
}
```

Vean el uso de `disco.GetType().Equals(CD)` y de `disco.GetType().Equals(DVD)` que se debe usar para averiguar la clase de un objeto; luego vean `((CD)disco).Interprete.Equals(interprete)` y  `((DVD)disco).ActuaActor(actor)` para convertir una referencia a una instancia de `Object` en una referencia una instancia de `CD` y `DVD` respectivamente. Esto último es llamado **typecast** o **downcast**. 

Preguntar a un objeto su clase, generalmente, no es una buena práctica de programación, básicamente porque implica que no sabemos el tipo del objeto, lo que no parece una buena señal. Cuando preguntamos por la clase de un objeto para diferenciar subclases entre sí, como es este caso, generalmente, significa que el objeto de la subclase no es «exactamente» del tipo de la superclase. En general, en estos casos, lo más ventajoso es intentar aprovechar el polimorfismo.  

En esta presentación teníamos una clase `Disco` y a partir de ella creamos las clases `CD` y `DVD`. También es posible tener clases que inicialmente están separadas, y hasta fueron creadas en diferentes momentos, pero reconocer en ellas ciertas similitudes; es posible crear una nueva clase y cambiar las existentes para que sean subclases de la primera. Usando las clases del ejemplo, tendríamos las clases `CD` y `DVD`, cada una con los atributos `nombre`, `genero` y `anio` y las propiedades correspondientes, y crearíamos la clase `Disco` y haríamos que `CD` y `DVD` heredaran de está. Deberíamos borrar de las clases `CD` y `DVD` los atributos y los métodos que pasan a estar en `Disco`. 

Vean como `Discoteca` no necesita saber con qué implementación particular del tipo está trabajando para interactuar con un `Disco`, es decir, no necesita saber si el `Disco` es un `CD` o un `DVD`. Al enviar el mensaje, tampoco sabe cómo va a ser procesado, es decir, qué método va a ser ejecutado, si el de `CD` o el de `DVD`. Nuestra operación de obtener favoritos es polimórfica. 

Como no lo sabe `Discoteca`, tampoco lo sabe el compilador que genera el código de `Discoteca`. Por lo tanto es necesario que el código generado por el compilador sea capaz de identificar el método y código que ha de ejecutarse mientras está siendo ejecutado. La capacidad de “encadenar” o “enlazar” una llamada al código correcto en tiempo de ejecución es lo que se conoce como encadenamiento dinámico o tardío (porque se retrasta hasta el último momento posible que es cuando se debe ejecutar el método).  

Cuando el compilador ve que se está llamando un método que posiblemente (no necesariamente) está sobreescrito en una subclase, en lugar de generar el código del método a ejecutar, genera código que se encarga de buscar el método en la clase del objeto que recibe el mensaje y sus superclases para luego ejecutarlo. Debe hacerse de esta forma, ya que el compilador no puede saber, en todos los casos, cual es la clase del objeto contenido o referenciado por una variable cualquiera. 

Es importante notar que el encadenamiento dinámico no solo se utiliza cuando se hereda de una clase. Cuando se implementan tipos también se usa encadenamiento dinámico, debido a que no se sabe cuál es la clase del objeto contenido o referenciado por la variable a la cual se está mandando el mensaje. En este caso, el encadenamiento dinámico es generalmente la única opción; el código debe encadenarse en tiempo de ejecución ya que no es posible determinar otro código que se pueda encadenar simplemente mirando la declaración de la variable (que normalmente es del tipo de la interface que define el tipo).  

Sin el mecanismo de encadenamiento dinámico, el compilador debería determinar el código a ejecutar en tiempo de compilación y se perdería el polimorfismo. Si siempre se ejecuta el mismo código en cada llamada, no importa si variamos la instancia del objeto pasado por parámetro, siempre sucederá lo mismo. Este tipo de sistemas son llamados monomórficos. 

Los métodos que se encadenan dinámicamente reciben distintos nombres en distintos lenguajes. En C# y C++ por ejemplo, se llaman métodos virtuales y se definen con la palabra clave virtual. En el caso de Java, todos los métodos son por defecto encadenados dinámicamente.  

Lo contrario a encadenamiento dinámico es encadenamiento estático o temprano. En este caso, el código que se va a ejecutar está definido al momento de compilar. Dado que el código se determina al momento de compilación, siempre se va a ejecutar el mismo fragmento de código. Por lo cual, independientemente de si el método encadenado estáticamente es llamado sobre un `CD` o un `DVD`, el código ejecutado es siempre el mismo.  

¿Cómo es posible definir de forma estática el código a ejecutar? Solo si éste está definido estrictamente por el tipo con el cual fue declarada la variable receptora y la firma del método. Si éste no se pudiera definir a partir de estos dos componentes (por ejemplo, debido a que el tipo de la variable es una interfaz y por lo tanto no puede haber código asociado a la firma del método), el compilador no podría determinar el método a ejecutar y por lo tanto no podría existir encadenamiento dinámico. En definitiva, el método a ejecutar queda determinado de forma unívoca por el tipo con el que está declarada la variable a la cual estoy usando para enviar el mensaje.  

¿Para que existe entonces el encadenamiento estático? Existen una serie de ventajas el estático con respecto al dinámico. Primero, es más rápido debido a que en el dinámico es necesario resolver que método ejecutar mientras se está ejecutando el programa. Si pensamos en una clase que tiene diez ancestras podríamos tener que realizar una búsqueda muy larga para identificar que método realmente tengo que ejecutar (dependiendo de qué clase de esas diez es la última en definir el método). Segundo, podemos saber exactamente que método se va a ejecutar, con lo cual no tenemos incertidumbre con respecto a que va a suceder. Esta ventaja pierde peso si seguimos algunas buenas prácticas, como veremos más adelante. 

En C#, los métodos son normalmente encadenados estáticamente, excepto que se definan como virtuales. En Java, es necesario utilizar la palabra clave final para que el método se resuelva de forma estática. 

Es claro que, si para identificar el código de un método definido con encadenamiento estático que se va a ejecutar el compilador solo toma en cuenta el tipo bajo el cual está definido la variable y el selector del método, no es posible sobreescribir un método estático en otra clase. La sobreescritura en estos casos no se permite o se crea un nuevo método (que no sobrescribiría el método de la clase anterior como sucede en el caso de encadenamiento dinámico). 

<br/>

> [5.3 Lecturas Sugeridas »](./5_3_Lecturas_Sugeridas.md)

<br/>

