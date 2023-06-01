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
            refreshTable();
        });
        function refreshTable() {
            var symbols = ["MSFT", "AAPL", "GOOGL", "AMZN"];
            var tableRows = [];
            for (var i = 0; i < symbols.length; i++) {
                var symbol = symbols[i];$.ajax({
                    url: "https://alpha-vantage.p.rapidapi.com/query",
                    headers: {
                        "X-RapidAPI-Key": "86d3c88c86mshe0398d184fbafbdp102e5bjsn36861be80236",
                        "X-RapidAPI-Host": "alpha-vantage.p.rapidapi.com"
                    },
                    data: {
                        interval: "5min",
                        function: "TIME_SERIES_INTRADAY",
                        symbol: symbol,
                        datatype: "json",
                        output_size: "compact"
                    },
                    async: false,
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
                            favorite: favorites.includes(stockName)
                        };
                        tableRows.push(tableRow);
                    },
                    error: function() {
                        console.log("Failed to fetch stock data for symbol: " + symbol);
                    }
                });
            }
            renderTable(tableRows);
            saveStockDataToLocalStorage(tableRows);
        }
        function getLatestTimestamp(timeSeriesData) {
            var timestamps = Object.keys(timeSeriesData);
            return timestamps[0];
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
        function toggleFavorite(rowIndex) {
            var $table = $("#stock-table");
            var $row = $table.find("tbody tr").eq(rowIndex);
            var stockSymbol = $row.find("td").eq(0).text();
            var $favoriteIcon = $row.find(".favorite");
            if ($favoriteIcon.hasClass("favorite")) {$favoriteIcon.removeClass("favorite");$favoriteIcon.html("&#9734;");
                favorites = favorites.filter(function(symbol) {
                    return symbol !== stockSymbol;
                });
            } else {$favoriteIcon.addClass("favorite");$favoriteIcon.html("&#9733;");
                favorites.push(stockSymbol);
            }
            saveFavoritesToLocalStorage();
        }
        function saveFavoritesToLocalStorage() {
            localStorage.setItem("favorites", JSON.stringify(favorites));
        }
        function loadFavoritesFromLocalStorage() {
            var storedFavorites = localStorage.getItem("favorites");
            if (storedFavorites !== null) {
                favorites = JSON.parse(storedFavorites);
            }
        }
        function saveStockDataToLocalStorage(stockData) {
            localStorage.setItem("stockData", JSON.stringify(stockData));
        }
        function loadStockDataFromLocalStorage() {
            var storedStockData = localStorage.getItem("stockData");
            if (storedStockData !== null) {
                return JSON.parse(storedStockData);
            }
            return [];
        }
    </script>
</head>
<body>
    <h1>Stock Data</h1>
    <table id="stock-table">
        <thead>
            <tr>
                <th class="sortable" onclick="sortTable(0)">Symbol</th>
                <th class="sortable" onclick="sortTable(1)">Timestamp</th>
                <th class="sortable" onclick="sortTable(2)">Open</th>
                <th class="sortable" onclick="sortTable(3)">High</th>
                <th class="sortable" onclick="sortTable(4)">Low</th>
                <th class="sortable" onclick="sortTable(5)">Close</th>
                <th class="sortable" onclick="sortTable(6)">Volume</th>
            </tr>
        </thead>
        <tbody>
            <!-- Table rows will be dynamically added here -->
        </tbody>
    </table>
    <script>
        function sortTable(columnIndex) {
            var $table = $("#stock-table");
            var rows = $table.find("tbody tr").get();
            rows.sort(function(a, b) {
                var aValue = $(a).children("td").eq(columnIndex).text();
                var bValue = $(b).children("td").eq(columnIndex).text();
                if (columnIndex === 1) {
                    // Sort by timestamp in descending order
                    return new Date(bValue) - new Date(aValue);
                } else {
                    // Sort by other columns in ascending order
                    return aValue.localeCompare(bValue);
                }
            });$.each(rows, function(index, row) {$table.children("tbody").append(row);
            });
        }
    </script>
</body>
</html>