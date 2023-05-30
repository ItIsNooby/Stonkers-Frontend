<html>
<head>
  <title>Stock Data</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
      var charts = []; // Array to hold the chart objects
      var datasets = []; // Array to hold the chart datasets
      var maxDataPoints = 100; // Maximum number of data points to display on the chart

      function fetchAndDisplayStockData(symbol, chartIndex) {
          // Make a GET request to fetch new data from the API
          $.ajax({
              url: "https://alpha-vantage.p.rapidapi.com/query",
              headers: {
                  "X-RapidAPI-Key": "86d3c88c86mshe0398d184fbafbdp102e5bjsn36861be80236", // Replace with your RapidAPI key
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
                  // Clear the existing datasets
                  datasets[chartIndex] = [];
                  // Extract the time series data
                  var timeSeriesData = data['Time Series (1min)'];
                  var timestamps = Object.keys(timeSeriesData);
                  var openData = [];
                  var highData = [];
                  var lowData = [];
                  var closeData = [];
                  // Extract the OHLC (open, high, low, close) data for the chart
                  for (var i = timestamps.length - 1; i >= 0; i--) {
                      var timestamp = timestamps[i];
                      var row = timeSeriesData[timestamp];
                      openData.push(row['1. open']);
                      highData.push(row['2. high']);
                      lowData.push(row['3. low']);
                      closeData.push(row['4. close']);
                  }
                  // Reverse the order of timestamps and data arrays
                  timestamps.reverse();
                  openData.reverse();
                  highData.reverse();
                  lowData.reverse();
                  closeData.reverse();
                  // Trim the data arrays to the maximum number of data points
                  if (timestamps.length > maxDataPoints) {
                      timestamps = timestamps.slice(timestamps.length - maxDataPoints);
                      openData = openData.slice(openData.length - maxDataPoints);
                      highData = highData.slice(highData.length - maxDataPoints);
                      lowData = lowData.slice(lowData.length - maxDataPoints);
                      closeData = closeData.slice(closeData.length - maxDataPoints);
                  }
                  // Create the chart datasets
                  datasets[chartIndex].push({
                      label: 'Open',
                      data: openData,
                      borderColor: 'rgba(255, 99, 132, 1)',
                      fill: false
                  });
                  datasets[chartIndex].push({
                      label: 'High',
                      data: highData,
                      borderColor: 'rgba(54, 162, 235, 1)',
                      fill: false
                  });
                  datasets[chartIndex].push({
                      label: 'Low',
                      data: lowData,
                      borderColor: 'rgba(75, 192, 192, 1)',
                      fill: false
                  });
                  datasets[chartIndex].push({
                      label: 'Close',
                      data: closeData,
                      borderColor: 'rgba(153, 102, 255, 1)',
                      fill: false
                  });
                  // Destroy the existing chart (if any)
                  if (charts[chartIndex]) {
                      charts[chartIndex].destroy();
                  }
                  // Create a new chart with the updated data
                  var ctx = document.getElementById('chart-' + chartIndex).getContext('2d');
                  charts[chartIndex] = new Chart(ctx, {
                      type: 'line',
                      data: {
                          labels: timestamps.map(function(timestamp) {
                              return timestamp.split(' ')[1]; // Extract the time from the timestamp
                          }),
                          datasets: datasets[chartIndex]
                      },
                      options: {
                          responsive: true,
                          maintainAspectRatio: false, // Set maintainAspectRatio to false
                          scales: {
                              x: {
                                  display: true,
                                  title: {
                                      display: true,
                                      text: 'Time'
                                  },
                                  ticks: {
                                      maxRotation: 0 // Prevent label rotation on the X-axis
                                  }
                              },
                              y: {
                                  display: true,
                                  title: {
                                      display: true,
                                      text: 'Price'
                                  }
                              }
                          },
                          layout: {
                              padding: {
                                  left: 50,
                                  right: 50,
                                  top: 50,
                                  bottom: 50
                              }
                          }
                      }
                  });
              },
              error: function() {
                  console.log("Failed to fetch stock data.");
              }
          });
      }
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
      <button onclick="fetchAndDisplayStockData($('#symbol-input-1').val(), 0)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-0" class="chart-canvas"></canvas>
  </div>

  <div>
      <label for="symbol-input-2">Stock Symbol 2:</label>
      <input type="text" id="symbol-input-2">
      <button onclick="fetchAndDisplayStockData($('#symbol-input-2').val(), 1)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-1" class="chart-canvas"></canvas>
  </div>

  <div>
      <label for="symbol-input-3">Stock Symbol 3:</label>
      <input type="text" id="symbol-input-3">
      <button onclick="fetchAndDisplayStockData($('#symbol-input-3').val(), 2)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-2" class="chart-canvas"></canvas>
  </div>

  <div>
      <label for="symbol-input-4">Stock Symbol 4:</label>
      <input type="text" id="symbol-input-4">
      <button onclick="fetchAndDisplayStockData($('#symbol-input-4').val(), 3)">Fetch Data</button>
  </div>
  <div class="chart-container">
      <canvas id="chart-3" class="chart-canvas"></canvas>
  </div>
</body>
</html>
