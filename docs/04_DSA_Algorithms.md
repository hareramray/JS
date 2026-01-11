# 📕 DSA - Algorithms - Documentation

Advanced to expert algorithms: sorting, searching, DP, graphs, and more.

---

## 1. Sorting Algorithms

### Bubble Sort - O(n²)
```javascript
function bubbleSort(arr) {
    const n = arr.length;
    for (let i = 0; i < n; i++) {
        let swapped = false;
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                swapped = true;
            }
        }
        if (!swapped) break;
    }
    return arr;
}
```

### Quick Sort - O(n log n) avg
```javascript
function quickSort(arr, left = 0, right = arr.length - 1) {
    if (left < right) {
        const pivotIndex = partition(arr, left, right);
        quickSort(arr, left, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, right);
    }
    return arr;
}

function partition(arr, left, right) {
    const pivot = arr[right];
    let i = left - 1;
    for (let j = left; j < right; j++) {
        if (arr[j] < pivot) { i++; [arr[i], arr[j]] = [arr[j], arr[i]]; }
    }
    [arr[i + 1], arr[right]] = [arr[right], arr[i + 1]];
    return i + 1;
}
```

### Merge Sort - O(n log n)
```javascript
function mergeSort(arr) {
    if (arr.length <= 1) return arr;
    const mid = Math.floor(arr.length / 2);
    return merge(mergeSort(arr.slice(0, mid)), mergeSort(arr.slice(mid)));
}

function merge(left, right) {
    const result = [];
    let i = 0, j = 0;
    while (i < left.length && j < right.length)
        result.push(left[i] <= right[j] ? left[i++] : right[j++]);
    return result.concat(left.slice(i)).concat(right.slice(j));
}
```

---

## 2. Searching

### Binary Search - O(log n)
```javascript
function binarySearch(arr, target) {
    let left = 0, right = arr.length - 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

### Q1: Search in Rotated Sorted Array
```javascript
function searchRotated(nums, target) {
    let left = 0, right = nums.length - 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] === target) return mid;
        if (nums[left] <= nums[mid]) {
            if (target >= nums[left] && target < nums[mid]) right = mid - 1;
            else left = mid + 1;
        } else {
            if (target > nums[mid] && target <= nums[right]) left = mid + 1;
            else right = mid - 1;
        }
    }
    return -1;
}
```

---

## 3. Backtracking

### Q2: Generate All Subsets
```javascript
function subsets(nums) {
    const result = [];
    function backtrack(index, current) {
        result.push([...current]);
        for (let i = index; i < nums.length; i++) {
            current.push(nums[i]);
            backtrack(i + 1, current);
            current.pop();
        }
    }
    backtrack(0, []);
    return result;
}
```

### Q3: Generate Permutations
```javascript
function permute(nums) {
    const result = [];
    function backtrack(current, remaining) {
        if (!remaining.length) { result.push([...current]); return; }
        for (let i = 0; i < remaining.length; i++) {
            current.push(remaining[i]);
            backtrack(current, [...remaining.slice(0, i), ...remaining.slice(i + 1)]);
            current.pop();
        }
    }
    backtrack([], nums);
    return result;
}
```

### Q4: N-Queens
```javascript
function solveNQueens(n) {
    const result = [], board = Array(n).fill().map(() => Array(n).fill('.'));
    function isValid(row, col) {
        for (let i = 0; i < row; i++) if (board[i][col] === 'Q') return false;
        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] === 'Q') return false;
        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++)
            if (board[i][j] === 'Q') return false;
        return true;
    }
    function backtrack(row) {
        if (row === n) { result.push(board.map(r => r.join(''))); return; }
        for (let col = 0; col < n; col++) {
            if (isValid(row, col)) {
                board[row][col] = 'Q';
                backtrack(row + 1);
                board[row][col] = '.';
            }
        }
    }
    backtrack(0);
    return result;
}
```

---

## 4. Dynamic Programming

### Q5: Fibonacci (DP)
```javascript
// Memoization
function fibMemo(n, memo = {}) {
    if (n <= 1) return n;
    if (memo[n]) return memo[n];
    return memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
}

