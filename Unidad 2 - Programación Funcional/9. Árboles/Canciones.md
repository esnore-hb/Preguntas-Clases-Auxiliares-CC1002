---
tags: [ABB, Árboles Binarios]
---

# Árboles de Canciones

El gran `DJAlvarez` está preparando su repertorio de canciones para su concierto de fin de año. Para ello, utiliza un Árbol de Búsqueda Binaria (ABB) donde guarda las canciones, y cada canción es representada como una estructura, que almacena su nombre y duración en segundos (el ABB está ordenado por duración, y puede asumir que no hay dos canciones que duran lo mismo).

```python
# Cancion: nombre(str) duracion(int)
estructura.crear('Cancion','nombre duracion')

# suponga que existe la variable abbCanciones, que almacena el ABB de mas abajo
```

![arbol canciones](./img/arbol_canciones.PNG)

Para ayudarle en su puesta en escena, le pide ayuda con lo siguiente:

1. Cree la función ```agregarCancion(A,nombre,dur)```, que entrega el árbol $A$ con una nueva canción, de nombre y duración indicados. Ejemplo:

     - ```agregarCancion(abbCanciones, 'pythonisa', 180)``` agrega la canción a la derecha del nodo `'abs.. is fun'`.

2. Cree la función ```duraderas(A, s)```, que dado un ABB de canciones y una cantidad de segundos, entrega una lista con las canciones de $A$ que duren al menos `s` segundos. Ejemplo:

     - ```duraderas(abbCanciones, 300)``` entrega una lista con las canciones `'listaylor'` y `'la la lambda'`.

      **Indicación:** Puede asumir que existe la función ```juntar(L1,L2)``` que pega el contenido de L2 al final de L1.

3.  Cree la función ```repertorio(A)```, que dado un ABB de canciones, entrega una lista con todas las canciones de $A$ ordenadas por duración de menor a mayor.

## Pauta

```py
from AB import *
from ABB import *
from estructura import *
from lista import *

crear("Cancion", "nombre duracion")
crear("ABB", "valor izq der")
_ = None


def agregarCancion(A, nombre, dur):
    if A == None:
        return ABB(Cancion(nombre, dur), _, _)
    else:
        if dur < A.valor.duracion:
            result = agregarCancion(A.izq, nombre, dur)
            return ABB(A.valor, result, A.der)
        else:  # >=
            result = agregarCancion(A.der, nombre, dur)
            return ABB(A.valor, A.izq, result)


def duraderas(A, s):
    if A == None:
        return
    else:
        izq = duraderas(A.izq, s)
        der = duraderas(A.der, s)
        if A.valor.duracion >= s:
            return concatenar(izq, lista(A.valor, der))
        else:
            return der


def repertorio(A):
    if A == None:
        return
    else:
        izq = repertorio(A.izq)
        der = repertorio(A.der)
        return concatenar(izq, lista(A.valor, der))


abbCanciones = ABB(
    Cancion("el dueño del sistema", 212),
    ABB(
        Cancion("abstracion is fun", 128),
        ABB(Cancion("las recursivas", 54), _, _),
        _,
    ),
    ABB(
        Cancion("listaylor", 333),
        ABB(Cancion("ab lardo", 227), _, _),
        ABB(Cancion("la la lambda", 442), _, _),
    ),
)

assert agregarCancion(abbCanciones, "pythonisa", 180) == ABB(
    Cancion("el dueño del sistema", 212),
    ABB(
        Cancion("abstracion is fun", 128),
        ABB(Cancion("las recursivas", 54), _, _),
        ABB(Cancion("pythonisa", 180), _, _),
    ),
    ABB(
        Cancion("listaylor", 333),
        ABB(Cancion("ab lardo", 227), _, _),
        ABB(Cancion("la la lambda", 442), _, _),
    ),
)

assert duraderas(abbCanciones, 300) == lista(
    Cancion("listaylor", 333), lista(Cancion("la la lambda", 442), _)
)

assert repertorio(abbCanciones) == lista(
    Cancion("las recursivas", 54),
    lista(
        Cancion("abstracion is fun", 128),
        lista(
            Cancion("el dueño del sistema", 212),
            lista(
                Cancion("ab lardo", 227),
                lista(
                    Cancion("listaylor", 333),
                    lista(Cancion("la la lambda", 442), _),
                ),
            ),
        ),
    ),
)

```
