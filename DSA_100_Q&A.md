# DSA 100 Must-Do Questions - Complete Solution Guide
> Curated for Top Product Companies (Google, Amazon, Meta, Flipkart, Atlassian, JP Morgan, Uber)
> Language: Java | Focus: Optimal Approach + Interview Reasoning

---

## Table of Contents
1. [Arrays and Hashing](#pattern-1-arrays--hashing)
2. [Two Pointers](#pattern-2-two-pointers)
3. [Sliding Window](#pattern-3-sliding-window)
4. [Stack](#pattern-4-stack)
5. [Binary Search](#pattern-5-binary-search)
6. [Linked List](#pattern-6-linked-list)
7. [Trees](#pattern-7-trees)
8. [Heap / Priority Queue](#pattern-8-heap--priority-queue)
9. [Backtracking](#pattern-9-backtracking)
10. [Tries](#pattern-10-tries)
11. [Graphs](#pattern-11-graphs)
12. [Dynamic Programming 1D](#pattern-12-dynamic-programming---1d)
13. [Dynamic Programming 2D](#pattern-13-dynamic-programming---2d)
14. [Top Interview Questions and Answers](#top-dsa-interview-questions--answers)

---

## Pattern 1: Arrays & Hashing

### Q1. Two Sum (Easy)
**Approach:** HashMap - store complement lookups  
**Time:** O(n) | **Space:** O(n)

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement))
            return new int[]{map.get(complement), i};
        map.put(nums[i], i);
    }
    return new int[]{};
}
```
**Why this works:** One-pass HashMap avoids O(n^2) brute force. Store value-to-index mapping; check complement before inserting.

---

### Q2. Contains Duplicate (Easy)
**Approach:** HashSet  
**Time:** O(n) | **Space:** O(n)

```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (!seen.add(num)) return true;
    }
    return false;
}
```
**Interview note:** `set.add()` returns false if element already exists - cleaner than `contains` + `add`.

---

### Q3. Valid Anagram (Easy)
**Approach:** Frequency count array (26 chars)  
**Time:** O(n) | **Space:** O(1)

```java
class Solution {

    public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        int[] freq = new int[26];

        for (int i = 0; i < s.length(); i++) {

            freq[s.charAt(i) - 'a']++;

            freq[t.charAt(i) - 'a']--;
        }

        for (int count : freq) {

            if (count != 0) {
                return false;
            }
        }

        return true;
    }
}
```
**Follow-up:** Unicode chars? Use HashMap instead of int[26].

---

### Q4. Group Anagrams (Medium)
**Approach:** Sort each word as key in HashMap  
**Time:** O(n * k log k) | **Space:** O(n * k)

```java
Input:
strs = ["eat","tea","tan","ate","nat","bat"]

Output:
[
  ["eat","tea","ate"],
  ["tan","nat"],
  ["bat"]
]
import java.util.*;

class Solution {

    public List<List<String>> groupAnagrams(String[] strs) {

        HashMap<String, List<String>> map = new HashMap<>();

        for (String word : strs) {

            char[] arr = word.toCharArray();

            Arrays.sort(arr);

            String key = new String(arr);

            map.putIfAbsent(key, new ArrayList<>());

            map.get(key).add(word);
        }

        return new ArrayList<>(map.values());
    }
}
```
**Optimization:** Use frequency string as key: "a2b1" instead of sorting (O(k) vs O(k log k)).

---

### Q5. Top K Frequent Elements (Medium)
**Approach:** HashMap + Min Heap (Most Common Interview Solution)  
**Time:** O(n) | **Space:** O(n)

Input:

nums = [1,1,1,2,2,3]

k = 2

Output:

[1,2]

```java

import java.util.*;

class Solution {

    public int[] topKFrequent(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();

        // Count frequency
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);   //this way we get the value or default 0 then we add +1
        }

        // Min Heap based on frequency
        PriorityQueue<Map.Entry<Integer, Integer>> pq =
                new PriorityQueue<>((a, b) -> a.getValue() - b.getValue());     //set queue min value based on top

        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {

            pq.offer(entry);

            if (pq.size() > k) {
                pq.poll();
            }
        }

        int[] result = new int[k];

        for (int i = k - 1; i >= 0; i--) {
            result[i] = pq.poll().getKey();
        }

        return result;
    }
}

```
---
**Kth Largest Element in an Array**

nums = [3,2,1,5,6,4]

k = 2

Output:

5
```java
import java.util.PriorityQueue;

class Solution {

    public int findKthLargest(int[] nums, int k) {

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        for (int num : nums) {

            minHeap.offer(num);

            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }

        return minHeap.peek();
    }
}
```



---

### Q6. Product of Array Except Self (Medium)
**Approach:** Prefix and Suffix product in O(1) extra space  
**Time:** O(n) | **Space:** O(1) (output array not counted)
Input:

nums = [1,2,3,4]

Output:

[24,12,8,6]

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];

        for (int i = 0; i < n; i++) {
            int product = 1;

            for (int j = 0; j < n; j++) {
                if (i != j)
                    product *= nums[j];
            }

            ans[i] = product;
        }

        return ans;
    }
}
```
**Interview note:** No division allowed - classic constraint. Two-pass: left products then multiply right products in place.

---

### Q7. Longest Consecutive Sequence (Medium)
**Approach:** HashSet - only start counting from sequence start  
**Time:** O(n) | **Space:** O(n)

```java
public int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int n : nums) set.add(n);
    int longest = 0;
    for (int n : set) {
        if (!set.contains(n - 1)) { // start of sequence
            int len = 1;
            while (set.contains(n + len)) len++;
            longest = Math.max(longest, len);
        }
    }
    return longest;
}
```
**Key insight:** Only count from sequence starts (n-1 not in set). Prevents O(n^2) re-counting.

---

## Pattern 2: Two Pointers

### Q8. Valid Palindrome (Easy)
**Approach:** Two pointers from both ends, skip non-alphanumeric  
**Time:** O(n) | **Space:** O(1)

Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

```java
class Solution {
    public boolean isPalindrome(String s) {

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {

            while (left < right &&
                   !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }

            while (left < right &&
                   !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            if (Character.toLowerCase(s.charAt(left)) !=
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

---

### Q9. Two Sum II - Sorted Array (Medium)
**Approach:** Two pointers (works because array is sorted)  
**Time:** O(n) | **Space:** O(1)

```java
public int[] twoSum(int[] numbers, int target) {
    int l = 0, r = numbers.length - 1;
    while (l < r) {
        int sum = numbers[l] + numbers[r];
        if (sum == target) return new int[]{l+1, r+1};
        else if (sum < target) l++;
        else r--;
    }
    return new int[]{};
}
```

---

### Q10. 3Sum (Medium)
**Approach:** Sort + Two Pointers, skip duplicates carefully  
**Time:** O(n^2) | **Space:** O(1)

```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        int l = i + 1, r = nums.length - 1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                res.add(Arrays.asList(nums[i], nums[l], nums[r]));
                while (l < r && nums[l] == nums[l+1]) l++;
                while (l < r && nums[r] == nums[r-1]) r--;
                l++; r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return res;
}
```
**Interview note:** Duplicate skipping is the hardest part - skip both outer `i` loop and inner `l`/`r` pointers.

---

### Q11. Container With Most Water (Medium)
**Approach:** Two pointers - always move the shorter side  
**Time:** O(n) | **Space:** O(1)

```java
public int maxArea(int[] height) {
    int l = 0, r = height.length - 1, max = 0;
    while (l < r) {
        max = Math.max(max, Math.min(height[l], height[r]) * (r - l));
        if (height[l] < height[r]) l++;
        else r--;
    }
    return max;
}
```
**Why move shorter side:** Moving taller side can only decrease width with no gain in height.

---

### Q12. Trapping Rain Water (Hard)
**Approach:** Two Pointers with max tracking  
**Time:** O(n) | **Space:** O(1)

```java
public int trap(int[] height) {
    int l = 0, r = height.length - 1;
    int leftMax = 0, rightMax = 0, water = 0;
    while (l < r) {
        if (height[l] <= height[r]) {
            leftMax = Math.max(leftMax, height[l]);
            water += leftMax - height[l];
            l++;
        } else {
            rightMax = Math.max(rightMax, height[r]);
            water += rightMax - height[r];
            r--;
        }
    }
    return water;
}
```
**Key insight:** Water at position i = min(maxLeft, maxRight) - height[i]. The pointer on the smaller-max side is the bottleneck.

---

## Pattern 3: Sliding Window

### Q13. Best Time to Buy and Sell Stock (Easy)
**Approach:** Track running minimum, compute max profit  
**Time:** O(n) | **Space:** O(1)

```java
public int maxProfit(int[] prices) {
    int minPrice = Integer.MAX_VALUE, maxProfit = 0;
    for (int price : prices) {
        minPrice = Math.min(minPrice, price);
        maxProfit = Math.max(maxProfit, price - minPrice);
    }
    return maxProfit;
}
```

---

### Q14. Longest Substring Without Repeating Characters (Medium)
**Approach:** Sliding Window + HashMap for last index  
**Time:** O(n) | **Space:** O(min(m,n))

```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();
    int max = 0, left = 0;
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (map.containsKey(c) && map.get(c) >= left)
            left = map.get(c) + 1;
        map.put(c, right);
        max = Math.max(max, right - left + 1);
    }
    return max;
}
```
**Interview note:** Jump `left` directly to `last_index + 1` instead of shrinking one by one.

---

### Q15. Longest Repeating Character Replacement (Medium)
**Approach:** Sliding Window - track max frequency char in window  
**Time:** O(n) | **Space:** O(1)

```java
public int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int left = 0, maxFreq = 0, result = 0;
    for (int right = 0; right < s.length(); right++) {
        maxFreq = Math.max(maxFreq, ++count[s.charAt(right) - 'A']);
        while ((right - left + 1) - maxFreq > k) {
            count[s.charAt(left++) - 'A']--;
        }
        result = Math.max(result, right - left + 1);
    }
    return result;
}
```
**Key insight:** Window is valid if `windowSize - maxFreq <= k`. We only need to shrink when invalid.

---

### Q16. Permutation in String (Medium)
**Approach:** Fixed-size sliding window with frequency matching  
**Time:** O(n) | **Space:** O(1)

```java
public boolean checkInclusion(String s1, String s2) {
    if (s1.length() > s2.length()) return false;
    int[] s1Count = new int[26], s2Count = new int[26];
    for (int i = 0; i < s1.length(); i++) {
        s1Count[s1.charAt(i) - 'a']++;
        s2Count[s2.charAt(i) - 'a']++;
    }
    if (Arrays.equals(s1Count, s2Count)) return true;
    for (int i = s1.length(); i < s2.length(); i++) {
        s2Count[s2.charAt(i) - 'a']++;
        s2Count[s2.charAt(i - s1.length()) - 'a']--;
        if (Arrays.equals(s1Count, s2Count)) return true;
    }
    return false;
}
```

---

### Q17. Minimum Window Substring (Hard)
**Approach:** Sliding Window with `have`/`need` counter  
**Time:** O(n) | **Space:** O(n)

```java
public String minWindow(String s, String t) {
    if (t.isEmpty()) return "";
    Map<Character, Integer> need = new HashMap<>(), window = new HashMap<>();
    for (char c : t.toCharArray()) need.merge(c, 1, Integer::sum);
    int have = 0, total = need.size(), l = 0;
    int[] res = {-1, 0, 0};
    for (int r = 0; r < s.length(); r++) {
        char c = s.charAt(r);
        window.merge(c, 1, Integer::sum);
        if (need.containsKey(c) && window.get(c).equals(need.get(c))) have++;
        while (have == total) {
            if (res[0] == -1 || r - l + 1 < res[0]) res = new int[]{r-l+1, l, r};
            char lc = s.charAt(l);
            window.merge(lc, -1, Integer::sum);
            if (need.containsKey(lc) && window.get(lc) < need.get(lc)) have--;
            l++;
        }
    }
    return res[0] == -1 ? "" : s.substring(res[1], res[2] + 1);
}
```

---

### Q18. Sliding Window Maximum (Hard)
**Approach:** Monotonic Deque (decreasing)  
**Time:** O(n) | **Space:** O(k)

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> dq = new ArrayDeque<>(); // stores indices
    for (int i = 0; i < n; i++) {
        if (!dq.isEmpty() && dq.peekFirst() < i - k + 1) dq.pollFirst();
        while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) dq.pollLast();
        dq.offerLast(i);
        if (i >= k - 1) result[i - k + 1] = nums[dq.peekFirst()];
    }
    return result;
}
```
**Key insight:** Deque stores indices in decreasing order of values. Front is always the max of current window.

---

## Pattern 4: Stack

### Q19. Valid Parentheses (Easy)
**Approach:** Stack - push open brackets, match on close  
**Time:** O(n) | **Space:** O(n)

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '{' || c == '[') stack.push(c);
        else {
            if (stack.isEmpty()) return false;
            char top = stack.pop();
            if (c == ')' && top != '(') return false;
            if (c == '}' && top != '{') return false;
            if (c == ']' && top != '[') return false;
        }
    }
    return stack.isEmpty();
}
```

---

### Q20. Min Stack (Medium)
**Approach:** Two stacks - one for values, one for minimums  
**Time:** O(1) all ops | **Space:** O(n)

```java
class MinStack {
    Deque<Integer> stack = new ArrayDeque<>();
    Deque<Integer> minStack = new ArrayDeque<>();

