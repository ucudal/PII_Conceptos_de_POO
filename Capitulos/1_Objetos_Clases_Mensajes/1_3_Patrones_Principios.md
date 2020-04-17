![UCU](../../Assets/logo-ucu.png)

[Conceptos de Programación Orientada a Objetos](../../)


# 1. Objetos, Clases y Mensajes

## 1.3 Patrones y Principios

Vimos antes que cada objeto tiene una parte de los datos y una parte de la lógica de programa, la responsabilidad de conocer la parte de los datos que le corresponde y de hacer la parte de la lógica que le corresponde.

### Patrón Experto

#### Problema

> ¿Cuál es el principio básico para asignar responsabilidades?

#### Solución

> Asignar la responsabilidad al experto en los datos, es decir, a la clase que tiene los datos necesarios para cumplir la responsabilidad.

La clase `Phrase` tiene la responsabilidad las palabras, es razonable que tenga también la responsabilidad de armar el texto de la frase. Vean que la clase `Phrase` conoce las palabras, pero no el texto de las palabras; la responsabilidad de conocer el texto de las palabras es de la clase `Word`. Por lo tanto, para cumplir con la responsabilidad de armar el texto de la frase, la clase `Phrase` pide colaboración a la clase `Word`.

### Principio de Responsabilidad Única (SRP)

> Nunca debería haber más de una razón para que una clase cambie.

Aunque este ejemplo es muy simple, separamos la responsabilidad de conocer las palabras de una frase de la responsabilidad de conocer el texto de las palabras; la primera responsabilidad es de la clase `Phrase` y la segunda de la clase `Word`.

El texto de las palabras de una frase podría estar en la clase `Phrase`. Podríamos querer agregar un comportamiento para buscar palabras en el diccionario o cambiar la forma en la que se almacenan las palabras<sup>7</sup>, y en ese caso habría que cambiar la clase `Phrase`. Tal como está programado ahora, estos cambios los podríamos implementar en la clase `Word`, sin modificar la clase `Phrase`.

<br/>

> [1.4 Lecturas sugeridas »](./1_4_Lecturas_Sugeridas.md)

<br/>

*******

_<sup>7</sup> Vean por ejemplo el [Patrón Flyweight](https://en.wikipedia.org/wiki/Flyweight_pattern)._