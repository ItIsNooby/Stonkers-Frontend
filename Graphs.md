<!DOCTYPE html>
<html>
<head>
  <title>Main Stock Graph</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    #stock-chart {
      max-width: 1200px;
      max-height: 1200px;
    }
    button {
      background-color: lavender;
      color: lavender;
    }
  </style>
  <script>
    var chart;
    var datasets = [];
    var maxDataPoints = 100;
    
    function handleKeyPress(event) {
      if (event.key === 'Enter') {
        event.preventDefault();
        fetchAndDisplayStockData();
      }
    }
    
    function fetchAndDisplayStockData() {
      var symbol = $("#symbol-input").val().toUpperCase(); // Capitalize the input text
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
          // Clear the existing datasets
          datasets = [];
          // Extract the time series data
          var timeSeriesData = data['Time Series (1min)'];
          var timestamps = Object.keys(timeSeriesData);
          var openData = [];
          var highData = [];
          var lowData = [];
          var closeData = [];
          // Extract the OHLC (open, high, low, close) data for the chart
          timestamps.reverse(); // Reverse the order of timestamps and data arrays
          timestamps.forEach(function(timestamp) {
            var row = timeSeriesData[timestamp];
            openData.push(row['1. open']);
            highData.push(row['2. high']);
            lowData.push(row['3. low']);
            closeData.push(row['4. close']);
          });
          // Trim the data arrays to the maximum number of data points
          if (timestamps.length > maxDataPoints) {
            timestamps = timestamps.slice(timestamps.length - maxDataPoints);
            openData = openData.slice(openData.length - maxDataPoints);
            highData = highData.slice(highData.length - maxDataPoints);
            lowData = lowData.slice(lowData.length - maxDataPoints);
            closeData = closeData.slice(closeData.length - maxDataPoints);
          }
          // Create the chart datasets
          datasets.push({
            label: 'Open',
            data: openData,
            borderColor: 'rgba(255, 99, 132, 1)',
            fill: false
          });
          datasets.push({
            label: 'High',
            data: highData,
            borderColor: 'rgba(54, 162, 235, 1)',
            fill: false
          });
          datasets.push({
            label: 'Low',
            data: lowData,
            borderColor: 'rgba(75, 192, 192, 1)',
            fill: false
          });
          datasets.push({
            label: 'Close',
            data: closeData,
            borderColor: 'rgba(153, 102, 255, 1)',
            fill: false
          });
          // Destroy the existing chart (if any)
          if (chart) {
            chart.destroy();
          }
          // Create a new chart with the updated data
          var ctx = document.getElementById('stock-chart').getContext('2d');
          chart = new Chart(ctx, {
            type: 'line',
            data: {
              labels: timestamps.map(function(timestamp) {
                return timestamp.split(' ')[1]; // Extract the time from the timestamp
              }),
              datasets: datasets
            },
            options: {
              responsive: true,
              maintainAspectRatio: true,
              aspectRatio: 1,
              scales: {
                x: {
                  display: true,
                  title: {
                    display: true,
                    text: 'Time'
                  },
                  ticks: {
                    maxRotation: 0
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
</head>
<body>
  <div>
    <label for="symbol-input">Stock Symbol:</label>
    <input type="text" id="symbol-input" onkeypress="handleKeyPress(event)">
    <button onclick="fetchAndDisplayStockData()">Fetch Data</button>
  </div>
  <canvas id="stock-chart"></canvas>
</body>
</html>
