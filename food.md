<html>
<head>
    <title>Stock Data</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        function fetchStockData() {
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
                    symbol: "MSFT",
                    datatype: "json",
                    output_size: "compact"
                },
                success: function(data) {
                    // Extract the time series data
                    var timeSeriesData = data['Time Series (1min)'];
                    var timestamps = Object.keys(timeSeriesData);
                    var openPrices = [];
                    var highPrices = [];
                    var lowPrices = [];
                    var closePrices = [];
                    var volumes = [];
                    
                    // Collect the data points for each category
                    for (var timestamp in timeSeriesData) {
                        if (timeSeriesData.hasOwnProperty(timestamp)) {
                            var row = timeSeriesData[timestamp];
                            openPrices.push(parseFloat(row['1. open']));
                            highPrices.push(parseFloat(row['2. high']));
                            lowPrices.push(parseFloat(row['3. low']));
                            closePrices.push(parseFloat(row['4. close']));
                            volumes.push(parseFloat(row['5. volume']));
                        }
                    }
                    
                    // Create the chart using Chart.js
                    var ctx = document.getElementById("stock-chart").getContext("2d");
                    var chart = new Chart(ctx, {
                        type: 'line',
                        data: {
                            labels: timestamps,
                            datasets: [
                                {
                                    label: 'Open',
                                    data: openPrices,
                                    borderColor: 'rgba(255, 99, 132, 1)',
                                    backgroundColor: 'rgba(255, 99, 132, 0.2)'
                                },
                                {
                                    label: 'High',
                                    data: highPrices,
                                    borderColor: 'rgba(54, 162, 235, 1)',
                                    backgroundColor: 'rgba(54, 162, 235, 0.2)'
                                },
                                {
                                    label: 'Low',
                                    data: lowPrices,
                                    borderColor: 'rgba(255, 206, 86, 1)',
                                    backgroundColor: 'rgba(255, 206, 86, 0.2)'
                                },
                                {
                                    label: 'Close',
                                    data: closePrices,
                                    borderColor: 'rgba(75, 192, 192, 1)',
                                    backgroundColor: 'rgba(75, 192, 192, 0.2)'
                                },
                                {
                                    label: 'Volume',
                                    data: volumes,
                                    borderColor: 'rgba(153, 102, 255, 1)',
                                    backgroundColor: 'rgba(153, 102, 255, 0.2)'
                                }
                            ]
                        },
                        options: {
                            responsive: true,
                            scales: {
                                x: {
                                    display: true,
                                    title: {
                                        display: true,
                                        text: 'Timestamp'
                                    }
                                },
                                y: {
                                    display: true,
                                    title: {
                                        display: true,
                                        text: 'Value'
                                    }
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

        // Fetch stock data immediately on page load
        fetchStockData();

        // Fetch stock data every 1 minute
        setInterval(fetchStockData, 60000);
    </script>
</head>
<body>
    <canvas id="stock-chart"></canvas>
</body>
</html>
