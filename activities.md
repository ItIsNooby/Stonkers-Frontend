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
        var favorites = [];
        function refreshTable() {
            var symbols = ["MSFT", "AAPL", "GOOGL", "AMZN", "TSLA", "META", "AMD"];  // Replace with your desired stock symbols
            var tableRows = [];
            symbols.forEach(function(symbol) {
                var settings = {
                    async: true,
                    crossDomain: true,
                    url: 'https://realstonks.p.rapidapi.com/' + symbol,
                    method: 'GET',
                    headers: {
                        'X-RapidAPI-Key': '24a738dc44msh1340883298de7f6p133977jsnb8399f963780',
                        'X-RapidAPI-Host': 'realstonks.p.rapidapi.com'
                    }
                };$.ajax(settings).done(function(response) {
                    var stockData = response[0]; // Assuming the response is an array with one item
                    var tableRow = {
                        symbol: stockData.symbol,
                        timestamp: stockData.timestamp,
                        open: stockData.open,
                        high: stockData.high,
                        low: stockData.low,
                        close: stockData.close,
                        volume: stockData.volume,
                        favorite: favorites.includes(stockData.symbol)
                    };
                    tableRows.push(tableRow);
                    // Render the table once all stocks have been processed
                    if (tableRows.length === symbols.length) {
                        renderTable(tableRows);
                    }
                }).fail(function() {
                    console.log('Failed to fetch stock data for symbol: ' + symbol);
                });
            });
        }
        function renderTable(tableRows) {
            var $tableBody = $('#stock-table tbody');$tableBody.empty();
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
            var $table = $('#stock-table');
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
            refreshTable();
        });
    </script>
</head>
<body>
    <h1>Stock Data</h1>
    <table id="stock-table" border="1">
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