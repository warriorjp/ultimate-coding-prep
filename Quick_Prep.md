**1.Problem Statement**

Given two strings original and input, determine whether input is a circular rotation of original.

A circular rotation means you can rotate the original string any number of times and obtain the input.

Examples
    original = "ABCD"
    input    = "CDAB"
    
    Output: true

```java
public class CircularRotation {

    public static boolean isCircularRotation(String original, String input) {

        if (original == null || input == null)
            return false;

        if (original.length() != input.length())
            return false;

        String doubled = original + original;

        return doubled.contains(input);
    }

    public static void main(String[] args) {

        System.out.println(isCircularRotation("ABCD", "CDAB")); // true
        System.out.println(isCircularRotation("ABCD", "DABC")); // true
        System.out.println(isCircularRotation("ABCD", "ACBD")); // false
        System.out.println(isCircularRotation("AAAA", "AAAA")); // true
    }
}
```

---


***2.Find a Pair of sums for target value  :**

For Sorted Array: Use Two-Pointer Technique


Time Complexity: O(n)

Space Complexity: O(1)

    int[] nums = {4, 2, 7, 5};
    
    int target = 9;

```java
public static void findPairsSorted(int[] arr, int targetSum) {
    int left = 0, right = arr.length - 1;

    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == targetSum) {
            System.out.println("(" + arr[left] + ", " + arr[right] + ")");
            left++;
            right--;
        } else if (sum < targetSum) {
            left++;
        } else {
            right--;
        }
    }
}
```
---

**3.✅ For Unsorted Array: Using hashSet**

    int[] nums = {4, 2, 7, 5};
    
    int target = 9;

```
    static boolean findPair(int[] arr, int target) {
        HashSet<Integer> set = new HashSet<>();

        for (int num: arr) {
            int complement = target - num;
            if (set.contains(complement)) {
                System.out.println("Pair is: (" + num + ", " + complement + ")");
                return true;
            }
            set.add(num);
        }

        return false;
    }

```

---

**4.Valid Anagram (Easy)**

Approach: Frequency count array (26 chars)

Time: O(n) | Space: O(1)

```
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
---

**5.Group Anagrams (Medium)**
Approach: Sort each word as key in HashMap

Time: O(n * k log k) | Space: O(n * k)

    Input:
    strs = ["eat","tea","tan","ate","nat","bat"]
    
    Output:
    
    [
      ["eat","tea","ate"],
      
      ["tan","nat"],
      
      ["bat"]
      
    ]

```
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

---

**6.Kth Largest Element in an Array**

    nums = [3,2,1,5,6,4]
    k = 2
    Output:
    5

```
import java.util.PriorityQueue;

class Solution {

    public int findKthLargest(int[] nums, int k) {

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();   //by default it store max value at bottom

        # To store max value at top 
       // PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);  
       // PriorityQueue<Integer> maxHeap =  new PriorityQueue<>(Collections.reverseOrder());
 

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

**7.Valid Palindrome (Easy)**

Approach: Two pointers from both ends, skip non-alphanumeric

Time: O(n) | Space: O(1)

    Input: s = "A man, a plan, a canal: Panama" 
    
    Output: true.
    
    Explanation: "amanaplanacanalpanama" is a palindrome.

```
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

**8.Valid Parentheses (Easy)**

Approach: Stack - push open brackets, match on close

Time: O(n) | Space: O(n)

**Example**

    Input: "()[]{}"
    
    Output: true
    
    Input: "([)]"
    
    Output: false

```
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
**9.Binary Search**

Time: O(log n) | Space: O(1)
    
    int[] num = {2,3,4,5,6,7,8,9};
    target = 5;
```
public int search(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - l) / 2; // avoids integer overflow
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```