    public void push(int val) {
        stack.push(val);
        minStack.push(minStack.isEmpty() ? val : Math.min(val, minStack.peek()));
    }
    public void pop() { stack.pop(); minStack.pop(); }
    public int top() { return stack.peek(); }
    public int getMin() { return minStack.peek(); }
}
```

---

### Q21. Evaluate Reverse Polish Notation (Medium)
**Approach:** Stack - push numbers, pop on operator  
**Time:** O(n) | **Space:** O(n)

```java
public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String t : tokens) {
        if ("+-*/".contains(t)) {
            int b = stack.pop(), a = stack.pop();
            switch (t) {
                case "+": stack.push(a + b); break;
                case "-": stack.push(a - b); break;
                case "*": stack.push(a * b); break;
                case "/": stack.push(a / b); break;
            }
        } else stack.push(Integer.parseInt(t));
    }
    return stack.pop();
}
```

---

### Q22. Generate Parentheses (Medium)
**Approach:** Backtracking with open/close count  
**Time:** O(4^n / sqrt(n)) | **Space:** O(n)

```java
public List<String> generateParenthesis(int n) {
    List<String> res = new ArrayList<>();
    backtrack(res, new StringBuilder(), 0, 0, n);
    return res;
}
void backtrack(List<String> res, StringBuilder sb, int open, int close, int n) {
    if (sb.length() == 2 * n) { res.add(sb.toString()); return; }
    if (open < n) { sb.append('('); backtrack(res, sb, open+1, close, n); sb.deleteCharAt(sb.length()-1); }
    if (close < open) { sb.append(')'); backtrack(res, sb, open, close+1, n); sb.deleteCharAt(sb.length()-1); }
}
```

---

### Q23. Daily Temperatures (Medium)
**Approach:** Monotonic decreasing stack (store indices)  
**Time:** O(n) | **Space:** O(n)

```java
public int[] dailyTemperatures(int[] temperatures) {
    int n = temperatures.length;
    int[] result = new int[n];
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
            int idx = stack.pop();
            result[idx] = i - idx;
        }
        stack.push(i);
    }
    return result;
}
```

---

### Q24. Car Fleet (Medium)
**Approach:** Sort by position descending, use stack for fleets  
**Time:** O(n log n) | **Space:** O(n)

```java
public int carFleet(int target, int[] position, int[] speed) {
    int n = position.length;
    int[][] cars = new int[n][2];
    for (int i = 0; i < n; i++) cars[i] = new int[]{position[i], speed[i]};
    Arrays.sort(cars, (a, b) -> b[0] - a[0]);
    Deque<Double> stack = new ArrayDeque<>();
    for (int[] car : cars) {
        double time = (double)(target - car[0]) / car[1];
        if (stack.isEmpty() || time > stack.peek()) stack.push(time);
    }
    return stack.size();
}
```

---

### Q25. Largest Rectangle in Histogram (Hard)
**Approach:** Monotonic stack - find left/right boundaries  
**Time:** O(n) | **Space:** O(n)

```java
public int largestRectangleArea(int[] heights) {
    Deque<Integer> stack = new ArrayDeque<>();
    int maxArea = 0;
    for (int i = 0; i <= heights.length; i++) {
        int h = (i == heights.length) ? 0 : heights[i];
        while (!stack.isEmpty() && h < heights[stack.peek()]) {
            int height = heights[stack.pop()];
            int width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    return maxArea;
}
```

---

## Pattern 5: Binary Search

### Q26. Binary Search (Easy)
**Time:** O(log n) | **Space:** O(1)

```java
public int search(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2; // avoids integer overflow
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}
```

---

### Q27. Search a 2D Matrix (Medium)
**Approach:** Treat matrix as 1D sorted array  
**Time:** O(log(m*n)) | **Space:** O(1)

```java
public boolean searchMatrix(int[][] matrix, int target) {
    int m = matrix.length, n = matrix[0].length;
    int l = 0, r = m * n - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        int val = matrix[mid / n][mid % n];
        if (val == target) return true;
        else if (val < target) l = mid + 1;
        else r = mid - 1;
    }
    return false;
}
```

---

### Q28. Koko Eating Bananas (Medium)
**Approach:** Binary search on answer (speed range)  
**Time:** O(n log m) | **Space:** O(1)

```java
public int minEatingSpeed(int[] piles, int h) {
    int l = 1, r = Arrays.stream(piles).max().getAsInt();
    while (l < r) {
        int mid = l + (r - l) / 2;
        long hours = 0;
        for (int p : piles) hours += (p + mid - 1) / mid;
        if (hours <= h) r = mid;
        else l = mid + 1;
    }
    return l;
}
```

---

### Q29. Find Minimum in Rotated Sorted Array (Medium)
**Approach:** Modified binary search - compare mid with right  
**Time:** O(log n) | **Space:** O(1)

```java
public int findMin(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] > nums[r]) l = mid + 1;
        else r = mid;
    }
    return nums[l];
}
```

---

### Q30. Search in Rotated Sorted Array (Medium)
**Approach:** Binary search - identify which half is sorted  
**Time:** O(log n) | **Space:** O(1)

```java
public int search(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        if (nums[l] <= nums[mid]) { // left half sorted
            if (target >= nums[l] && target < nums[mid]) r = mid - 1;
            else l = mid + 1;
        } else { // right half sorted
            if (target > nums[mid] && target <= nums[r]) l = mid + 1;
            else r = mid - 1;
        }
    }
    return -1;
}
```

---

### Q31. Time Based Key-Value Store (Medium)
**Approach:** HashMap of sorted lists + binary search on timestamps  
**Time:** O(log n) per get | **Space:** O(n)

```java
class TimeMap {
    Map<String, List<int[]>> map = new HashMap<>();
    public void set(String key, String value, int timestamp) {
        map.computeIfAbsent(key, k -> new ArrayList<>()).add(new int[]{timestamp, value.hashCode()});
        // Store as pair for real impl
    }
    // Use TreeMap<Integer, String> per key for cleaner binary search
}
```
**Cleaner approach:** Use `TreeMap<Integer, String>` per key and call `floorKey(timestamp)`.

---

### Q32. Median of Two Sorted Arrays (Hard)
**Approach:** Binary search on smaller array partition  
**Time:** O(log(min(m,n))) | **Space:** O(1)

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) return findMedianSortedArrays(nums2, nums1);
    int m = nums1.length, n = nums2.length;
    int l = 0, r = m;
    while (l <= r) {
        int i = (l + r) / 2, j = (m + n + 1) / 2 - i;
        int maxL1 = (i == 0) ? Integer.MIN_VALUE : nums1[i-1];
        int minR1 = (i == m) ? Integer.MAX_VALUE : nums1[i];
        int maxL2 = (j == 0) ? Integer.MIN_VALUE : nums2[j-1];
        int minR2 = (j == n) ? Integer.MAX_VALUE : nums2[j];
        if (maxL1 <= minR2 && maxL2 <= minR1) {
            if ((m + n) % 2 == 0)
                return (Math.max(maxL1, maxL2) + Math.min(minR1, minR2)) / 2.0;
            return Math.max(maxL1, maxL2);
        } else if (maxL1 > minR2) r = i - 1;
        else l = i + 1;
    }
    return 0;
}
```

