```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scatter con Chart.js</title>
  <style>
    .graph {
        width: 50%;
    }
  </style>
</head>
<body>
  <h2>Ejemplo Scatter Plot con Chart.js</h2>
  <div class="graph">
    <canvas id="myChart" width="400" height="200"></canvas>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    // Datos dummy
    const dataPoints = [
      { t: "1", y: 1.23 },
      { t: "2", y: -1.23 },
      { t: "3", y: 3.23 },
      { t: "4", y: 4.21 }
    ];

    const scatterData = dataPoints.map(p => ({ x: p.t, y: p.y }));

    // Crear el gráfico
    const ctx = document.getElementById('myChart').getContext('2d');
    new Chart(ctx, {
      type: 'scatter',
      data: {
        datasets: [{
          label: 'Valores t vs y',
          data: scatterData,
          showLine: true, 
        }]
      },
    });
  </script>
</body>
</html>
```
