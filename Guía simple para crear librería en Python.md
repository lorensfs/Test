
## **Creación de una Librería en Python en VS Code**

### 1. **Configurar el Directorio del Proyecto**
Crea una nueva carpeta para tu proyecto (librería) en Python y ábrela en VS Code.

```bash
mkdir mi_libreria_python
cd mi_libreria_python
code .
```

### 2. **Organizar la Estructura del Proyecto**
La estructura típica de tu proyecto debería verse algo así:

```
mi_libreria_python/
    mi_libreria/       # Carpeta que contiene tu paquete
        __init__.py    # Hace que esta carpeta sea un paquete
        mi_modulo.py   # Módulo de ejemplo
    tests/             # Pruebas unitarias para tu librería
    setup.py           # Script de empaquetado
    README.md          # Documentación del proyecto
    requirements.txt   # Dependencias externas (opcional)
```

### 3. **Crear el Paquete Python**
Dentro de la carpeta `mi_libreria/`, crea tus módulos de Python, como `mi_modulo.py`.

Ejemplo:
```python
# mi_libreria/mi_modulo.py
def saludar():
    return "¡Hola desde mi librería personalizada!"
```

### 4. **Crear `setup.py`**
Este archivo se usa para empaquetar e instalar tu librería. Un ejemplo básico de `setup.py` podría verse así:

```python
from setuptools import setup, find_packages

setup(
    name="mi_libreria_python",  # Nombre de tu librería
    version="0.1.0",  # Número de versión
    description="Una librería Python personalizada",  # Descripción
    author="Tu Nombre",
    packages=find_packages(),  # Incluye automáticamente todos los paquetes
    install_requires=[],  # Dependencias, si las hay
)
```

### 5. **(Opcional) Añadir `requirements.txt`**
Si tu librería tiene dependencias, agrégalas en un archivo `requirements.txt`. Puedes instalarlas localmente con:

```bash
pip install -r requirements.txt
```

### 6. **Construir la Librería**
Para empaquetar tu librería como un archivo `.whl` o `.tar.gz`, utiliza los siguientes comandos:

```bash
# Asegúrate de tener setuptools y wheel instalados
pip install setuptools wheel

# Construir la librería
python setup.py sdist bdist_wheel
```

Esto generará una carpeta `dist/` que contiene el archivo `.whl` o `.tar.gz` de tu librería.

### 7. **Probar tu Librería**
Antes de instalarla o distribuirla, puedes probarla localmente ejecutando scripts de Python o utilizando pruebas unitarias en la carpeta `tests/`.

> *By Lorenzo Fuentes Soto*