[Conceptos de Programación Orientada a Objetos](../..)

# 1. Objetos, Clases y Mensajes

## 1.3 Guías para distribuir responsabilidades

Vimos antes que cada objeto tiene una parte de los datos y una parte de la
lógica de programa, la responsabilidad de conocer la parte de los datos que le
corresponde y de hacer la parte de la lógica que le corresponde.

A continuación veremos algunas guías para distribuir esas responsabiliades.

### Expert

#### Problema

> ¿Cuál es el principio básico para asignar responsabilidades?

#### Solución

> Asignar la responsabilidad al experto en los datos, es decir, a la clase que
> tiene los datos necesarios para cumplir la responsabilidad.

A la clase `Person` que definimos antes le agregamos la
responsabilidad de conocer su edad:

```csharp
public class Person
{
    ...
    public int Age { get; set;}
    ...
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v6/src/Library/Person.cs#L54-L54)

Si hubiera varias clases ¿a cuál le asignamos la responsabilidad de determinar
si una persona es mayor de edad?

Como para determinar si una persona es mayor de edad necesitamos conocer la edad
de la persona, y la edad de la persona es una responsabilidad de conocer la
clase `Person`, entonces según [Expert](#expert) debemos asignar esa
responsabilidad de hacer también a la clase `Person`.

```csharp
public class Person
{
    ...
    public int Age { get; set;}

    public bool IsAdult()
    {
        return this.Age >= 18;
    }
    ...
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v6/src/Library/Person.cs#L73-L76)

> [!TIP]
> Lee más sobre Expert
> [aquí](https://github.com/ucudal/PII_Principios_Patrones/blob/master/Expert.md).

### Responsabilidad Única

El principio de responsabilidad única también se conoce como **SRP** por sus
siglas en inglés *Single Responsibility Principle*. Esta guía dice:

> Nunca debería haber más de una razón para que una clase cambie.

A la clase `Person` que definimos antes le agregamos ahora las responsabilidades
de conocer el peso y la altura:

```csharp
public class Person
{
    ...
    public double Weight { get; set; }
    public double Height { get; set; }
    ...
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v7/src/Library/Person.cs#L56-L66)

Ahora queremos agregar la responsabilidad de determinar si es persona tiene
sobrepeso o no; una forma de hacerlo es mediante el [índice de masa
corporal](https://www.cdc.gov/healthyweight/spanish/assessing/bmi/adult_bmi/index.html#calcula-el-IMC),
que se calcula en función de la altura y el peso.

La responsabilidad de determinar si la persona tiene sobrepeso o no deberíamos
asignarla por [Expert](#expert) a la clase `Person` que conoce los datos que se
necesitan para eso: la altura y el peso.

Sin embargo, la forma para determinar si una persona tiene sobrepeso podría ser
diferente. En caso de que lo fuera, la clase `Person` debería cambiar, pero por
algo que no tiene que ver con la persona, sino con la forma en que se determina
si una persona tiene sobrepeso.

Entonces, según lo que nos dice SRP, la responsabilidad de determinar si una
persona tiene sobrepeso, debe estar en una clase diferente de `Person`; en este
ejemplo cremos una clase `OverweigthCalculator`:

```csharp
public static class OverweigthCalculator
{
    public static bool HasOverweight(Person person)
    {
        double imc = person.Height / Math.Pow(person.Weight, 2);
        return imc > 25;
    }
}
```

> [Ver en repositorio »](https://github.com/ucudal/PII_Person/blob/main/v7/src/Library/OverweigthCalculator.cs)

> [!TIP]
> Lee más sobre SRP
> [aquí](https://github.com/ucudal/PII_Principios_Patrones/blob/master/SRP.md).

<br/>

> [1.4 Lecturas sugeridas »](./1_4_Lecturas_Sugeridas.md)

<br/>

****

_<sup>1</sup> Vean por ejemplo el [Patrón Flyweight](https://en.wikipedia.org/wiki/Flyweight_pattern)._