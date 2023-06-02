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
    #symbol-input {
      text-transform: uppercase;
    }
  </style>
  <script>
    // Global variables
    var chart;
    var datasets = [];
    var maxDataPoints = 100;

    // Handle Enter key press in the input field
    function handleKeyPress(event) {
      if (event.key === 'Enter') {
        event.preventDefault();
        fetchAndDisplayStockData();
      }
    }

    // Fetch and display stock data
    function fetchAndDisplayStockData() {
      // Get the stock symbol from the input field and convert it to uppercase
      var symbol = $("#symbol-input").val().toUpperCase();
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
          // Success callback - handle fetched data and update the chart
          var timeSeriesData = data["Time Series (1min)"];
          var stockData = [];
          for (var timestamp in timeSeriesData) {
            var dataPoint = {
              x: new Date(timestamp),
              y: parseFloat(timeSeriesData[timestamp]["4. close"])
            };
            stockData.push(dataPoint);
          }
          var stockDataset = {
            label: symbol,
            data: stockData,
            backgroundColor: "rgba(0, 123, 255, 0.5)",
            borderColor: "rgba(0, 123, 255, 1)",
            borderWidth: 1
          };
          datasets.push(stockDataset);
          if (datasets.length > maxDataPoints) {
            datasets.shift();
          }
          if (chart) {
            chart.data.datasets = datasets;
            chart.update();
          } else {
            var ctx = document.getElementById("stock-chart").getContext("2d");
            chart = new Chart(ctx, {
              type: "line",
              data: {
                datasets: datasets
              },
              options: {
                responsive: true,
                scales: {
                  x: {
                    type: "time",
                    time: {
                      unit: "minute"
                    }
                  },
                  y: {
                    beginAtZero: false
                  }
                }
              }
            });
          }
        },
        error: function(xhr, status, error) {
          // Error callback - handle error and display error message and definition
          var errorMessage = xhr.responseJSON ? xhr.responseJSON.Note : error;
          var errorDefinition = getErrorDefinition(xhr.status);
          console.log("Failed to fetch stock data. Error: " + errorMessage);
          console.log("Error Definition: " + errorDefinition);
        }
      });
    }

    // Get the definition of an HTTP status code error
    function getErrorDefinition(statusCode) {
      var definitions = {
        400: "Bad Request - The request was invalid or cannot be served.",
        401: "Unauthorized - Authentication is required or has failed.",
        403: "Forbidden - The request is understood, but it has been refused.",
        404: "Not Found - The requested resource could not be found.",
        500: "Internal Server Error - An unexpected condition was encountered."
        // Add more error definitions as needed
      };
      return definitions[statusCode] || "Unknown Error";
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
