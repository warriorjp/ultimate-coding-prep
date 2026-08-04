| Pattern                    | Complexity |
| -------------------------- | ---------- |
| No loop                    | O(1)       |
| One loop                   | O(n)       |
| Nested loops               | O(n²)      |
| `i *= 2`                   | O(log n)   |
| `i /= 2`                   | O(log n)   |
| Fixed inner loop (`j < 5`) | O(n)       |

  
  ##  1. Problem Statement

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


  ##  2.Find a Pair of sums for target value 

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

  ##  3.For Unsorted Array: Using hashSet

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

  ## 4.Valid Anagram (Easy)

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

  ## 5.Group Anagrams (Medium)
  
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

  ##  6.Kth Largest Element in an Array

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

  ##  7.Valid Palindrome (Easy)

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

  ##  8.Valid Parentheses (Easy)

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
        int mid = left + (right - left) / 2; // avoids integer overflow
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

  ##  10.Given an unsorted array of length n, find the smallest missing positive integer

The best possible approach is:

Time Complexity: O(n)

Space Complexity: O(1)

Example

    Input:
    [3, 4, -1, 1]
    
    After rearranging:
    [1, -1, 3, 4]

    Ans : 2

```
public int firstMissingPositive(int[] nums) {
    int n = nums.length;

    for (int i = 0; i < n; i++) {
        while (nums[i] > 0 &&
               nums[i] <= n &&
               nums[nums[i] - 1] != nums[i]) {

            int temp = nums[i];
            nums[i] = nums[temp - 1];
            nums[temp - 1] = temp;
        }
    }

    for (int i = 0; i < n; i++) {
        if (nums[i] != i + 1) {
            return i + 1;
        }
    }

    return n + 1;
}
```

  ##  11 . Given a binary matrix (containing only 0s and 1s), find the row that contains the maximum number of 1s.

Example :

        int[][] arr = {
            {0, 1, 1, 1},
            {0, 0, 1, 1},
            {1, 1, 1, 1},
            {0, 0, 0, 1}
        };

Result :

        Row = 2
        Count = 4

Code :

    public class Main {

    public static void main(String[] args) {

        int[][] arr = {
                {0,1,1,1},
                {0,0,1,1},
                {1,1,1,1},
                {0,0,0,1}
        };

        int maxCount = 0;
        int rowIndex = -1;

        for(int i = 0; i < arr.length; i++) {

            int count = 0;

            for(int j = 0; j < arr[i].length; j++) {
                if(arr[i][j] == 1) {
                    count++;
                }
            }

            if(count > maxCount) {
                maxCount = count;
                rowIndex = i;
            }
        }

        System.out.println("Row = " + rowIndex);
        System.out.println("Maximum 1s = " + maxCount);
    }
  }

  ---
  
  ## 12.Reverse a String Without Using Built-in Functions

         public class ReverseString {
        
            public static void main(String[] args) {
        
                String str = "Java";
                char[] arr = str.toCharArray();
        
                int left = 0;
                int right = arr.length - 1;
        
                while (left < right) {
        
                    char temp = arr[left];
                    arr[left] = arr[right];
                    arr[right] = temp;
        
                    left++;
                    right--;
                }
        
                System.out.println(new String(arr));
            }
        }

---

  ##  13.Missing Integer

    public int missingNumber_SumFormula(int[] arr) {
        long n = arr.length + 1;         // largest element will number element + 1 (because missing 1 element missing) 
        long expectedSum = n * (n + 1) / 2;
        long actualSum   = 0;
    
        for (int num : arr) {
            actualSum += num;
        }
    
        return (int)(expectedSum - actualSum);
    }

```
arr = [1, 2, 3, 5, 6]

n = 6  (largest element, since one is missing)

Expected sum = 6 × (6+1) / 2 = 21
Actual sum   = 1+2+3+5+6    = 17

Missing = 21 - 17 = 4 ✅
```

## Move Zero To Right

      public static void moveZeroes(int[] nums) {
      
          int j = 0;
      
          for (int i = 0; i < nums.length; i++) {
      
              if (nums[i] != 0) {
      
                  int temp = nums[i];
                  nums[i] = nums[j];
                  nums[j] = temp;
      
                  j++;
              }
          }
      }
