# Transformación de Estructura JSON Cliente-Orden
---
## Descripción del Algoritmo

Este algoritmo transforma una estructura JSON que contiene información de cliente y orden en un formato estandarizado de salida.

1. **Validación de entrada**: Verifica que los datos de entrada sean válidos
2. **Extracción segura**: Obtiene datos de objetos anidados con manejo de valores nulos
3. **Transformación de campos**: Mapea y renombra propiedades según el formato requerido
4. **Combinación de datos**: Concatena nombre y apellido en un campo único
5. **Valores por defecto**: Aplica valores seguros cuando faltan datos

### Mapeo de Campos

| Entrada | Salida | Transformación |
|---------|--------|----------------|
| `orden.id` | `orderId` | Copia directa (default: 0) |
| `cliente.nombre` + `cliente.apellido` | `customerName` | Concatenación con espacio (default: "") |
| `orden.monto` | `total` | Copia directa (default: 0) |

---

## Análisis de Complejidad

### Complejidad Temporal: **O(1)**

El algoritmo ejecuta en tiempo constante porque:
- Todas las operaciones son accesos directos a propiedades de objetos
- La concatenación de strings es de longitud fija (nombre + apellido)
- No contiene bucles, iteraciones ni recursión
- Las validaciones son comparaciones simples que se ejecutan en tiempo constante

**Justificación**: Independientemente del tamaño de los valores de entrada, el número de operaciones permanece constante.

### Complejidad Espacial: **O(1)**

El algoritmo usa espacio constante porque:
- Se crea un único objeto de salida de tamaño fijo (3 propiedades)
- Variables temporales son constantes en número (nombre, apellido, orderId, total)
- No hay estructuras de datos que crezcan proporcionalmente a la entrada
- El espacio usado es predecible y no depende del tamaño de entrada

**Justificación**: El espacio de memoria usado es fijo y no escala con la entrada.

---

## Casos de Prueba

### Caso 1: Entrada Completa Estándar

**Descripción**: Todos los campos presentes y válidos

**Entrada**:
```json
{
  "cliente": {
    "nombre": "Juan",
    "apellido": "Pérez"
  },
  "orden": {
    "id": 123,
    "monto": 500
  }
}
```

**Salida Esperada**:
```json
{
  "orderId": 123,
  "customerName": "Juan Pérez",
  "total": 500
}
```

**Validación**: Mapeo correcto de todos los campos, concatenación exitosa

---

### Caso 2: Cliente Sin Apellido

**Descripción**: Campo apellido vacío o faltante

**Entrada**:
```json
{
  "cliente": {
    "nombre": "Ana",
    "apellido": ""
  },
  "orden": {
    "id": 456,
    "monto": 750
  }
}
```

**Salida Esperada**:
```json
{
  "orderId": 456,
  "customerName": "Ana",
  "total": 750
}
```

**Validación**: Manejo correcto de apellido vacío, sin espacios adicionales

---

### Caso 3: Campos Faltantes

**Descripción**: Algunos campos no existen en la estructura

**Entrada**:
```json
{
  "cliente": {
    "nombre": "Carlos"
  },
  "orden": {
    "id": 789
  }
}
```

**Salida Esperada**:
```json
{
  "orderId": 789,
  "customerName": "Carlos",
  "total": 0
}
```

**Validación**: Valores por defecto aplicados correctamente (apellido vacío, monto 0)

---

### Caso 4: Campos Nulos

**Descripción**: Campos presentes pero con valores nulos

**Entrada**:
```json
{
  "cliente": {
    "nombre": "María",
    "apellido": null
  },
  "orden": {
    "id": 101,
    "monto": null
  }
}
```

**Salida Esperada**:
```json
{
  "orderId": 101,
  "customerName": "María",
  "total": 0
}
```

**Validación**: Manejo apropiado de valores nulos, aplicación de defaults

---

### Caso 5: Estructura Inválida

**Descripción**: Entrada completamente inválida o nula

**Entrada**:
```json
null
```

**Salida Esperada**:
```json
{
  "orderId": 0,
  "customerName": "",
  "total": 0
}
```

**Validación**: Retorno seguro con objeto válido con valores por defecto

---