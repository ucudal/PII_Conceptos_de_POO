[Conceptos de Programación Orientada a Objetos](../../)

# 6. Composición - Delegación

## 6.2 Presentación

En el capítulo de Tipos vimos una clase **Car** que implementa un tipo
**IElectric**; el tipo **IElectric** representa las cosas que puedo prender y
apagar.

```c#
public interface IElectric
{
    Boolean IsOn { get; }
    void TurnOn();
    void TurnOff();
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_CompositionDelegation/blob/master/src/Library/IElectric.cs)

La implementación de **Car** de los miembros de **IElectric** se muestra a
continuación. Vean que **Car** implementa los miembros de **IElectric** sin
pedir colaboración a ninguna otra clase.

```c#
public class Car : IElectric
{
    private bool isOn;

    public Carv1(String model)
    {
        this.Model = model;
        this.isOn = false;
    }

    public String Model { get; }

    public Boolean IsOn
    {
        get
        {
            return this.isOn;
        }
    }

    public void TurnOn()
    {
        this.isOn = true;
    }

    public void TurnOff()
    {
        this.isOn = false;
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_CompositionDelegation/blob/master/src/Library/Car.cs)

Consideremos ahora una clase **Lamp** que representa una lámpara. Como también
puedo prender y apagar una lámpara, los objetos de la clase **Lamp** tienen
también el tipo **IElectric**.

A la hora de implementar los métodos necesarios para que la clase **Lamp**
implemente la interfaz **IElectric**, podemos repetir la implementación de
**Car**; tendríamos que repetir el mismo código que ya escribimos en Car, lo
cual no es una cosa buena.


> [!TIP]
> DRY[^1]: Don’t Repeat Yourself
>Todo conocimiento debe tener una representación autoritaria, inequívoca y única
>dentro de un sistema. Cuando este principio se aplica con éxito, una
>modificación de cualquier elemento individual de un sistema no requiere un
>cambio en otros elementos lógicamente no relacionados. Además, los elementos
>que están relacionados lógicamente cambian de manera predecible y uniforme y,
>por lo tanto, se mantienen sincronizados.

[^1]: El principio ha sido formulado por Andy Hunt y Dave Thomas en su libro The
    Pragmatic Programmer.

Todos los objetos de tipo **IElectric** se encienden y se apagan. Los autos que
son instancias de **Car** y las lámparas que son instancias de **Lamp** son de
tipo **IElectric** y, como tales, se encienden y se apagan. Las llaves son
usadas para encender y apagar cosas, por ejemplo, autos[^2] y lámparas.
En esos objetos, la llave es parte del objeto; cuando compramos el auto o la
lámpara, compramos la llave; aunque es posible reemplazar la llave, cuando se
rompe, con una nueva llave, los autos y lámparas que tienen llave, siempre
tienen una llave que es parte de ellos.

[^2]: Algunos autos y lámparas más modernos son *keyless* y se encienden y apagan
    sin llave… el ejemplo es *vintage*.

Decimos que la llave es un componente del auto o de la lámpara o que un auto o
una lámpara están compuestos por una llave.

Sea una clase Switch definida así:

```c#
public class Switch
{
    private Status status;

    private enum Status
    {
        On = 0,
        Off = 1
    }

    public Switch()
    {
        this.status = Status.Off;
    }

    public bool IsOn
    {
        get
        {
            return this.status == Status.On;
        }
    }

    public void Toggle()
    {
        this.status = this.status == Status.On ? Status.Off : Status.On;
        // La sentencia anterior es equivalente a:
        // if (this.status == Status.On)
        // {
        // this.status = Status.Off;
        // }
        // else
        // {
        // this.status = Status.On;
        // }
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_CompositionDelegation/blob/master/src/Library/Switch.cs)

La clase **Lamp** que mencionamos antes implementa la interfaz **IElectric**.
Esto implica agregar a la clase Lamp los métodos **void** TurnOn y **void**
TurnOff y la propiedad `bool IsOn { get; }` de la interfaz **IElectric**.

Pero justamente estas tres últimas operaciones son las que sabe hacer una llave.
Podemos agregar a nuestra Lamp un objeto **Switch** para encenderla y apagarla.
Tendremos que escribir estos métodos en la clase **Lamp**, pero usaremos el
objeto **Switch** para implementarlos.

La instancia de **Switch** que usa la clase **Lamp** se almacena en la variable
de instancia privada onOff, que la usa para implementar las operaciones
definidas en **IElectric**. Vean que la instancia de **Switch** se crea en la
declaración de la variable onOff, pero podría haber sido creada en el
constructor de **Lamp**.

```c#
public class Lamp : IElectric
{
    private Switch onOff = new Switch();

