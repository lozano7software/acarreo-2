<!DOCTYPE html>
<html lang="es">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Match Camiones y Palas</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f9;
    }

    .header {
      width: 100%;
      height: 100vh;
      background-image: url('https://www.sonda.com/images/default-source/noticias/miner%C3%ADa-.png?sfvrsn=34027d15_1');
      background-size: cover;
      background-position: center;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      color: #ffffff;
      position: relative;
    }

    .header h1 {
      font-size: 50px;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
      margin: 0;
    }

    .content {
      padding: 30px;
    }

    .container {
      max-width: 1000px;
      margin: auto;
      background: #fff;
      padding: 30px;
      /* border-radius: 12px; Eliminar el borde curvado */
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      display: flex;
      flex-direction: column;
      gap: 30px;
    }

    .selection-container {
      display: flex;
      gap: 30px;
      flex-wrap: wrap;
    }

    .list-container {
      flex: 1;
      min-width: 280px;
    }

    .list-container input {
      width: 100%;
      padding: 12px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      /* border-radius: 8px; Eliminar el borde curvado */
      font-size: 16px;
    }

    .list-container ul {
      list-style: none;
      padding: 0;
      border: 1px solid #ccc;
      /* border-radius: 8px; Eliminar el borde curvado */
      max-height: 250px;
      overflow-y: auto;
    }

    .list-container ul li {
      padding: 12px;
      cursor: pointer;
      border-bottom: 1px solid #eee;
      transition: background-color 0.3s;
    }

    .list-container ul li:hover {
      background-color: #f0f0f0;
    }

    .selected-container {
      flex: 1;
      border: 1px solid #ccc;
      /* border-radius: 8px; Eliminar el borde curvado */
      padding: 20px;
      background: #fafafa;
    }

    .selected-container h4 {
      margin-top: 0;
      color: #333;
    }

    .selected-container ul {
      list-style: none;
      padding: 0;
    }

    .selected-container ul li {
      margin: 5px 0;
      padding: 8px;
      background-color: #e1f5fe;
      /* border-radius: 4px; Eliminar el borde curvado */
      cursor: pointer;
    }

    .selected-container ul li:hover {
      background-color: #b3e5fc;
    }

    button {
      width: 100%;
      padding: 14px;
      background-color: #007bff;
      color: #fff;
      border: none;
      /* border-radius: 8px; Eliminar el borde curvado */
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    button:hover {
      background-color: #0056b3;
    }

    .result {
      margin-top: 30px;
      padding: 15px;
      background-color: #e9f7e9;
      border: 1px solid #b6e6b6;
      /* border-radius: 8px; Eliminar el borde curvado */
    }

    .grafica {
      margin-top: 20px;
    }
  </style>

  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>

<body>
  <div class="header">
    <h1>Match Camiones y Palas</h1>
  </div>

  <div class="content">
    <div class="container">
      <div class="selection-container">
        <div class="list-container">
          <h3>Selecciona Camiones:</h3>
          <input type="text" id="filterCamiones" placeholder="Filtrar camiones...">
          <ul id="camionList">
            <li data-name="CAT 797" data-capacidad="371" data-productividad="6.045">CAT 797 (371t)</li>
            <li data-name="KOM980" data-capacidad="392" data-productividad="6.045">KOM980 (392t)</li>
            <li data-name="XDE 320" data-capacidad="300" data-productividad="5.536">XDE 320 (300t)</li>
          </ul>
        </div>
        <div class="list-container">
          <h3>Selecciona Palas:</h3>
          <input type="text" id="filterPalas" placeholder="Filtrar palas...">
          <ul id="palaList">
            <li data-name="Pala CAT 7495" data-capacidad="89.8">Pala CAT 7495 (89.8t)</li>
            <li data-name="LT 2350 HL" data-capacidad="62.4">LT 2350 HL (62.4t)</li>
          </ul>
        </div>
        <div class="selected-container">
          <h4>Camiones Seleccionados:</h4>
          <ul id="selectedCamiones"></ul>
          <h4>Palas Seleccionadas:</h4>
          <ul id="selectedPalas"></ul>
        </div>
      </div>
      <button onclick="calcularMatch()">Calcular Mejor Opción</button>
      <div id="resultados" class="result" style="display: none;"></div>
      <div class="grafica" id="grafica"></div>
    </div>
  </div>

  <script>
    document.getElementById('filterCamiones').addEventListener('input', function () {
      const filter = this.value.toLowerCase();
      const items = document.querySelectorAll('#camionList li');
      items.forEach(item => {
        const text = item.textContent.toLowerCase();
        item.style.display = text.includes(filter) ? '' : 'none';
      });
    });

    document.getElementById('filterPalas').addEventListener('input', function () {
      const filter = this.value.toLowerCase();
      const items = document.querySelectorAll('#palaList li');
      items.forEach(item => {
        const text = item.textContent.toLowerCase();
        item.style.display = text.includes(filter) ? '' : 'none';
      });
    });

    document.querySelectorAll('#camionList li').forEach(item => {
      item.addEventListener('click', function () {
        agregarSeleccion(this, 'selectedCamiones');
      });
    });

    document.querySelectorAll('#palaList li').forEach(item => {
      item.addEventListener('click', function () {
        agregarSeleccion(this, 'selectedPalas');
      });
    });

    function agregarSeleccion(item, targetId) {
      const target = document.getElementById(targetId);
      const clone = item.cloneNode(true);
      clone.addEventListener('click', function () {
        this.remove();
      });
      target.appendChild(clone);
    }

    function calcularMatch() {
      const camiones = Array.from(document.querySelectorAll('#selectedCamiones li'))
        .map(item => ({
          nombre: item.dataset.name,
          capacidad: parseFloat(item.dataset.capacidad),
          productividad: parseFloat(item.dataset.productividad)
        }));

      const palas = Array.from(document.querySelectorAll('#selectedPalas li'))
        .map(item => ({
          nombre: item.dataset.name,
          capacidad: parseFloat(item.dataset.capacidad)
        }));

      if (camiones.length === 0 || palas.length === 0) {
        alert('Por favor selecciona al menos un camión y una pala.');
        return;
      }

      let resultados = [];
      camiones.forEach(camion => {
        palas.forEach(pala => {
          const ratio = (camion.capacidad / pala.capacidad).toFixed(2);
          resultados.push({
            camion: camion.nombre,
            pala: pala.nombre,
            ratio: ratio,
            productividad: camion.productividad
          });
        });
      });

      resultados.sort((a, b) => Math.abs(a.ratio - 1) - Math.abs(b.ratio - 1));

      const resultadosDiv = document.getElementById('resultados');
      resultadosDiv.style.display = 'block';
      resultadosDiv.innerHTML = '<h3>Resultados:</h3>';
      resultados.forEach(res => {
        resultadosDiv.innerHTML += `<p>Camión: ${res.camion}, Pala: ${res.pala}, Ratio: ${res.ratio}, Productividad: ${res.productividad} t/h</p>`;
      });

      generarGrafica(resultados);
    }

    function generarGrafica(resultados) {
      const labels = resultados.map(res => `${res.camion} / ${res.pala}`);
      const data = resultados.map(res => res.productividad);

      const trace = {
        x: labels,
        y: data,
        type: 'bar',
        marker: {
          color: '#007bff'
        }
      };

      const layout = {
        title: 'Productividad por Combinación de Camión y Pala',
        xaxis: {
          title: 'Combinación Camión / Pala'
        },
        yaxis: {
          title: 'Productividad (t/h)',
          rangemode: 'tozero'
        }
      };

      Plotly.newPlot('grafica', [trace], layout);
    }
  </script>

</body>

</html>
