# Examen de visualización de datos · 18%
En este examen deberás consumir **tres endpoints HTTP (GET)**.  
Cada endpoint devuelve una **serie de tiempo**, pero cada una viene con una estructura distinta.

Su tarea es
1. **Hacer el GET Request.**
2. **Analizar la estructura de la respuesta.**
3. **Transformarla a un arreglo de objetos**, donde cada objeto tenga exactamente este formato compatible con Chart.js:

```js
[
  { x: 1, y: 2},
  { x: 2, y: 3},
  ...
]
```
4. **Graficar un scatter chart en Chart.js** usando los datos transformados.

---

# Nivel 1 (33.3%)
Grafique X vs Y. Para eso use el enpoint
```
GET https://example.com/api/talller6/basic
```
Ejemplo de respuesta
```json
[
  {"x": 1, "y": 10},
  {"x": 2, "y": 12},
  {"x": 3, "y": 9}
]
```

---

# Nivel 2 (33.3%)
Grafique time VS amplitude. Para eso use el endpoint
```
GET https://example.com/api/taller6/intermediate
```
Ejemplo de respuesta
```json
{
  "data": {
    "series": [
      { "time": 1000, "amplitude": 0.45 },
      { "time": 2000, "amplitude": 0.51 },
      { "time": 3000, "amplitude": 0.48 }
    ]
  }
}
```

# Nivel 3 (33.3%)
Grafique timestamps VS readings. Para eso use el endpoint
```
GET https://example.com/api/taller6/advanced
```
Ejemplo de respuesta
```json
{
  "meta": {
    "samplingRate": 250,
    "unit": "mV"
  },
  "values": {
    "timestamps": [1000, 1040, 1080, 1120],
    "readings":   [0.1, 0.13, 0.08, 0.15]
  }
}
```
