<html>
<head>
    <title>Favorites</title>
    <style>
        table {
            border-collapse: collapse;
        }
        th, td {
            border: 1px solid black;
            padding: 5px;
        }
    </style>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <h1>Favorites</h1>
    <table id="favorites-table">
        <thead>
            <tr>
                <th>Stock</th>
                <th>Current Price</th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with favorited stocks -->
        </tbody>
    </table>
    <button onclick="clearFavorites()">Clear Favorites</button>
    <a href="stocks.html" id="stocks-link">View Stocks</a>
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
            var $tableBody = $("#favorites-table tbody");$tableBody.empty();
            for (var i = 0; i < favorites.length; i++) {
                var symbol = favorites[i];
                var stockData = getStockData(symbol);
                if (stockData) {
                    var tableRow = "<tr>" +
                        "<td>" + symbol + "</td>" +
                        "<td>" + stockData.close + "</td>" +
                        "</tr>";$tableBody.append(tableRow);
                }
            }
        }
        function getStockData(symbol) {
            var storedData = localStorage.getItem(symbol);
            if (storedData) {
                var stockData = JSON.parse(storedData);
                return stockData;
            }
            return null;
        }
        function clearFavorites() {
            favorites = [];
            localStorage.removeItem("favorites");
            renderTable();
        }
    </script>
</body>
</html>