<html>
<head>
    <title>Stock Data</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
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
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
        }

        .input-container label {
            margin-right: 10px;
        }

        .chart-container {
            background-color: #000;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        canvas {
            width: 100%;
            max-width: 800px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="input-container">
            <label for="symbol-input">Stock Symbol:</label>
            <input type="text" id="symbol-input" value="MSFT">
            <button onclick="fetchAndDisplayStockData()">Fetch Data</button>
        </div>
        <div class="chart-container">
            <canvas id="stock-chart"></canvas>
        </div>
    </div>

    <script>
        var chart; // Variable to hold the chart object
        var datasets = []; // Array to hold the chart datasets
        var maxDataPoints = 100; // Maximum number of data points to display on the chart

        function fetchAndDisplayStockData() {
            var symbol = document.getElementById("symbol-input").value; // Get the stock symbol from the input field

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
                    datasets = [];

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
                        openData.push(parseFloat(row['1. open']));
                        highData.push(parseFloat(row['2. high']));
                        lowData.push(parseFloat(row['3. low']));
                        closeData.push(parseFloat(row['4. close']));
                    }

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
                            labels: timestamps,
                            datasets: datasets
                        },
                        options: {
                            responsive: true,
                            maintainAspectRatio: false,
                            scales: {
                                x: {
                                    grid: {
                                        color: 'rgba(255, 255, 255, 0.1)'
                                    },
                                    ticks: {
                                        color: 'rgba(255, 255, 255, 0.8)'
                                    }
                                },
                                y: {
                                    grid: {
                                        color: 'rgba(255, 255, 255, 0.1)'
                                    },
                                    ticks: {
                                        color: 'rgba(255, 255, 255, 0.8)'
                                    }
                                }
                            },
                            plugins: {
                                legend: {
                                    labels: {
                                        color: 'rgba(255, 255, 255, 0.8)'
                                    }
                                }
                            },
                            elements: {
                                point: {
                                    radius: 0,
                                    hoverRadius: 3,
                                    hoverBorderWidth: 1,
                                    backgroundColor: 'rgba(255, 255, 255, 0.8)',
                                    borderColor: 'rgba(255, 255, 255, 0.8)'
                                },
                                line: {
                                    tension: 0.4,
                                    borderColor: 'rgba(255, 255, 255, 0.8)',
                                    backgroundColor: 'rgba(255, 255, 255, 0.1)'
                                }
                            },
                            animation: {
                                duration: 0
                            },
                            responsiveAnimationDuration: 0,
                            layout: {
                                padding: {
                                    top: 10,
                                    right: 10,
                                    bottom: 10,
                                    left: 10
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
</body>
</html>
