<html>
<head>
    <title>Stock Data</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        function refreshTable() {
            var symbols = ["MSFT", "AAPL", "GOOGL"];  // Replace with your desired stock symbols
            var tableRows = "";
            for (var i = 0; i < symbols.length; i++) {
                var symbol = symbols[i];$.ajax({
                    url: "https://alpha-vantage.p.rapidapi.com/query",
                    headers: {
                        "X-RapidAPI-Key": "86d3c88c86mshe0398d184fbafbdp102e5bjsn36861be80236", // Replace with your RapidAPI key
                        "X-RapidAPI-Host": "alpha-vantage.p.rapidapi.com"
                    },
                    data: {
                        interval: "5min",
                        function: "TIME_SERIES_INTRADAY",
                        symbol: symbol,
                        datatype: "json",
                        output_size: "compact"
                    },
                    async: false,  // Ensures synchronous execution of the requests
                    success: function(data) {
                        var timeSeriesData = data['Time Series (5min)'];
                        var stockName = data['Meta Data']['2. Symbol'];
                        for (var timestamp in timeSeriesData) {
                            if (timeSeriesData.hasOwnProperty(timestamp)) {
                                var row = timeSeriesData[timestamp];
                                tableRows += "<tr>";
                                tableRows += "<td>" + stockName + "</td>";
                                tableRows += "<td>" + timestamp + "</td>";
                                tableRows += "<td>" + row['1. open'] + "</td>";
                                tableRows += "<td>" + row['2. high'] + "</td>";
                                tableRows += "<td>" + row['3. low'] + "</td>";
                                tableRows += "<td>" + row['4. close'] + "</td>";
                                tableRows += "<td>" + row['5. volume'] + "</td>";
                                tableRows += "</tr>";
                            }
                        }
                    },
                    error: function() {
                        console.log("Failed to fetch stock data for symbol: " + symbol);
                    }
                });
            }$("#stock-table tbody").html(tableRows);
        }
</script>
</head>
<body>
    <button onclick="refreshTable()">View Data</button>
    <table id="stock-table">
        <thead>
            <tr>
                <th>Stock</th>
                <th>Timestamp</th>
                <th>Open</th>
                <th>High</th>
                <th>Low</th>
                <th>Close</th>
                <th>Volume</th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with data fetched from the API -->
        </tbody>
    </table>
</body>
</html>