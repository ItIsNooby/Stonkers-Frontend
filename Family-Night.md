<html>
<head>
  <title>Stock Data</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
      var charts = []; 
      var datasets = [];
      var maxDataPoints = 100;

      function fetchAndDisplayStockData(symbol, chartIndex) {
          localStorage.setItem('symbol-' + chartIndex, symbol);
          $.ajax({
              url: "https://alpha-vantage.p.rapidapi.com/query",
              headers: {
                  "X-RapidAPI-Key": "86d3c88c86mshe0398d184fbafbdp102e5bjsn36861be80236",
                  "X-RapidAPI-Host": "alpha-vantage.p.rapidapi.com"
              },
              data: {
                  interval: "1min",
                  function: "TIME_SERIES_INTRADAY",
                  symbol: symbol,
                  datatype: "json",
                  output_size: "compact"
              },
              success: function(data) {
                  // The rest of your success function here...
              },
              error: function() {
                  console.log("Failed to fetch stock data.");
              }
          });
      }

      function loadAndFetchStockData(chartIndex) {
          var symbol = localStorage.getItem('symbol-' + chartIndex);
          if (symbol) {
              document.getElementById('symbol-input-' + (chartIndex + 1)).value = symbol;
              fetchAndDisplayStockData(symbol, chartIndex);
          }
      }

      window.onload = function() {
          for (var i = 0; i < 4; i++) {
              loadAndFetchStockData(i);
          }
      };
  </script>
  <style>
      .chart-container {
          display: inline-block;
          width: 800px;
          height: 500px;
          margin-right: 50px;
      }

      .chart-canvas {
          max-width: 100%;
          max-height: 100%;
      }

      button {
          background-color: lavender;
          color: lavender;
      }
  </style>
</head>
<body>
  <div>
      <label for="symbol-input-1">Stock Symbol 1:</label>
      <input type="text" id="symbol-input-1">
      <button onclick="fetchAndDisplayStockData(document.getElementById('symbol-input-1').value, 0)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-0" class="chart-canvas"></canvas>
  </div>

  <div>
      <label for="symbol-input-2">Stock Symbol 2:</label>
      <input type="text" id="symbol-input-2">
      <button onclick="fetchAndDisplayStockData(document.getElementById('symbol-input-2').value, 1)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-1" class="chart-canvas"></canvas>
  </div>

  <div>
      <label for="symbol-input-3">Stock Symbol 3:</label>
      <input type="text" id="symbol-input-3">
      <button onclick="fetchAndDisplayStockData(document.getElementById('symbol-input-3').value, 2)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-2" class="chart-canvas"></canvas>
  </div>

  <div>
      <label for="symbol-input-4">Stock Symbol 4:</label>
      <input type="text" id="symbol-input-4">
      <button onclick="fetchAndDisplayStockData(document.getElementById('symbol-input-4').value, 3)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-3" class="chart-canvas"></canvas>
  </div>
</body>
</html>
.
