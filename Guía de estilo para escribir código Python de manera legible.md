# Guía de estilo para escribir código Python de manera clara, legible y coherente

PEP8 es la guía de estilo oficial para escribir código Python. Seguir estas convenciones no solo hace que tu código sea más legible, sino que también mejora la colaboración en proyectos con otros programadores. A continuación, se presenta una guía organizada con las normas básicas de PEP8, escrita en español, y explicaciones para facilitar el aprendizaje.

---

### Instalación de herramientas para aplicar PEP8 en Databricks Notebooks

Para facilitar la aplicación de PEP8 en tu código, puedes usar herramientas automáticas como **`black`**, que ayuda a formatear el código automáticamente según estas reglas.

#### Python black formatter library en Databricks

**Python Black** es una herramienta popular para el formateo automático de código en Python, que sigue las reglas de PEP8. En **Databricks Notebooks**, es posible utilizar Black para formatear el código directamente.

- **Estado**: Esta funcionalidad está en **Vista Pública Preliminar**.

En **Databricks Runtime 11.3 LTS y superior**, las bibliotecas `black` y `tokenize-rt` ya vienen preinstaladas, por lo que puedes usar el formateador directamente sin necesidad de instalarlas. En versiones anteriores, puedes instalarlas ejecutando el siguiente comando en tu notebook o en el clúster:

```python
%pip install black==22.3.0 tokenize-rt==4.2.1
```

Para archivos y notebooks en carpetas de Git en Databricks, puedes configurar el formateador de Python basado en un archivo `pyproject.toml`. Simplemente crea este archivo en el directorio raíz de la carpeta Git y edita la sección `[tool.black]` para aplicar la configuración.

#### Cómo formatear celdas de Python y SQL
Puedes activar el formateador en Databricks de las siguientes maneras:

- **Atajo de teclado**: Presiona `Cmd + Shift + F`.
- **Menú de contexto**: Haz clic derecho en una celda de Python o SQL y selecciona "Format Python" o "Format SQL".
- **Menú Editar**: Selecciona una celda y elige `Edit > Format Cell(s)`.

También puedes formatear múltiples celdas o todas las celdas Python y SQL de un notebook seleccionando `Edit > Format Notebook`.

> **Limitaciones**: Black impone la regla de PEP8 de utilizar 4 espacios para la indentación. Además, no admite el formateo de cadenas de Python incrustadas en una función SQL UDF, ni SQL incrustado en una función Python UDF.

---

### Normas de PEP8

#### 1. **Identación (Indentation)**

La identación debe ser de **4 espacios** por nivel de indentación. Nunca se deben usar tabulaciones.

```python
def funcion():
    if True:
        print("Hola, mundo")
```

#### 2. **Documentación del módulo**

Preferiblemente, y si ayuda a aclarar el propósito, cada archivo Python debe comenzar con un comentario o una cadena de texto (docstring) que describa el propósito del módulo.

```python
"""
Este módulo realiza operaciones matemáticas básicas.
"""
```

#### 3. **Importaciones**

Las importaciones siempre deben estar en la parte superior del archivo y en líneas separadas. No utilices comodines (`*`) en las importaciones.

```python
import os
import sys

# Correcto: Especifica claramente qué módulos importas.
from os import path, stat
```

#### 4. **Nombres de Variables y Métodos**

- **Variables**: Los nombres de las variables deben estar en **snake_case**.

```python
variable_en_snake_case = 1
```

- **Métodos**: Los nombres de los métodos también deben estar en **snake_case**.

```python
def calcular_promedio():
    pass
```

- **Constantes**: Las constantes deben escribirse en **MAYÚSCULAS**.

```python
CONSTANTE_GLOBAL = 100
```

#### 5. **Nombres de Clases**

Las clases deben seguir el estilo **PascalCase** (es decir, cada palabra comienza con mayúscula).

```python
class MiClaseBase:
    pass
```

#### 6. **Cadenas de texto multilinea**