---

## Pattern 6: Linked List

### Q33. Reverse Linked List (Easy)
**Approach:** Iterative 3-pointer  
**Time:** O(n) | **Space:** O(1)

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

---

### Q34. Merge Two Sorted Lists (Easy)
**Approach:** Dummy node + iterative merge  
**Time:** O(m+n) | **Space:** O(1)

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0), curr = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
        else { curr.next = l2; l2 = l2.next; }
        curr = curr.next;
    }
    curr.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

---

### Q35. Linked List Cycle (Easy)
**Approach:** Floyd's Tortoise and Hare  
**Time:** O(n) | **Space:** O(1)

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

---

### Q36. Reorder List (Medium)
**Approach:** Find middle + Reverse second half + Merge  
**Time:** O(n) | **Space:** O(1)

```java
public void reorderList(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next; fast = fast.next.next;
    }
    ListNode second = reverseList(slow.next);
    slow.next = null;
    ListNode first = head;
    while (second != null) {
        ListNode tmp1 = first.next, tmp2 = second.next;
        first.next = second; second.next = tmp1;
        first = tmp1; second = tmp2;
    }
}
```

---

### Q37. Remove Nth Node From End (Medium)
**Approach:** Two pointers with n-gap  
**Time:** O(n) | **Space:** O(1)

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode fast = dummy, slow = dummy;
    for (int i = 0; i <= n; i++) fast = fast.next;
    while (fast != null) { slow = slow.next; fast = fast.next; }
    slow.next = slow.next.next;
    return dummy.next;
}
```

---

### Q38. Copy List with Random Pointer (Medium)
**Approach:** HashMap old-to-new node mapping  
**Time:** O(n) | **Space:** O(n)

```java
public Node copyRandomList(Node head) {
    Map<Node, Node> map = new HashMap<>();
    Node curr = head;
    while (curr != null) { map.put(curr, new Node(curr.val)); curr = curr.next; }
    curr = head;
    while (curr != null) {
        map.get(curr).next = map.get(curr.next);
        map.get(curr).random = map.get(curr.random);
        curr = curr.next;
    }
    return map.get(head);
}
```

---

### Q39. Add Two Numbers (Medium)
**Approach:** Digit-by-digit addition with carry  
**Time:** O(max(m,n)) | **Space:** O(max(m,n))

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0), curr = dummy;
    int carry = 0;
    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;
        if (l1 != null) { sum += l1.val; l1 = l1.next; }
        if (l2 != null) { sum += l2.val; l2 = l2.next; }
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
    }
    return dummy.next;
}
```

---

### Q40. LRU Cache (Medium)
**Approach:** HashMap + Doubly Linked List  
**Time:** O(1) all ops | **Space:** O(capacity)

```java
class LRUCache {
    private Map<Integer, Node> map;
    private int capacity;
    private Node head, tail; // dummy nodes

    class Node {
        int key, val;
        Node prev, next;
        Node(int k, int v) { key = k; val = v; }
    }

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        head = new Node(0, 0); tail = new Node(0, 0);
        head.next = tail; tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        remove(node); insert(node);
        return node.val;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) remove(map.get(key));
        Node node = new Node(key, value);
        map.put(key, node);
        insert(node);
        if (map.size() > capacity) {
            Node lru = head.next;
            remove(lru); map.remove(lru.key);
        }
    }

    private void remove(Node node) {
        node.prev.next = node.next; node.next.prev = node.prev;
    }
    private void insert(Node node) { // insert before tail (MRU position)
        node.prev = tail.prev; node.next = tail;
        tail.prev.next = node; tail.prev = node;
    }
}
```

---

### Q41. Merge K Sorted Lists (Hard)
**Approach:** Min-Heap (PriorityQueue)  
**Time:** O(n log k) | **Space:** O(k)

```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
    for (ListNode l : lists) if (l != null) pq.offer(l);
    ListNode dummy = new ListNode(0), curr = dummy;
    while (!pq.isEmpty()) {
        ListNode node = pq.poll();
        curr.next = node; curr = curr.next;
        if (node.next != null) pq.offer(node.next);
    }
    return dummy.next;
}
```

---

### Q42. Reverse Nodes in K-Group (Hard)
**Approach:** Iterative k-group reversal with pointer management  
**Time:** O(n) | **Space:** O(1)

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode groupPrev = dummy;
    while (true) {
        ListNode kth = getKth(groupPrev, k);
        if (kth == null) break;
        ListNode groupNext = kth.next;
        ListNode prev = groupNext, curr = groupPrev.next;
        while (curr != groupNext) {
            ListNode tmp = curr.next;
            curr.next = prev; prev = curr; curr = tmp;
        }
        ListNode tmp = groupPrev.next;
        groupPrev.next = kth;
        groupPrev = tmp;
    }
    return dummy.next;
}
ListNode getKth(ListNode curr, int k) {
    while (curr != null && k > 0) { curr = curr.next; k--; }
    return curr;
}
```

---

## Pattern 7: Trees

### Q43. Invert Binary Tree (Easy)
**Time:** O(n) | **Space:** O(h)

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    TreeNode tmp = root.left;
    root.left = invertTree(root.right);
    root.right = invertTree(tmp);
    return root;
}
```

---

### Q44. Maximum Depth of Binary Tree (Easy)
**Time:** O(n) | **Space:** O(h)

