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
                var stockName = favorites[i];
                var tableRow = "<tr>" +
                    "<td>" + stockName + "</td>" +
                    "</tr>";$tableBody.append(tableRow);
            }
        }
    </script>
</head>
<body>
    <h1>Favorites</h1>
    <table id="favorites-table">
        <thead>
            <tr>
                <th>Stock</th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with favorited stocks -->
        </tbody>
    </table>
</body>
</html>