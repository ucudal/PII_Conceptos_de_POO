![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 0. Objetos, Clases y Mensajes

## 0.3 Desarrollo

Es tradición que el primer programa en cualquier lenguaje sea uno cuyo propósito consiste en imprimir en la consola el mensaje `¡Hola mundo!`. Fieles a esa tradición haremos ese programa a nuestra manera.

El programa que aparece a continuación se encuentra en muchos textos sobre C# pero no es realmente orientado a objetos. Creamos el programa en Visual Studio Code con el [comando](https://github.com/ucudal/PII_Comandos) `dotnet new console` y lo ejecutamos desde allí.

```c#
using System;

namespace HelloWorld
{
    public class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("¡Hola mundo!");
        }
    }
}
```

> ⚠️  **Nota**: <br/>La declaración `namespace` permite definir un espacio de nombre para los tipos definidos en el archivo de código fuente; es conveniente definir un espacio de nombres, pero no es obligatorio; en el código que puedes descargar de GitHub usamos espacios de nombre, pero en este documento lo omitimos por simplicidad.
>
>La declaración del método de clase `Main` incluye los argumentos que se reciben en el programa de consola desde el sistema operativo; aquí los omitimos porque no los vamos a usar.

Aunque este programa imprime en la consola `"¡Hola Mundo!"`, no nos permite ver cómo es un programa orientado a objetos, pues no usa clases ni crea objetos como ocurre en un programa orientado a objetos<sup>6</sup>.

Los cambios que introducimos en la nueva versión de programa nos permiten mostrarles cómo colaboran un conjunto de objetos para hacer lo mismo que la versión original. Propongámonos convertir el texto `"¡Hola Mundo!"` en un objeto de la clase `Phrase` que contiene un conjunto de objetos de la clase `Word`.
Nuevamente haremos el programa en Visual Studio Code y lo ejecutaremos desde allí.

```c#
using System;

public class Word
{
    private string text;

    public Word(string text)
    {
        this.Text = text;
    }

    public string Text
    {
        get
        {
            return this.text;
        }
        set
        {
            this.text = value.Trim();
        }
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_WordsPhrases_v1/blob/master/src/Library/Word.cs)

<br>

La clase `Word` tiene una propiedad `Text` de lectura y escritura; y un constructor Word(string) que recibe una `string` como argumento, que se asigna a la propiedad `Text`. Noten el uso de `Trim()` para que el texto de la palabra no tenga espacios.

```c#
using System.Collections;
using System.Text;

public class Phrase
{
    private ArrayList words;

    public Phrase()
    {
        this.words = new ArrayList();
    }

    public void AddWord(Word word)
    {
        this.words.Add(word);
    }

    public void RemoveWord(Word word)
    {
        this.words.Remove(word);
    }

    public string GetPhrase()
    {
        StringBuilder phrase = new StringBuilder();

        foreach (Word word in this.words)
        {
            phrase.Append(" ");
            phrase.Append(word.Text);
        }

        string result = phrase.ToString();
        result = result.TrimStart();

        return result;
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_WordsPhrases_v1/blob/master/src/Library/Phrase.cs)

<br>

```c#
public class Program
{
    public static void Main()
    {
        Word hello = new Word("Hola");
        Word world = new Word("Mundo!");
        Phrase greeting = new Phrase();

        greeting.AddWord(hello);
        greeting.AddWord(world);

        Console.WriteLine(greeting.GetPhrase());
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_WordsPhrases_v1/blob/master/src/Program/Program.cs)

<br>

Esta nueva versión del programa declara dos variables del tipo `Word` llamadas `hello` y `world`. Luego crea dos objetos de la clase `Word` que son almacenados en esas variables. Las palabras deben tener un texto, por lo cual los constructores reciben los textos `"Hola"` y `"mundo"` respectivamente como argumento.

Luego el programa declara una variable de tipo `Phrase` llamada `greeting`. Luego crea un objeto de la clase `Phrase` que es almacenado en esa variable. El programa luego envía mensajes con selector `AddWord` al objeto referenciado en la variable `greeting` pasando como argumento de los mensajes los objetos de tipo `Word` creados anteriormente.

Por último, el programa envía un mensaje con selector `GetPhrase` a ese mismo objeto, y el resultado se imprime en la consola.

<br/>

> [0.4 Patrones y Principios »](./0_4_Patrones_Principios.md)

<br/>

*****

_<sup>6</sup> Esto es parcialmente cierto. El código declara una clase `Program`; sin embargo, la clase `Program` no es usada por nosotros, sino por el entorno en tiempo de ejecución -runtime environment-. El código también crea un objeto de `String` mediante el literal `Hola mundo`; sin embargo, ningún mensaje es enviado a ese objeto._