```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

### Q45. Diameter of Binary Tree (Easy)
**Approach:** DFS - diameter at each node = leftHeight + rightHeight  
**Time:** O(n) | **Space:** O(h)

```java
int diameter = 0;
public int diameterOfBinaryTree(TreeNode root) {
    dfs(root);
    return diameter;
}
int dfs(TreeNode node) {
    if (node == null) return 0;
    int left = dfs(node.left), right = dfs(node.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
}
```

---

### Q46. Balanced Binary Tree (Easy)
**Approach:** DFS - return -1 if subtree is unbalanced  
**Time:** O(n) | **Space:** O(h)

```java
public boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}
int checkHeight(TreeNode node) {
    if (node == null) return 0;
    int left = checkHeight(node.left);
    if (left == -1) return -1;
    int right = checkHeight(node.right);
    if (right == -1) return -1;
    if (Math.abs(left - right) > 1) return -1;
    return 1 + Math.max(left, right);
}
```

---

### Q47. Same Tree (Easy)
**Time:** O(n) | **Space:** O(h)

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;
    if (p == null || q == null) return false;
    return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

---

### Q48. Subtree of Another Tree (Easy)
**Time:** O(m*n) | **Space:** O(m+n)

```java
public boolean isSubtree(TreeNode root, TreeNode subRoot) {
    if (root == null) return false;
    if (isSameTree(root, subRoot)) return true;
    return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
}
```

---

### Q49. Lowest Common Ancestor of BST (Medium)
**Approach:** Use BST property - go left/right based on values  
**Time:** O(h) | **Space:** O(1)

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    while (root != null) {
        if (p.val < root.val && q.val < root.val) root = root.left;
        else if (p.val > root.val && q.val > root.val) root = root.right;
        else return root;
    }
    return null;
}
```

---

### Q50. Binary Tree Level Order Traversal (Medium)
**Approach:** BFS with queue, snapshot size per level  
**Time:** O(n) | **Space:** O(n)

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        res.add(level);
    }
    return res;
}
```

---

### Q51. Binary Tree Right Side View (Medium)
**Approach:** BFS - collect last node of each level  
**Time:** O(n) | **Space:** O(n)

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            TreeNode node = q.poll();
            if (i == size - 1) res.add(node.val);
            if (node.left != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
    }
    return res;
}
```

---

### Q52. Count Good Nodes in Binary Tree (Medium)
**Approach:** DFS tracking max value on path  
**Time:** O(n) | **Space:** O(h)

```java
public int goodNodes(TreeNode root) {
    return dfs(root, Integer.MIN_VALUE);
}
int dfs(TreeNode node, int maxSoFar) {
    if (node == null) return 0;
    int count = (node.val >= maxSoFar) ? 1 : 0;
    int newMax = Math.max(maxSoFar, node.val);
    return count + dfs(node.left, newMax) + dfs(node.right, newMax);
}
```

---

### Q53. Validate Binary Search Tree (Medium)
**Approach:** DFS with valid range [min, max]  
**Time:** O(n) | **Space:** O(h)

```java
public boolean isValidBST(TreeNode root) {
    return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
}
boolean validate(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return validate(node.left, min, node.val) && validate(node.right, node.val, max);
}
```

---

### Q54. Kth Smallest in BST (Medium)
**Approach:** Inorder traversal (BST inorder = sorted)  
**Time:** O(h+k) | **Space:** O(h)

```java
public int kthSmallest(TreeNode root, int k) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) { stack.push(curr); curr = curr.left; }
        curr = stack.pop();
        if (--k == 0) return curr.val;
        curr = curr.right;
    }
    return -1;
}
```

---

### Q55. Construct Binary Tree from Preorder and Inorder (Medium)
**Approach:** HashMap for inorder index, recursive split  
**Time:** O(n) | **Space:** O(n)

```java
Map<Integer, Integer> inorderIdx;
int preIdx = 0;
public TreeNode buildTree(int[] preorder, int[] inorder) {
    inorderIdx = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) inorderIdx.put(inorder[i], i);
    return build(preorder, 0, inorder.length - 1);
}
TreeNode build(int[] preorder, int left, int right) {
    if (left > right) return null;
    int rootVal = preorder[preIdx++];
    TreeNode root = new TreeNode(rootVal);
    int mid = inorderIdx.get(rootVal);
    root.left = build(preorder, left, mid - 1);
    root.right = build(preorder, mid + 1, right);
    return root;
}
```

---

### Q56. Binary Tree Maximum Path Sum (Hard)
**Approach:** DFS - track max gain per subtree, update global max  
**Time:** O(n) | **Space:** O(h)

```java
int maxSum = Integer.MIN_VALUE;
public int maxPathSum(TreeNode root) {
    gain(root);
    return maxSum;
}
int gain(TreeNode node) {
    if (node == null) return 0;
    int leftGain = Math.max(gain(node.left), 0);
    int rightGain = Math.max(gain(node.right), 0);
    maxSum = Math.max(maxSum, node.val + leftGain + rightGain);
    return node.val + Math.max(leftGain, rightGain);
}
```

---

### Q57. Serialize and Deserialize Binary Tree (Hard)
**Approach:** Level-order BFS serialization  
**Time:** O(n) | **Space:** O(n)

```java
public String serialize(TreeNode root) {
    if (root == null) return "";
    StringBuilder sb = new StringBuilder();
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    while (!q.isEmpty()) {
        TreeNode node = q.poll();
        if (node == null) { sb.append("null,"); continue; }
        sb.append(node.val).append(",");
        q.offer(node.left); q.offer(node.right);
    }
    return sb.toString();
}
public TreeNode deserialize(String data) {
    if (data.isEmpty()) return null;
    String[] vals = data.split(",");
    TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    int i = 1;
    while (!q.isEmpty() && i < vals.length) {
        TreeNode node = q.poll();
        if (!vals[i].equals("null")) { node.left = new TreeNode(Integer.parseInt(vals[i])); q.offer(node.left); }
        i++;
        if (i < vals.length && !vals[i].equals("null")) { node.right = new TreeNode(Integer.parseInt(vals[i])); q.offer(node.right); }
        i++;
    }
    return root;
}
```

---

## Pattern 8: Heap / Priority Queue

### Q58. Kth Largest Element in a Stream (Easy)
**Approach:** Min-heap of size k  
**Time:** O(n log k) | **Space:** O(k)

```java
class KthLargest {
    PriorityQueue<Integer> pq = new PriorityQueue<>();
    int k;
    public KthLargest(int k, int[] nums) {
        this.k = k;
        for (int n : nums) add(n);
    }
    public int add(int val) {
        pq.offer(val);
        if (pq.size() > k) pq.poll();
        return pq.peek();
    }
}
```

---

### Q59. Last Stone Weight (Easy)
**Approach:** Max-heap simulation  
**Time:** O(n log n) | **Space:** O(n)

```java
public int lastStoneWeight(int[] stones) {
    PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
    for (int s : stones) pq.offer(s);
    while (pq.size() > 1) {
        int a = pq.poll(), b = pq.poll();
        if (a != b) pq.offer(a - b);
    }
    return pq.isEmpty() ? 0 : pq.poll();
}
```

---

### Q60. K Closest Points to Origin (Medium)
**Approach:** Max-heap of size k  
**Time:** O(n log k) | **Space:** O(k)

```java
public int[][] kClosest(int[][] points, int k) {
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) ->
        (b[0]*b[0] + b[1]*b[1]) - (a[0]*a[0] + a[1]*a[1]));
    for (int[] p : points) {
        pq.offer(p);
        if (pq.size() > k) pq.poll();
    }
    return pq.toArray(new int[k][]);
}
```

---

### Q61. Kth Largest Element in an Array (Medium)
**Approach:** QuickSelect - average O(n)  
**Time:** O(n) avg | **Space:** O(1)

```java
public int findKthLargest(int[] nums, int k) {
    return quickSelect(nums, 0, nums.length - 1, nums.length - k);
}
int quickSelect(int[] nums, int l, int r, int k) {
    int pivot = nums[r], p = l;
    for (int i = l; i < r; i++)
        if (nums[i] <= pivot) swap(nums, i, p++);
    swap(nums, p, r);
    if (p > k) return quickSelect(nums, l, p - 1, k);
    if (p < k) return quickSelect(nums, p + 1, r, k);
    return nums[p];
}
void swap(int[] nums, int i, int j) { int t = nums[i]; nums[i] = nums[j]; nums[j] = t; }
```

---

### Q62. Task Scheduler (Medium)
**Approach:** Math formula using max frequency task  
**Time:** O(n) | **Space:** O(1)

