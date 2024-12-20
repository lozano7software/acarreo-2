# acarreo-2
<div class="container">
  <h1>Match Camiones y Palas</h1>
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
  <canvas id="grafica" width="900" height="400"></canvas>
</div>
