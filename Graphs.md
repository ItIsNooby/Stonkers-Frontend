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
</head>
<body>
    <div>
        <label for="symbol-input">Stock Symbol:</label>
        <input type="text" id="symbol-input">
        <button onclick="fetchAndDisplayStockData()">Fetch Data</button>
    </div>
    <canvas id="stock-chart"></canvas>
    <script>
        var chart; // Variable to hold the chart object
        var datasets = []; // Array to hold the chart datasets
        var maxDataPoints = 100; // Maximum number of data points to display on the chart, Get the stock symbol from the input field, Make a GET request to fetch new data from the API
        function fetchAndDisplayStockData() {
            var symbol = $("#symbol-input").val(); $.ajax({
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
                            maintainAspectRatio: true, // Set maintainAspectRatio to true
                            aspectRatio: 1, // Set the aspect ratio to 1 for a square chart
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
</body>
</html>