Cuando utilices cadenas de texto (strings) de varias líneas, siempre usa triples comillas dobles `"""`:

```python
multi_line_string = """
Este es un string que ocupa
varias líneas.
"""
```

#### 7. **Uso de espacios en blanco (Whitespace)**

Asegúrate de usar correctamente los espacios en blanco:
- No debe haber espacio antes de una coma o paréntesis de apertura.
- Coloca un espacio después de comas en listas y diccionarios.

```python
# Correcto
foo(ham[1], {'eggs': 2})
foo = (0,)

# Incorrecto
foo( ham[ 1 ], { 'eggs' : 2 } )
```

#### 8. **Operadores Binarios**

Coloca los operadores binarios (como `+`, `-`, `*`, `/`) con un espacio antes y después del operador.

```python
i = i + 1
x = x * 2 - 1
```

#### 9. **Métodos en clases**

Dentro de una clase, los métodos de instancia deben llevar `self` como primer parámetro, y los métodos de clase deben llevar `cls` como primer parámetro. **Además, separa los métodos dentro de una clase por una línea en blanco.**

```python
class MiClase:
    def metodo_de_instancia(self):
        pass

    @classmethod
    def metodo_de_clase(cls):
        pass
```

#### 10. **Espacios entre funciones y clases**

Se deben dejar **dos líneas en blanco** entre funciones y clases a nivel de **módulo**.

```python
def sumar(a: int, b: int) -> int:
    """
    Suma dos números enteros.

    Args:
        a (int): El primer número.
        b (int): El segundo número.

    Returns:
        int: La suma de a y b.
    """
    return a + b


def restar(a: int, b: int) -> int:
    """
    Resta dos números enteros.

    Args:
        a (int): El primer número.
        b (int): El segundo número.

    Returns:
        int: La resta de a y b.
    """
    return a - b
```

#### 11. **Valores por defecto en funciones**

Cuando definas funciones con argumentos que tienen valores por defecto, no debes incluir espacios alrededor del símbolo `=`.

```python
# Correcto
def funcion_compleja(real, imag=0.0):
    return magia(r=real, i=imag)

# Incorrecto
def funcion_compleja(real, imag = 0.0):
    return magia(r = real, i = imag)
```

#### 12. **Comparación con None**

Para comparar una variable con `None`, usa `is` o `is not`, en lugar de `==` o `!=`.

```python
# Correcto
if x is None:
    pass

# Incorrecto
if x == None:
    pass
```

#### 13. **Ejemplo de comparación de valores**

Un ejemplo para comparar objetos y valores, mostrando las diferencias entre `==` y `is`.

```python
def comparar_valores(x, y):
    if x == y:
        print("x y y tienen el mismo valor")
    else:
        print("x y y tienen valores diferentes")

    if x is y:
        print("x y y son el mismo objeto en memoria")
    else:
        print("x y y son objetos diferentes")
```

#### 14. **Documentación y Tipos**

Es importante documentar las funciones y clases con docstrings. Además, es recomendable utilizar anotaciones de tipos para mejorar la claridad del código.

```python
from typing import List

class Solucion:
    """
    Clase que resuelve el problema de Two Sum.

    Métodos:
    -------
    two_sum(nums: List[int], target: int) -> List[int]:
        Encuentra los índices de dos números en 'nums' que suman el 'target'.
    """

    def two_sum(self, nums: List[int], target: int) -> List[int]:
        """
        Encuentra dos números en la lista 'nums' que sumen el valor 'target'.

        Args:
            nums (List[int]): Lista de enteros.
            target (int): Suma objetivo que buscamos.

        Returns:
            List[int]: Lista con los índices de los dos números que suman 'target'.
        """
        num_map = {}
        for i, num in enumerate(nums):
            complemento = target - num
            if complemento in num_map:
                return [num_map[complemento], i]
            num_map[num] = i
        return []
```

> *By Lorenzo Fuentes Soto*
