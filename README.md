# leetcode-30days-challenge
With many of us around the world being encouraged to stay indoors and work from home, [LeetCode](https://leetcode.com) thought this is the perfect opportunity for us to focus on studying up for future coding interviews. To help us stay focused, `LeetCode` is running the [30-Day LeetCoding Challenge](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/) for April 2020. This is basically my journal where I'm dumping the solutions each day and how I'm thinking about the problems and the process of the solution. Also trying to add complexity(time, space) analysis of the the solutions that I have attached.

# Problems by Days
> April 2020
1. [Single Number](#1-single-number)
2. [Happy Number](#2-happy-number)
3. [Maximum Subarray](#3-maximum-subarray)
4. [Move Zeroes](#4-move-zeroes)
5. [Best Time to Buy and Sell Stock II](#5-best-time-to-buy-and-sell-stock-II)
6. [Group Anagrams](#6-group-anagrams)
7. [Counting Elements](#7-counting-elements)
8. [Middle of the Linked List](#8-middle-of-the-linked-list)
9. [Backspace String Compare](#9-backspace-string-compare)
10. [Min Stack](#10-min-stack)
11. [Diameter of Binary Tree](#11-diameter-of-binary-tree)
12. [Last Stone Weight](#12-last-stone-weight)
13. [Contiguous Array](#13-contiguous-array)
14. [Perform String Shifts](#14-perform-string-shifts)
15. [Product of Array Except Self](#15-product-of-array-except-self)
16. [Valid Parenthesis String](#16-valid-parenthesis-string)
17. [Number of Islands](#17-number-of-slands)
18. [Minimum Path Sum](#18-minimum-path-sum)
19. [Search in Rotated Sorted Array](#19-search-in-Rrtated-sorted-array)
20. [Construct Binary Search Tree from Preorder Traversal](#20-construct-binary-search-tree-from-preorder-traversal)
21. [Leftmost Column with at Least a One](#21-leftmost-column-with-at-least-a-one)
22. [Subarray Sum Equals K](#22-subarray-sum-equals-k)
23. [Bitwise AND of Numbers Range](#23-bitwise-and-of-numbers-range)
24. [LRU Cache](#24-lru-cache)
25. [Jump Game](#25-jump-game)
26. [Longest Common Subsequence](#26-longest-common-subsequence)
27. [Maximal Square](#27-maximal-square)
28. [First Unique Number](#28-first-unique-number)
29. [Binary Tree Maximum Path Sum](#29-binary-tree-maximum-path-sum)
30. [Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree](#30-check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree)

> May 2020
1. [First Bad Version](#1-first-bad-version)

# 1. Single Number

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3283/

## Solution Approach
The first day of leetcode-30day-challenge and they got a very easy problem here! But two things need to be noted though -

1. The algorithm should have a linear runtime complexity
2. Implement it without using any extra space/memory

So, the first solution I thought was to use a **HashMap** to keep how many times a number presents in the array and from thereafter traversing all the entries of the hashmap we can easily find which number presents only once in the array. Here, we're obeying note 1 but violating note 2.

To follow both of the notes; we'll implement a solution that includes the technique of binary operations. We know that `XOR` operator gives us `FALSE` or 0 when both of the operands are the same, right? If we can run `XOR` operation to all the values of the array and keep it in a `result` variable; finally we'll get the lonely/orphan value in the result which presents in the array once. Because `XOR` minus out the same values which exist twice in the array by returning 0. And any value `XOR`s with 0 returns the same value, that's why we will get our result in the variable.

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0;
        for(int n : nums) {
            result ^= n;
        }

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
We're traversing the array once, so the worst-case time complexity will be the length of the array

### Space Complexity `O(1)`
This solution has constant space complexity as we're using any additional data structure


# 2. Happy Number

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3284/

## Solution Approach
First, we need to clearly understand what is a *Happy Number* because that's the core of the problem. So, we need to run a loop and in each iteration to generate the sum of the squares of each digit of the number and that sum will be treated as the new value of the number in the next iteration. So, when that sum value will be 1, we can say that it's a *Happy Number* but what if we are not getting 1 ever! In the problem description, it says when a number is not *happy* it **loops endlessly in a cycle**. That's our logic to find the *unhappy* numbers. We'll keep track of the numbers we are generating in each iteration of the loop to a `HashMap` and check if the newly generated number is already there in the `HashMap`, if it exists then we can say for sure that an endless loop will occur and then immediately return that the number is *unhappy*

```cpp
class Solution {
public:
    int getSquareOfDigits(int n) {
        int sum = 0;

        while(n) {
            int p = n % 10;
            sum += (p * p);
            n /= 10;
        }

        return sum;
    }

    bool isHappy(int n) {
        unordered_map<int, int> M;

        int newNumber = n;
        M[newNumber] = 1;
        while(newNumber != 1) {
            newNumber = getSquareOfDigits(newNumber);
            if(M.find(newNumber) == M.end()) M[newNumber] = 1;
            else return false;
        }

        return true;
    }
};
```

## Complexity Analysis

### Time Complexity `O(*)` [WIP]
First, we need to calculate the time-complexity of `getSquareOfDigits()`; because in the main loop we're executing that function in every iteration. We're getting the squares of each digit of `n` which suffice that the loop of getting the square of digits take `O(logn)` time.

But finding the overall time-complexity of the `isHappy()` function seems a bit trickier; skipping it for now (will update it later)

### Space Complexity `O(*)` [WIP]
skipping it for now (will update it later)


# 3. Maximum Subarray

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3285/

## Solution Approach


```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSubArraySum = INT_MIN, contiguousSubArraySum = 0;

        for(int n : nums) {
            contiguousSubArraySum = max(n, contiguousSubArraySum + n);
            maxSubArraySum = max(maxSubArraySum, contiguousSubArraySum);
        }

        return maxSubArraySum;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`






# 4. Move Zeroes

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3286/

## Solution Approach


```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int len = nums.size();
        int i = 0, j = 0;

        while(i < len && j < len) {
            while(j < len && nums[j] != 0) j++;
            while(i < len && nums[i] == 0) i++;

            if(i > j && i < len && j < len) swap(nums[i], nums[j]);
            else i++;
        }
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 5. Best Time to Buy and Sell Stock II

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3287/

## Solution Approach


```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        if(len == 0) return 0;

        int profit = 0;
        for(int i = 0; i < len - 1; i++) {
            if(prices[i] < prices[i + 1]) {
                profit += prices[i + 1] - prices[i];
            }
        }

        return profit;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 6. Group Anagrams

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3288/

## Solution Approach


```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        int len = strs.size();
        vector<string> sortedStrs = strs;
        for(int i = 0; i < len; i++) {
            sort(sortedStrs[i].begin(), sortedStrs[i].end());
        }

        unordered_map<string, vector<string>> M;
        for(int i = 0; i < len; i++) {
            M[sortedStrs[i]].push_back(strs[i]);
        }

        vector<vector<string>> result;
        for(pair<string, vector<string>> entries : M) {
            result.push_back(entries.second);
        }

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 7. Counting Elements

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3289/

## Solution Approach


```cpp
class Solution {
public:
    int countElements(vector<int>& arr) {
        map<int, int> M;

        for(int num : arr) {
            if(M.find(num) == M.end()) M[num] = 1;
            else M[num]++;
        }

        int count = 0;
        for(pair<int, int> entries : M) {
            if(M.find(entries.first + 1) != M.end()) {
                count += M[entries.first];
            }
        }

        return count;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 8. Middle of the Linked List

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3290/

## Solution Approach


```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        if(head == NULL) return head;

        ListNode* hare = head;
        ListNode* tortoise = head;
        while(tortoise != NULL) {
            if(tortoise->next == NULL) return hare;

            hare = hare->next;
            tortoise = tortoise->next->next;
        }

        return hare;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 9. Backspace String Compare

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3291/

## Solution Approach


```cpp
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        stack<char> stackS, stackT;

        for(char s : S) {
            if(s != '#') stackS.push(s);
            else if(!stackS.empty()) stackS.pop();
        }

        for(char s : T) {
            if(s != '#') stackT.push(s);
            else if(!stackT.empty()) stackT.pop();
        }

        if(stackS.size() == stackT.size()) {
            while(!stackS.empty()) {
                char a = stackS.top();
                char b = stackT.top();
                if(a != b) return false;
                stackS.pop();
                stackT.pop();
            }

            return true;
        }

        return false;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 10. Min Stack

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3292/

## Solution Approach


```cpp
class Solution {
public:
    stack<pair<int, int>> S;

    MinStack() {

    }

    void push(int x) {
        pair<int, int> temp;
        if(S.empty()) {
            temp.first = x;
            temp.second = x;
        } else {
            pair<int, int> top = S.top();
            int min = top.second > x ? x : top.second;
            temp.first = x;
            temp.second = min;
        }

        S.push(temp);
    }

    void pop() {
        S.pop();
    }

    int top() {
        pair<int, int> top = S.top();
        return top.first;
    }

    int getMin() {
        pair<int, int> top = S.top();
        return top.second;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 11. Diameter of Binary Tree

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3293/

## Solution Approach


```cpp
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        int result = 0;
        dfs(root, result);

        return result;
    }

    int dfs(TreeNode* root, int& result) {
        if(root == NULL) return 0;

        int edgesInLeft = dfs(root->left, result);
        int edgesInRight = dfs(root->right, result);
        result = max(result, edgesInLeft + edgesInRight);

        return max(edgesInLeft, edgesInRight) + 1;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 12. Last Stone Weight

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3297/

## Solution Approach
After reading the problem statement, the first thing that hit my head was using `max-heap` because every-time we'll pick the **heaviest** stones from the collection and sometimes we'll put a new stone to the collection but the collection needs to be sorted in non-increasing order. We know that in c++ `Priority Queues` are a type of container adapters, which specifically designed such that the first element of the queue is the greatest of all elements in the queue and all the elements are in decreasing order(but we can create our custom priority queue where priority will be dependent on different factors rather than the *max* property).

To solve that problem, first I've declared a priority queue `PQ` which will contain integers. And by traversing through the vector list of stones we'll push each element to our newly created priority queue. After that, we'll run a loop over our priority queue unless there is more than 1 element in the queue. In each iteration of the loop, we'll get the top two elements of the queue and pop them out. Depending on the condition given -

- If `x == y`, both stones are destroyed i.e. we don't need to push any new elements to the queue
- If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `x - y`. So, here we need to push a new element to the queue of `x - y` weight.

Finally, when we'll be out of the loop that means our priority queue has less than 2 elements we'll return the result. But here we need to think about the two possible states that our queue might be:

- Having 1 element; then we'll return the top element of the queue
- Having 0 elements; that means all the stones are being destroyed and we'll return 0 in this case

```cpp
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> PQ;

        for(int stone : stones) {
            PQ.push(stone);
        }

        while(PQ.size() > 1) {
            int x = PQ.top();
            PQ.pop();

            int y = PQ.top();
            PQ.pop();

            if(x != y) {
                int z = x - y;
                PQ.push(z);
            }
        }

        return PQ.empty() ? 0 : PQ.top();
    }
};
```

## Complexity Analysis

### Time Complexity `O(nlogn)`
Converting an array into a priority queue takes `O(n)` time but the way we have done populating our priority queue, it will take `O(nlogn)` time.

Our final loop which iterates over the priority queue runs up to `n - 1` times. In each iteration it's doing up to three `O(logn)` operation; two *remove*s, and an optional *add*. Like always, the three is an ignored constant. So we're doing a total of `O(nlogn)` operations in the worst case.

### Space Complexity `O(n)`
We have linear space complexity here because of declaring `n` sized priority queue.

# 13. Contiguous Array

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3298/

## Solution Approach
Whenever we encounter any subarray related problems; the ready-made bruteforce approach of time complexity `O(n^2)` solution automatically comes into our head! I was quite confident that I can code that solution and eventually did but got **TLE**. So, finally, the solution that I implemented got **AC** was a classic counting approach!

We have a binary-array i.e. the array contains only 1's and 0's; we'll have a `count` variable and traversing through the array in each iteration we'll modify the value of the count regarding the value of the array. If we encounter 1 then we'll increment and for 0 we'll decrement the count. Now, maybe you are thinking like what are we getting from this count thing? So, the interesting part of this solution approach is this - whenever we encounter the same `count` value, that means we have a subarray of the same number of 1's and 0's and that's why the `count` becomes same again, right? To memorize every count value and their array-indexes we need some sort of data-structure. I've chosen HashMap to store count as key and index of the array as value. In this way whenever we find that we have a `count` that we encountered previously (i.e. exists in the hashmap) we can update our result by checking if it can beat the previous maximum length of the subarray.

```cpp
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> M;
        int len = nums.size(), result = 0, count = 0;

        /*
            why we're putting this here? Because we need to keep track of
            the initial count, because our count value can come to 0/initial
            value again, right? Then we can find the length of subarray by
            simply subtracting the indexes
        */
        M[count] = -1;
        for(int i = 0; i < len; i++) {
            count += nums[i] == 0 ? -1 : 1;

            if(M.find(count) != M.end()) {
                result = max(result, i - M[count]);
            } else {
                M[count] = i;
            }
        }

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
We're only traversing the array once; so the time complexity will be the length of the array.

### Space Complexity `O(n)`
We've added a hashmap to keep track of our count values and array-indexes. In the worst case where each value of the array is either 0 or 1, then the length of the hashmap will be the length of the array.

# 14. Perform String Shifts

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/529/week-2/3299/

## Solution Approach
The problem has basically two parts; one is to find an algorithm to solve how to do the left-right shifts/rotations in a string. And other is to find the minimum number of operations(left/right shifts) that need to be done into the given string to get the final result.

So, first, we'll be figuring out the algorithm to do the right and left shifts. If we can figure one out, the other will be simple because we know that one shift is a complement to another. If I say I need to do 3 amount of left shifts to a string that also suffice that I can find the same result by doing `lengthOfString - 3` number of right shifts. If you're not getting the idea here, just pick a pen and paper and do the shifts by hand, you'll definitely get it :) We'll implement the shifts in-place i.e. without using any extra data structure for storing the modified string and also which will be done in `O(L)` time-complexity. The approach is based on *reversal algorithm* for rotation. Let's deep dive into it. Say we have `abcdefg` as the given string and finally we've figured out that we need to do **3 left-shifts**. So, let's break it to a stepwise mind-map of our reversal algorithm:

```
"abcdefg"

/*
    Step 1: Break the original string into two parts,
    first 3 (same as the amount of rotation we have to do)
    will be one and later characters will form the other string
*/
"abc|defg"

/*
    Step 2: Reverse both parts of the string separately
*/
"cba|gfed"

/*
    Step 3: Finally reverse the whole string
*/
"defgabc"

^ We've got it, that's our result; just cross-check it!!
```

I hope, you have got the idea! So, we'll be only writing the reversal algorithm to do the left-shift and for right-shift, we'll use the same function of left-shift.

Now, we've come to the later problem, where we need to minimize the number of operations we'll do over the given string. So, we know left-shift and right-shift complements each other. So, in our given matrix of `shift` we've got multiple entries of both right and left shifts. We need to find the final number of shifts that actually matters, right? Cause there will be so many entries where one right-shift will negate other left-shift, or vice versa. We've taken two different variables i.e. `left`, `right` to sum up all the right shifts and left shifts by traversing through our shift matrix. And then we can find the difference between them and identify which one wins (i.e. right or left either will have maximum count over other) or they draw (same number of operation in both right and left, and we don't need to do any operation in that case; yayy!). We'll do one last thing here, we'll get the modulo of the final `numberOfOperations` for getting the `finalNumberOfOperations` because think about it this way - *"When we have a string of `n` characters and we do `n` number of left/right-shifts we'll get back the same string again, gotcha?"* So, by doing modulus we are removing those `n` number of operations which really doesn't affect our original string at all!

```cpp
class Solution {
public:
    string stringShift(string s, vector<vector<int>>& shift) {
        int left = 0, right = 0;
        for(vector<int> entry : shift) {
            if(entry[0] == 0) left += entry[1];
            else right += entry[1];
        }

        int opCount = left - right;
        if(opCount > 0) {
            leftShift(abs(opCount) % s.length(), s);
        } else {
            rightShift(abs(opCount) % s.length(), s);
        }

        return s;
    }

    void leftShift(int amount, string& s) {
        reverse(s.begin(), s.begin() + amount);
        reverse(s.begin() + amount, s.end());
        reverse(s.begin(), s.end());
    }

    void rightShift(int amount, string& s) {
        leftShift(s.length() - amount, s);
    }
};
```

## Complexity Analysis

### Time Complexity `O(N + L)`
Our solution approach had two parts, as we have discussed above. Let's find time complexity for both parts separately and finally we'll merge them.

The first step traverses through each of the N entries in the shift array, and we're adding up the total number of left shifts and the total number of right shifts. Handling each entry is an `O(1)` operation, so this first step has a total cost of `O(N)`.

The second step applies a single string-shift (right/left) operation. As discussed in the solution approach that our algorithm of a string-shift operation has a cost of `O(L)`. Here, `L` is the length of the given string.

As we are doing these steps one after the other, and we don't know which one is the max; `N` or `L`, we add them to get a final time complexity of `O(N + L)`

### Space Complexity `O(1)`
The first step uses constant extra space to keep track of the counts.
And also we're doing the string-shift operation in-place by using a mutable string. So, we've taken `O(1)` space-complexity in our solution.

# 15. Product of Array Except Self

> Problem Description: https://leetcode.com/explore/challenge/card/30-day-leetcoding-challenge/530/week-3/3300/

## Solution Approach
The problem is easy, right? But yeah the note they gave (not to use division) made the problem a tough one! The idea that we're using here to solve the problem is this - "For every given index, `[i]`, we will make use of the product of all the numbers to the left of it and multiply it by the product of all the numbers to the right. This will give us the product of all the numbers except the one at the given index `[i]`"

We'll use two different arrays to get the products of all the numbers from left and another one for getting the products of all the numbers from right. Before populating `left` and `right` we'll fill the boundary start value with 1; for `left` it'll be `0`<sup>th</sup> index and for `right` it'll be `len - 1`<sup>th</sup> index.

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int len = nums.size();
        vector<int> left(len), right(len), result(len);

        left[0] = 1;
        for(int i = 1; i < len; i++) left[i] = nums[i - 1] * left[i - 1];

        right[len - 1] = 1;
        for(int i = len - 2; i >= 0; i--) right[i] =  nums[i + 1] * right[i + 1];

        for(int i = 0; i < len; i++) result[i] = left[i] * right[i];

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Where `n` represents the number of elements in the input array. We use one iteration to construct the array `left`, one to construct the array `right` and one last to construct the `result` array using `left` and `right`.

### Space Complexity `O(n)`
Two intermediate arrays i.e. `right`, `left` that we constructed to keep track of product of elements to the left and right.

# 16. Valid Parenthesis String

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3301/

## Solution Approach


```cpp
class Solution {
public:
    bool checkValidString(string s) {
        int len = s.length();

        if(len == 0 || !s.compare("*")) return true;
        if(len == 1) return false;

        int left = 0;
        for(int i = 0; i < len; i++) {
            if(s[i] == ')') left--;
            else left++;

            if(left < 0) return false;
        }

        if(left == 0) return true;

        int right = 0;
        for(int i = len - 1; i >= 0; i--) {
            if(s[i] == '(') right--;
            else right++;

            if(right < 0) return false;
        }

        return true;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 17. Number of Islands

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3302/

## Solution Approach


```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int r = grid.size();
        if(r == 0) return 0;
        int c = grid[0].size();

        if(r == 0 && c == 0) return 0;

        int numberOfIslands = 0;
        for(int i = 0; i < r; i++) {
            for(int j = 0; j < c; j++) {
                if(grid[i][j] == '1') {
                    numberOfIslands += dfs(grid, i, j);
                }
            }
        }

        return numberOfIslands;
    }

    int dfs(vector<vector<char>>& grid, int i, int j) {
        int r = grid.size();
        int c = grid[0].size();

        if(i < 0 || i >= r || j < 0 || j >= c || grid[i][j] == '0') return 0;
        grid[i][j] = '0';

        dfs(grid, i, j - 1);
        dfs(grid, i, j + 1);
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);

        return 1;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 18. Minimum Path Sum

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3303/

## Solution Approach


```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int r = grid.size();
        int c = grid[0].size();

        vector<vector<int>> distance(r);

        for(int i = 0; i < r; i++) {
            distance[i] = vector<int>(c);
            for(int j = 0; j < c; j++) {
                distance[i][j] = grid[i][j];

                if(i > 0 && j > 0) {
                    distance[i][j] += min(distance[i][j - 1], distance[i - 1][j]);
                } else if(i > 0) {
                    distance[i][j] += distance[i - 1][j];
                } else if(j > 0) {
                    distance[i][j] += distance[i][j - 1];
                }
            }
        }


        return distance[r-1][c-1];
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`


# 19. Search in Rotated Sorted Array

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3304/

## Solution Approach


```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int len = nums.size();

        int pivot = findPivot(nums, target, 0, len - 1);

        if(pivot == -1) return binarySearch(nums, target, 0, len - 1);
        if(nums[pivot] == target) return pivot;

        if(nums[0] <= target) return binarySearch(nums, target, 0, pivot - 1);
        return binarySearch(nums, target, pivot + 1, len - 1);
    }

    int findPivot(vector<int>& nums, int target, int low, int high) {
        if(low > high) return -1;

        int mid = low + (high - low) / 2;
        if(mid < high && nums[mid] > nums[mid + 1]) return mid;
        if(mid > low && nums[mid - 1] > nums[mid]) return mid - 1;

        if(nums[low] > nums[mid]) return findPivot(nums, target, low, mid - 1);
        return findPivot(nums, target, mid + 1, high);
    }

    int binarySearch(vector<int>& nums, int target, int low, int high) {
        if(low > high) return -1;

        int mid = low + (high - low) / 2;
        if(nums[mid] == target) return mid;

        if(nums[mid] > target) return binarySearch(nums, target, low, mid - 1);
        return binarySearch(nums, target, mid + 1, high);
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 20. Construct Binary Search Tree from Preorder Traversal

> Problem Description:

## Solution Approach


```cpp
class Solution {
public:
    int currentIndex = 0;

    TreeNode* bstFromPreorder(vector<int>& preorder) {
        return helper(preorder, INT_MAX, INT_MIN);
    }

    TreeNode* helper(vector<int>& preorder, int upper, int lower) {
        if(currentIndex == preorder.size()) return NULL;

        int currentValue = preorder[currentIndex];
        if(currentValue > upper || currentValue < lower) return NULL;
        currentIndex++;

        TreeNode* root = new TreeNode(currentValue);
        root->left = helper(preorder, currentValue, lower);
        root->right = helper(preorder, upper, currentValue);

        return root;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 21. Leftmost Column with at Least a One

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3306/

## Solution Approach


```cpp
class Solution {
public:
    int leftMostColumnWithOne(BinaryMatrix &binaryMatrix) {
        vector<int> d = binaryMatrix.dimensions();
        int n = d[0];
        int m = d[1];

        vector<int> indexes;
        for(int i = 0; i < n; i++) {
            int index = binarySearch(binaryMatrix, i, 0, m - 1);
            if(index != -1) indexes.push_back(index);
        }

        sort(indexes.begin(), indexes.end());
        return indexes.empty() ? -1 : indexes[0];
    }

    int binarySearch(BinaryMatrix &binaryMatrix, int x, int low, int high) {
        if(low > high) return -1;

        int y = low + (high - low) / 2;
        int val = binaryMatrix.get(x, y);

        if(val == 1 && (y == 0 || binaryMatrix.get(x, y-1) == 0)) return y;
        else if(val == 1) return binarySearch(binaryMatrix, x, low, y - 1);
        return binarySearch(binaryMatrix, x, y + 1, high);
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 22. Subarray Sum Equals K

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3307/

## Solution Approach


```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> S;
        int len = nums.size();
        int result = 0;
        int sum = 0;

        for (int i = 0; i < len; i++) {
            sum += nums[i];
            if (sum == k) result++;
            if (S.find(sum - k) != S.end()) result += S[sum - k];

            S[sum]++;
        }

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 23. Bitwise AND of Numbers Range

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3308/

## Solution Approach


```cpp
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int shiftCount = 0;

        while(m != n) {
            m >>= 1; n >>= 1;
            shiftCount++;
        }

        return m << shiftCount;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 24. LRU Cache

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3309/

## Solution Approach


```cpp
class DLL {
public:
    int key;
    int value;
    DLL* prev;
    DLL* next;

    DLL(int _key, int _value) {
        key = _key;
        value = _value;
        prev = NULL;
        next = NULL;
    }
};

class LRUCache {
private:
    unordered_map<int, DLL*> M;
    int capacity;
    int totalEntries = 0;
    DLL* head = new DLL(0, 0);
    DLL* tail = new DLL(0, 0);

public:
    LRUCache(int _capacity) {
        capacity = _capacity;
        head->next = tail;
        tail->prev = head;
    }

    DLL* addInHead(int key, int value) {
        DLL* temp = new DLL(key, value);
        temp->next = head->next;
        head->next->prev = temp;
        temp->prev = head;
        head->next = temp;

        return temp;
    }

    void removeItem(DLL* item) {
        item->prev->next = item->next;
        item->next->prev = item->prev;

        delete item;
    }

    void removeFromTail() {
        DLL* temp = tail->prev;
        tail->prev = temp->prev;
        temp->prev->next = tail;

        delete temp;
    }

    int get(int key) {
        if(M.find(key) != M.end()) {
            int value = M[key]->value;
            DLL* temp = addInHead(key, value);
            removeItem(M[key]);
            M[key] = temp;
            return value;
        }

        return -1;
    }

    void put(int key, int value) {
        if(M.find(key) != M.end()) {
            DLL* temp = addInHead(key, value);
            removeItem(M[key]);
            M[key] = temp;
        } else {
            DLL* temp = addInHead(key, value);
            M[key] = temp;
            totalEntries++;
        }

        if(totalEntries > capacity) {
            DLL* temp = tail->prev;
            M.erase(temp->key);
            removeFromTail();
            totalEntries--;
        }
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 25. Jump Game

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3310/

## Solution Approach


```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int len = nums.size();
        int lastSuccessfulIndex = len - 1;

        for(int i = len - 2; i >= 0; i--) {
            if(i + nums[i] >= lastSuccessfulIndex) {
                lastSuccessfulIndex = i;
            }
        }

        return lastSuccessfulIndex == 0;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 26. Longest Common Subsequence

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3311/

## Solution Approach


```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int r = text1.length();
        int c = text2.length();

        vector<vector<int>> dp(r + 1);
        for(int i = 0; i <= r; i++) {
            vector<int> col(c + 1);
            dp[i] = vector<int>(col);
        }

        for(int i = 0; i <= r; i++) {
            for(int j = 0; j <= c; j++) {
                if(i == 0 || j == 0) dp[i][j] = 0;
            }
        }

        int result = 0;
        for(int i = 1; i <= r; i++) {
            for(int j = 1; j <= c; j++) {
                if(text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }

        return dp[r][c];
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 27. Maximal Square

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3312/

## Solution Approach


```cpp
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int result = 0;
        int r = matrix.size();
        if(r == 0) return result;

        int c = matrix[0].size();
        vector<vector<int>> dp(r + 1);
        for(int i = 0; i <= r; i++) {
            dp[i] = vector<int>(c + 1);
        }

        for(int i = 0; i <= r; i++) {
            for(int j = 0; j <= c; j++) {
                if(i == 0 || j == 0) dp[i][j] = 0;
            }
        }

        for(int i = 1; i <= r; i++) {
            for(int j = 1; j <= c; j++) {
                if(matrix[i - 1][j - 1] == '0') dp[i][j] = 0;
                else dp[i][j] = 1 + min(dp[i - 1][j - 1], min(dp[i][j - 1], dp[i - 1][j]));

                result = max(result, dp[i][j]);
            }
        }

        return result * result;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 28. First Unique Number

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/531/week-4/3313/

## Solution Approach


```cpp
class DLL {
public:
    int value;
    DLL* prev;
    DLL* next;

    DLL(int _value) {
        value = _value;
        prev = NULL;
        next = NULL;
    }
};

class FirstUnique {
public:
    unordered_map<int, DLL*> M;
    DLL* head = new DLL(0);
    DLL* tail = new DLL(0);

    FirstUnique(vector<int>& nums) {
        int len = nums.size();
        head->next = tail;
        tail->prev = head;

        for(int i = 0; i < len; i++) add(nums[i]);
    }

    void removeItem(DLL* item) {
        item->prev->next = item->next;
        item->next->prev = item->prev;

        delete item;
    }

    DLL* addInTail(int value) {
        DLL* temp = new DLL(value);
        temp->next = tail;
        temp->prev = tail->prev;
        tail->prev->next = temp;
        tail->prev = temp;

        return temp;
    }

    int showFirstUnique() {
        return head->next == tail ? -1 : head->next->value;
    }

    void add(int value) {
        if(M.find(value) == M.end()) {
            DLL* temp = addInTail(value);
            M[value] = temp;
        } else if(M[value] != NULL) {
            removeItem(M[value]);
            M[value] = NULL;
        }
    }
};

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique* obj = new FirstUnique(nums);
 * int param_1 = obj->showFirstUnique();
 * obj->add(value);
 */s
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 29. Binary Tree Maximum Path Sum

> Problem Description:

## Solution Approach


```cpp
class Solution {
public:
    int result = INT_MIN;
    int maxPathSum(TreeNode* root) {
        helper(root);
        return result;
    }

    int helper(TreeNode* root) {
        if(root == NULL) return 0;

        int left = max(0, helper(root->left));
        int right = max(0, helper(root->right));
        result = max(result, left + right + root->val);
        return max(left, right) + root->val;
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 30. Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/532/week-5/3315/

## Solution Approach


```cpp
class Solution {
public:
    bool isValidSequence(TreeNode* root, vector<int>& arr) {
        return helper(root, arr, 0);
    }

    bool helper(TreeNode* node, vector<int>& arr, int currentIndex) {
        if(node == NULL || currentIndex == arr.size()) return false;

        if(node->val == arr[currentIndex]) {
            if(currentIndex == arr.size() - 1 && node->left == NULL && node->right == NULL) {
                return true;
            }

            int left = helper(node->left, arr, currentIndex + 1);
            int right = helper(node->right, arr, currentIndex + 1);

            return (left || right);
        } else {
            return false;
        }
    }
};
```

## Complexity Analysis

### Time Complexity `O()`

### Space Complexity `O()`

# 1. First Bad Version

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3316/

## Solution Approach
This is a straight forward binary search problem. If we get that `mid` element is bad-version then we'll check if the previous version was good, then this is the first occurence of bad-version i.e. our result. Otherwise we'll do recursive binary-search from `low` to `mid - 1` because first occurance of the bad-version will be in the left-side. Finally, If the mid element is not bad then we know that the bad-version element will be in the right-side, so we'll do binary-search from `mid + 1` to `high`.

```cpp
class Solution {
public:
    int firstBadVersion(int n) {
        return binarySearch(1, n);
    }

    int binarySearch(int low, int high) {
        if(low > high) return -1;
        int mid = low + (high - low) / 2;

        if(isBadVersion(mid)) {
            if(!isBadVersion(mid - 1)) return mid;
            else return binarySearch(low, mid - 1);
        } else {
            return binarySearch(mid + 1, high);
        }
    }
};
```

## Complexity Analysis

### Time Complexity `O(logn)`
The `binarySearch` will run `log(n)` times in the worst case

### Space Complexity `O(logn)`
As we are using a recursive solution, the call-stack will use `log(n)` space