// Space Optimized O(1)
function fib(n) {
    if (n <= 1) return n;
    let prev2 = 0, prev1 = 1;
    for (let i = 2; i <= n; i++) [prev2, prev1] = [prev1, prev1 + prev2];
    return prev1;
}
```

### Q6: Climbing Stairs
```javascript
function climbStairs(n) {
    if (n <= 2) return n;
    let prev2 = 1, prev1 = 2;
    for (let i = 3; i <= n; i++) [prev2, prev1] = [prev1, prev1 + prev2];
    return prev1;
}
```

### Q7: Coin Change
```javascript
function coinChange(coins, amount) {
    const dp = Array(amount + 1).fill(Infinity);
    dp[0] = 0;
    for (let i = 1; i <= amount; i++)
        for (const coin of coins)
            if (coin <= i) dp[i] = Math.min(dp[i], dp[i - coin] + 1);
    return dp[amount] === Infinity ? -1 : dp[amount];
}
```

### Q8: Longest Common Subsequence
```javascript
function longestCommonSubsequence(text1, text2) {
    const m = text1.length, n = text2.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    for (let i = 1; i <= m; i++)
        for (let j = 1; j <= n; j++)
            dp[i][j] = text1[i-1] === text2[j-1] ? dp[i-1][j-1] + 1 : Math.max(dp[i-1][j], dp[i][j-1]);
    return dp[m][n];
}
```

### Q9: 0/1 Knapsack
```javascript
function knapsack(weights, values, capacity) {
    const n = weights.length;
    const dp = Array(n + 1).fill().map(() => Array(capacity + 1).fill(0));
    for (let i = 1; i <= n; i++)
        for (let w = 1; w <= capacity; w++)
            dp[i][w] = weights[i-1] <= w
                ? Math.max(dp[i-1][w], values[i-1] + dp[i-1][w - weights[i-1]])
                : dp[i-1][w];
    return dp[n][capacity];
}
```

### Q10: Longest Increasing Subsequence
```javascript
function lengthOfLIS(nums) {
    const tails = [];
    for (const num of nums) {
        let left = 0, right = tails.length;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (tails[mid] < num) left = mid + 1;
            else right = mid;
        }
        tails[left] = num;
    }
    return tails.length;
}
```

---

## 5. Graph Algorithms

### Q11: Dijkstra's (Shortest Path)
```javascript
function dijkstra(graph, start) {
    const dist = {}, visited = new Set(), pq = [[0, start]];
    for (const node of graph.adjacencyList.keys()) dist[node] = Infinity;
    dist[start] = 0;
    while (pq.length) {
        pq.sort((a, b) => a[0] - b[0]);
        const [d, node] = pq.shift();
        if (visited.has(node)) continue;
        visited.add(node);
        for (const { node: neighbor, weight } of graph.getNeighbors(node))
            if (d + weight < dist[neighbor]) {
                dist[neighbor] = d + weight;
                pq.push([dist[neighbor], neighbor]);
            }
    }
    return dist;
}
```

### Q12: Topological Sort
```javascript
function topologicalSort(numCourses, prerequisites) {
    const graph = new Map(), inDegree = Array(numCourses).fill(0);
    for (let i = 0; i < numCourses; i++) graph.set(i, []);
    for (const [course, prereq] of prerequisites) {
        graph.get(prereq).push(course);
        inDegree[course]++;
    }
    const queue = [], result = [];
    for (let i = 0; i < numCourses; i++) if (inDegree[i] === 0) queue.push(i);
    while (queue.length) {
        const node = queue.shift();
        result.push(node);
        for (const neighbor of graph.get(node))
            if (--inDegree[neighbor] === 0) queue.push(neighbor);
    }
    return result.length === numCourses ? result : [];
}
```

### Q13: Union Find
```javascript
class UnionFind {
    constructor(n) {
        this.parent = Array(n).fill().map((_, i) => i);
        this.rank = Array(n).fill(0);
    }
    find(x) {
        if (this.parent[x] !== x) this.parent[x] = this.find(this.parent[x]);
        return this.parent[x];
    }
    union(x, y) {
        const [rx, ry] = [this.find(x), this.find(y)];
        if (rx === ry) return false;
        if (this.rank[rx] < this.rank[ry]) this.parent[rx] = ry;
        else if (this.rank[rx] > this.rank[ry]) this.parent[ry] = rx;
        else { this.parent[ry] = rx; this.rank[rx]++; }
        return true;
    }
}
```

---

## 6. Sliding Window & Two Pointers

### Q14: Max Sum Subarray of Size K
```javascript
function maxSumSubarray(arr, k) {
    let windowSum = 0, maxSum = 0;
    for (let i = 0; i < k; i++) windowSum += arr[i];
    maxSum = windowSum;
    for (let i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

### Q16: 3Sum
```javascript
function threeSum(nums) {
    nums.sort((a, b) => a - b);
    const result = [];
    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        let left = i + 1, right = nums.length - 1;
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                left++; right--;
            } else if (sum < 0) left++;
            else right--;
        }
    }
    return result;
}
```

---

## 7. Greedy

### Q18: Activity Selection
```javascript
function maxActivities(activities) {
    activities.sort((a, b) => a[1] - b[1]);
    const selected = [activities[0]];
    let lastEnd = activities[0][1];
    for (let i = 1; i < activities.length; i++)
        if (activities[i][0] >= lastEnd) {
            selected.push(activities[i]);
            lastEnd = activities[i][1];
        }
    return selected;
}
```

### Q19: Jump Game
```javascript
function canJump(nums) {
    let maxReach = 0;
    for (let i = 0; i < nums.length; i++) {
        if (i > maxReach) return false;
        maxReach = Math.max(maxReach, i + nums[i]);
    }
    return true;
}
```

---

## 8. Bit Manipulation

### Q20: Single Number (XOR)
```javascript
function singleNumber(nums) {
    return nums.reduce((a, b) => a ^ b, 0);
}
```

### Common Bit Tricks
```javascript
const isEven = n => (n & 1) === 0;
const isPowerOfTwo = n => n > 0 && (n & (n - 1)) === 0;
const countSetBits = n => n.toString(2).replace(/0/g, '').length;
```

---

## Complexity Cheat Sheet

| Algorithm | Time (Avg) | Time (Worst) | Space |
|-----------|------------|--------------|-------|
| Bubble Sort | O(n²) | O(n²) | O(1) |
| Quick Sort | O(n log n) | O(n²) | O(log n) |
| Merge Sort | O(n log n) | O(n log n) | O(n) |
| Binary Search | O(log n) | O(log n) | O(1) |
| BFS/DFS | O(V + E) | O(V + E) | O(V) |
| Dijkstra | O(E log V) | O(E log V) | O(V) |

---

*Previous: [DSA Data Structures](03_DSA_Data_Structures.md)*
