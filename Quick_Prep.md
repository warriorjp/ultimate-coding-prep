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
