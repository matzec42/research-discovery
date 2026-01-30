## Greedy Solution Problems

### 121. Best Time to Buy Stock (LeetCode)

- Greedy approach
    - initialize variables for lowest price (**lowest**) and maximum profit (**maxProfit**)
    - loop over the passed in array
        - first, check if current price is lowest --- if so, reassign
        - then, compare maxProfit to the diff. b/w current and lowest prices (calculate current profit)
            - if greater than maxProfit, update it w/ new maximum profit

```js
var maxProfit = function(prices) {
    let lowest = prices[0];
    let maxProfit = 0;

    for (let i = 0; i < prices.length; i++) {
        const current = prices[i];
        if (current < lowest) {
            lowest = current;
        }
        if (maxProfit < current - lowest) {
            maxProfit = current - lowest;
        }
    }
    return maxProfit
};
```