# leetcode-30days-challenge
With many of us around the world being encouraged to stay indoors and work from home, LeetCode though this is the perfect opportunity for us to focus on studying up for future coding interviews. To help us stay focused, LeetCode is running the [30-Day LeetCoding Challenge](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/) for April 2020.

# Problems by Days

12.  [Last Stone Weight](#12-last-stone-weight)

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
