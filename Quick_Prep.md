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


***2.✅ Find a Pair of sums for target value  :**

For Sorted Array: Use Two-Pointer Technique

int[] nums = {4, 2, 7, 5};

int target = 9;

Time Complexity: O(n)

Space Complexity: O(1)

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
