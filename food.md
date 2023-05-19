<!DOCTYPE html>
<html>
<head>
    <title>Stock Data</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        var chart; // Variable to hold the chart object

        function fetchAndDisplayStockData() {
            var symbol = $("#symbol-input").val(); // Get the stock symbol from the input field
            
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
                    // Destroy the existing chart if it exists
                    if (chart) {
                        chart.destroy();
                    }
                    
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
                    
                    // Create the chart using Chart.js
                    var ctx = document.getElementById("stock-chart").getContext("2d");
                    chart = new Chart(ctx, {
                        type: 'line',
                        data: {
                            labels: timestamps,
                            datasets: [{
                                label: 'Open',
                                data: openData,
                                borderColor: 'rgba(255, 99, 132, 1)',
                                fill: false
                            }, {
                                label: 'High',
                                data: highData,
                                borderColor: 'rgba(54, 162, 235, 1)',
                                fill: false
                            }, {
                                label: 'Low',
                                data: lowData,
                                borderColor: 'rgba(75, 192, 192, 1)',
                                fill: false
                            }, {
                                label: 'Close',
                                data: closeData,
                                borderColor: 'rgba(153, 102, 255, 1)',
                                fill: false
                            }]
                        },
                        options: {
                            responsive: true,
                            maintainAspectRatio: false
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
    <input type="text" id="symbol-input" value="MSFT">
    <button onclick="fetchAndDisplayStockData()">Fetch Data</button>
    <canvas id="stock-chart"></canvas>
</body>
</html>
