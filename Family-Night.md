<!DOCTYPE html>
<html>
<head>
  <title>Stock Watchlist</title>
  <style>
    * {
      box-sizing: border-box;
    }
    
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f5f5f5;
    }
    
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }
    
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-bottom: 20px;
    }
    
    .header h1 {
      font-size: 24px;
      font-weight: bold;
      margin: 0;
    }
    
    .watchlist {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      grid-gap: 20px;
    }
    
    .stock-card {
      background-color: #fff;
      padding: 20px;
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .stock-card h2 {
      font-size: 18px;
      font-weight: bold;
      margin: 0 0 10px;
    }
    
    .stock-graph {
      height: 200px;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>Stock Watchlist</h1>
    </div>
  
    <div class="watchlist" id="watchlist"></div>
  </div>
  
  <script>
    function createWatchlist(stocks) {
      var watchlistElement = document.getElementById("watchlist");
      
      for (var i = 0; i < stocks.length; i++) {
        var stockSymbol = stocks[i];
        var stockCard = createStockCard(stockSymbol);
        watchlistElement.appendChild(stockCard);
      }
    }
    
    function createStockCard(stockSymbol) {
      var stockCard = document.createElement("div");
      stockCard.className = "stock-card";
      
      var stockHeading = document.createElement("h2");
      stockHeading.textContent = stockSymbol;
      
      stockCard.appendChild(stockHeading);
      
      var stockGraph = document.createElement("div");
      stockGraph.className = "stock-graph";
      stockCard.appendChild(stockGraph);
      
      return stockCard;
    }
    
    // Example usage
    var watchlist = ["AAPL", "MSFT", "GOOGL"];
    createWatchlist(watchlist);
  </script>
</body>
</html>
