# leetcode-30days-challenge
With many of us around the world being encouraged to stay indoors and work from home, [LeetCode](https://leetcode.com) thought this is the perfect opportunity for us to focus on studying up for future coding interviews. To help us stay focused, `LeetCode` is running the [30-Day LeetCoding Challenge](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/) for April 2020. This is basically my journal where I'm dumping the solutions each day and how I'm thinking about the problems and the process of the solution. Also trying to add complexity(time, space) analysis of the the solutions that I have attached.

# Problems by Days

1. [Single Number](#1-single-number)
2. [Happy Number](#2-happy-number)
12. [Last Stone Weight](#12-last-stone-weight)
13. [Contiguous Array](#13-contiguous-array)
14. [Perform String Shifts](#14-perform-string-shifts)


# 1. Single Number

> Problem Description: https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/528/week-1/3283/

## Solution Approach
The first day of leetcode-30day-challenge and they got a very easy problem here! But two things need to be noted though -

1. The algorithm should have a linear runtime complexity
2. Implement it without using any extra space/memory

So, the first solution I thought was to use a **HashMap** to keep how many times a number presents in the array and from thereafter traversing all the entries of the hashmap we can easily find which number presents only once in the array. Her, we're obeying note 1 but violating note 2.

To follow both of the notes; we'll implement a solution that includes the technique of binary operations. We know that `XOR` operator gives us `FALSE` or 0 when both of the operands are the same, right? If we can run `XOR` operation to all the values of the array and keep it in a `result` variable; finally we'll get the lonely/orphan value in the result which presents in the array once. Because `XOR` minus out the same values which exist twice in the array by returning 0. And any value `XOR`s with 0 returns the same value, that's why we will get our result in the variable which is used to do all the binary operations on the array values.

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