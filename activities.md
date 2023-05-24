<html>
<head>
    <title>Stock Data</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <style>
        .sortable {
            cursor: pointer;
        }
        .favorite {
            color: gold;
            cursor: pointer;
        }
    </style>
    <script>
        var favorites = []; // Array to store the favorite stocks
        var tableRows = []; // Declare tableRows outside the refreshTable function       
        function refreshTable() {
            var symbols = ["MSFT", "AAPL", "GOOGL", "AMZN", "TSLA", "META", "AMD"];  // Replace with your desired stock symbols
            tableRows = []; // Clear tableRows before fetching new data
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
                        var latestTimestamp = getLatestTimestamp(timeSeriesData);
                        var row = timeSeriesData[latestTimestamp];
                        var tableRow = {
                            symbol: stockName,
                            timestamp: latestTimestamp,
                            open: row['1. open'],
                            high: row['2. high'],
                            low: row['3. low'],
                            close: row['4. close'],
                            volume: row['5. volume'],
                            favorite: favorites.includes(stockName) // Check if the stock is already a favorite
                        };
                        tableRows.push(tableRow);
                    },
                    error: function() {
                        console.log("Failed to fetch stock data for symbol: " + symbol);
                    }
                });
            }
            sortTable(tableRows, 0); // Sort the table rows initially by the first column (stock symbol)
            renderTable(tableRows);
        }
        function getLatestTimestamp(timeSeriesData) {
            var timestamps = Object.keys(timeSeriesData);
            return timestamps[0];  // Assumes the timestamps are in descending order
        }
        function renderTable(tableRows) {
            var $tableBody = $("#stock-table tbody");$tableBody.empty();
            for (var i = 0; i < tableRows.length; i++) {
                var row = tableRows[i];
                var favoriteIcon = row.favorite ? '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9733;</span>' : '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9734;</span>';
                var tableRow = "<tr>" +
                    "<td>" + row.symbol + favoriteIcon + "</td>" +
                    "<td>" + row.timestamp + "</td>" +
                    "<td>" + row.open + "</td>" +
                    "<td>" + row.high + "</td>" +
                    "<td>" + row.low + "</td>" +
                    "<td>" + row.close + "</td>" +
                    "<td>" + row.volume + "</td>" +
                    "</tr>";$tableBody.append(tableRow);
            }
        }
        function sortTable(tableRows, columnIndex) {
            tableRows.sort(function(a, b) {
                var aValue = a[Object.keys(a)[columnIndex]];
                var bValue = b[Object.keys(b)[columnIndex]];
                return aValue.localeCompare(bValue);
            });
        }       
        function toggleFavorite(rowIndex) {
            var $table = $("#stock-table");
            var $row = $table.find("tbody tr").eq(rowIndex);
            var stockName = $row.find("td").eq(0).text();
            if (favorites.includes(stockName)) {
                favorites = favorites.filter(function(value) {
                    return value !== stockName;
                });$row.find(".favorite").html("&#9734;");
            } else {
                favorites.push(stockName);$row.find(".favorite").html("&#9733;");
            }
        }
    </script>
</head>
<body>
    <button onclick="refreshTable()">View Data</button>
    <table id="stock-table">
        <thead>
            <tr>
                <th class="sortable" onclick="sortTable(tableRows, 0)">
                    Stock
                </th>
                <th class="sortable" onclick="sortTable(tableRows, 1)">
                    Timestamp
                </th>
                <th class="sortable" onclick="sortTable(tableRows, 2)">
                    Open
                </th>
                <th class="sortable" onclick="sortTable(tableRows, 3)">
                    High
                </th>
                <th class="sortable" onclick="sortTable(tableRows, 4)">
                    Low
                </th>
                <th class="sortable" onclick="sortTable(tableRows, 5)">
                    Close
                </th>
                <th class="sortable" onclick="sortTable(tableRows, 6)">
                    Volume
                </th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with data fetched from the API -->
        </tbody>
    </table>
</body>
</html>