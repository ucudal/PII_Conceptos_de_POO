[Conceptos de Programación Orientada a Objetos](../../)

# 2. Tipos

## 2.3 Guías para la asignación de responsabilidades

Vimos antes que los tipos definen un contrato entre los objetos: el objeto que
envía el mensaje está obligado a enviar mensajes para ejecutar solamente los
métodos incluidos en el tipo; y el objeto que recibe los mensajes tiene el
derecho de implementar sólo los métodos incluidos en el tipo.

A continuación veremos algunas guías para distribuir responsabiliades basándos
en el concepto de tipos. Estas guías buscan que cuando un objeto colabora con
otro, el objeto que pide la colaboración no dependa de la clase del objeto, sino
que dependa del tipo del objeto. Al principio puede parer lo mismo, pero si
depende del tipo, cualquier objeto que tengan ese tipo —aunque sea de diferentes
clases— puede colaborar. Esto crea programas más fáciles de extender.

### Polymorphism

Decimos que una operación es **polimórfica** cuando es implementada por dos o
más objetos de diferentes tipos. Relacionado con esto está esta guía llamada
**Polymorfism**.

## Problema

> ¿Cómo manejar alternativas de comportamiento basadas en el tipo? ¿Cómo crear
> componentes de software conectables?

## Solución

> Cuando alternativas o comportamientos relacionados varían según el tipo, asignar
> las responsabilidades por el comportamiento usando operaciones polimórficas a
> los tipos cuyo comportamiento varía.

> [!IMPORTANT]
> No pregunten por el tipo de un objeto y usen lógica condicional para ejecutar
> diferentes fragmentos de código que varían según el tipo.

> [!TIP]
> Lee más sobre Polymorphism
> [aquí](https://github.com/ucudal/PII_Guias/blob/main/Polymorphism.md).

### Sustitución de Liskov

Bárbara Liskov escribió en 1988 el principio de sustitución que lleva su nombre.
Este principio también se conoce como **LSP** por sus siglas en inglés *Liskov
Substitution Principle*. Esta guía dice:

> Si por cada objeto o1 de tipo S hay un objeto o2 de tipo T tal que para todos
> los programas P definidos en términos de T, el comportamiento de P no cambia
> cuando o1 se sustituye por o2, entonces S es un subtipo de T.

O, dicho de otra forma, el código que envía mensajes a un objeto de un tipo T
debe funcionar igual cuando ese objeto es de un subtipo S del tipo T.

> [!TIP]
> Lee más sobre el principio de sustitución de Liskov
> [aquí](https://github.com/ucudal/PII_Guias/blob/main/LSP.md).

***

<br>

> [2.4 Lecturas sugeridas »](./2_4_Lecturas_Sugeridas.md)

</br>
