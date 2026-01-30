## Greedy Solution Problems

### 121. Best Time to Buy Stock (LeetCode)

- Greedy iterative approach
    - initialize variables for lowest price (**lowest**) and maximum profit (**maxProfit**)
    - loop over the passed in array
        - first, check if current price is lowest --- if so, reassign
        - then, compare maxProfit to the diff. b/w current and lowest prices (calculate current profit)
            - if greater than maxProfit, update it w/ new maximum profit
    - Time: O(n). Space: O(1)

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


### 134. Gas Station (LeetCode)

- Greedy iterative approach
    - initialize variables for necessary data to track
        - `startingIndex` --> the index that you'll return if a complete circuit is possible
        - `totalTank` --> stores the total amount of gas used for whole trip; if above 0 by end of function, then a circuit is possible (and startingIndex gets returned --- see ternary operator)
        - `currentGasInTank` --> stores the amount of gas at current station; checked w/ each loop iteration to see if you can move one station more
    - for loop
        - iterates over gas array, starting from 0th index; calculates netCost (amount of gas minus cost), which is used to update both currentGasInTank and totalTank (i.e., you need to update both in order to see if you can make it to the next station and, long-term, if you'll end up with enough gas in total to complete a circuit)
        - `if` conditional checks at end of each iteration if it's possible to make it to the next station
            - if yes, conditional is skipped
            - if not possible, then startingIndex and currentGasInTank are updated/reset ahead of next iteration of loop
        
    - Time: O(n). Space: O(1)

```js
var canCompleteCircuit = function(gas, cost) {
    let startingIndex = 0;
    let totalTank = 0;
    let currentGasInTank = 0;

    for (let i = 0; i < gas.length; i++) {
        const netCost = gas[i] - cost[i];
        currentGasInTank += netCost;
        totalTank += netCost;
        
        if (currentGasInTank < 0) {
            startingIndex = i + 1;
            currentGasInTank = 0;
        }
    }
    return (totalTank >= 0 ? startingIndex : -1)
}
```