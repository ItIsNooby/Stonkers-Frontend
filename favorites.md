<html>
<head>
    <title>My Favorite Stocks</title>
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
                <th>Timestamp</th>
                <th>Open</th>
                <th>High</th>
                <th>Low</th>
                <th>Close</th>
                <th>Volume</th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with favorited stocks -->
        </tbody>
    </table>
    <button onclick="clearLocalStorage()">Clear Favorites</button>
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
                var stockData = favorites[i];
                var tableRow = "<tr>" +
                    "<td>" + stockData.symbol + "</td>" +
                    "<td>" + stockData.timestamp + "</td>" +
                    "<td>" + stockData.open + "</td>" +
                    "<td>" + stockData.high + "</td>" +
                    "<td>" + stockData.low + "</td>" +
                    "<td>" + stockData.close + "</td>" +
                    "<td>" + stockData.volume + "</td>" +
                    "</tr>";$tableBody.append(tableRow);
            }
        }
        function clearLocalStorage() {
            favorites = [];
            localStorage.removeItem("favorites");
            renderTable();
        }
    </script>
</body>
</html>