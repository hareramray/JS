# 📙 DSA - Data Structures - Documentation

Implementing and using data structures from basic to advanced.

---

## 1. Arrays & Strings

### Q1: Rotate Array by K Positions
**Time**: O(n) | **Space**: O(1)
```javascript
function rotateArray(arr, k) {
    k = k % arr.length;
    const reverse = (start, end) => {
        while (start < end) {
            [arr[start], arr[end]] = [arr[end], arr[start]];
            start++; end--;
        }
    };
    reverse(0, arr.length - 1);
    reverse(0, k - 1);
    reverse(k, arr.length - 1);
    return arr;
}
```

### Q2: Maximum Subarray Sum (Kadane's)
**Time**: O(n) | **Space**: O(1)
```javascript
function maxSubarraySum(arr) {
    let maxSum = arr[0], currentSum = arr[0];
    for (let i = 1; i < arr.length; i++) {
        currentSum = Math.max(arr[i], currentSum + arr[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
```

### Q3: Longest Substring Without Repeating
**Time**: O(n) | **Space**: O(min(m,n))
```javascript
function lengthOfLongestSubstring(s) {
    const charMap = new Map();
    let maxLen = 0, left = 0;
    for (let right = 0; right < s.length; right++) {
        if (charMap.has(s[right]) && charMap.get(s[right]) >= left)
            left = charMap.get(s[right]) + 1;
        charMap.set(s[right], right);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

---

## 2. Linked Lists

### Q4: Reverse Linked List
**Time**: O(n) | **Space**: O(1)
```javascript
function reverseList(head) {
    let prev = null, curr = head;
    while (curr) {
        const next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### Q5: Detect Cycle (Floyd's)
**Time**: O(n) | **Space**: O(1)
```javascript
function hasCycle(head) {
    let slow = head, fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) return true;
    }
    return false;
}
```

### Q6: Merge Two Sorted Lists
**Time**: O(n+m) | **Space**: O(1)
```javascript
function mergeTwoLists(l1, l2) {
    const dummy = new ListNode(0);
    let current = dummy;
    while (l1 && l2) {
        if (l1.val <= l2.val) { current.next = l1; l1 = l1.next; }
        else { current.next = l2; l2 = l2.next; }
        current = current.next;
    }
    current.next = l1 || l2;
    return dummy.next;
}
```

---

## 3. Stacks & Queues

### Q7: Valid Parentheses
```javascript
function isValidParentheses(s) {
    const stack = [];
    const pairs = { ')': '(', ']': '[', '}': '{' };
    for (const char of s) {
        if ('([{'.includes(char)) stack.push(char);
        else if (stack.pop() !== pairs[char]) return false;
    }
    return stack.length === 0;
}
```

### Q8: Min Stack (O(1) getMin)
```javascript
class MinStack {
    constructor() { this.stack = []; this.minStack = []; }
    push(val) {
        this.stack.push(val);
        const min = !this.minStack.length ? val : Math.min(val, this.minStack.at(-1));
        this.minStack.push(min);
    }
    pop() { this.minStack.pop(); return this.stack.pop(); }
    getMin() { return this.minStack.at(-1); }
}
```

---

## 4. Hash Tables

### Q9: Group Anagrams
**Time**: O(n × k log k)
```javascript
function groupAnagrams(strs) {
    const map = new Map();
    for (const str of strs) {
        const sorted = str.split('').sort().join('');
        if (!map.has(sorted)) map.set(sorted, []);
        map.get(sorted).push(str);
    }
    return Array.from(map.values());
}
```

### Q10: LRU Cache
```javascript
class LRUCache {
    constructor(capacity) { this.capacity = capacity; this.cache = new Map(); }
    get(key) {
        if (!this.cache.has(key)) return -1;
        const val = this.cache.get(key);
        this.cache.delete(key);
        this.cache.set(key, val);
        return val;
    }
    put(key, val) {
        if (this.cache.has(key)) this.cache.delete(key);
        else if (this.cache.size >= this.capacity)
            this.cache.delete(this.cache.keys().next().value);
        this.cache.set(key, val);
    }
}
```

---

## 5. Trees

### Q11: Tree Traversals
```javascript
function inorder(node, res = []) {
    if (node) { inorder(node.left, res); res.push(node.val); inorder(node.right, res); }
    return res;
}
function preorder(node, res = []) {
    if (node) { res.push(node.val); preorder(node.left, res); preorder(node.right, res); }
    return res;
}
function levelOrder(root) {
    if (!root) return [];
    const result = [], queue = [root];
    while (queue.length) {
        const level = [];
        for (let i = queue.length; i > 0; i--) {
            const node = queue.shift();
            level.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        result.push(level);
    }
    return result;
}
```

### Q12: Maximum Depth
```javascript
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### Q13: Validate BST
```javascript
function isValidBST(root, min = -Infinity, max = Infinity) {
    if (!root) return true;
    if (root.val <= min || root.val >= max) return false;
    return isValidBST(root.left, min, root.val) && isValidBST(root.right, root.val, max);
}
```

---

## 6. Heaps

### Q14: Kth Largest Element
**Time**: O(n log k)
```javascript
function findKthLargest(nums, k) {
    const heap = new MinHeap();
    for (const num of nums) {
        heap.insert(num);
        if (heap.size() > k) heap.extractMin();
    }
    return heap.peek();
}
```

---

## 7. Graphs

### Q15: BFS and DFS
```javascript
function bfs(graph, start) {
    const visited = new Set([start]), result = [], queue = [start];
    while (queue.length) {
        const v = queue.shift();
        result.push(v);
        for (const { node } of graph.getNeighbors(v))
            if (!visited.has(node)) { visited.add(node); queue.push(node); }
    }
    return result;
}

function dfs(graph, start, visited = new Set(), result = []) {
    visited.add(start);
    result.push(start);
    for (const { node } of graph.getNeighbors(start))
        if (!visited.has(node)) dfs(graph, node, visited, result);
    return result;
}
```

### Q16: Number of Islands
```javascript
function numIslands(grid) {
    let count = 0;
    const rows = grid.length, cols = grid[0]?.length || 0;
    function dfs(r, c) {
        if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] === '0') return;
        grid[r][c] = '0';
        dfs(r+1,c); dfs(r-1,c); dfs(r,c+1); dfs(r,c-1);
    }
    for (let r = 0; r < rows; r++)
        for (let c = 0; c < cols; c++)
            if (grid[r][c] === '1') { count++; dfs(r, c); }
    return count;
}
```

---

## 8. Trie

```javascript
class Trie {
    constructor() { this.root = {}; }
    insert(word) {
        let node = this.root;
        for (const c of word) { node[c] = node[c] || {}; node = node[c]; }
        node.end = true;
    }
    search(word) {
        let node = this.root;
        for (const c of word) { if (!node[c]) return false; node = node[c]; }
        return !!node.end;
    }
    startsWith(prefix) {
        let node = this.root;
        for (const c of prefix) { if (!node[c]) return false; node = node[c]; }
        return true;
    }
}
```

---

*Previous: [JS Advanced](02_JavaScript_Advanced.md) | Next: [DSA Algorithms](04_DSA_Algorithms.md)*
