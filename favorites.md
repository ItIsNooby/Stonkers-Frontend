<html>
<head>
    <title>Favorites</title>
    <style>
        .sortable {
            cursor: pointer; 
        }
        .favorite {
            color: gold;
            cursor: pointer;
        }
    </style>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        var favorites = [];$(document).ready(function() {
            loadFavoritesFromLocalStorage();
            refreshTable();
        });
        function refreshTable() {
            var storedStockData = localStorage.getItem("stockData");
            if (storedStockData) {
                var stockData = JSON.parse(storedStockData);
                renderTable(stockData);
            }
        }
        function renderTable(stockData) {
            var $tableBody = $("#stock-table tbody");$tableBody.empty();
            for (var i = 0; i < stockData.length; i++) {
                var stock = stockData[i];
                var favoriteIcon = '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9733;</span>';
                var tableRow = "<tr>" +
                    "<td>" + stock.symbol + favoriteIcon + "</td>" +
                    "<td>" + stock.timestamp + "</td>" +
                    "<td>" + (stock.open || "") + "</td>" +
                    "<td>" + (stock.high || "") + "</td>" +
                    "<td>" + (stock.low || "") + "</td>" +
                    "<td>" + (stock.close || "") + "</td>" +
                    "<td>" + (stock.volume || "") + "</td>" +
                    "</tr>";$tableBody.append(tableRow);
            }
        }
        function toggleFavorite(rowIndex) {
            var storedStockData = localStorage.getItem("stockData");
            if (storedStockData) {
                var stockData = JSON.parse(storedStockData);
                var $table = $("#stock-table");
                var $row = $table.find("tbody tr").eq(rowIndex);
                var stock = stockData[rowIndex];
                var stockName = stock.symbol;
                if (favorites.includes(stockName)) {
                    favorites = favorites.filter(function(value) {
                        return value !== stockName;
                    });$row.find(".favorite").html("&#9734;");
                } else {
                    favorites.push(stockName);$row.find(".favorite").html("&#9733;");
                }
                saveFavoritesToLocalStorage();
            }
        }
        function loadFavoritesFromLocalStorage() {
            var storedFavorites = localStorage.getItem("favorites");
            if (storedFavorites) {
                favorites = JSON.parse(storedFavorites);
            }
        }
        function saveFavoritesToLocalStorage() {
            localStorage.setItem("favorites", JSON.stringify(favorites));
        }
    </script>
</head>
<body>
    <h1>Favorites</h1>
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
            <!-- Table body will be dynamically populated here -->
        </tbody>
    </table>
    <button onclick="localStorage.clear(); refreshTable();">Clear Local Storage</button>
</body>
</html>