```java
public int leastInterval(char[] tasks, int n) {
    int[] freq = new int[26];
    for (char c : tasks) freq[c - 'A']++;
    Arrays.sort(freq);
    int maxFreq = freq[25];
    int idleTime = (maxFreq - 1) * n;
    for (int i = 24; i >= 0 && freq[i] > 0; i--)
        idleTime -= Math.min(freq[i], maxFreq - 1);
    return tasks.length + Math.max(0, idleTime);
}
```

---

### Q63. Design Twitter (Medium)
**Approach:** HashMap of user tweets + PriorityQueue for feed  
**Time:** O(n log k) | **Space:** O(n)

```java
class Twitter {
    private int time = 0;
    private Map<Integer, List<int[]>> tweets = new HashMap<>();
    private Map<Integer, Set<Integer>> follows = new HashMap<>();

    public void postTweet(int userId, int tweetId) {
        tweets.computeIfAbsent(userId, k -> new ArrayList<>()).add(new int[]{time++, tweetId});
    }
    public List<Integer> getNewsFeed(int userId) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        Set<Integer> followees = follows.getOrDefault(userId, new HashSet<>());
        followees.add(userId);
        for (int f : followees) {
            List<int[]> t = tweets.getOrDefault(f, new ArrayList<>());
            for (int i = t.size() - 1; i >= Math.max(0, t.size() - 10); i--) pq.offer(t.get(i));
        }
        List<Integer> res = new ArrayList<>();
        while (!pq.isEmpty() && res.size() < 10) res.add(pq.poll()[1]);
        return res;
    }
    public void follow(int f, int ee) { follows.computeIfAbsent(f, k -> new HashSet<>()).add(ee); }
    public void unfollow(int f, int ee) { follows.getOrDefault(f, new HashSet<>()).remove(ee); }
}
```

---

### Q64. Find Median from Data Stream (Hard)
**Approach:** Two heaps - max-heap (lower half) + min-heap (upper half)  
**Time:** O(log n) add, O(1) find | **Space:** O(n)

```java
class MedianFinder {
    PriorityQueue<Integer> small = new PriorityQueue<>(Collections.reverseOrder()); // max-heap
    PriorityQueue<Integer> large = new PriorityQueue<>(); // min-heap

    public void addNum(int num) {
        small.offer(num);
        if (!large.isEmpty() && small.peek() > large.peek()) large.offer(small.poll());
        if (small.size() > large.size() + 1) large.offer(small.poll());
        if (large.size() > small.size()) small.offer(large.poll());
    }
    public double findMedian() {
        if (small.size() == large.size()) return (small.peek() + large.peek()) / 2.0;
        return small.peek();
    }
}
```

---

## Pattern 9: Backtracking

### Q65. Subsets (Medium)
**Approach:** Backtracking - include or exclude each element  
**Time:** O(n * 2^n) | **Space:** O(n)

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), res);
    return res;
}
void backtrack(int[] nums, int start, List<Integer> curr, List<List<Integer>> res) {
    res.add(new ArrayList<>(curr));
    for (int i = start; i < nums.length; i++) {
        curr.add(nums[i]);
        backtrack(nums, i + 1, curr, res);
        curr.remove(curr.size() - 1);
    }
}
```

---

### Q66. Combination Sum (Medium)
**Approach:** Backtracking - reuse elements, prune when sum exceeds  
**Time:** O(2^(target/min)) | **Space:** O(target/min)

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(candidates);
    backtrack(candidates, 0, target, new ArrayList<>(), res);
    return res;
}
void backtrack(int[] cands, int start, int remain, List<Integer> curr, List<List<Integer>> res) {
    if (remain == 0) { res.add(new ArrayList<>(curr)); return; }
    for (int i = start; i < cands.length && cands[i] <= remain; i++) {
        curr.add(cands[i]);
        backtrack(cands, i, remain - cands[i], curr, res);
        curr.remove(curr.size() - 1);
    }
}
```

---

### Q67. Permutations (Medium)
**Approach:** Backtracking with used[] array  
**Time:** O(n * n!) | **Space:** O(n)

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    boolean[] used = new boolean[nums.length];
    backtrack(nums, used, new ArrayList<>(), res);
    return res;
}
void backtrack(int[] nums, boolean[] used, List<Integer> curr, List<List<Integer>> res) {
    if (curr.size() == nums.length) { res.add(new ArrayList<>(curr)); return; }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        used[i] = true; curr.add(nums[i]);
        backtrack(nums, used, curr, res);
        used[i] = false; curr.remove(curr.size() - 1);
    }
}
```

---

### Q68. Subsets II - with Duplicates (Medium)
**Approach:** Sort + skip duplicates at same level  
**Time:** O(n * 2^n) | **Space:** O(n)

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), res);
    return res;
}
void backtrack(int[] nums, int start, List<Integer> curr, List<List<Integer>> res) {
    res.add(new ArrayList<>(curr));
    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i-1]) continue; // skip dups at same level
        curr.add(nums[i]);
        backtrack(nums, i + 1, curr, res);
        curr.remove(curr.size() - 1);
    }
}
```

---

### Q69. Combination Sum II (Medium)
**Approach:** Sort + skip duplicates + single-use elements  
**Time:** O(2^n) | **Space:** O(n)

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    Arrays.sort(candidates);
    List<List<Integer>> res = new ArrayList<>();
    backtrack(candidates, 0, target, new ArrayList<>(), res);
    return res;
}
void backtrack(int[] cands, int start, int remain, List<Integer> curr, List<List<Integer>> res) {
    if (remain == 0) { res.add(new ArrayList<>(curr)); return; }
    for (int i = start; i < cands.length && cands[i] <= remain; i++) {
        if (i > start && cands[i] == cands[i-1]) continue;
        curr.add(cands[i]);
        backtrack(cands, i + 1, remain - cands[i], curr, res);
        curr.remove(curr.size() - 1);
    }
}
```

---

### Q70. Word Search (Medium)
**Approach:** DFS backtracking on grid, mark visited  
**Time:** O(m*n*4^L) | **Space:** O(L)

```java
public boolean exist(char[][] board, String word) {
    for (int i = 0; i < board.length; i++)
        for (int j = 0; j < board[0].length; j++)
            if (dfs(board, word, i, j, 0)) return true;
    return false;
}
boolean dfs(char[][] board, String word, int i, int j, int idx) {
    if (idx == word.length()) return true;
    if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] != word.charAt(idx)) return false;
    char tmp = board[i][j];
    board[i][j] = '#';
    boolean found = dfs(board, word, i+1, j, idx+1) || dfs(board, word, i-1, j, idx+1)
                 || dfs(board, word, i, j+1, idx+1) || dfs(board, word, i, j-1, idx+1);
    board[i][j] = tmp;
    return found;
}
```

---

### Q71. Palindrome Partitioning (Medium)
**Approach:** Backtracking + isPalindrome check  
**Time:** O(n * 2^n) | **Space:** O(n)

```java
public List<List<String>> partition(String s) {
    List<List<String>> res = new ArrayList<>();
    backtrack(s, 0, new ArrayList<>(), res);
    return res;
}
void backtrack(String s, int start, List<String> curr, List<List<String>> res) {
    if (start == s.length()) { res.add(new ArrayList<>(curr)); return; }
    for (int end = start + 1; end <= s.length(); end++) {
        String sub = s.substring(start, end);
        if (isPalin(sub)) {
            curr.add(sub);
            backtrack(s, end, curr, res);
            curr.remove(curr.size() - 1);
        }
    }
}
boolean isPalin(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
    return true;
}
```

---

### Q72. Letter Combinations of Phone Number (Medium)
**Approach:** Backtracking with phone digit map  
**Time:** O(4^n * n) | **Space:** O(n)

```java
public List<String> letterCombinations(String digits) {
    if (digits.isEmpty()) return new ArrayList<>();
    String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    List<String> res = new ArrayList<>();
    backtrack(digits, 0, new StringBuilder(), map, res);
    return res;
}
void backtrack(String digits, int idx, StringBuilder curr, String[] map, List<String> res) {
    if (idx == digits.length()) { res.add(curr.toString()); return; }
    for (char c : map[digits.charAt(idx) - '0'].toCharArray()) {
        curr.append(c);
        backtrack(digits, idx + 1, curr, map, res);
        curr.deleteCharAt(curr.length() - 1);
    }
}
```

---

### Q73. N-Queens (Hard)
**Approach:** Backtracking with column/diagonal tracking sets  
**Time:** O(n!) | **Space:** O(n)

```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> res = new ArrayList<>();
    Set<Integer> cols = new HashSet<>(), diag1 = new HashSet<>(), diag2 = new HashSet<>();
    char[][] board = new char[n][n];
    for (char[] row : board) Arrays.fill(row, '.');
    backtrack(0, n, board, cols, diag1, diag2, res);
    return res;
}
void backtrack(int row, int n, char[][] board, Set<Integer> cols, Set<Integer> d1, Set<Integer> d2, List<List<String>> res) {
    if (row == n) {
        List<String> solution = new ArrayList<>();
        for (char[] r : board) solution.add(new String(r));
        res.add(solution); return;
    }
    for (int col = 0; col < n; col++) {
        if (cols.contains(col) || d1.contains(row - col) || d2.contains(row + col)) continue;
        cols.add(col); d1.add(row - col); d2.add(row + col);
        board[row][col] = 'Q';
        backtrack(row + 1, n, board, cols, d1, d2, res);
        board[row][col] = '.';
        cols.remove(col); d1.remove(row - col); d2.remove(row + col);
    }
}
```

---

## Pattern 10: Tries

### Q74. Implement Trie (Medium)
**Time:** O(n) per op | **Space:** O(n * 26)

```java
class Trie {
    Trie[] children = new Trie[26];
    boolean isEnd = false;

