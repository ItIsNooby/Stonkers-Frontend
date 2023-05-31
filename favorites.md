<html>
<head>
    <title>Favorite Stocks</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <style>
        .sortable {
            cursor: pointer;
        }
    </style>
    <script>
        var favorites = [];
        function loadFavoritesTable() {
            var settings = {
                async: true,
                crossDomain: true,
                url: 'https://realstonks.p.rapidapi.com/favorites',
                method: 'GET',
                headers: {
                    'X-RapidAPI-Key': '24a738dc44msh1340883298de7f6p133977jsnb8399f963780',
                    'X-RapidAPI-Host': 'realstonks.p.rapidapi.com'
                }
            };$.ajax(settings).done(function(response) {
                var tableRows = [];
                for (var i = 0; i < response.length; i++) {
                    var stockData = response[i];
                    var tableRow = {
                        symbol: stockData.symbol,
                        timestamp: stockData.timestamp,
                        open: stockData.open,
                        high: stockData.high,
                        low: stockData.low,
                        close: stockData.close,
                        volume: stockData.volume,
                        favorite: true
                    };
                    tableRows.push(tableRow);
                }
                renderTable(tableRows);
            }).fail(function() {
                console.log('Failed to fetch favorite stock data');
            });
        }
        function renderTable(tableRows) {
            var $tableBody = $('#favorite-table tbody');$tableBody.empty();
            for (var i = 0; i < tableRows.length; i++) {
                var row = tableRows[i];
                var favoriteIcon = row.favorite ? '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9733;</span>' : '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9734;</span>';
                var tableRow = '<tr>' +
                    '<td>' + row.symbol + favoriteIcon + '</td>' +
                    '<td>' + row.timestamp + '</td>' +
                    '<td>' + row.open + '</td>' +
                    '<td>' + row.high + '</td>' +
                    '<td>' + row.low + '</td>' +
                    '<td>' + row.close + '</td>' +
                    '<td>' + row.volume + '</td>' +
                    '</tr>';$tableBody.append(tableRow);
            }
        }
        function toggleFavorite(rowIndex) {
            var $table = $('#favorite-table');
            var $row = $table.find('tbody tr').eq(rowIndex);
            var stockName = $row.find('td').eq(0).text().trim();
            var $favoriteIcon = $row.find('.favorite');
            if ($favoriteIcon.hasClass('favorite')) {$favoriteIcon.removeClass('favorite').html('&#9734;');
                favorites = favorites.filter(function(favorite) {
                    return favorite !== stockName;
                });
            } else {$favoriteIcon.addClass('favorite').html('&#9733;');
                favorites.push(stockName);
            }
        }$(document).ready(function() {
            loadFavoritesTable();
        });
    </script>
</head>
<body>
    <h1>Favorite Stocks</h1>
    <table id="favorite-table" border="1">
        <thead>
            <tr>
                <th>Symbol</th>
                <th>Timestamp</th>
                <th>Open</th>
                <th>High</th>
                <th>Low</th>
                <th>Close</th>
                <th>Volume</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</body>
</html>