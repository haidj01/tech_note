1. one time traction

```
private static int getMaxProfit(int [] prices){
        if(prices==null || prices.length < 2){
            return 0;
        }

        int minPrice = prices[0];
        int maxProfit = 0;
        for( int i = 1; i < prices.length ; i++ ) {
            int profit =  prices[i] - minPrice;

            if ( profit > maxProfit){
                maxProfit = profit;
            }

            if (minPrice > prices[i]){
                minPrice = prices[i];
            }

        }

        return maxProfit;
    }
```

2. multiple trancions
```
private static int getMaxProfitWithMultipleTime(int [] prices){
        if(prices==null || prices.length < 2){
            return 0;
        }

        int maxProfit = 0;

        for( int i = 1; i < prices.length ; i++ ) {
            if (prices[i] > prices[i -1]) {
                maxProfit += prices[i]- prices[i-1];
            }
        }

        return maxProfit;
    }
```

3. transaction with at most two times
```
private static int getStockProfitAtMostTwo(int [] prices) {
        if (prices == null || prices.length == 0) return 0;

        int buy1 = Integer.MIN_VALUE, sell1 = 0;
        int buy2 = Integer.MIN_VALUE, sell2 = 0;

        for (int price : prices) {
            // First buy: maximize negative cost (buy cheap)
            buy1 = Math.max(buy1, -price);
            // First sell: maximize profit after first buy
            sell1 = Math.max(sell1, buy1 + price);
            // Second buy: maximize profit after first sell (reinvest)
            buy2 = Math.max(buy2, sell1 - price);
            // Second sell: maximize total profit after second buy
            sell2 = Math.max(sell2, buy2 + price);
        }

        return sell2;

    }

```