    public void insert(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            if (node.children[c - 'a'] == null) node.children[c - 'a'] = new Trie();
            node = node.children[c - 'a'];
        }
        node.isEnd = true;
    }
    public boolean search(String word) {
        Trie node = find(word);
        return node != null && node.isEnd;
    }
    public boolean startsWith(String prefix) { return find(prefix) != null; }
    private Trie find(String s) {
        Trie node = this;
        for (char c : s.toCharArray()) {
            if (node.children[c - 'a'] == null) return null;
            node = node.children[c - 'a'];
        }
        return node;
    }
}
```

---

### Q75. Design Add and Search Words Data Structure (Medium)
**Approach:** Trie + DFS for '.' wildcard matching  
**Time:** O(m) insert, O(26^m) worst search | **Space:** O(n*m)

```java
class WordDictionary {
    WordDictionary[] children = new WordDictionary[26];
    boolean isEnd = false;

    public void addWord(String word) {
        WordDictionary node = this;
        for (char c : word.toCharArray()) {
            if (node.children[c - 'a'] == null) node.children[c - 'a'] = new WordDictionary();
            node = node.children[c - 'a'];
        }
        node.isEnd = true;
    }
    public boolean search(String word) { return dfs(word, 0, this); }
    private boolean dfs(String word, int idx, WordDictionary node) {
        if (idx == word.length()) return node.isEnd;
        char c = word.charAt(idx);
        if (c == '.') {
            for (WordDictionary child : node.children)
                if (child != null && dfs(word, idx + 1, child)) return true;
            return false;
        }
        if (node.children[c - 'a'] == null) return false;
        return dfs(word, idx + 1, node.children[c - 'a']);
    }
}
```

---

### Q76. Word Search II (Hard)
**Approach:** Trie + DFS on board - backtrack and prune dead branches  
**Time:** O(m*n*4^L) | **Space:** O(total word chars)

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    String word = null;
}
public List<String> findWords(char[][] board, String[] words) {
    TrieNode root = new TrieNode();
    for (String w : words) {
        TrieNode node = root;
        for (char c : w.toCharArray()) {
            if (node.children[c-'a'] == null) node.children[c-'a'] = new TrieNode();
            node = node.children[c-'a'];
        }
        node.word = w;
    }
    List<String> res = new ArrayList<>();
    for (int i = 0; i < board.length; i++)
        for (int j = 0; j < board[0].length; j++)
            dfs(board, i, j, root, res);
    return res;
}
void dfs(char[][] board, int i, int j, TrieNode node, List<String> res) {
    if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] == '#') return;
    char c = board[i][j];
    TrieNode next = node.children[c - 'a'];
    if (next == null) return;
    if (next.word != null) { res.add(next.word); next.word = null; } // prune
    board[i][j] = '#';
    dfs(board, i+1, j, next, res); dfs(board, i-1, j, next, res);
    dfs(board, i, j+1, next, res); dfs(board, i, j-1, next, res);
    board[i][j] = c;
}
```

---

## Pattern 11: Graphs

### Q77. Number of Islands (Medium)
**Approach:** DFS flood fill - sink visited land  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public int numIslands(char[][] grid) {
    int count = 0;
    for (int i = 0; i < grid.length; i++)
        for (int j = 0; j < grid[0].length; j++)
            if (grid[i][j] == '1') { dfs(grid, i, j); count++; }
    return count;
}
void dfs(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] != '1') return;
    grid[i][j] = '0';
    dfs(grid, i+1, j); dfs(grid, i-1, j); dfs(grid, i, j+1); dfs(grid, i, j-1);
}
```

---

### Q78. Clone Graph (Medium)
**Approach:** DFS/BFS with HashMap old-to-new node  
**Time:** O(n+e) | **Space:** O(n)

```java
Map<Node, Node> map = new HashMap<>();
public Node cloneGraph(Node node) {
    if (node == null) return null;
    if (map.containsKey(node)) return map.get(node);
    Node clone = new Node(node.val);
    map.put(node, clone);
    for (Node neighbor : node.neighbors) clone.neighbors.add(cloneGraph(neighbor));
    return clone;
}
```

---

### Q79. Max Area of Island (Medium)
**Approach:** DFS returning area count  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public int maxAreaOfIsland(int[][] grid) {
    int max = 0;
    for (int i = 0; i < grid.length; i++)
        for (int j = 0; j < grid[0].length; j++)
            max = Math.max(max, dfs(grid, i, j));
    return max;
}
int dfs(int[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] == 0) return 0;
    grid[i][j] = 0;
    return 1 + dfs(grid, i+1, j) + dfs(grid, i-1, j) + dfs(grid, i, j+1) + dfs(grid, i, j-1);
}
```

---

### Q80. Pacific Atlantic Water Flow (Medium)
**Approach:** Reverse BFS from each ocean, find intersection  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    int m = heights.length, n = heights[0].length;
    boolean[][] pac = new boolean[m][n], atl = new boolean[m][n];
    for (int i = 0; i < m; i++) { dfs(heights, pac, i, 0); dfs(heights, atl, i, n-1); }
    for (int j = 0; j < n; j++) { dfs(heights, pac, 0, j); dfs(heights, atl, m-1, j); }
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < m; i++) for (int j = 0; j < n; j++)
        if (pac[i][j] && atl[i][j]) res.add(Arrays.asList(i, j));
    return res;
}
void dfs(int[][] h, boolean[][] vis, int i, int j) {
    vis[i][j] = true;
    int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
    for (int[] d : dirs) {
        int ni = i+d[0], nj = j+d[1];
        if (ni<0||nj<0||ni>=h.length||nj>=h[0].length||vis[ni][nj]||h[ni][nj]<h[i][j]) continue;
        dfs(h, vis, ni, nj);
    }
}
```

---

### Q81. Surrounded Regions (Medium)
**Approach:** DFS from border O's, mark safe, flip rest  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public void solve(char[][] board) {
    int m = board.length, n = board[0].length;
    for (int i = 0; i < m; i++) { dfs(board, i, 0); dfs(board, i, n-1); }
    for (int j = 0; j < n; j++) { dfs(board, 0, j); dfs(board, m-1, j); }
    for (int i = 0; i < m; i++) for (int j = 0; j < n; j++)
        board[i][j] = (board[i][j] == 'S') ? 'O' : 'X';
}
void dfs(char[][] board, int i, int j) {
    if (i<0||j<0||i>=board.length||j>=board[0].length||board[i][j]!='O') return;
    board[i][j] = 'S';
    dfs(board,i+1,j); dfs(board,i-1,j); dfs(board,i,j+1); dfs(board,i,j-1);
}
```

---

### Q82. Rotting Oranges (Medium)
**Approach:** Multi-source BFS from all rotten oranges  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public int orangesRotting(int[][] grid) {
    Queue<int[]> q = new LinkedList<>();
    int fresh = 0, m = grid.length, n = grid[0].length;
    for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
        if (grid[i][j] == 2) q.offer(new int[]{i, j});
        else if (grid[i][j] == 1) fresh++;
    }
    int mins = 0;
    int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
    while (!q.isEmpty() && fresh > 0) {
        mins++;
        for (int size = q.size(); size > 0; size--) {
            int[] cell = q.poll();
            for (int[] d : dirs) {
                int ni = cell[0]+d[0], nj = cell[1]+d[1];
                if (ni<0||nj<0||ni>=m||nj>=n||grid[ni][nj]!=1) continue;
                grid[ni][nj] = 2; fresh--; q.offer(new int[]{ni, nj});
            }
        }
    }
    return fresh == 0 ? mins : -1;
}
```

---

### Q83. Course Schedule (Medium)
**Approach:** Topological sort - detect cycle via DFS  
**Time:** O(V+E) | **Space:** O(V+E)

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    List<List<Integer>> adj = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
    for (int[] p : prerequisites) adj.get(p[1]).add(p[0]);
    int[] state = new int[numCourses]; // 0=unvisited, 1=visiting, 2=done
    for (int i = 0; i < numCourses; i++)
        if (state[i] == 0 && hasCycle(adj, state, i)) return false;
    return true;
}
boolean hasCycle(List<List<Integer>> adj, int[] state, int node) {
    state[node] = 1;
    for (int next : adj.get(node)) {
        if (state[next] == 1) return true;
        if (state[next] == 0 && hasCycle(adj, state, next)) return true;
    }
    state[node] = 2;
    return false;
}
```

