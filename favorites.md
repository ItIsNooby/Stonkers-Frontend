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
                var symbol = stockData.symbol || "";
                var timestamp = stockData.timestamp || "";
                var open = stockData.open || "";
                var high = stockData.high || "";
                var low = stockData.low || "";
                var close = stockData.close || "";
                var volume = stockData.volume || "";
                var tableRow = "<tr>" +
                    "<td>" + symbol + "</td>" +
                    "<td>" + timestamp + "</td>" +
                    "<td>" + open + "</td>" +
                    "<td>" + high + "</td>" +
                    "<td>" + low + "</td>" +
                    "<td>" + close + "</td>" +
                    "<td>" + volume + "</td>" +
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