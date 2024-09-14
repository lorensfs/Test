# Guía Básica para Usar Markdown en Databricks

Markdown en **Databricks Notebooks** sigue las reglas básicas de Markdown con algunas características adicionales específicas para interactuar mejor con los datos y gráficos en el entorno de Databricks. En esta guía aprenderás las funciones básicas y las adicionales que Databricks ofrece para mejorar la legibilidad y presentación de tu código y datos.

---

## 1. **Encabezados**

Los encabezados en Databricks funcionan como en el Markdown estándar, usando `#` para definir niveles de títulos.

```markdown
# Encabezado 1
## Encabezado 2
### Encabezado 3
```

Resultado:

# Encabezado 1
## Encabezado 2
### Encabezado 3

---

## 2. **Negritas, Cursivas y Combinación de Estilos**

Para aplicar **negritas** o *cursivas*, puedes usar los siguientes símbolos:

- **Negritas**: `**texto**` o `__texto__`
- *Cursivas*: `*texto*` o `_texto_`
- ***Negritas y Cursivas***: `***texto***` o `___texto___`

```markdown
**Texto en negritas**
*Texto en cursivas*
***Texto en negritas y cursivas***
```

Resultado:

**Texto en negritas**  
*Texto en cursivas*  
***Texto en negritas y cursivas***

---

## 3. **Listas**

### Listas ordenadas
Las listas ordenadas se crean con números seguidos de un punto `.`:

```markdown
1. Primer elemento
2. Segundo elemento
3. Tercer elemento
```

Resultado:

1. Primer elemento  
2. Segundo elemento  
3. Tercer elemento

### Listas desordenadas
Las listas desordenadas se crean con `*`, `+` o `-`.

```markdown
- Elemento 1
- Elemento 2
    - Sub-elemento 1
    - Sub-elemento 2
```

Resultado:

- Elemento 1  
- Elemento 2  
    - Sub-elemento 1  
    - Sub-elemento 2

---

## 4. **Enlaces e Imágenes**

### Enlaces

Puedes añadir enlaces utilizando el formato estándar de Markdown:

```markdown
[Texto del enlace](https://www.baccredomatic.com/es-cr)
```

Resultado:  
[Ingresa a la página del BAC](https://www.baccredomatic.com/es-cr)

### Imágenes

Para insertar imágenes, usa `!` seguido de la sintaxis de enlace.

```markdown
![Texto alternativo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTTS2P_37j-X0tKGl6tpk8WDiQVeAMJCrpZnw&s)
```

Resultado:  
![Texto alternativo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTTS2P_37j-X0tKGl6tpk8WDiQVeAMJCrpZnw&s)

---

## 5. **Citas**

Para crear citas, utiliza `>` al inicio de la línea.

```markdown
> Esta es una cita en Databricks
```

Resultado:

> Esta es una cita en Databricks

---

## 6. **Código**

### Código en línea
Usa una comilla invertida \` para insertar código en línea.

```markdown
`Código en línea`
```

Resultado:  
`Código en línea`

### Bloques de código
Para bloques de código, usa tres comillas invertidas \``` antes y después del código. Puedes especificar el lenguaje para resaltar la sintaxis.

Resultado:

```python
print("Hola, Databricks!")
```

---

## 7. **Tablas**

Puedes crear tablas en Databricks usando la misma sintaxis que en Markdown estándar:

```markdown
| Encabezado 1 | Encabezado 2 |
| ------------ | ------------ |
| Celda 1      | Celda 2      |
| Celda 3      | Celda 4      |
```

Resultado:

| Encabezado 1 | Encabezado 2 |
| ------------ | ------------ |
| Celda 1      | Celda 2      |
| Celda 3      | Celda 4      |

---

## 8. **Separadores**

Para crear un separador horizontal, usa tres o más guiones `---`, asteriscos `***`, o guiones bajos `___`.

```markdown
---
***
___
```

Resultado:

---
***
___

---

## 9. **Listas de Tareas**

Las listas de tareas son útiles para marcar elementos completados o pendientes en Databricks Notebooks.

```markdown
- [x] Tarea completada
- [ ] Tarea pendiente
```

Resultado:

- [x] Tarea completada  
- [ ] Tarea pendiente

---

## 10. **Comandos mágicos específicos de Databricks**

Databricks incluye "comandos mágicos" que puedes usar dentro de tus celdas Markdown para ejecutar código SQL, Python, Scala, R, o incluso shell script (`%sh`).

### Ejemplo de un comando mágico de SQL:

```markdown
%sql
SELECT * FROM tabla
```

Este comando ejecutará la consulta SQL directamente en la celda de Markdown y mostrará los resultados en el notebook.

### Ejemplo de un comando mágico de Python:

```markdown
%python
print("Hola desde Databricks!")
```

Esto ejecutará el código Python y mostrará el resultado directamente en la celda.

### Ejemplo de un comando mágico para la shell:

```markdown
%sh
ls -l
```

Esto ejecuta comandos de la shell y muestra el resultado en la celda.

---

## 11. **Mostrar resultados gráficos en Markdown**

Databricks tiene la capacidad de renderizar gráficos en celdas de código Python o SQL. Simplemente ejecuta una consulta o función gráfica en una celda, y Databricks mostrará automáticamente el gráfico generado.

### Ejemplo con Python y Matplotlib:

```python
import matplotlib.pyplot as plt

# Datos de ejemplo
x = [1, 2, 3, 4]
y = [10, 20, 25, 30]

plt.plot(x, y)
plt.title('Gráfico en Databricks')
plt.show()
```

### Ejemplo con SQL:

```markdown
%sql
SELECT columna_x, SUM(columna_y)
FROM tabla
GROUP BY columna_x
```

Databricks renderizará el gráfico automáticamente si los resultados pueden representarse visualmente.

---

## 12. **Tablas de resultados en Markdown**

Databricks también permite ejecutar consultas SQL o código Python que devuelve tablas de datos, y las muestra de manera interactiva. Simplemente escribe el comando mágico correspondiente (`%sql` o `%python`) y la consulta:

```markdown
%sql
SELECT * FROM tabla LIMIT 10;
```

Este comando mostrará los primeros 10 resultados de la tabla en una tabla interactiva en Databricks.

---

## 13. **Fórmulas matemáticas con LaTeX**

Databricks admite la inclusión de fórmulas matemáticas utilizando LaTeX. Para agregar fórmulas, usa el símbolo `$` antes y después de la fórmula.

```markdown
Esta es una fórmula en LaTeX: $E = mc^2$
```

Resultado:  
Esta es una fórmula en LaTeX: $E = mc^2$

---

## 14. **Ocultar contenido en Markdown**

Puedes ocultar o colapsar contenido en Databricks para mantener la visualización más limpia. Usa este formato para colapsar secciones:

```markdown
<details>
  <summary>Haz clic para expandir</summary>
  
  Aquí está el contenido oculto.

</details>
```

Resultado:

<details>
  <summary>Haz clic para expandir</summary>
  
  <img src="https://ih1.redbubble.net/image.1079895004.9123/st,small,507x507-pad,600x600,f8f8f8.jpg" alt="Texto alternativo" width="300" />

  > *By Lorenzo Fuentes Soto*
  
</details>