---

### Q84. Course Schedule II (Medium)
**Approach:** Topological sort via Kahn's Algorithm (BFS)  
**Time:** O(V+E) | **Space:** O(V+E)

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    int[] inDegree = new int[numCourses];
    List<List<Integer>> adj = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
    for (int[] p : prerequisites) { adj.get(p[1]).add(p[0]); inDegree[p[0]]++; }
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) if (inDegree[i] == 0) q.offer(i);
    int[] order = new int[numCourses];
    int idx = 0;
    while (!q.isEmpty()) {
        int node = q.poll();
        order[idx++] = node;
        for (int next : adj.get(node)) if (--inDegree[next] == 0) q.offer(next);
    }
    return idx == numCourses ? order : new int[]{};
}
```

---

### Q85. Word Ladder (Hard)
**Approach:** BFS with word transformation levels  
**Time:** O(m^2 * n) | **Space:** O(m^2 * n)

```java
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;
    Queue<String> q = new LinkedList<>();
    q.offer(beginWord);
    int steps = 1;
    while (!q.isEmpty()) {
        steps++;
        for (int size = q.size(); size > 0; size--) {
            char[] word = q.poll().toCharArray();
            for (int i = 0; i < word.length; i++) {
                char orig = word[i];
                for (char c = 'a'; c <= 'z'; c++) {
                    word[i] = c;
                    String next = new String(word);
                    if (next.equals(endWord)) return steps;
                    if (wordSet.remove(next)) q.offer(next);
                }
                word[i] = orig;
            }
        }
    }
    return 0;
}
```

---

## Pattern 12: Dynamic Programming - 1D

### Q86. Climbing Stairs (Easy)
**Approach:** Fibonacci sequence (2 variables)  
**Time:** O(n) | **Space:** O(1)

```java
public int climbStairs(int n) {
    int a = 1, b = 1;
    for (int i = 2; i <= n; i++) { int c = a + b; a = b; b = c; }
    return b;
}
```

---

### Q87. Min Cost Climbing Stairs (Easy)
**Time:** O(n) | **Space:** O(1)

```java
public int minCostClimbingStairs(int[] cost) {
    int a = cost[0], b = cost[1];
    for (int i = 2; i < cost.length; i++) { int c = cost[i] + Math.min(a, b); a = b; b = c; }
    return Math.min(a, b);
}
```

---

### Q88. House Robber (Medium)
**Approach:** DP - rob[i] = max(rob[i-2]+nums[i], rob[i-1])  
**Time:** O(n) | **Space:** O(1)

```java
public int rob(int[] nums) {
    int prev2 = 0, prev1 = 0;
    for (int n : nums) { int curr = Math.max(prev1, prev2 + n); prev2 = prev1; prev1 = curr; }
    return prev1;
}
```

---

### Q89. House Robber II (Medium)
**Approach:** Run House Robber twice (exclude first, exclude last)  
**Time:** O(n) | **Space:** O(1)

```java
public int rob(int[] nums) {
    if (nums.length == 1) return nums[0];
    return Math.max(rob(nums, 0, nums.length - 2), rob(nums, 1, nums.length - 1));
}
int rob(int[] nums, int l, int r) {
    int prev2 = 0, prev1 = 0;
    for (int i = l; i <= r; i++) { int curr = Math.max(prev1, prev2 + nums[i]); prev2 = prev1; prev1 = curr; }
    return prev1;
}
```

---

### Q90. Longest Palindromic Substring (Medium)
**Approach:** Expand around center  
**Time:** O(n^2) | **Space:** O(1)

```java
public String longestPalindrome(String s) {
    int start = 0, maxLen = 1;
    for (int i = 0; i < s.length(); i++) {
        for (int[] range : new int[][]{{i, i}, {i, i+1}}) {
            int l = range[0], r = range[1];
            while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) { l--; r++; }
            if (r - l - 1 > maxLen) { maxLen = r - l - 1; start = l + 1; }
        }
    }
    return s.substring(start, start + maxLen);
}
```

---

### Q91. Palindromic Substrings (Medium)
**Approach:** Expand around center, count all  
**Time:** O(n^2) | **Space:** O(1)

```java
public int countSubstrings(String s) {
    int count = 0;
    for (int i = 0; i < s.length(); i++) {
        count += expand(s, i, i);
        count += expand(s, i, i+1);
    }
    return count;
}
int expand(String s, int l, int r) {
    int count = 0;
    while (l >= 0 && r < s.length() && s.charAt(l--) == s.charAt(r++)) count++;
    return count;
}
```

---

### Q92. Decode Ways (Medium)
**Approach:** DP - one digit + two digit decode  
**Time:** O(n) | **Space:** O(1)

```java
public int numDecodings(String s) {
    if (s.charAt(0) == '0') return 0;
    int prev2 = 1, prev1 = 1;
    for (int i = 1; i < s.length(); i++) {
        int curr = 0;
        if (s.charAt(i) != '0') curr = prev1;
        int twoDigit = Integer.parseInt(s.substring(i-1, i+1));
        if (twoDigit >= 10 && twoDigit <= 26) curr += prev2;
        prev2 = prev1; prev1 = curr;
    }
    return prev1;
}
```

---

### Q93. Coin Change (Medium)
**Approach:** Bottom-up DP - min coins for each amount  
**Time:** O(amount * n) | **Space:** O(amount)

```java
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++)
        for (int coin : coins)
            if (coin <= i) dp[i] = Math.min(dp[i], dp[i - coin] + 1);
    return dp[amount] > amount ? -1 : dp[amount];
}
```

---

### Q94. Maximum Product Subarray (Medium)
**Approach:** Track both max and min (negatives flip sign)  
**Time:** O(n) | **Space:** O(1)

```java
public int maxProduct(int[] nums) {
    int maxP = nums[0], minP = nums[0], res = nums[0];
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] < 0) { int tmp = maxP; maxP = minP; minP = tmp; }
        maxP = Math.max(nums[i], maxP * nums[i]);
        minP = Math.min(nums[i], minP * nums[i]);
        res = Math.max(res, maxP);
    }
    return res;
}
```

---

### Q95. Word Break (Medium)
**Approach:** DP - dp[i] = true if s[0..i] can be segmented  
**Time:** O(n^2 * m) | **Space:** O(n)

```java
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    for (int i = 1; i <= s.length(); i++)
        for (int j = 0; j < i; j++)
            if (dp[j] && wordSet.contains(s.substring(j, i))) { dp[i] = true; break; }
    return dp[s.length()];
}
```

---

### Q96. Longest Increasing Subsequence (Medium)
**Approach:** Binary search patience sorting  
**Time:** O(n log n) | **Space:** O(n)

```java
public int lengthOfLIS(int[] nums) {
    List<Integer> tails = new ArrayList<>();
    for (int n : nums) {
        int l = 0, r = tails.size();
        while (l < r) {
            int mid = (l + r) / 2;
            if (tails.get(mid) < n) l = mid + 1;
            else r = mid;
        }
        if (l == tails.size()) tails.add(n);
        else tails.set(l, n);
    }
    return tails.size();
}
```

---

## Pattern 13: Dynamic Programming - 2D

### Q97. Unique Paths (Medium)
**Approach:** DP grid - paths[i][j] = paths[i-1][j] + paths[i][j-1]  
**Time:** O(m*n) | **Space:** O(n) with 1D optimization

```java
public int uniquePaths(int m, int n) {
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[j] += dp[j-1];
    return dp[n-1];
}
```

---

### Q98. Longest Common Subsequence (Medium)
**Approach:** Classic 2D DP  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m+1][n+1];
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = (text1.charAt(i-1) == text2.charAt(j-1))
                ? dp[i-1][j-1] + 1
                : Math.max(dp[i-1][j], dp[i][j-1]);
    return dp[m][n];
}
```

---

### Q99. Edit Distance (Medium)
**Approach:** DP - min ops (insert, delete, replace)  
**Time:** O(m*n) | **Space:** O(m*n)

