# Algoritmo de Cálculo de Saldo Financiero

## Descripción

Este algoritmo procesa una lista de transacciones financieras en formato JSON y calcula el saldo final resultante. Comienza con un saldo inicial de 0 y procesa cada transacción según su tipo:

- **Ingreso**: suma el monto al saldo
- **Egreso**: resta el monto del saldo

El algoritmo incluye validaciones robustas para garantizar la integridad de los datos y continuar procesando aunque existan transacciones inválidas.

## Análisis de Complejidad

### Complejidad Temporal

**O(n)** - Lineal

Donde `n` es el número de transacciones en la lista `datos`.

**Justificación:**
- El algoritmo recorre la lista de transacciones exactamente una vez
- Cada transacción se procesa en tiempo constante O(1):
  - Validaciones: comparaciones simples
  - Operaciones aritméticas: suma/resta
  - No hay bucles anidados ni recursión
- Por lo tanto: n transacciones × O(1) por transacción = **O(n)**

### Complejidad Espacial

**O(1)** - Constante

**Justificación:**
- Solo se utiliza una variable `saldo` para almacenar el resultado
- Las validaciones no crean estructuras de datos adicionales
- El espacio utilizado no depende del tamaño de la entrada
- No se realizan copias de la lista de datos

**Nota:** No se cuenta el espacio de entrada (objeto JSON) en el análisis de complejidad espacial, solo el espacio auxiliar utilizado por el algoritmo.

## Casos de Prueba

### Caso 1: Transacciones Mixtas (Ingresos y Egresos)

**Entrada:**
```json
{
  "datos": [
    {"id": 1, "monto": 1000, "tipo": "ingreso"},
    {"id": 2, "monto": 500, "tipo": "egreso"},
    {"id": 3, "monto": 200, "tipo": "ingreso"},
    {"id": 4, "monto": 150, "tipo": "egreso"}
  ]
}
```

**Proceso:**
1. Saldo inicial: 0
2. Transacción 1 (ingreso +1000): 0 + 1000 = 1000
3. Transacción 2 (egreso -500): 1000 - 500 = 500
4. Transacción 3 (ingreso +200): 500 + 200 = 700
5. Transacción 4 (egreso -150): 700 - 150 = 550

**Salida esperada:** `550`

---

### Caso 2: Solo Egresos (Saldo Negativo)

**Entrada:**
```json
{
  "datos": [
    {"id": 1, "monto": 300, "tipo": "egreso"},
    {"id": 2, "monto": 450, "tipo": "egreso"},
    {"id": 3, "monto": 100, "tipo": "egreso"}
  ]
}
```

**Proceso:**
1. Saldo inicial: 0
2. Transacción 1 (egreso -300): 0 - 300 = -300
3. Transacción 2 (egreso -450): -300 - 450 = -750
4. Transacción 3 (egreso -100): -750 - 100 = -850

**Salida esperada:** `-850`

---

### Caso 3: Lista Vacía

**Entrada:**
```json
{
  "datos": []
}
```

**Proceso:**
1. Saldo inicial: 0
2. No hay transacciones para procesar
3. Se retorna el saldo inicial

**Salida esperada:** `0`

---

### Caso 4: Datos Inválidos

**Entrada:**
```json
{
  "datos": [
    {"id": 1, "monto": 1000, "tipo": "ingreso"},
    {"id": 2, "monto": -500, "tipo": "egreso"},
    {"id": 3, "monto": "abc", "tipo": "ingreso"},
    {"id": 4, "monto": 200, "tipo": "pago"},
    {"id": 5, "monto": 300, "tipo": "egreso"}
  ]
}
```

**Proceso:**
1. Saldo inicial: 0
2. Transacción 1 (válida, ingreso +1000): 0 + 1000 = 1000
3. Transacción 2 (inválida, monto negativo): ignorada → saldo = 1000
4. Transacción 3 (inválida, monto no numérico): ignorada → saldo = 1000
5. Transacción 4 (inválida, tipo desconocido): ignorada → saldo = 1000
6. Transacción 5 (válida, egreso -300): 1000 - 300 = 700

**Salida esperada:** `700`

**Validaciones aplicadas:**
- Monto debe ser numérico y positivo
- Tipo debe ser exactamente "ingreso" o "egreso"
- Transacciones inválidas se ignoran sin detener el procesamiento

---
