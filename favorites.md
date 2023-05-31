<html>
<head>
    <title>My Favorite Stocks</title>
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
            var $tableBody = $("#favorites-table tbody");$tableBody.empty();
            for (var i = 0; i < favorites.length; i++) {
                var symbol = favorites[i];
                var stockData = JSON.parse(localStorage.getItem(symbol));
                var tableRow = "<tr>" +
                    "<td>" + symbol + "</td>" +
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
            localStorage.clear();
            favorites = [];
            renderTable();
        }
    </script>
</head>
<body>
    <button onclick="renderTable()">Refresh Data</button>
    <table id="favorites-table">
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
            <!-- The table body will be populated with favorite stock data -->
        </tbody>
    </table>
    <button onclick="clearLocalStorage()">Clear Favorites</button> 
</body>
</html>