```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m+1][n+1];
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = (word1.charAt(i-1) == word2.charAt(j-1))
                ? dp[i-1][j-1]
                : 1 + Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1]));
    return dp[m][n];
}
```

---

### Q100. Burst Balloons (Hard)
**Approach:** Interval DP - think of last balloon to burst in range  
**Time:** O(n^3) | **Space:** O(n^2)

```java
public int maxCoins(int[] nums) {
    int n = nums.length;
    int[] balloons = new int[n + 2];
    balloons[0] = balloons[n+1] = 1;
    for (int i = 0; i < n; i++) balloons[i+1] = nums[i];
    int[][] dp = new int[n+2][n+2];
    for (int len = 1; len <= n; len++) {
        for (int l = 1; l <= n - len + 1; l++) {
            int r = l + len - 1;
            for (int k = l; k <= r; k++) {
                dp[l][r] = Math.max(dp[l][r],
                    balloons[l-1] * balloons[k] * balloons[r+1] + dp[l][k-1] + dp[k+1][r]);
            }
        }
    }
    return dp[1][n];
}
```

---

## Top DSA Interview Questions & Answers

### General Concepts

**Q: What is the difference between BFS and DFS? When to use which?**
- BFS uses a queue, explores level by level. Best for: shortest path in unweighted graph, level-order traversal, nearest neighbor problems
- DFS uses a stack/recursion, explores depth first. Best for: cycle detection, topological sort, backtracking, path existence
- Rule of thumb: shortest path = BFS, exhaustive search = DFS

---

**Q: When do you use a monotonic stack?**
- When you need to find the next greater/smaller element efficiently
- Pattern: maintain stack in sorted order; pop when current element violates monotonicity
- Problems: Daily Temperatures, Largest Rectangle in Histogram, Next Greater Element
- Key: store indices not values; O(n) instead of O(n^2)

---

**Q: How does QuickSelect work and what is its time complexity?**
- Partition array around a pivot (like quicksort)
- Only recurse into the side containing the kth element
- Average O(n), worst case O(n^2). Use random pivot to avoid worst case.
- Used for: Kth Largest, Kth Smallest

---

**Q: What is the sliding window technique? Fixed vs variable?**
- Fixed window: maintain window of exact size k, slide one element at a time
- Variable window: expand right pointer, shrink left when condition violated
- When to use: contiguous subarray/substring problems with optimization goal
- Examples: Maximum Sum Subarray of Size K (fixed), Longest Substring Without Repeating Characters (variable)

---

**Q: Explain the two-pointer technique and its variants.**
- Opposite ends: sorted array, palindrome check (Two Sum II, Valid Palindrome)
- Same direction: fast/slow for cycle detection, remove duplicates (Floyd's, sliding window)
- Prerequisite: usually array must be sorted or have some ordering property

---

**Q: When do you use a HashMap vs TreeMap in Java?**
- HashMap: O(1) average get/put, no order, use for frequency counts, memoization
- TreeMap: O(log n) get/put, sorted by key, use when you need floor/ceiling/range queries
- Example: Time Based Key-Value Store uses TreeMap for floorKey(timestamp)

---

**Q: What is memoization vs tabulation in DP?**
- Memoization (top-down): recursive + cache results, only computes needed states
- Tabulation (bottom-up): iterative, fills entire DP table, often more space-efficient
- Bottom-up preferred in interviews: no recursion overhead, easier space optimization

---

**Q: How to approach a DP problem?**
1. Identify if overlapping subproblems exist
2. Define state clearly - what does dp[i] or dp[i][j] represent
3. Write recurrence relation
4. Identify base cases
5. Optimize space if possible (1D rolling array)

---

**Q: How do you detect a cycle in a directed vs undirected graph?**
- Directed: DFS with 3-color marking (white/gray/black = unvisited/visiting/done). Gray-to-gray edge = cycle
- Undirected: DFS with parent tracking - if neighbor visited and not parent, cycle exists. Or Union-Find

---

**Q: What is topological sort and when is it used?**
- Linear ordering of vertices in a DAG such that for every edge u-v, u comes before v
- Used for: course scheduling, build dependency ordering, task sequencing
- Two approaches: DFS (reverse postorder) or Kahn's BFS (process nodes with in-degree 0)

---

**Q: Explain binary search on answer (parametric search).**
- Use when: you can define a condition function f(x) that is monotonic (true/false boundary)
- Binary search on the answer space, not the array
- Examples: Koko Eating Bananas (minimize eating speed), Capacity To Ship Packages
- Template: search in [lo, hi] range, check if mid satisfies condition, move boundary

---

**Q: What is the difference between a complete binary tree and a perfect binary tree?**
- Complete: all levels full except possibly the last, filled left to right (used in heaps)
- Perfect: all internal nodes have 2 children, all leaves at same level
- Full: every node has 0 or 2 children

---

**Q: How does a PriorityQueue work in Java? Min vs max heap?**
- Default PriorityQueue is a min-heap (smallest element at top)
- Max-heap: `new PriorityQueue<>(Collections.reverseOrder())`
- Custom comparator: `new PriorityQueue<>((a, b) -> b - a)`
- Used for: Top K problems, Dijkstra's, Merge K Sorted Lists

---

**Q: What is the difference between backtracking and dynamic programming?**
- Backtracking: explore all possibilities with pruning, no overlapping subproblems (or we don't cache them)
- DP: optimal substructure + overlapping subproblems, cache results to avoid recomputation
- Sometimes both apply: Palindrome Partitioning can use backtracking + DP memoization

---

**Q: How do you handle duplicates in backtracking problems?**
1. Sort the input array first
2. At each recursion level, skip `nums[i] == nums[i-1]` for `i > start`
3. This ensures we don't generate duplicate subsets/combinations
- Pattern used in: Subsets II, Combination Sum II, Permutations II

---

**Q: What is the significance of `mid = l + (r - l) / 2` vs `(l + r) / 2`?**
- `(l + r) / 2` can overflow if l and r are large integers (both near Integer.MAX_VALUE)
- `l + (r - l) / 2` is mathematically equivalent but overflow-safe
- Always use the safe version in interviews

---

**Q: What is Union-Find (Disjoint Set) and when is it used?**
- Data structure for tracking connected components
- Operations: find(x) - get root with path compression, union(x,y) - merge components
- Used for: Number of Connected Components, Redundant Connection, Accounts Merge
- Time: near O(1) amortized with path compression + union by rank

---

**Q: How do you decide the time complexity of a recursive algorithm?**
- Use recurrence relations: T(n) = 2T(n/2) + O(n) = O(n log n) (merge sort)
- Count work per level * number of levels
- For backtracking: O(branches^depth * work per node)

---

**Q: What does "greedy works here" actually mean?**
- A locally optimal choice leads to globally optimal solution
- Proof: exchange argument - show that swapping any two adjacent elements to match greedy order doesn't improve result
- Examples: Activity Selection, Jump Game (furthest reach), Task Scheduler

---

**Q: What is amortized time complexity?**
- Average time per operation over a sequence of operations, even if individual ops vary
- Example: ArrayList.add() is O(1) amortized - occasional O(n) resize spread over many O(1) adds
- Example: Stack/Queue using two stacks - each element pushed/popped at most twice = O(1) amortized

---

**Q: When to use Trie vs HashMap for string problems?**
- HashMap: frequency count, anagram grouping, exact match lookup - O(n) per operation
- Trie: prefix search, autocomplete, word search with wildcards - O(m) per op, m = word length
- Trie advantage: shared prefixes save space, supports startsWith() natively

---

**Q: Explain Kadane's algorithm.**
- Finds maximum sum contiguous subarray in O(n)
- Key insight: at each position, either extend previous subarray or start fresh
- `currentMax = max(num, currentMax + num)`
- If all negatives: returns the least negative number (the maximum)

---

**Q: What is the difference between DFS preorder, inorder, and postorder?**
- Preorder (Root, Left, Right): copy tree, serialize, prefix expression
- Inorder (Left, Root, Right): BST gives sorted output - use for kth smallest, validate BST
- Postorder (Left, Right, Root): delete tree, evaluate expression tree, calculate heights

---

**Q: How to approach graph problems systematically?**
1. Identify: directed/undirected, weighted/unweighted, cyclic/acyclic
2. Choose representation: adjacency list (sparse) vs matrix (dense)
3. Choose traversal: BFS (shortest path, levels) vs DFS (cycles, components, topo sort)
4. Consider: Union-Find for connectivity, Dijkstra for weighted shortest path, Bellman-Ford for negative weights

---

*Last updated: 2026 | Covers NeetCode 150 core patterns*
*Target: Google, Amazon, Meta, Flipkart, Atlassian, JP Morgan, Uber*
