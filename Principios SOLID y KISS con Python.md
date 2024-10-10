## **Guía: Principios SOLID con Python**

### **1. Principio de Responsabilidad Única (SRP)**

El **principio de responsabilidad única** establece que una clase debe tener **una sola responsabilidad**. Esto significa que la clase debe tener **una sola razón para cambiar**. Si una clase tiene más de una responsabilidad, terminará con múltiples razones para cambiar, lo que la hará más difícil de mantener.

#### **Problema:**
Imagina que tienes una clase `Pedido` que maneja tanto la gestión de productos (agregar productos, calcular el total) como el procesamiento de pagos (tarjeta de débito, crédito). Si la forma de procesar los pagos cambia, tendrás que modificar la clase `Pedido`, lo que no debería ocurrir.

#### **Solución:**
Separaremos las responsabilidades en dos clases diferentes:
1. **Pedido**: Gestiona la lista de productos y su precio total.
2. **ProcesadorPagos**: Gestiona el procesamiento del pago.

#### **Implementación**

```python
from typing import List

class Pedido:
    """
    Clase Pedido que representa una orden de compra.

    Attributes:
        items (List[str]): Lista de nombres de los productos.
        cantidades (List[int]): Lista de cantidades por producto.
        precios (List[float]): Lista de precios por producto.
        estado (str): Estado del pedido, puede ser "abierto" o "pagado".
    """

    def __init__(self) -> None:
        self.items: List[str] = []
        self.cantidades: List[int] = []
        self.precios: List[float] = []
        self.estado: str = "abierto"

    def agregar_item(self, nombre: str, cantidad: int, precio: float) -> None:
        """
        Agrega un producto al pedido.

        Args:
            nombre (str): Nombre del producto.
            cantidad (int): Cantidad del producto.
            precio (float): Precio unitario del producto.
        """
        self.items.append(nombre)
        self.cantidades.append(cantidad)
        self.precios.append(precio)

    def precio_total(self) -> float:
        """
        Calcula el precio total del pedido.

        Returns:
            float: El total a pagar basado en las cantidades y precios de los productos.
        """
        total: float = 0
        for i in range(len(self.precios)):
            total += self.cantidades[i] * self.precios[i]
        return total


class ProcesadorPagos:
    """
    Clase ProcesadorPagos que maneja los pagos.
    """

    def pagar_debito(self, pedido: Pedido, codigo_seguridad: str) -> None:
        """
        Procesa un pago por débito.

        Args:
            pedido (Pedido): Instancia de la clase Pedido.
            codigo_seguridad (str): Código de seguridad de la tarjeta.
        """
        print("Procesando pago con débito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"

    def pagar_credito(self, pedido: Pedido, codigo_seguridad: str) -> None:
        """
        Procesa un pago por crédito.

        Args:
            pedido (Pedido): Instancia de la clase Pedido.
            codigo_seguridad (str): Código de seguridad de la tarjeta.
        """
        print("Procesando pago con crédito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


# Ejemplo de uso:
pedido = Pedido()
pedido.agregar_item("Teclado", 1, 50.0)
pedido.agregar_item("SSD", 1, 150.0)
pedido.agregar_item("Cable USB", 2, 5.0)

print(pedido.precio_total())  # Muestra el total del pedido

procesador = ProcesadorPagos()
procesador.pagar_debito(pedido, "123456")
```

---

### **2. Principio Abierto/Cerrado (OCP)**

El **principio abierto/cerrado** establece que las entidades de software (clases, módulos, funciones) deben estar **abiertas para su extensión** pero **cerradas para modificación**. Esto significa que podemos extender una clase para agregar nueva funcionalidad sin modificar el código existente.

#### **Problema:**
Si agregamos más métodos de pago a la clase `ProcesadorPagos`, tendríamos que modificar la clase cada vez que se añada un nuevo tipo de pago.

#### **Solución:**
Usaremos **herencia y abstracción** para permitir que nuevas clases extiendan la funcionalidad de procesamiento de pagos sin modificar la clase base.

#### **Implementación**

