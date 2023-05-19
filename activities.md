<html>
<head>
    <title>Stock Data</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        function refreshTable() {
            // Make a GET request to fetch new data from the API
            $.ajax({
                url: "https://alpha-vantage.p.rapidapi.com/query",
                headers: {
                    "X-RapidAPI-Key": "86d3c88c86mshe0398d184fbafbdp102e5bjsn36861be80236", // Replace with your RapidAPI key
                    "X-RapidAPI-Host": "alpha-vantage.p.rapidapi.com"
                },
                data: {
                    interval: "5min",
                    function: "TIME_SERIES_INTRADAY",
                    symbol: "MSFT",
                    datatype: "json",
                    output_size: "compact"
                },
                success: function(data) {
                    // Extract the time series data
                    var timeSeriesData = data['Time Series (5min)'];
                    // Generate the HTML table rows dynamically
                    var tableRows = "";
                    for (var timestamp in timeSeriesData) {
                        if (timeSeriesData.hasOwnProperty(timestamp)) {
                            var row = timeSeriesData[timestamp];
                            tableRows += "<tr>";
                            tableRows += "<td>" + timestamp + "</td>";
                            tableRows += "<td>" + row['1. open'] + "</td>";
                            tableRows += "<td>" + row['2. high'] + "</td>";
                            tableRows += "<td>" + row['3. low'] + "</td>";
                            tableRows += "<td>" + row['4. close'] + "</td>";
                            tableRows += "<td>" + row['5. volume'] + "</td>";
                            tableRows += "</tr>";
                        }
                    }
                    // Update the table body with the new data
                    $("#stock-table tbody").html(tableRows);
                },
                error: function() {
                    console.log("Failed to fetch stock data.");
                }
            });
        }
    </script>
</head>
<body>
    <button onclick="refreshTable()">View Data</button>
    <table id="stock-table">
        <thead>
            <tr>
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