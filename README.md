# Proyecto No. 3 – Algoritmo MTF (Move-to-Front) e IMTF

**Análisis y Diseño de Algoritmos – Sección 10**  
Universidad del Valle de Guatemala · 2026  
Cindy Gualim – 21226

---

## Descripción

Implementación del algoritmo **Move-to-Front (MTF)** y su variante mejorada **IMTF** (Improved Move-to-Front con look-ahead), desarrollada como parte del Proyecto No. 3 de Análisis y Diseño de Algoritmos. El proyecto explora el análisis amortizado y competitivo de algoritmos on-line de autoorganización de listas.

---

## Cumplimiento con los requisitos del proyecto

| # | Requisito | Estado |
|---|-----------|--------|
| 1 | MTF sobre secuencia `[0,1,2,3,4] x4` con tabla paso a paso y costo total | ✅ |
| 2 | MTF sobre secuencia alternada `4,3,2,1,0,1,2,3,4,...` con tabla y costo total | ✅ |
| 3 | Secuencia de 20 solicitudes con mínimo costo total y su valor | ✅ |
| 4 | Secuencia de 20 solicitudes con peor costo total (generada con greedy) y su valor | ✅ |
| 5 | MTF con elemento 2 repetido 20 veces, elemento 3 repetido 20 veces, y patrón generalizado | ✅ |
| 6 | IMTF aplicado al mejor y peor caso de MTF, con tablas paso a paso y resumen comparativo | ✅ |

---

## Estructura del repositorio

```
.
├── Proyecto3_MTF.ipynb   # Notebook principal con toda la implementación
└── README.md
```

---

## Implementación

### MTF (Move-to-Front)

```python
def mtf_access(lst: list, element) -> tuple[int, list]:
    """
    Accede a `element` en `lst`.
    Retorna (costo, nueva_lista).
    Costo = posición 1-indexada del elemento.
    Mueve el elemento al frente mediante intercambios adyacentes.
    """
```

```python
def run_mtf(initial_list: list, requests: list, verbose: bool = True) -> int:
    """
    Ejecuta MTF sobre `requests` partiendo de `initial_list`.
    Si verbose=True imprime tabla detallada paso a paso.
    Retorna el costo total de accesos.
    """
```

### IMTF (Improved Move-to-Front – Mohanty & Tripathy)

```python
def imtf_access(lst: list, requests: list, idx: int) -> tuple[int, list]:
    """
    Accede al elemento en requests[idx] usando look-ahead.
    Mueve al frente SOLO si el elemento aparece en los
    próximos (i-1) elementos de la secuencia restante,
    donde i es la posición 1-indexada del elemento en la lista.
    """
```

```python
def run_imtf(initial_list: list, requests: list, verbose: bool = True) -> int:
    """
    Ejecuta IMTF sobre `requests` partiendo de `initial_list`.
    Retorna el costo total de accesos.
    """
```

---

## Resultados principales

### Ejercicio 1 – Secuencia `[0,1,2,3,4]` x4
- **Costo total: 90**
- Tras los primeros 5 accesos la lista queda invertida `[4,3,2,1,0]`, causando que los 15 accesos restantes costen 5 cada uno.

### Ejercicio 2 – Secuencia alternada (17 solicitudes)
- **Costo total: 67**

### Ejercicio 3 – Mínimo costo
- **Secuencia:** `[0, 0, 0, ..., 0]` (20 veces)
- **Costo total mínimo: 20** (cada acceso cuesta 1)

### Ejercicio 4 – Peor caso
- **Secuencia:** `[4, 3, 2, 1, 0, 4, 3, 2, 1, 0, 4, 3, 2, 1, 0, 4, 3, 2, 1, 0]`
- **Costo total peor caso: 100** (cada acceso cuesta 5)
- Generada con algoritmo greedy: en cada paso elige el elemento al final de la lista.

### Ejercicio 5 – Patrón de repetición
Al repetir el mismo elemento `n` veces, el costo total sigue el patrón:

```
costo_total = posicion_inicial + (n - 1)
```

| Elemento | Posición inicial | Fórmula | Costo (n=20) |
|----------|-----------------|---------|--------------|
| 0 | 1 | 1 + 19 | 20 |
| 1 | 2 | 2 + 19 | 21 |
| 2 | 3 | 3 + 19 | 22 |
| 3 | 4 | 4 + 19 | 23 |
| 4 | 5 | 5 + 19 | 24 |

### Ejercicio 6 – MTF vs IMTF

| Caso | MTF | IMTF |
|------|-----|------|
| Mejor caso | 20 | 20 |
| Peor caso | 100 | **60** |

IMTF reduce el costo del peor caso en un **40%** gracias al look-ahead: ningún elemento se mueve al frente porque el siguiente acceso no lo repetirá, manteniendo la lista en `[0,1,2,3,4]` durante toda la secuencia.

