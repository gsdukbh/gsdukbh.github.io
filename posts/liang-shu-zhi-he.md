---
title: '两数之和'
date: 2023-03-07 21:12:31
tags: []
published: true
hideInList: false
feature: 
isTop: false
---

```java
public class TwoSum {
     public static int[] twoSum(int[] numbers, int target) {
        int high = numbers.length - 1, low = 0;
        while (low < high) {
            int sum = numbers[low] + numbers[high];
            if (sum == target) {
                return new int[] { low + 1, high + 1 };
            } else if (sum > target) {
                --high;
            } else {
                ++low;
            }
        }
        return new int[] { -1, -1 };
    }

    public static int[] twoSum1(int[] numbers, int target) {
        for (int i = 0; i < numbers.length; ++i) {
            int low = i + 1, high = numbers.length - 1;
            while (low < high) {
                int mid = (high - low) / 2 + low;
                if (target == numbers[mid] + numbers[i]) {
                    return new int[] { i + 1, mid + 1 };
                } else if (target > numbers[mid] + numbers[i]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }

        }
        return new int[] { -1, -1 };
    }
}
```