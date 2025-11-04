# Monotonic

**Monotonic:** *(of a function or quantity) varying in such a way that it either never decreases or never increases.*

A monotonic stack or queue is one whose elements are always sorted. It can be sorted either ascending or descending, depending on the algorithm. Monotonic stacks and queues maintain their sorted property by removing elements that would violate the property before adding new elements. For example, let's say you had a monotonically increasing stack, currently `stack = [1, 5, 8, 15, 23]`. You want to push `14` onto the stack. To maintain the sorted property, we need to first pop the `15` and `23` before pushing the `14` - after the push operation, we have `stack = [1, 5, 8, 14]`.

Example: **Sliding Window Maximum**

Given an integer array `nums` and an integer `k`, there is a sliding window of size `k` that moves from the very left to the very right. For each window, find the maximum element in the window.

---

Explanation:

It's easy to know what the maximum number is for a given window. You can just record it when you build it. The difficult part of this problem is, when the maximum number leaves the window, how do you know what is the second largest? When that number leaves, what's next?

We are concerned about the largest elements. We want to store the elements in a way that when the maximum element is removed, we know the second maximum, and when that element is removed, we know the third maximum, and so forth. This should also be updated on new elements being added. Let's say that the window was `[5, 3, 7, 1]`. Then the order of maximum elements would be `[7, 5, 3, 1]`. What happens if we add a `6`? We no longer care about the `5`, `3`, or `1`. Because the `6` came after them, it won't be removed before them, and since it is larger, there is no chance that the `5`, `3`, or `1` will ever be a maximum.

**When we see a number, we no longer care about any numbers in the window smaller than it, because they have no chance of ever being the maximum**. Thus, a monotonically decreasing data structure is what we should aim for (we remove elements smaller than new ones = the newest element is the smallest element). Should we use a stack or a queue? The problem describes a sliding window, which we know adds elements on the right and removes them from the left - this suggests a queue since operations are happening at opposite ends. However, the logic of removing elements smaller than the current number needs to happen from the right when we're talking about monotonic logic. Therefore, we will be adding and removing from the right, and removing from the left as the window tightens - this means we should use a **double-ended queue (deque)** for efficient *O(1)* operations.

Because we need to keep the window of size `k`, we should store indices instead of the actual numbers in our deque. This way, we know when the maximum element is beyond our window.

**More detailed explanation:**

This is a tough one! If we find a huge element, it may be the answer for a while, but it won't be the answer forever because eventually, the window will slide past it.

If we keep a monotonic decreasing data structure that contains only elements in the current window, then the first element will always be the maximum element of the window. If the window slides past this element, we can just remove it from the data structure, and the second largest element slides over to become the new largest element.

By storing indices, we can easily check when the window slides past the element at the first position, which means we need to remove it.

The tricky part is maintaining the data structure so that it is always in decreasing order and only contains elements in the current window. As explained above, when we see an element `curr`, **any element less than curr in the current window** is now useless. The window will surely slide past those elements before `curr`, so any future window that contains them will also contain `curr`, and of course `curr` is larger than them, so there is no chance they will ever be part of the answer.

That's why we should remove them in the same fashion as in the previous problem. In fact, if you check the code, it is very similar to the previous problem. The manner in which we maintain the monotonic property is identical. The main difference is that we need to additionally check if the element at the front is no longer part of the window.

Lastly, we only add to the answer once we have processed at least `k` elements. The first element in our monotonic data structure will always be the answer for the current window.

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function(nums, k) {
    let ans = [];
    let queue = [];
    for (let i = 0; i < nums.length; i++) {
        // maintain monotonic decreasing.
        // all elements in the deque smaller than the current one
        // have no chance of being the maximum, so get rid of them
        while (queue.length && nums[i] > nums[queue[queue.length - 1]]) {
            queue.pop();
        }
        
        queue.push(i);
        
        // queue[0] is the index of the maximum element.
        // if queue[0] + k == i, then it is outside the window
        if (queue[0] + k == i) {
             queue.shift();
        }
        
        // only add to the answer once our window has reached size k
        if (i >= k - 1) {
            ans.push(nums[queue[0]]);
        }
    }
    
    return ans;
};
```

While monotonic stacks and queues are not super popular, it's important to understand the logic behind them as it would be very difficult to derive it yourself in an interview. Try these upcoming practice problems before moving on to the quiz. These practice problems integrate concepts that we learned in the previous chapters.