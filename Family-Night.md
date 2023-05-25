<html>
<head>
    <title>Stock Data</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .input-container {
            display: flex;
            justify-content: space-around;
            align-items: center;
            margin-bottom: 20px;
        }

        .input-container label {
            margin-right: 10px;
        }

        .chart-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
        }

        .chart-container {
            width: 45%;
            background-color: #000;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .fetch-button {
            display: block;
            margin: 0 auto;
            margin-bottom: 20px;
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .fetch-button:hover {
            background-color: #45a049;
        }

        canvas {
            width: 100%;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="input-container">
            <label for="symbol-input-1">Stock Symbol 1:</label>
            <input type="text" id="symbol-input-1" value="MSFT">
            <label for="symbol-input-2">Stock Symbol 2:</label>
            <input type="text" id="symbol-input-2" value="AAPL">
            <label for="symbol-input-3">Stock Symbol 3:</label>
            <input type="text" id="symbol-input-3" value="GOOGL">
            <label for="symbol-input-4">Stock Symbol 4:</label>
            <input type="text" id="symbol-input-4" value="AMZN">
            <button class="fetch-button" onclick="fetchAndDisplayStockData()">Fetch Data</button>
        </div>
        <div class="chart-row">
            <div class="chart-container">
                <canvas id="stock-chart-1"></canvas>
            </div>
            <div class="chart-container">
                <canvas id="stock-chart-2"></canvas>
            </div>
        </div>
        <div class="chart-row">
            <div class="chart-container">
                <canvas id="stock-chart-3"></canvas>
            </div>
            <div class="chart-container">
                <canvas id="stock-chart-4"></canvas>
            </div>
        </div>
    </div>

    <script>
        var maxDataPoints = 100; // Maximum number of data points to display on the chart
        var charts = []; // Array to hold the chart objects

        function fetchAndDisplayStockData() {
            // Get the stock symbols from the input fields
            var symbols = [
                document.getElementById("symbol-input-1").value,
                document.getElementById("symbol-input-2").value,
                document.getElementById("symbol-input-3").value,
                document.getElementById("symbol-input-4").value
            ];

            symbols.forEach(function(symbol, index) {
                // Make a GET request to fetch new data from the API
                $.ajax({
                    url: "https://alpha-vantage.p.rapidapi.com/query",
                    headers: {
                        "X-RapidAPI-Key": "YOUR_RAPIDAPI_KEY",
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
                        // Extract the time series data
                        var timeSeriesData = data['Time Series (1min)'];
                        var timestamps = Object.keys(timeSeriesData);
                        var closeData = [];

                        // Extract the close data for the chart
                        for (var i = timestamps.length - 1; i >= 0; i--) {
                            var timestamp = timestamps[i];
                            var row = timeSeriesData[timestamp];
                            closeData.push(parseFloat(row['4. close']));
                        }

                        // Trim the data arrays to the maximum number of data points
                        if (timestamps.length > maxDataPoints) {
                            timestamps = timestamps.slice(timestamps.length - maxDataPoints);
                            closeData = closeData.slice(closeData.length - maxDataPoints);
                        }

                        // Destroy the existing chart (if any)
                        if (charts[index]) {
                            charts[index].destroy();
                        }

                        // Create a new chart with the updated data
                        var ctx = document.getElementById('stock-chart-' + (index + 1)).getContext('2d');
                        charts[index] = new Chart(ctx, {
                            type: 'line',
                            data: {
                                labels: timestamps,
                                datasets: [{
                                    label: symbol,
                                    data: closeData,
                                    borderColor: 'rgba(75, 192, 192, 1)',
                                    fill: false
                                }]
                            },
                            options: {
                                responsive: true,
                                scales: {
                                    x: {
                                        display: true,
                                        title: {
                                            display: true,
                                            text: 'Time'
                                        }
                                    },
                                    y: {
                                        display: true,
                                        title: {
                                            display: true,
                                            text: 'Price'
                                        }
                                    }
                                }
                            }
                        });
                    },
                    error: function() {
                        console.log("Failed to fetch stock data for " + symbol + ".");
                    }
                });
            });
        }
    </script>
</body>
</html>
