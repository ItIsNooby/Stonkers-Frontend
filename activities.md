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
        function refreshTable() {
            var symbols = ["MSFT", "AAPL", "GOOGL", "AMZN", "TSLA", "META", "AMD"];  // Replace with your desired stock symbols
            var tableRows = [];
            for (var i = 0; i < symbols.length; i++) {
<<<<<<< HEAD
                var symbol = symbols[i];$.ajax({
=======
                var symbol = symbols[i]; $.ajax({
>>>>>>> 1f73a19bc244c77d20dc8faa60c6c5cc8ab3f8cb
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
            renderTable(tableRows);
        }
<<<<<<< HEAD
function getLatestTimestamp(timeSeriesData) {
            var timestamps = Object.keys(timeSeriesData);
            return timestamps[0];  // Assumes the timestamps are in descending order
        }
function renderTable(tableRows) {
            var $tableBody = $("#stock-table tbody");$tableBody.empty();
for (var i = 0; i < tableRows.length; i++) {
=======
        function getLatestTimestamp(timeSeriesData) {
            var timestamps = Object.keys(timeSeriesData);
            return timestamps[0];  // Assumes the timestamps are in descending order
        }
        function renderTable(tableRows) {
            var $tableBody = $("#stock-table tbody"); $tableBody.empty();
            for (var i = 0; i < tableRows.length; i++) {
>>>>>>> 1f73a19bc244c77d20dc8faa60c6c5cc8ab3f8cb
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
function sortTable(columnIndex) {
            var $table = $("#stock-table");
            var rows = $table.find("tbody tr").toArray();
            rows.sort(function(a, b) {
                var aValue = $(a).find("td").eq(columnIndex).text();
                var bValue = $(b).find("td").eq(columnIndex).text();
                if (columnIndex === 0) {
                    return aValue.localeCompare(bValue);  // Sort alphabetically for stock column
                } else {
                    return parseFloat(bValue) - parseFloat(aValue);  // Sort numerically for other columns
                }
            });$table.find("tbody").empty().append(rows);
<<<<<<< HEAD
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
=======
>>>>>>> 1f73a19bc244c77d20dc8faa60c6c5cc8ab3f8cb
        }
    </script>
</head>
<body>
    <button onclick="refreshTable()">View Data</button>
    <table id="stock-table">
        <thead>
            <tr>
                <th class="sortable" onclick="sortTable(0)">
                    Stock
                </th>
                <th class="sortable" onclick="sortTable(1)">
                    Timestamp
                </th>
                <th class="sortable" onclick="sortTable(2)">
                    Open
                </th>
                <th class="sortable" onclick="sortTable(3)">
                    High
                </th>
                <th class="sortable" onclick="sortTable(4)">
                    Low
                </th>
                <th class="sortable" onclick="sortTable(5)">
                    Close
                </th>
                <th class="sortable" onclick="sortTable(6)">
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