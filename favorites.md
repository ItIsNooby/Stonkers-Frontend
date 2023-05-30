<html>
<head>
    <title>Favorited Stocks</title>
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
            // Load the favorited stocks from localStorage when the page loads
            loadFavoritesFromLocalStorage();
            // Call a function to load the favorited stocks table
            loadFavoritesTable();
        });
        function loadFavoritesFromLocalStorage() {
            var storedFavorites = localStorage.getItem("favorites");
            if (storedFavorites) {
                favorites = JSON.parse(storedFavorites);
            }
        }
        function saveFavoritesToLocalStorage() {
            localStorage.setItem("favorites", JSON.stringify(favorites));
        }
        function loadFavoritesTable() {
            var tableRows = [];
            // Iterate through the favorite stocks
            for (var i = 0; i < favorites.length; i++) {
                var symbol = favorites[i];$.ajax({
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
                            favorite: true
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
        function getLatestTimestamp(timeSeriesData) {
            var timestamps = Object.keys(timeSeriesData);
            return timestamps[0];  // Assumes the timestamps are in descending order
        }
        function renderTable(tableRows) {
            var $tableBody = $("#favorites-table tbody");$tableBody.empty();
            for (var i = 0; i < tableRows.length; i++) {
                var row = tableRows[i];
                var favoriteIcon = '<span class="favorite" onclick="removeFromFavorites(' + i + ')">&#9733;</span>';
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
        function removeFromFavorites(rowIndex) {
            favorites.splice(rowIndex, 1);
            saveFavoritesToLocalStorage();
            loadFavoritesTable();
        }
    </script>
</head>
<body>
    <h1>Favorited Stocks</h1>
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
            <!-- The table body will be populated with favorited stocks data -->
        </tbody>
    </table>
</body>
</html>