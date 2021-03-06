# Coding Challenges
I started this repository as a journal of my [30-Day LeetCoding Challenge](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/) for April 2020! Later in the month of May, [LeetCode](https://leetcode.com) arranged another [May LeetCoding Challenge](https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/). Finally this has become my core journal where I put the solution approach and the thought process behind each coding problem that I do. Also I have added complexity(time, space) analysis of the the solutions.

# Problems by Collection
> [April 2020](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/)
1. [Single Number](#single-number)
2. [Happy Number](#happy-number)
3. [Maximum Subarray](#maximum-subarray)
4. [Move Zeroes](#move-zeroes)
5. [Best Time to Buy and Sell Stock II](#best-time-to-buy-and-sell-stock-II)
6. [Group Anagrams](#group-anagrams)
7. [Counting Elements](#counting-elements)
8. [Middle of the Linked List](#middle-of-the-linked-list)
9. [Backspace String Compare](#backspace-string-compare)
10. [Min Stack](#min-stack)
11. [Diameter of Binary Tree](#diameter-of-binary-tree)
12. [Last Stone Weight](#last-stone-weight)
13. [Contiguous Array](#contiguous-array)
14. [Perform String Shifts](#perform-string-shifts)
15. [Product of Array Except Self](#product-of-array-except-self)
16. [Valid Parenthesis String](#valid-parenthesis-string)
17. [Number of Islands](#number-of-islands)
18. [Minimum Path Sum](#minimum-path-sum)
19. [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)
20. [Construct Binary Search Tree from Preorder Traversal](#construct-binary-search-tree-from-preorder-traversal)
21. [Leftmost Column with at Least a One](#leftmost-column-with-at-least-a-one)
22. [Subarray Sum Equals K](#subarray-sum-equals-k)
23. [Bitwise AND of Numbers Range](#bitwise-and-of-numbers-range)
24. [LRU Cache](#lru-cache)
25. [Jump Game](#jump-game)
26. [Longest Common Subsequence](#longest-common-subsequence)
27. [Maximal Square](#maximal-square)
28. [First Unique Number](#first-unique-number)
29. [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)
30. [Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree](#check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree)
---
> [May 2020](https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/)
1. [First Bad Version](#first-bad-version)
2. [Jewels and Stones](#jewels-and-stones)
3. [Ransom Note](#ransom-note)
4. [Number Complement](#number-complement)
5. [First Unique Character in a String](#first-unique-character-in-a-string)
6. [Majority Element](#majority-element)
7. [Cousins in Binary Tree](#cousins-in-binary-tree)
8. [Check If It Is a Straight Line](#check-if-it-is-a-straight-line)
9. [Valid Perfect Square](#valid-perfect-square)
10. [Find the Town Judge](#find-the-town-judge)
11. [Flood Fill](#flood-fill)
---
> [Blind75](https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU)

1. [Two Sum](#two-sum)
2. [Best Time to Buy and Sell Stock](#best-time-to-buy-and-sell-stock)
3. [Contains Duplicate](#contains-duplicate)
4. [Product of Array Except Self](#product-of-array-except-self)
5. [Maximum Subarray](#maximum-subarray)
6. [Maximum Product Subarray](#maximum-product-subarray)
7. [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
8. [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)
9. [3Sum](#3Sum)
10. [Container With Most Water](#container-with-most-water)
11. [Sum of Two Integers](#sum-of-two-integers)
12. [Number of 1 Bits](#number-of-1-bits)

---

# Single Number

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


# Happy Number

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


# Maximum Subarray

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3285/

## Solution Approach
The solution of this problem is very intutive! We'll solve it in a greedy approach. We'll take two variables upfront, i) `contiguousSubArraySum` and ii) `maxSubArraySum`. We'll iterate through the array and in each iteration we'll check if adding that value to our previous subArray increases the chance of getting the maximum `contiguousSubArraySum` rather taking the value itself by starting to form a new sub-array makes our `contiguousSubArraySum` higher than previous. By this process we're actually forming candidate sub-arrays and their sum. Also, in each iteration we'll check if the `maxSubArraySum` value can be updated i.e. getting the mximumSum from those subArray candidates.

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

### Time Complexity `O(n)`
Linear time solution. We are iterating through the array of numbers once, which has `n` number of elements

### Space Complexity `O(1)`
We are just taking two variables for storing sums, which give us a constant space solution


# Move Zeroes

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3286/

## Solution Approach
Bruteforce solution approach is pretty easy; just traverse through the given array an for all non-zero elements we'll push that element into a new array. Finally fill the new array's available indexes with zeroes. But, this solution has `O(n)` space complexity, as we are creating a new array of size `n`.

Now, we'll try to solve this problem in-place i.e. without using an additional array. We'll have an `index` variable starting from `0` and traversing through the array, whenever we will encounter a non-zero element, we'll put that element in that `index` and increment the `index`. Finally we'll fill the rest of the array from `index` to the last index with zeroes. As you can see, here we are doing array write operation `n` times for any case. Let's go through the code -

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int len = nums.size();
        int index = 0;

        for(int i = 0; i < len; i++) {
            if(nums[i] != 0) {
                nums[index++] = nums[i];
            }
        }

        for(int i = index; i < len; i++) {
            nums[i] = 0;
        }
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time solution, the length of the given array is `n`

### Space Complexity `O(1)`
Constant space

# Best Time to Buy and Sell Stock II

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3287/

## Solution Approach
This problem is tricky; just becuase of this - _You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times)._ So, if you miss this, you'll dive into a more bizarre state of the solution! The description says we can buy and sell a share many times; so, as many transactions as we can make, eventually we'll maximize our profit. So, to do that, we'll buy one share at day `i` and sell that at day `i + 1`, and also buy the same stock again at day `i + 1` and sell at day `i + 2`, going until the end of the days/prices. But we just need to check if we can make profit out of that buy and sell by making sure that `ith` day price is lower than the `(i + 1)th` day price.

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

### Time Complexity `O(n)`
Linear time

### Space Complexity `O(1)`
Constant space

# Group Anagrams

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

# Counting Elements

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

# Middle of the Linked List

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

# Backspace String Compare

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

# Min Stack

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

# Diameter of Binary Tree

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

# Last Stone Weight

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

# Contiguous Array

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

# Perform String Shifts

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



# Valid Parenthesis String

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

# Number of Islands

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

# Minimum Path Sum

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


# Search in Rotated Sorted Array

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/530/week-3/3304/

## Solution Approach
We are given a sorted array but rotated! First, we'll try to find the pivot point, where the array is rotated, and then we'll search our target using binary-search either in the left or side sub-array of the pivot!

In the begining, we need to check two things, i) if the `low` and `high` are same i.e. the array has only one element, we'll instantly return (if the target and that single element is equal then we'll return 0 otherwise -1) or ii) array is not rotated; we will do binary-search on the whole array. Awesome!

For finding the pivot point, we'll use binary-search technique! In each iteration we'll use these conditions and move `low` or `high` indexes accordingly -

```
i) if the mid element is bigger than the mid + 1; return mid as pivot-point
ii) if the mid element is lower than the mid - 1; return mid - 1 as pivot-point
iii) if the lower element is bigger than the mid element; we'll move the high index to mid - 1;
iv) if the lower element is lower than the mid element; we'll move the low index to mid + 1
```

After finding the pivot point, we'll find in which sub-array we'll do the binary-search, by the following logic -
```
if the lower element of the given array is bigger then the target that means our target lies in the right-subarray of the pivot and otherewise the left sub-array (including the pivot element)
```
here's the code -


```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(!nums.size()) return -1;

        int low = 0, high = nums.size() - 1;

        if(low == high) return nums[0] == target ? 0 : -1;
        if(nums[low] < nums[high]) return binarySearch(nums, low, high, target);

        int pivot = findPivot(nums, low, high);
        if(nums[low] > target) return binarySearch(nums, pivot + 1, high, target);
        else if(nums[low] <= target) return binarySearch(nums, low, pivot, target);

        return -1;
    }

    int binarySearch(vector<int>& nums, int low, int high, int target) {
        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target) return mid;
            else if(nums[mid] > target) high = mid - 1;
            else if(nums[mid] < target) low = mid + 1;
        }

        return -1;
    }

    int findPivot(vector<int>& nums, int low, int high) {
        if(low == high) return -1;

        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] > nums[mid + 1]) return mid;
            else if(nums[mid] < nums[mid - 1]) return mid - 1;

            if(nums[low] > nums[mid]) high = mid - 1;
            else if(nums[low] < nums[mid]) low = mid + 1;
        }

        return -1;
    }
};
```

## Complexity Analysis

### Time Complexity `O(logn)`
We have done binary-search in finding the pivot and then finding the target

### Space Complexity `O(1)`
Constant space; no additional space to solve the problem

# Construct Binary Search Tree from Preorder Traversal

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

# Leftmost Column with at Least a One

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

# Subarray Sum Equals K

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

# Bitwise AND of Numbers Range

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

# LRU Cache

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

# Jump Game

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

# Longest Common Subsequence

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

# Maximal Square

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

# First Unique Number

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

# Binary Tree Maximum Path Sum

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

# Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree

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

---

# First Bad Version

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


# Jewels and Stones

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3317/

## Solution Approach
This is a very basic search problem. We have to search if characters from one string is present in another! So, brute-force approach would cost `O(m * n)`. So, we can use an `unordered_map` or `unordered_set` to store the jewels upfront and later we can find from that container in constant time.

```cpp
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        unordered_set<char> jewels;
        for(char c : J) jewels.insert(c);

        int count = 0;
        for(char c : S) if(jewels.find(c) != jewels.end()) count++;

        return count;
    }
};
```

## Complexity Analysis

### Time Complexity `O(max(m, n))`
Here, `m` is the length of the string `J` and `n` is the length of the string `S`. We are traversing through the both string; and finding in `set` is constant time! So, the maximum of `(m, n)` will be our worst case time-complexity.

### Space Complexity `O(m)`
We are using an extra data-structure i.e. `set` to store the `jewels`, so that we can find in `constant` time!

# Ransom Note

> Problem Description: https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3318/

## Solution Approach
If we can count the characters of `magazine` and later check that the required characters for generating the `ransomNote` is there in the `magazine`, will give us the result. To store the counts, we'll use an `unordered_map`, which will allow us to search/retrieve characters and count in constant time! If we don't find any character from `ransomNote` in the hash-map, we'll return `false`, otherwise we'll kepp decrementing the count of each characters that we've encountered. In the meantime if we get any character that presents in the hash-map but has no count i.e. 0 we'll also return false in that case, because we don't have the required characters in the `magazine` to form our `ransomNote`.


```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> M;

        for(char ch : magazine) {
            if(M.find(ch) == M.end()) M[ch] = 1;
            else M[ch]++;
        }

        for(char ch : ransomNote) {
            if(M.find(ch) == M.end() || M[ch] == 0){
                return false;
            } else {
                M[ch]--;
            }
        }

        return true;
    }
};
```

## Complexity Analysis

### Time Complexity `O(max(m, n))`
Here, `m`, `n` is the respective size of `ransomNote` and `magazine`. We're looping around (traversing) two strings, so the time complexity would be `O(max(m, n))`

### Space Complexity `O(n)`
Additional unordered_map of `n` size to map the magazine characters with their number of occurences in the magazine

# Number Complement

> Problem Description: https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3319/

## Solution Approach
First of all I tried to solve this in a very naive approach which is -

- Convert the decimal number into a binary number (string)
- Complement each character of that binary string
- Finally convert that binary string to a decimal number

I was doing this process just to brush up my binary-decimal conversion logics!! xD Here is the code -


```cpp
class Solution {
public:
    string decimalToBinary(int num) {
        string binary = "";
        while(num) {
            int a = num % 2;
            binary = to_string(a) + binary;
            num /= 2;
        }

        return binary;
    }

    int binaryToDecimal(string binary) {
        int len = binary.length();
        int decimal = 0;

        for(int i = 0; i < len; i++) {
            decimal += (binary[i] - '0') * pow(2, len - i -1);
        }

        return decimal;
    }

    string getComplement(string binary) {
        string result = "";

        for(char ch : binary) {
            result += ch == '1' ? '0' : '1';
        }

        return result;
    }

    int findComplement(int num) {
        string binary = decimalToBinary(num);
        string binaryComplement = getComplement(binary);
        int decimalComplement = binaryToDecimal(binaryComplement);

        return decimalComplement;
    }
};
```

But, let's do it in a more standard way. The problem itself says that it's a binary arithmetic problem! We need to flip each bit of the binary number and to do that we will use the property of `XOR`. We know that if we `XOR` any bit with 1, that bit will be flipped (complemented). So, we can use a mask bit i.e. 1 and do `XOR` with the original number. Let's say 5 is our given number; the binary of 5 is `101`. So, when we'll do `XOR` 5 with 1; we will get 4 i.e. `100` because only the LSB has been flipped. Now the question is, how to flip other bits, we can modify our `mask` to do that. After each `XOR` operation we'll `LeftShift` our mask by 1, which shift the mask-bit to the left and eventually we'll finish doing `XOR` on each bit of the number and get the result! So, second question comes to our mind is - how to know the number of `LeftShift` operations we have to do on the `mask`? We'll do left shift until the length of the original number, thus all the bits will be flipped; makes sense right? To simplify this process more, we can generate our `mask` in a single operation (not looping through the length of the number and doing left-shift each time). So, our desired mask will be a binary number whose length is the same as the given number but all the bits will be 1 (for achieving that flip on each bit). Suppose, our given number is 5; if we take 1 and then left shift that number by 3 (because the length of binary 5 -> 101 is 3) we'll get `1000` and then negating that number by 1 will give us `111`. Finally doing `XOR` between `mask` and the given number will give us the result.

_N.B. One point we missed above; when we're doing left-shift operation on 1 by the length of the number, then we'll have an overflow of 32 bit, right? So to tackle that we can typecast the number by (long) and eventually after subtracting by 1 we'll get an integer (32 bit)_

```cpp
class Solution {
public:
    int findComplement(int num) {
        int len = (int) log2(num);
        int mask = (long) (1 << len + 1) - 1;

        return num ^ mask;
    }
};
```

## Complexity Analysis

### Time Complexity `O(1)`
### Space Complexity `O(1)`


# First Unique Character in a String

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3320/

## Solution Approach
This problem has basically two parts;

1) Find the unique characters of the string and
2) Find the first index from those unique characters

For finding the first answer, we'll use a hash-map and store the count of each character of the string, and by this way we'll get the unique characters. But, to answer the second question we'll do a modification in our first approach; we won't store count against each character in the hash-map, we'll store their index. While traversing through the string and storing indexes in the hash-map, if we encounter a character which already exists in the hash-map we'll simply put `-1` against that character. Now, you can relate the whole thing together. Traversing through the hash-map entries and finding the minimum index will be our final step. If we can't find any unique character, i.e. either hash-map has no entries or all of them has a value `-1`, we'll return `-1`!

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        int len = s.length();
        unordered_map<char, int> M;

        for(int i = 0; i < len; i++) {
            if(M.find(s[i]) == M.end()) {
                M[s[i]] = i;
            } else {
                M[s[i]] = -1;
            }
        }

        int firstChar = INT_MAX;
        for(auto entry : M) {
            if(entry.second != -1) firstChar = min(firstChar, entry.second);
        }

        return firstChar == INT_MAX ? -1 : firstChar;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time solution; traversing through the string of length `n` and then traversing through the hash-map which will have maximum of `n` entries.

### Space Complexity `O(n)`
We're using a hash-map of size `n` to store the indexes against each character.


# Majority Element

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3321/

## Solution Approach
Another counting problem! Very basic, we're using a hash-map to store the count of occurances against each number from the given array. And after traversing through the given array and putting all the counts in the hash-map, we can then traverse through the hash-map and check what's the majority-element we got here!

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> M;
        int n = nums.size(), result;

        for(int i = 0; i < n; i++) M[nums[i]]++;

        for(auto entry : M) {
            if(entry.second > n/2) {
                result = entry.first;
                break;
            }
        }

        return result;
    }
};
```

But this solution approach has linear time complexity and linear space complexity! Is there any way that we can do it in constant space? I thought of an approach but that would increase the time complexity though. As we are sure that there will be definitely one major element in the array, then we can sort the array and return the middle element of the array as a major-element. Because if there's a major element, mid index will have the value which is the majority.

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        return nums[ceil(nums.size() / 2)];
    }
};
```

As you can see, this solution approach has `O(nlogn)` time-complexity because we are doing sorting here! Another optimal solution can be of constant-space and liner-time. To have the idea of the solution approach, let's think about the array of numbers into two sub-group; i) Consists of all the majority elements and ii) Has the non-majority numbers/elements. And obviously the length of the first sub-group will be always maximum because of the obvious case of having a majority element! Let's say the majority elements occured `m` times out of total `n` numbers in the array. By the definition of majority element, we can write `m/n` > `1/2`. At most one of the two distinct numbers that are discarded can be the majority element. So, if we discard the two elements from the array, the ratio of the number of remaining majority elements to the total number of elements will be either i) Two of them are non-majority numbers `(m/n-2)` and ii) One of them are the majority elements `(m - 1/ n - 2)`. It is easy to verify that if `m/n` > `1/2` then `m/n-2` > `1/2` and `m-1/n-2` > `1/2`. So, by discarding items from the array, the difference in the size of the two subgroups are really changed! So, if we can finish this discursion process and finally we'll get our majority element left! The algorithm is as folows- we'll have a major-element candidate, we'll track it's count too. We'll start iterating over the array and if we get the same number as the candidate-majority-element we will incresase the count, otherwise decrese. If at any stage of the iteration, count becomes zero then we'll assign the current-indexed-value as a candidate. After finishing the array traversal we will get our majority-element!

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        int majorityElement, count = 0;

        for(int i = 0; i < n; i++) {
            if(count == 0) majorityElement = nums[i];

            if(majorityElement != nums[i]) count--;
            else count++;
        }

        return majorityElement;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time

### Space Complexity `O(1)`
Constant space


# Cousins in Binary Tree

> Problem Description: https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/534/week-1-may-1st-may-7th/3322/

## Solution Approach

This is a classic tree problem! We can address this problem as a DFS/BFS problem at it's core! Because we have to find two things here, one is that each of the nodes are in the same depth/level of the tree and they don't have the same parent (that's the rule to become cousins, amirite?). To do that we have to traverse the tree, we can either choose DFS or BFS to do the traversal. First, I did a DFS solution approach but later I thought that the BFS would be better because we can early return from the traversal in BFS. We know, in BFS we do level order traversal, so after each level traversal if we get that we have found only one of the elements in this level, that means our two of the nodes are not in the same level. But in DFS we know that we end up traversing to the very deep of a path, so we cannot say before traversing the whole tree that two of our nodes are in same level or not! And to solve the second problem, we have to track the parent of each node and finally check whether they are same or not for the two given nodes!

```cpp
class Solution {
public:
    bool isCousins(TreeNode* root, int x, int y) {
        queue<pair<TreeNode*, int>> Q;
        Q.push(make_pair(root, 0));

        int found = 0, parentX = 0, parentY = 0;
        while(!Q.empty()) {
            int n = Q.size();

            for(int i = 0; i < n; i++) {
                pair<TreeNode*, int> entry = Q.front();
                TreeNode* node = entry.first;
                int parent = entry.second;

                if(node->val == x) {
                    parentX = parent;
                    found++;
                } else if(node->val == y) {
                    parentY = parent;
                    found++;
                }

                if(node->right != NULL) Q.push(make_pair(node->right, node->val));
                if(node->left != NULL) Q.push(make_pair(node->left, node->val));
                Q.pop();
            }

            switch(found) {
                case 1:
                    return false;
                    break;
                case 2:
                    return parentX != parentY;
                    break;
            }
        }

        return false;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
`n` is the number of nodes in the binary tree. In the worst case, we might have to visit all the nodes of the binary tree.

### Space Complexity `O(n)`
In the worst case, we need to store all the nodes of the last level in the queue. The last level of a binary tree can have a maximum of `n/2` nodes. Also we have to store `parentX`, `parentY` and `found` variables, this will take constant space in term of `n`. That results in a space complexity of `O(n/2 + c) = O(n)`

# Check If It Is a Straight Line

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3323/

## Solution Approach
The problem is a bit related to basic concepts 2D geometry. So, we have to identify if the given points can form a straight line or not! To do that we'll use the concept of `slope` from 2D geometry. First, if there are only two given points, without any calculation we can say that these two points are able to form a straight line! So, if we have more than two points given, we'll calculate the slope from first two points and later, traversing through the rest of the points and calculate slope between new two points in each iteration; if the previously calculated slope is mismatched with the `newSlope`, they can't form a straight line.

To calculate the slope between two coordinates in the 2d plane, we use this formula -
```
(x1, y1) -> (x2, y2)

slope = | (y2 - y1) / (x2 - x1) |
```
We are taking the modulus/abs of the slope because the sign denotes the direction of the slope, we are not aware of the direction for this problem, we only consider that whethere the points are in a straight line or not! Also as you can see that we are dividing by `(x2 - x1)`, so in any point if this value becomes `0` we have to return `Infinity` as slope! For simplicity we are just returning the maximum double i.e. `DBL_MAX` number to identify as `Infinity`!

```cpp
class Solution {
public:
    double getSlope(vector<int> c1, vector<int> c2) {
        double x = c2[0] - c1[0];
        double y = c2[1] - c1[1];

        if(x == 0) return DBL_MAX;
        return abs(y / x);
    }

    bool checkStraightLine(vector<vector<int>>& coordinates) {
        int n = coordinates.size();
        if(n <= 2) return true;

        double slope = getSlope(coordinates[0], coordinates[1]);

        for(int i = 2; i < n; i++) {
            double newSlope = getSlope(coordinates[i - 1], coordinates[i]);
            if(slope != newSlope) return false;
        }

        return true;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time; only traversing the array once and in each iteration we are calculating the slope; which takes constant time i.e. `O(1)`. So, our whole task takes `O(n)` time, here `n` is the number of given points.

### Space Complexity `O(1)`
Constant space

# Valid Perfect Square

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3324/

## Solution Approach
For `num > 2` the square root is always less than `num / 2` and greater than `1` i.e. the square root will be in this range - `1 < x < num / 2`. Since `x` is an integer, the problem goes down to the search in the sorted set of integer numbers. Binary search is a standard way to proceed in such a situation.

Let's fo through the algorithm -

```
- if num < 2, return true
- set the low = 2, and the high = num / 2.

- while left <= right:
    - take mid = (left + right) / 2 as a guess, compute sqr = mid * mid and compare it with given num:

    - if sqr == num, then the given number is perfect square, return true
    - if sqr > num, move high to mid - 1
    - otherwise, move low to mid + 1

if we're here, the number is not a prefect square; return false
```
Let's have an example -
```
16

i = 0
    low = 2, high = 8
    mid = 5
    sqr = 25
    sqr > num
        high = mid - 1 = 4

i = 1
    low = 2, high = 4
    mid = 3
    sqr = 9
    sqr < num
        low = mid + 1 = 4

i = 2
    low = 4, high = 4
    mid = 4
    sqr = 16
    mid == sqr
        return true
```
here's the code -
```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        if(num < 2) return true;
        long long low = 2, high = num / 2, mid, sqr;

        while(low <= high) {
            mid = low + (high - low) / 2;
            sqr = mid * mid;

            if(sqr == num) {
                return true;
            } else if(sqr > num) {
                high = mid - 1;
            } else if (sqr < num) {
                low = mid + 1;
            }
        }

        return false;
    }
};
```

## Complexity Analysis

### Time Complexity `O(logn)`
Logarithmic time; as we are running binary search over `n/2`

### Space Complexity `O(1)`
Constant space, we're not using any extra space

# Find the Town Judge

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3325/

## Solution Approach
If we go through the problem description carefully, the main points of a person being the judge are these -
- The town judge trusts *nobody*
- *Everybody* (except for the town judge) trusts the town judge

So, there are given array of vertices, each vertice looks like this - `[x, y]` which denotes `x` trusts `y`. If we can count the trusts that each person is getting, then we can easily find the judge because everyone except the judge will trust that person. Suppose, we have `N` people, so for being a judge, s/he has to have the trust-count `N - 1`, amirite? Okay, so how are we gonna get that count? It's very simple -

```
For x trusts y
i) person[y]++
ii) person[x]--
```
So, basically we are incrementing the count if someone gets a trust or decrement if gives! As the judge won't give any trust to anyone and also gets trust from all persons except him/herself, final count will be `N - 1`.

However, there's an early exit logic we can put here. As we have seen that, a person should get `N - 1` trusts to become a judge. So, our trust array should be atleast `N - 1` in size, otherwise we can say that there is no judge in the community! Here's the code -

```cpp
class Solution {
public:
    int findJudge(int N, vector<vector<int>>& trust) {
        if (trust.length < N - 1) return -1;

        vector<int> person(N + 1, 0);
        for(auto t : trust) {
            person[t[1]]++;
            person[t[0]]--;
        }

        for(int i = 1; i <= N; i++) {
            if(person[i] == N - 1) return i;
        }

        return -1;
    }
};
```

## Complexity Analysis

### Time Complexity `O(E)`
We traverse through the `trust` array once. The cost of doing this is `O(E)`. We then loop over the people. The cost of doing this is `O(N)`.

Going through this, it looks like one of those graph problems where the cost is `O(max(N, E) = O(N + E)`. After all, we don't know whether `E` or `N` is the bigger one, right?

However, in the best case where we are doing the early exit, the time-complexity is `O(1)`. And in the worst case, we know that `E ≥ N − 1`. Therefore, in the worst case, `E` has to be bigger, and so we can simply drop the `N`, and final time-complexity would be `O(E)`

### Space Complexity `O(N)`
We allocated an array of length `N + 1`; to store the count. Because in big-oh notation we drop constants, this leaves us with `O(N)`

# Flood Fill

> Problem Description: https://leetcode.com/explore/featured/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3326/

## Solution Approach
This is a graph traversal problem. We have to start from the `(sr, sc)` index and change the color of it along with all the adjacent cells to the given `newColor`, recursively! We can solve it using either BFS or DFS; I'm using DFS here. For marking the already colored cells, I've declared a 2D boolean array and after changing a cells color I'll mark `true` in that `colored` array. Here, my `dfs` function is a recursive one, which I'll use to color all the adjacent cells (4 directions) of the `image` 2D array.

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int r = image.size();
        int c = image[0].size();

        vector<vector<bool>> colored(r);
        for(int i = 0; i < r; i++) colored[i] = vector<bool>(c, false);

        dfs(image, colored, sr, sc, image[sr][sc], newColor);
        return image;
    }

    void dfs(vector<vector<int>>& image, vector<vector<bool>>& colored, int i, int j, int color, int newColor) {
        int r = image.size();
        int c = image[0].size();
        if(i < 0 || i > r - 1
           || j < 0 || j > c - 1
           || colored[i][j] != 0
           || image[i][j] != color) return;

        image[i][j] = newColor;
        colored[i][j] = true;

        dfs(image, colored, i, j - 1, color, newColor);
        dfs(image, colored, i, j + 1, color, newColor);
        dfs(image, colored, i + 1, j, color, newColor);
        dfs(image, colored, i - 1, j, color, newColor);
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Where `n` is the number of pixels in the image. We might process every pixel.

### Space Complexity `O(n)`
The size of the implicit call-stack when calling `dfs` function


---

# Two Sum

> Problem Description: https://leetcode.com/problems/two-sum/

> 📘 EPI: Page 40

## Solution Approach
Given an array of integers and a target value. We need to find out two numbers from the array which will sum the target value. To do this in brute-force approach we can easily check all possible cases to find two numbers which will get us the target sum. But this approach will cost `O(n^2)` time. We can minimize this time complexity by using a hash-table. We'll iterate through the array and for each number we'll store the subtracted value from the target and the index to the hash-table. And in each iteration we'll check if the current number exists in the hash-table. If we get the value in hash-table that means we've found the two values that will give us the target sum!

Suppose, given -

```
nums = [2, 3, 5, 7]
target = 9
```

Let's have our **HashMap** `M` and visualize what we're doing in each iteration -

```
i = 0:
check nums[i] = 2 exists in M
[NO] Insert 9 - 2 = (7 -> 0) in M
     M = { 7: 0 }

i = 1
check nums[i] = 3 exists in M
[NO] Insert 9 - 3 = (6 -> 1) in M
     M = { 7: 0, 6: 1 }

i = 2
check nums[i] = 5 exists in M
[NO] Insert 9 - 5 = (4 -> 2) in M
     M = { 7: 0, 6: 1, 4: 2 }

i = 3
check nums[i] = 7 exists in M
[YES] return { i, M[7]] }
```
That's basically the illustration of our thought process, let's now try to code -

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int size = nums.size();
        unordered_map<int, int> M;

        for(int i=0; i<size; i++) {
            int diff = target - nums[i];
            if(M.find(nums[i]) != M.end()) {
                return { M[nums[i]], i };
            }

            M[diff] = i;
        }

        return {};
    }
};
```

This solution has linear time-complexity and linear space-complexity i.e. `O(n)`. Can we achieve this in constant space by not using any additional data-structure? We can do that if the given array is sorted, then we can use an invariant to solve that in constant space! Having two pointers e.g. `i` and `j`, pointing to the `0`th index and the `n-1`th index of the array! In each iteration we'll check if we can get `A[i] + A[j] == target`, because that's the result we want! But, what will happen when there will be inequality or what will be the policy to move the indexes? Here, we are using the invariants. If we get `A[i] + A[j] < target`, we know that we cannot get the target by moving `i` forward, because it'll give us another larger value then the current `A[i]` as the array is sorted; so, we'll move `j` by decrementing the value, by this way we'll have a chance of minimizing the `A[i] + A[j]` and take closer to the `target` value. Otherwise, when `A[i] + A[j] > target`, if we understand the previous invariant then we can easily go with this, which is moving `i` forward because our sum i.e. `A[i] + A[j]` is lower than the target, if we move the `i`the index forward we'll get a highe value. Let's go through a illustration of our solution approach -

```
A = [2, 5, 7, 9, 13]
target = 14

i = 0, j = 4
A[i] + A[j] ::= 2 + 13 ::= 15 > target ::= 15 > 14
Decrement j ::= 3

i = 0, j = 3
A[i] + A[j] ::= 2 + 9 ::= 11 < target ::= 11 < 14
Increment i ::= 1

i = 1, j = 3
A[i] + A[j] ::= 5 + 9 ::= 14 = target ::= 14 = 14
Done, return { i, j } ::= { 1, 3 }
```
Here's the code of this solution -

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for(int i = 0, j = nums.size() - 1; i < j; ) {
            if(nums[i] + nums[j] == target) {
                return { i, j };
            } else if(nums[i] + nums[j] > target) {
                j--;
            } else {
                i++;
            }
        }

        return {};
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time; as we need to iterate the whole array in the worst case

### Space Complexity `O(1)`
Constant space; no additional data-structure applied


# Best Time to Buy and Sell Stock

> Problem Description: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

> 📘 EPI: 6.6, Page 1

## Solution Approach
We can solve that by iterating over the given array of prices once! We have to keep track of the minimum-price we have seen so far and In each day we'll check if we can maximize our profit by calculating the difference between minimum price we have seen so far and the price of the current day. Let's see an example -

```
A = [7, 1, 5, 3, 6]
maxProfit = 0, minPrice = MAX

In each iteration we'll do these
    minPrice = min(A[i], minPrice)
    maxProfit = max(maxProfit, A[i] - minPrice)

i = 0
minPrice = min(7, MAX) ::= 7
maxProfit = max(0, 7 - 7) ::= 0

i = 1
minPrice = min(1, 7) ::= 1
maxProfit = max(0, 1 - 1) ::= 0

i = 2
minPrice = min(5, 1) ::= 1
maxProfit = max(0, 5 - 1) ::= 4

i = 3
minPrice = min(3, 1) ::= 1
maxProfit = max(4, 3 - 1) ::= 4

i = 4
minPrice = min(6, 1) ::= 1
maxProfit = max(4, 6 - 1) ::= 5

return maxProfit ::= 5

```
Here's the code -

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxProfit = 0;
        int minPrice = INT_MAX;

        int size = prices.size();
        for(int i=0; i<size; i++) {
            int diff = prices[i] - minPrice;
            minPrice = min(minPrice, prices[i]);
            maxProfit = max(maxProfit, diff);
        }

        return maxProfit;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time solution; only traversing the array

### Space Complexity `O(1)`
Constant space

# Contains Duplicate

> Problem Description: https://leetcode.com/problems/contains-duplicate/

## Solution Approach
Brute-force approach of checking each pair of the array, if they are same or not will give us the solution, but not an optimal solution of `O(n^2)` time-complexity! Let's try to minimize the time-complexity. If we sort the given array and then traverse through the array checking if `A[i]` and `A[i + 1]` is equal or not, this will give us the solution. Sorting will take `O(nlogn)` and traversing the array takes `O(n)` which suffices that the solution has time-complexity of `O(nlogn)`. This solution approach is pretty straight forward and we can easily visualize what's happening, let's dive into the code -

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());

        for(int i = 1; i < n; i++) {
            if(nums[i] == nums[i - 1]) return true;
        }

        return false;
    }
};
```

The both of the above solution approaches have constant space-complexity. If we consider doing a trade-off between space and time-complexity, we can create a solution having linear time and space-complexity. By using a `HashMap` or `HashSet` we can store the values during the traversal of the array and in each iteration we can check whether the current-value is in the hash-map or not (this check operation can be done in constant time, because we're using hash-map or set here)! If it exists in the hash-map we can return `true`, otherwise return `false`. Let's illustrate the solution approach by an example first -

```
A = [1,2,3,1]
Define a Set S

In each iteration we'll do these
    Check A[i] exists in S
    [NO] Insert A[i] in the set
    [YES] return true

i = 0
S = {1}

i = 1
S = {1, 2}

i = 2
S = {1, 2, 3}

i = 3
return true
```
Here's the code -

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> S;

        for(int num : nums) {
            if(S.find(num) == S.end()) S.insert(num);
            else return true;
        }

        return false;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time

### Space Complexity `O(n)`
Linear space

# Product of Array Except Self

> LeetCode: https://leetcode.com/problems/product-of-array-except-self/

> 📘 EPI: Page 40

## Solution Approach
From the looks of it, this seems like a dead simple problem to solve in linear time and space. We can simply get the product of all the elements in the given array and then, for each of the elements `nums[i]` of the array, we can simply find product of array except self value by dividing the product by the `nums[i]` value.

Doing this for each of the elements would solve the problem. However, there's a note in the problem which says that we are not allowed to use **division** operation. That makes solving this problem a bit harder.

For getting the product of array except self-value of any index `i`, if we can find the product of all number in the left side of `i` and product of all the numbers in the right side of `i`, multiplying both of them will give us the result, let's illustrate this -

```
nums = [1, 2, 3, 4]

left = [1, 1, 2, 6]
right = [24, 12, 4, 1]

out = [1, 12, 8, 6]
```
To solve this, we need to have two arrays of `n` size to store the products of `left` and `right`. From these we can finally incorporate the `ans`.

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> L(n, 0), R(n, 0), ans(n);

        L[0] = 1;
        for(int i = 1; i < n; i++) L[i] = L[i - 1] * nums[i - 1];

        R[n - 1] = 1;
        for(int i = n - 2; i >= 0; i--) R[i] = R[i + 1] * nums[i + 1];

        for(int i = 0; i < n; i++) ans[i] = L[i] * R[i];

        return ans;
    }
};
```
This solution has linear i.e. `O(n)` time and space complexity. If we want to do this in constant space, we can do that by merging the `left` and `right` arrays into our `ans` array! The code is simple, let's look into it -

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, 0);

        ans[0] = 1;
        for(int i = 1; i < n; i++) ans[i] = ans[i - 1] * nums[i - 1];

        int R = 1;
        for(int i = n - 1; i >= 0; i--) {
            ans[i] = ans[i] * R;
            R *= nums[i];
        }

        return ans;
     }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time

### Space Complexity `O(1)`
Constant space


# Maximum Product Subarray

> LeetCode: https://leetcode.com/problems/maximum-product-subarray/

## Solution Approach
This problem is pretty much same as the maximum subarry sum, instead of sum, here we have to find the product/multiplication. For finding the sum, we have to only calculate the max-sum! But here, we have to keep track of both max and min product because two negative numbers can multiply and become a max number from a min number! In each iteration we have to find out the `maxProduct` from the current-number, previous maxProduct multiply by current-number and previous minProduct multiply by current-number. Also we'll update result/ans i.e. maximumProductSubArray in each iteration.

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int productMax = 1, productMin = 1, result = INT_MIN;

        for(int n : nums) {
            int tempProductMax = productMax;
            productMax = max(max(productMax * n, productMin * n), n);
            productMin = min(min(tempProductMax * n, productMin * n), n);
            result = max(result, productMax);
        }

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time
### Space Complexity `O(1)`
Constant space

# Find Minimum in Rotated Sorted Array

> LeetCode: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

## Solution Approach
This problem is kind of related to the _search in rotated sorted array_! So, we are given a sorted array but rotated! First, we'll try to find the pivot point, where the array is rotated. Finally, the minimum number will be in the `(pivot + 1)`th index because the array is sorted, _amirite_? In the begining we need to check two things, i) if the `low` and `high` are same i.e. the array has only one element or ii) array is not rotated; we will return the first element of the array. Awesome!

For finding the pivot point, we'll use binary-search technique! In each iteration we'll use these conditions and move `low` or `high` indexes accordingly -
```
i) if the mid element is bigger than the mid + 1; return mid as pivot-point
ii) if the mid element is lower than the mid - 1; return mid - 1 as pivot-point
iii) if the lower element is bigger than the mid element; we'll move the high index to mid - 1;
iv) if the lower element is lower than the mid element; we'll move the low index to mid + 1
```
here's the code -
```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        if(low == high || nums[low] < nums[high]) return nums[0];

        int pivot = findPivot(nums, low, high);
        return nums[pivot + 1];
    }

    int findPivot(vector<int>& nums, int low, int high) {
        while(low <= high) {
            int mid  = low + (high - low) / 2;

            if(nums[mid] > nums[mid + 1]) return mid;
            if(nums[mid] < nums[mid - 1]) return mid - 1;

            if(nums[low] < nums[mid]) low = mid + 1;
            else if(nums[low] > nums[mid]) high = mid - 1;
        }

        return -1;
    }
};
```

## Complexity Analysis

### Time Complexity `O(logn)`
Logarithmic time; we are doing binary-search to search the pivot point and then in constant time we're getting the minimum element of the array

### Space Complexity `O(1)`
Constant space

# 3Sum

> LeetCode: https://leetcode.com/problems/3sum

> 📘 EPI: 18.4

## Solution Approach
This problem is a bit tricky because in the result there cannot be any duplicate entry of `(i, j, k)` which sums `0`. We have already seen an invariant solution approach of solving two sum problem, we can implement that here! First, we'll sort our given array, it'll be easier to remove all the duplicates from a sorted array and also implement 2sum in `O(n)` time-complexity. After that, we'll iterate through the sorted-array. If the current value is greater than `0`, we'll early exit from the loop because remaining values cannot sum to `0` as they all are greater than `0`. Also, if the current value is the same as the one before, we'll skip it; otherwise, we'll call `sortedThreeSum` for the current position `i`. In the `sortedThreeSum` function we'll get two pointers, low = `i + 1` and high = `n - 1`, here n is the size of the given array. We'll run a loop until `low < high`. And in each iteration we'll check these -

```
- We'll calculate sum = nums[i] + nums[low] + nums[high]
- If the sum is greater than zero, decrement high. Also decrement high if the value is the same as for high + 1
- If sum is less than zero, increment low. Also increment low if the value is the same as for low - 1
- Otherwise, we found a triplet: push it to the result and decrement high, increment low
```

Here's the code -
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());

        vector<vector<int>> result;
        for(int i = 0; i < n && nums[i] <= 0; i++) {
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            sortedThreeSum(nums, i, result);
        }

        return result;
    }

    void sortedThreeSum(vector<int>& nums, int i, vector<vector<int>>& result) {
        int low = i + 1, high = nums.size() - 1;

        while(low < high) {
            int sum = nums[i] + nums[low] + nums[high];
            if(sum > 0 || (high < nums.size() - 1 && nums[high] == nums[high + 1])){
                high--;
            } else if(sum < 0 || (low > i + 1 && nums[low] == nums[low - 1])) {
                low++;
            } else if(sum == 0) {
                result.push_back({ nums[i], nums[low++], nums[high--] });
            }
        }
    }
};
```

## Complexity Analysis

### Time Complexity `O(n^2)`
Our `sortedThreeSum` takes `O(n)` time, and we call it `n` times. Sorting the array takes `O(nlogn)`, so overall complexity is `O(nlogn + n^2)`. This is asymptotically equivalent to `O(n^2)`

### Space Complexity `O(1)`
Constant space solution

# Container With Most Water

> LinkedIn: https://leetcode.com/problems/container-with-most-water

> 📘 EPI: 18.7

## Solution Approach
The intuition behind this solution approach is that the area formed between two lines will always be limited by the height of the shorter line. And, the more the distance between the lines, the more will be the area obtained.

This is a two-pointer technique based solution; one pointer at the beginning and another at the end of the array. We'll use `result` variable to store the maximum area obtained till now. In each iteration, we'll find out the area formed between them, update `result` and move the pointers. For finding the area we'll follow this -
```
- find the effective-height from the two sticks (minimum height will be effective)
- find area = effective-heigh x distance-between-two-pointers
```
And for moving the pointers we'll follow this algorithm -
```
We'll move the pointer which has the shorter-height stick; because by moving the shorter-one we are increasing the chance of increasing the effective-height, and also the area!
```
We can apply an early exit strategy, when the given array will have less than two elemments, we can return `0`. Here's the code -

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        if(height.size() < 2) return 0;

        int left = 0, right = height.size() - 1;
        int result = INT_MIN;
        while(left < right) {
            int effectiveHeight = min(height[left], height[right]);
            result = max(result, effectiveHeight * (right - left));

            if(effectiveHeight == height[left]) left++;
            else if(effectiveHeight == height[right]) right--;
        }

        return result;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time, we're only traversing the given array

### Space Complexity `O(1)`
Constant space

# Sum of Two Integers

> LeetCode: https://leetcode.com/problems/sum-of-two-integers

> 📘 EPI: 5.5

## Solution Approach
To solve this problem we'll use the logics behind *Half Adder* and *Carry Look-ahead Adder (CLA)*. Sum of two bits can be obtained by performing `XOR (^)` of the two bits. Carry bit can be obtained by performing `AND (&)` of two bits. And after each XOR and AND operation we'll left-shift the carry by 1 because *CLA* suggests us to add the carry to the next bit! Until there is no carry we'll do the same process.

*N.B. Use unsigned int data-type for carry, it fixes the negative number issue. Fid out, why!!*

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        while(b) {
            unsigned int carry = a & b;
            a = a ^ b;
            b = carry << 1;
        }

        return a;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Linear time, where `n` is the width of the operands i.e. `a` or `b`

### Space Complexity `O(1)`
Constant space

# Number of 1 Bits

> LeetCode: https://leetcode.com/problems/number-of-1-bits

> 📘 EPI: Page 44

## Solution Approach
We'll find what's in the `LSB` in each iteration we'll right-shift the original number by one bit and again check the `LSB` by doing `AND` operation to 1!

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;

        while(n) {
            count += n & 1;
            n >>= 1;
        }

        return count;
    }
};
```

## Complexity Analysis

### Time Complexity `O(n)`
Liner time, n is the number of bits of the given number

### Space Complexity `O(1)`
Constant space


