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
        var favorites = []; // Array to store the favorite stocks
        function refreshTable() {
            var symbols = ["MSFT", "AAPL", "GOOGL", "AMZN", "TSLA", "META", "AMD"]; // Replace with your desired stock symbols
            var tableRows = [];
            symbols.forEach(function(symbol) {$.ajax({
                    url: "https://yahoo-finance127.p.rapidapi.com/price/eth-usd" + symbol,
                    headers: {
                        "X-RapidAPI-Key": "f094bea0c1mshcd62745f861872ep1d1239jsn8736f8b21167",
                        "X-RapidAPI-Host": "yahoo-finance127.p.rapidapi.com"
                    },
                    success: function(response) {
                        console.log(response);
                        var stockName = response.quoteResponse.result[0].symbol;
                        var latestPrice = response.quoteResponse.result[0].regularMarketPrice.raw;
                        console.log("Stock: " + stockName + ", Price: " + latestPrice);
                        var tableRow = {
                            symbol: stockName,
                            price: latestPrice,
                            favorite: favorites.includes(stockName)
                        };
                        tableRows.push(tableRow);
                        renderTable(tableRows);
                    },
                    error: function(xhr, status, error) {
                        console.log("Error:", status, error);
                    }
                });
            });
        }
        function renderTable(tableRows) {
            var $tableBody = $("#stock-table tbody");$tableBody.empty();
            for (var i = 0; i < tableRows.length; i++) {
                var row = tableRows[i];
                var favoriteIcon = row.favorite ? '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9733;</span>' : '<span class="favorite" onclick="toggleFavorite(' + i + ')">&#9734;</span>';
                var tableRow = "<tr>" +
                    "<td>" + row.symbol + favoriteIcon + "</td>" +
                    "<td>" + row.price + "</td>" +
                    "</tr>";$tableBody.append(tableRow);
            }
        }
        function sortTable(columnIndex) {
            var $table = $("#stock-table");
            var rows = $table.find("tbody tr").toArray(); 
            rows.sort(function(a, b) {
                var aValue = $(a).find("td").eq(columnIndex).text();
                var bValue = $(b).find("td").eq(columnIndex).text();    
                if (columnIndex === 0) {
                    return aValue.localeCompare(bValue); // Sort alphabetically for stock column
                } else {
                    return parseFloat(bValue) - parseFloat(aValue); // Sort numerically for other columns
                }
            });$table.find("tbody").empty().append(rows);
        }        function toggleFavorite(rowIndex) {
            var $table = $("#stock-table");
            var $row = $table.find("tbody tr").eq(rowIndex);
            var stockName = $row.find("td").eq(0).text();
            if (favorites.includes(stockName)) {
                favorites = favorites.filter(function(value) {
                    return value !== stockName;
                });$row.find(".favorite").html("&#9734;");
            } else {
                favorites.push(stockName);$row.find(".favorite").html("&#9733;");
            }
        }
    </script>
</head>
<body>
    <button onclick="refreshTable()">View Data</button>
    <table id="stock-table">
        <thead>
            <tr>
                <th class="sortable" onclick="sortTable(0)">
                    Stock
                </th>
                <th class="sortable" onclick="sortTable(1)">
                    Price
                </th>
            </tr>
        </thead>
        <tbody>
            <!-- The table body will be populated with data fetched from the API -->
        </tbody>
    </table>
</body>
</html>