```python
from abc import ABC, abstractmethod
from typing import List

class Pedido:
    """
    Clase Pedido que representa una orden de compra.

    Attributes:
        items (List[str]): Lista de nombres de los productos.
        cantidades (List[int]): Lista de cantidades por producto.
        precios (List[float]): Lista de precios por producto.
        estado (str): Estado del pedido, puede ser "abierto" o "pagado".
    """

    def __init__(self) -> None:
        self.items: List[str] = []
        self.cantidades: List[int] = []
        self.precios: List[float] = []
        self.estado: str = "abierto"

    def agregar_item(self, nombre: str, cantidad: int, precio: float) -> None:
        """
        Agrega un producto al pedido.

        Args:
            nombre (str): Nombre del producto.
            cantidad (int): Cantidad del producto.
            precio (float): Precio unitario del producto.
        """
        self.items.append(nombre)
        self.cantidades.append(cantidad)
        self.precios.append(precio)

    def precio_total(self) -> float:
        """
        Calcula el precio total del pedido.

        Returns:
            float: El total a pagar basado en las cantidades y precios de los productos.
        """
        total: float = 0
        for i in range(len(self.precios)):
            total += self.cantidades[i] * self.precios[i]
        return total


class ProcesadorPagos(ABC):
    """
    Clase abstracta que define la interfaz para los procesadores de pago.
    """

    @abstractmethod
    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        """
        Método abstracto para procesar el pago.

        Args:
            pedido (Pedido): Instancia de la clase Pedido.
            codigo_seguridad (str): Código de seguridad para la autenticación.
        """
        pass


class ProcesadorDebito(ProcesadorPagos):
    """
    Clase que extiende ProcesadorPagos para procesar pagos con tarjeta de débito.
    """

    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        """
        Procesa un pago por débito.

        Args:
            pedido (Pedido): Instancia de la clase Pedido.
            codigo_seguridad (str): Código de seguridad de la tarjeta.
        """
        print("Procesando pago con débito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


class ProcesadorCredito(ProcesadorPagos):
    """
    Clase que extiende ProcesadorPagos para procesar pagos con tarjeta de crédito.
    """

    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        """
        Procesa un pago por crédito.

        Args:
            pedido (Pedido): Instancia de la clase Pedido.
            codigo_seguridad (str): Código de seguridad de la tarjeta.
        """
        print("Procesando pago con crédito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


# Ejemplo de uso:
pedido = Pedido()
pedido.agregar_item("Teclado", 1, 50.0)
pedido.agregar_item("SSD", 1, 150.0)

procesador = ProcesadorDebito()
procesador.pagar(pedido, "123456")
```
---

### **3. Principio de Sustitución de Liskov (LSP)**

El **principio de sustitución de Liskov** dice que **las subclases deben poder sustituir a sus clases base** sin alterar la funcionalidad del programa. Esto significa que si tienes una clase base y una subclase, deberías poder usar cualquiera de ellas de manera intercambiable, sin cambiar el comportamiento esperado.

#### **Ejemplo:**
Queremos añadir un procesador de pago vía PayPal sin cambiar la lógica existente de la clase base `ProcesadorPagos`.

#### **Implementación:**

```python
from abc import ABC, abstractmethod

class Pedido:
    def __init__(self) -> None:
        self.items: List[str] = []
        self.cantidades: List[int] = []
        self.precios: List[float] = []
        self.estado: str = "abierto"

    def agregar_item(self, nombre: str, cantidad: int, precio: float) -> None:
        self.items.append(nombre)
        self.cantidades.append(cantidad)
        self.precios.append(precio)

    def precio_total(self) -> float:
        total: float = 0
        for i in range(len(self.precios)):
            total += self.cantidades[i] * self.precios[i]
        return total


class ProcesadorPagos(ABC):
    """
    Clase abstracta que define la interfaz para los procesadores de pago.
    """

    @abstractmethod
    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        pass


class ProcesadorDebito(ProcesadorPagos):
    """
    Procesa pagos mediante tarjeta de débito.
    """
    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        print("Procesando pago con débito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


class ProcesadorCredito(ProcesadorPagos):
    """
    Procesa pagos mediante tarjeta de crédito.
    """
    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        print("Procesando pago con crédito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


class ProcesadorPaypal(ProcesadorPagos):
    """
    Procesa pagos mediante PayPal.
    """
    def pagar(self, pedido: Pedido, email: str) -> None:
        print("Procesando pago con PayPal")
        print(f"Verificando email: {email}")
        pedido.estado = "pagado"


# Ejemplo de uso
pedido = Pedido()
pedido.agregar_item("Teclado", 1, 50.0)
pedido.agregar_item("SSD", 1, 150.0)

# Usamos ProcesadorPaypal sin cambiar la lógica del Pedido
procesador = ProcesadorPaypal()
procesador.pagar(pedido, "mi@correo.com")
```

Aquí, la clase `ProcesadorPaypal` sustituye a `ProcesadorPagos` sin cambiar el comportamiento del programa. Esto cumple con el principio de Liskov.

---

### **4. Principio de Segregación de Interfaces (ISP)**

El **principio de segregación de interfaces** dice que **es mejor tener interfaces pequeñas y específicas** en lugar de una interfaz grande que obligue a las clases a implementar métodos que no necesitan. Esto ayuda a que las clases sean más fáciles de mantener y extender.

