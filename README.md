<div id="graficoFlujo" style="width: 100%; max-width: 700px; margin: auto;"></div>

<!-- Modal -->
<div id="modalInfo" style="display:none; position:fixed; top:10%; left:10%; width:80%; background:white; border-radius:10px; padding:20px; box-shadow: 0 0 15px rgba(0,0,0,0.4); z-index:1000;">
  <h2 id="modalTitulo"></h2>
  <p id="modalContenido"></p>
  <button onclick="document.getElementById('modalInfo').style.display='none'" style="padding:10px 20px; margin-top:20px;">Cerrar</button>
</div>

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  const pasos = [
    { titulo: "1. Descarga de Imágenes", detalle: "Imágenes Landsat 8 descargadas desde USGS Earth Explorer. Se recomienda seleccionar escenas libres de nubes." },
    { titulo: "2. Clasificación Supervisada", detalle: "Clasificación en base a bandas 6, 5, 2. Uso de entrenamiento supervisado para diferenciar coberturas." },
    { titulo: "3. Radianza Espectral", detalle: "Se calcula la radianza TOA usando coeficientes multiplicadores y aditivos del MTL." },
    { titulo: "4. Temperatura de Brillo", detalle: "Conversión de radianza a temperatura del sensor usando la fórmula de Planck simplificada." },
    { titulo: "5. Proporción de Vegetación (PV)", detalle: "Se calcula desde el NDVI para estimar cobertura vegetal." },
    { titulo: "6. Emisividad (ε)", detalle: "La emisividad se calcula con base en el PV para cada píxel." },
    { titulo: "7. LST", detalle: "Temperatura Superficial Terrestre usando la fórmula: LST = Tb / (1 + (λ * Tb / ρ) * ln(ε))." },
    { titulo: "8. NDVI", detalle: "Índice de vegetación normalizado. Fórmula: (NIR - Red) / (NIR + Red)." },
    { titulo: "9. NDBI", detalle: "Índice de construcción calculado como (SWIR - NIR) / (SWIR + NIR)." },
    { titulo: "10. ICU", detalle: "Cálculo de la Isla de Calor Urbana: ICU = LST - LST promedio." }
  ];

  const labels = pasos.map(p => p.titulo);
  const text = pasos.map(p => p.detalle);

  const data = [{
    type: "pie",
    labels: labels,
    values: Array(labels.length).fill(1),
    textinfo: "label",
    textposition: "inside",
    hoverinfo: "label+text",
    marker: { colors: ["#8dd3c7", "#ffffb3", "#bebada", "#fb8072", "#80b1d3", "#fdb462", "#b3de69", "#fccde5", "#d9d9d9", "#bc80bd"] }
  }];

  const layout = {
    title: "Flujo de procesamiento para el cálculo de LST e ICU",
    showlegend: false
  };

  Plotly.newPlot('graficoFlujo', data, layout);

  // Manejo de clics
  document.getElementById('graficoFlujo').on('plotly_click', function(data){
    const punto = data.points[0].pointIndex;
    document.getElementById('modalTitulo').innerText = pasos[punto].titulo;
    document.getElementById('modalContenido').innerText = pasos[punto].detalle;
    document.getElementById('modalInfo').style.display = 'block';
  });
</script>
