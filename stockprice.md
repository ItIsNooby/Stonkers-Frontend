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
            var symbols = ["AAPL", "MSFT", "GOOGL", "AMZN", "TSLA"]; // Replace with your desired stock symbols
            var tableRows = [];
            for (var i = 0; i < symbols.length; i++) {
                var symbol = symbols[i];$.ajax({
                    url: "https://nasdaq-stock-pricing.p.rapidapi.com/Stock/GetPrice/AAPL",
                    headers: {
                        "X-RapidAPI-Key": "b731fee7a5mshf2b6608334c0b07p13bf5fjsn09fcf5df26f4", // Replace with your RapidAPI key
                        "X-RapidAPI-Host": "nasdaq-stock-pricing.p.rapidapi.com"
                    },
                    data: {
                        symbol: symbol
                    },
                    async: false, // Ensures synchronous execution of the requests
                    success: function(data) {
                        var stockName = data[0]['symbol'];
                        var stockData = data[0]['data'];
                        var latestData = stockData[stockData.length - 1];
                        var tableRow = {
                            symbol: stockName,
                            timestamp: latestData[0],
                            open: latestData[1],
                            high: latestData[2],
                            low: latestData[3],
                            close: latestData[4],
                            volume: latestData[5],
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
        function sortTable(columnIndex) {
            var $table = $("#stock-table");
            var rows = $table.find("tbody tr").toArray();
            rows.sort(function(a, b) {
                var aValue = $(a).find("td").eq(columnIndex).text();
                var bValue = $(b).find("td").eq(columnIndex).text();
                if (columnIndex === 0) {
                    return aValue.localeCompare(bValue); // Sort alphabetically for stock column
                } else {
                    return parseFloat(bValue) - parseFloat(aValue); // Sort numerically for other columns
                }
            });$table.find("tbody").empty().append(rows);
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
