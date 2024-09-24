2个小时：10道单选+5道多选+3道编程

## 第一题：数学问题
定义 f(n) 为 n 的最大奇约数, f(1) = 1, f(2) = 1, f(3) = 3, f(4) = 1, f(5) = 5 ..... 以此类推。 定义函数 g(n) = f(1) + f(2) + f(3) .... + f(n) 。 使用Java编程实现g（n），并且时间复杂度尽可能低

输入：4
输出：6


### 代码实现
```java
public class Solution {
    /**
     * 计算 g(n) = f(1) + f(2) + ... + f(n)
     *
     * @param n 代表用户数量
     * @return g(n) 的值
     */
    public long g(int n) {
        long sum = 0;

        for (int i = 1; i <= n; i++) {
            sum += f(i);
        }

        return sum;
    }

    /**
     * 计算 n 的最大奇约数 f(n)
     *
     * @param n 需要计算的数
     * @return n 的最大奇约数
     */
    private int f(int n) {
        // 如果 n 是偶数，将其除以 2 直到变为奇数
        while (n % 2 == 0) {
            n /= 2;
        }
        return n; // 返回最终的奇数
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int n = 10; // 可以更改输入值进行测试
        System.out.println(solution.g(n)); // 输出 g(n)
    }
}
```

> 只过了 28%

## 第二题

系统中维护了用户之间的关注关系。现在有n个用户，这些用户的id用1到n的数字表示。这n个用户中可能混入了一个特殊的“运营用户”。已知：
1. 所有的其他用户都关注了这个”运营用户“；
2. 这个”运营用户“没有关注任何人；
3. 这n个用户中只有一个”运营用户“。


给定一个数字n，表示用户的数量；以及数组 relations，表示这n个用户之间的关注信息。$relations[i] = [a, b]$ 表示用户a关注了用户b。注意：数组relations中不存在重复的元素。
请找到这个特殊的“运营用户”的id，如果不存在，返回-1。

示例：
输入：
`3,[[1,3],[2,3]]`
输出：
`3`

说明：
所有的其他用户都关注了用户3，但用户3没有关注任何人


### 思路分析

通过维护一个用户关注情况的计数来解决这个问题。我们可以使用一个数组来记录每个用户关注的数量和被关注的数量。

### 代码实现

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    /**
     * 查找特殊的“运营用户”
     *
     * @param n 用户数量
     * @param relations 关注关系
     * @return 运营用户的 ID，如果不存在返回 -1
     */
    public int findOperatorUser(int n, int[][] relations) {
        int[] followers = new int[n + 1]; // 被关注的计数
        int[] following = new int[n + 1]; // 关注的计数

        // 处理关注关系
        for (int[] relation : relations) {
            int a = relation[0];
            int b = relation[1];
            following[a]++; // 用户 a 关注了用户 b
            followers[b]++;  // 用户 b 被用户 a 关注
        }

        // 查找运营用户
        for (int i = 1; i <= n; i++) {
            // 运营用户的条件
            if (following[i] == 0 && followers[i] == n - 1) {
                return i; // 找到运营用户
            }
        }

        return -1; // 未找到运营用户
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int n = 3;
        int[][] relations = {{1, 3}, {2, 3}};
        System.out.println(solution.findOperatorUser(n, relations)); // 输出 3
    }
}

```



## 第三题：双指针

一条路高低不平，下雨时水会留在低洼的地方。把路面简化成一个直线，用一个数组描述它的高低。 

比如 [ 3 2 4 ] 代表一条 3 个单位长的路，高度依次为 3 2 4，则下雨时，它的最大积水可以达到 1 。 

实现一个计算最大积水的 函数，输入是代表描述路面高低的数组，输出是最大积水量 

示例： 

输入： `[1,3,2,2,9,1,4]`
输出： `5`


### 思路分析

通过使用双指针的方法来计算最大积水量。基本思路是利用两个指针从数组两端向中间移动，同时维护左右两边的最高点，以确定当前点的积水量。



### 代码实现
```java
public class Solution {
    /**
     * 计算最大积水量
     *
     * @param heights 路面高低的数组
     * @return 最大积水量
     */
    public int calculateRainWater(int[] heights) {
        if (heights == null || heights.length == 0) {
            return 0;
        }

        int left = 0, right = heights.length - 1;
        int leftMax = 0, rightMax = 0;
        int waterTrapped = 0;

        while (left < right) {
            if (heights[left] < heights[right]) {
                if (heights[left] >= leftMax) {
                    leftMax = heights[left];
                } else {
                    waterTrapped += leftMax - heights[left];
                }
                left++;
            } else {
                if (heights[right] >= rightMax) {
                    rightMax = heights[right];
                } else {
                    waterTrapped += rightMax - heights[right];
                }
                right--;
            }
        }

        return waterTrapped;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int[] heights = {1, 3, 2, 2, 9, 1, 4};
        System.out.println(solution.calculateRainWater(heights)); // 输出 5
    }
}

```

> 过了 90%