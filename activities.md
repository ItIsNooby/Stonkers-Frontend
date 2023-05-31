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
        var favorites = [];$(document).ready(function() {
            loadFavoritesFromLocalStorage();
            renderTable();
        });
        function loadFavoritesFromLocalStorage() {
            var storedFavorites = localStorage.getItem("favorites");
            if (storedFavorites) {
                favorites = JSON.parse(storedFavorites);
            }
        }
        function renderTable() {
            var tableRows = [];
            for (var i = 0; i < favorites.length; i++) {
                var symbol = favorites[i];
                var stockData = localStorage.getItem(symbol);
                if (stockData) {
                    var data = JSON.parse(stockData);
                    var latestTimestamp = getLatestTimestamp(data);
                    var row = data[latestTimestamp];
                    var tableRow = {
                        symbol: symbol,
                        timestamp: latestTimestamp,
                        open: row['1. open'],
                        high: row['2. high'],
                        low: row['3. low'],
                        close: row['4. close'],
                        volume: row['5. volume'],
                        favorite: true
                    };
                    tableRows.push(tableRow);
                } else {
                    console.log("No stored data found for symbol: " + symbol);
                }
            }
            renderTableRows(tableRows);
        }
        function getLatestTimestamp(timeSeriesData) {
            var timestamps = Object.keys(timeSeriesData);
            return timestamps[0];
        }
        function renderTableRows(tableRows) {
            var $tableBody = $("#stock-table tbody");$tableBody.empty();
            for (var i = 0; i < tableRows.length; i++) {
                var row = tableRows[i];
                var favoriteIcon = '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9733;</span>';
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
            saveFavoritesToLocalStorage();
        }
        function saveFavoritesToLocalStorage() {
            localStorage.setItem("favorites", JSON.stringify(favorites));
        }
    </script>
</head>
<body>
    <table id="stock-table">
        <thead>
            <tr>
                <th class="sortable">Stock</th>
                <th class="sortable">Timestamp</th>
                <th class="sortable">Open</th>
                <th class="sortable">High</th>
                <th class="sortable">Low</th>
                <th class="sortable">Close</th>
                <th class="sortable">Volume</th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with favorite stocks data -->
        </tbody>
    </table>
</body>
</html>