#### **Problema:**
No todas las formas de pago requieren autenticación por SMS. Si forzamos a todas las clases de pago a implementar la autenticación por SMS, esto violaría el principio.

#### **Solución:**
Dividimos las interfaces para que solo las clases que realmente necesitan autenticación por SMS implementen esos métodos.

#### **Implementación:**

```python
from abc import ABC, abstractmethod

class ProcesadorPagos(ABC):
    """
    Clase abstracta para procesar pagos sin autenticación adicional.
    """

    @abstractmethod
    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        pass


class ProcesadorPagosConAutenticacion(ProcesadorPagos):
    """
    Clase abstracta que incluye autenticación adicional (por ejemplo, SMS).
    """

    @abstractmethod
    def autenticar_sms(self, codigo: str) -> None:
        pass


class ProcesadorDebito(ProcesadorPagosConAutenticacion):
    """
    Procesa pagos por débito y requiere autenticación por SMS.
    """
    def __init__(self, codigo_seguridad: str) -> None:
        self.codigo_seguridad = codigo_seguridad
        self.autenticado: bool = False

    def autenticar_sms(self, codigo: str) -> None:
        print(f"Autenticando con código SMS: {codigo}")
        self.autenticado = True

    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        if not self.autenticado:
            raise Exception("No autorizado")
        print("Procesando pago con débito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


class ProcesadorCredito(ProcesadorPagos):
    """
    Procesa pagos con tarjeta de crédito sin autenticación adicional.
    """
    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        print("Procesando pago con crédito")
        print(f"Verificando código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


# Ejemplo de uso
pedido = Pedido()
pedido.agregar_item("Teclado", 1, 50.0)

procesador = ProcesadorDebito("123456")
procesador.autenticar_sms("456789")  # Solo es necesario para débito
procesador.pagar(pedido, "123456")
```

En este caso, separamos las interfaces para que solo los procesadores de pago que realmente necesitan autenticación (como el débito) implementen los métodos relacionados con la autenticación por SMS.

---

### **5. Principio de Inversión de Dependencias (DIP)**

El **principio de inversión de dependencias** dice que **las clases de alto nivel no deben depender de clases de bajo nivel**. Ambas deberían depender de **abstracciones**. Esto reduce el acoplamiento y facilita el mantenimiento del código.

#### **Problema:**
Si el procesador de pagos depende directamente de una clase específica de autorización (como `AutorizarSMS`), estaremos acoplando el código. Esto va en contra de la flexibilidad y reutilización del código.

#### **Solución:**
Creamos una abstracción para la autorización (`Autorizar`), y las clases concretas (`AutorizarSMS`, `AutorizarGoogle`) implementan esta abstracción.

#### **Implementación:**

```python
from abc import ABC, abstractmethod

class Autorizar(ABC):
    """
    Clase abstracta para autorización.
    """

    @abstractmethod
    def esta_autorizado(self) -> bool:
        pass


class AutorizarSMS(Autorizar):
    """
    Autorización mediante código SMS.
    """
    def __init__(self) -> None:
        self.autorizado: bool = False

    def verificar_codigo(self, codigo: str) -> None:
        print(f"Verificando código SMS: {codigo}")
        self.autorizado = True

    def esta_autorizado(self) -> bool:
        return self.autorizado


class AutorizarGoogle(Autorizar):
    """
    Autorización mediante Google Authenticator.
    """
    def __init__(self) -> None:
        self.autorizado: bool = False

    def verificar_codigo(self, codigo: str) -> None:
        print(f"Verificando código de Google Auth: {codigo}")
        self.autorizado = True

    def esta_autorizado(self) -> bool:
        return self.autorizado


class ProcesadorPagos:
    """
    Clase que procesa los pagos, depende de la abstracción Autorizar.
    """
    def __init__(self, autorizador: Autorizar) -> None:
        self.autorizador = autorizador

    def pagar(self, pedido: Pedido, codigo_seguridad: str) -> None:
        if not self.autorizador.esta_autorizado():
            raise Exception("No autorizado")
        print(f"Procesando pago con código de seguridad: {codigo_seguridad}")
        pedido.estado = "pagado"


# Ejemplo de uso
pedido = Pedido()
pedido.agregar_item("Teclado", 1, 50.0)

autorizador = AutorizarSMS()
autorizador.verificar_codigo("123456")

procesador = ProcesadorPagos(autorizador)
procesador.pagar(pedido, "123456")
```