    public Boolean IsOn
    {
        get
        {
            return this.onOff.IsOn;
        }
    }

    public void TurnOn()
    {
        if (!this.onOff.IsOn)
        {
            this.onOff.Toggle();
        }
    }
º
    public void TurnOff()
    {
        if (!this.onOff.IsOn)
        {
            this.onOff.Toggle();
        }
    }
}
```
> [Ver en repositorio »](https://github.com/ucudal/PII_CompositionDelegation/blob/master/src/Library/Lamp.cs)

Los objetos de la clase **Lamp** están compuestos por objetos de la clase
**Switch**; cada uno de estos objetos **Switch** es parte del objeto **Lamp**
que lo contiene. Decimos que los objetos **Lamp** están compuestos por objetos
**Switch** o que los objetos **Switch** son componentes de los objetos **Lamp**.
Aunque decimos estas cosas de los objetos, en realidad es en las clases donde
esta relación se define.

Generalmente el objeto compuesto crea los objetos componentes y éstos existen
mientras exista el compuesto. En general no es posible manipular los objetos
componentes, sino que el objeto compuesto se encarga de ello. Habitualmente no
es posible agregar, ni reemplazar componentes. Muchas veces los objetos
componentes son creados justamente con ese propósito, es decir, que sean parte
de otros objetos, y no pueden tener o no tiene sentido que tenga una existencia
independiente. De hecho, un **Switch** separado de un aparato al cual encender o
apagar, es completamente inútil.

En el caso de las clases **Lamp** y **Switch**, decimos que la clase o un objeto
de la clase **Switch** es parte de la clase o de los objetos de la clase
**Lamp**. Pero una cosa más interesante es cómo la clase **Lamp** implementa la
interfaz IElectric. Aunque provee un método para cada operación de la interfaz,
en cada método simplemente se envía un mensaje al componente **Switch**. Se
podría decir que los objetos de la clase **Lamp** no hacen nada, o por lo menos,
no hacen nada más que enviar el mensaje apropiado al objeto **Switch** que los
compone; el verdadero trabajo lo está haciendo el objeto **Switch**.

En esos métodos, decimos que la clase **Lamp** delega en la clase **Switch** el
procesamiento de los mensajes correspondientes.

La capacidad de polimorfismo utilizada siguiendo el LSP nos permite, entre otras
cosas, hacer nuestras aplicaciones flexibles usando composición. En general, al
no usar polimorfismo, nuestra aplicación podía variar su comportamiento
basándose en los datos de entrada. Es decir, dados ciertos valores de entrada,
que podían ser los datos a procesar y parámetros de configuración de la
aplicación, nuestro programa genera cierto resultado. Utilizando polimorfismo
podemos configurar tanto datos como los algoritmos utilizados para procesar esos
datos. Esto gracias a que podemos elegir la clase que se va a instanciar en
tiempo de ejecución; no tiene porqué estar definida al momento de compilar.

Por ejemplo, tenemos un programa que recibe como entrada un conjunto de números
y utiliza una instancia de tipo Ordenador para generar como salida una lista
ordenada de esos números. Dependiendo del número de números a ordenar,
disponibilidad de memoria, orden inicial de los números, etc., podría ser útil
usar diferentes algoritmos de ordenación[^3].

[^3]: Por ejemplo, para dos números el algoritmo *bublesort* podría ser excelente,
    pero para 1000 probablemente lo sea *quicksort*.

Gracias a polimorfismo, en un caso podríamos instanciar una clase que ordene
usando quicksort, en otra usando burbuja, en otra usando heapsort, etc., siempre
y cuando la clase sea de tipo Ordenador. La decisión de que clase utilizar es
tomada en tiempo de ejecución. Incluso, algunos lenguajes como Java o C#,
permiten definir la clase de forma completamente dinámica. Simplemente se pasa
como parámetro al programa el nombre de la clase que queremos utilizar, y la
plataforma puede, a partir de este nombre, crear la clase correcta. Por lo cual
nuestro programa podría llegar a usar instancias de tipo Ordenador de clases que
nosotros no conocemos, y que de hecho podrían no existir cuando nosotros
escribimos nuestra aplicación.

<br/>

> [6.3 Guías »](./6_3_Guias.md)

<br/>
