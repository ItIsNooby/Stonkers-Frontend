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
    <!-- Include jQuery library -->
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
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
    <button id="clear-storage-btn">Clear Local Storage</button>
    <script>
        // Initialize an empty array to store favorites
        var favorites = []; $(document).ready(function() { // Wait for the document to be ready,  Load favorites from local storage
            loadFavoritesFromLocalStorage();
            // Render the table with the loaded favorites
            renderTable(); $('#clear-storage-btn').click(function() { // Attach a click event listener to the clear storage button
                // Clear the local storage and re-render the table
                clearLocalStorage();
                renderTable();
            });
        });
        // Function to load favorites from local storage
        function loadFavoritesFromLocalStorage() {
            // Retrieve favorites from local storage
            var storedFavorites = localStorage.getItem("favorites");
            if (storedFavorites) {
                favorites = JSON.parse(storedFavorites);
            }
        }
        // Function to render the favorites table
        function renderTable() {
            var $tableBody = $("#favorites-table tbody"); $tableBody.empty(); // Clear the table body
            // Loop through each favorite and add a table row
            favorites.forEach(function(stockName) {
                var tableRow = "<tr><td>" + stockName + "</td></tr>"; $tableBody.append(tableRow); // Append the row to the table body
            });
        }
        // Function to clear local storage and favorites array
        function clearLocalStorage() {
            favorites = [];
            localStorage.removeItem("favorites");
        }
    </script>
</body>
</html>