En este ejemplo, la clase `ProcesadorPagos` depende de una abstracción (`Autorizar`), lo que le permite trabajar con diferentes tipos de autorizadores (SMS, Google, etc.) sin estar acoplada a una implementación específica.

---
## **Guía Sencilla para el Principio KISS (Keep It Simple, Stupid)**

El principio **KISS** es un concepto de diseño que promueve la simplicidad en el desarrollo de software. Se basa en la idea de que el sistema y su código deben ser lo más simples y directos posibles. Un diseño simple es más fácil de entender, mantener y extender.

### **Objetivo del Principio KISS:**
- **Evitar complejidad innecesaria.**
- **Facilitar la comprensión y mantenimiento del código.**
- **Asegurar que el código sea fácil de modificar en el futuro.**

### **Ejemplos del Principio KISS:**

#### **1. Evitar código demasiado complejo o excesivo**
A veces, los desarrolladores complican el código al intentar hacer que cubra todas las posibilidades desde el principio, lo que lleva a soluciones innecesariamente complejas.

#### **Ejemplo de código sin seguir el principio KISS (demasiado complejo):**

```python
def calcular_descuento(precio: float, tipo_cliente: str) -> float:
    if tipo_cliente == "estudiante" or tipo_cliente == "docente":
        if tipo_cliente == "estudiante":
            if precio > 100:
                descuento = 0.15
            else:
                descuento = 0.05
        else:
            if precio > 200:
                descuento = 0.10
            else:
                descuento = 0.07
    elif tipo_cliente == "regular":
        descuento = 0.02
    else:
        descuento = 0
    return precio - (precio * descuento)
```

Este código tiene demasiadas condiciones anidadas, lo que lo hace difícil de leer y de mantener. Podríamos simplificarlo para que sea más claro.

#### **Ejemplo siguiendo el principio KISS (más simple):**

```python
def calcular_descuento(precio: float, tipo_cliente: str) -> float:
    descuentos = {
        "estudiante": 0.15 if precio > 100 else 0.05,
        "docente": 0.10 if precio > 200 else 0.07,
        "regular": 0.02
    }
    descuento = descuentos.get(tipo_cliente, 0)
    return precio - (precio * descuento)
```

#### **Por qué este ejemplo sigue KISS:**
- **Simplificación del código:** Hemos eliminado las condiciones anidadas y usamos un diccionario para gestionar los descuentos de manera más clara y directa.
- **Fácil de entender:** El flujo del programa es claro y es fácil ver cómo funciona el cálculo de descuentos.
- **Fácil de mantener:** Si se necesita cambiar un descuento, solo se actualiza el diccionario.

### **2. Evitar métodos largos y complicados**

#### **Ejemplo sin KISS (método largo y complicado):**

```python
def procesar_pedido(productos: List[dict]) -> None:
    total = 0
    for producto in productos:
        precio = producto['precio']
        cantidad = producto['cantidad']
        if 'descuento' in producto:
            precio -= precio * producto['descuento']
        total += precio * cantidad
    print(f"Total a pagar: {total}")
    if total > 500:
        print("Descuento adicional aplicado")
        total *= 0.95
    print(f"Total final: {total}")
```

#### **Ejemplo con KISS (métodos más pequeños y claros):**

```python
def calcular_total(productos: List[dict]) -> float:
    total = 0
    for producto in productos:
        total += calcular_precio_producto(producto)
    return total

def calcular_precio_producto(producto: dict) -> float:
    precio = producto['precio']
    if 'descuento' in producto:
        precio -= precio * producto['descuento']
    return precio * producto['cantidad']

def aplicar_descuento(total: float) -> float:
    if total > 500:
        print("Descuento adicional aplicado")
        return total * 0.95
    return total

def procesar_pedido(productos: List[dict]) -> None:
    total = calcular_total(productos)
    total = aplicar_descuento(total)
    print(f"Total final: {total}")
```

#### **Por qué este ejemplo sigue KISS:**
- **Métodos cortos y claros:** Cada método hace una tarea específica, lo que facilita la comprensión de su funcionalidad.
- **Mejor mantenibilidad:** Si es necesario cambiar la lógica del cálculo del precio o del descuento, cada tarea está encapsulada en un método separado, lo que facilita los cambios sin afectar al resto del código.

### **Consejos para aplicar KISS:**

- **Divide las funciones:** Si una función está haciendo más de una tarea, divídela en funciones más pequeñas y específicas.
- **Evita la sobreingeniería:** No intentes resolver problemas que aún no existen. Mantén el código simple y añade complejidad solo cuando sea absolutamente necesario.
- **Usa estructuras claras:** Utiliza diccionarios, listas y otras estructuras de datos cuando simplifiquen el código.

> *By Lorenzo Fuentes Soto*
