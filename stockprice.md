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
                    url: "https://latest-stock-price.p.rapidapi.com/price?Indices=%3CREQUIRED%3E",
                    headers: {
                        "X-RapidAPI-Key": "b731fee7a5mshf2b6608334c0b07p13bf5fjsn09fcf5df26f4", // Replace with your RapidAPI key
                        "X-RapidAPI-Host": "latest-stock-price.p.rapidapi.com"
                    },
                    data: {
                        Indices: symbol
                    },
                    success: function(data) {
                        var stockName = Object.keys(data)[0];
                        var latestPrice = data[stockName] && data[stockName].price;
                        var tableRow = {
                            symbol: stockName,
                            price: latestPrice,
                            favorite: favorites.includes(stockName) // Check if the stock is already a favorite
                        };
                        tableRows.push(tableRow);
                    },
                    error: function() {
                        console.log("Failed to fetch stock data for symbol: " + symbol);
                    }
                });
            });
renderTable(tableRows);
        }
function renderTable(tableRows) {
            var $tableBody = $("#stock-table tbody");$tableBody.empty();
tableRows.forEach(function(row) {
                var favoriteIcon = row.favorite ? '<span class="favorite" onclick="toggleFavorite(' + row.symbol + ')">&#9733;</span>' : '<span class="favorite" onclick="toggleFavorite(' + row.symbol + ')">&#9734;</span>';
                var tableRow = "<tr>" +
                    "<td>" + row.symbol + favoriteIcon + "</td>" +
                    "<td>" + row.price + "</td>" +
                    "</tr>";$tableBody.append(tableRow);
            });
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
        }
function toggleFavorite(stockName) {
            var index = favorites.indexOf(stockName);
            if (index !== -1) {
                favorites.splice(index, 1);
            } else {
                favorites.push(stockName);
            }
            refreshTable();
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
