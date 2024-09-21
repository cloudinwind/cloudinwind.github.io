## Vivo
先做了人才测评, 9.12日晚发了笔试邮件，9.13下午3点笔试
题目类型：6道单选，4道多选，3道编程题

### 第一道编程题：签到题
#### 题目描述
Vivo项目组新老员工分组完成任务，员工数组staff，其中0表示新员工，1表示老员工
分组规则如下：
1、一个小组至多3个员工
2、一个小组中最多有1个老员工
3、如果一个小组中有1个老员工，那么这组最多有2个员工

求最小的分组数。


输入描述：
输入员工数组 staff，元素只包含0,1
输出描述：
输出一个整数，表示最小分组数



### 第二道编程题：滑动窗口
#### 题目描述
你是一名手机应用开发工程师，需要分析应用在手机上的内存使用情况。你有一个数组 memoryUsage，其中 memoryUsage[i] 表示应用在第 i 秒的内存使用量（以MB为单位）。为了评估应用的稳定性，你需要找出每个连续 k 秒内的内存使用量的波动范围（即最大值与最小值的差值），并返回这些波动范围。

输入
```
[80, 100, 70, 90, 60, 85, 75, 95, 110],4
```

输出 
```
[30,40,30,30,35,35]
```

#### 答案

使用滑动窗口技术来高效地计算每个连续 k 秒内的内存使用量的波动范围。
使用双端队列（Deque）来维护当前窗口中的最大值和最小值。这样可以在 O(n) 时间复杂度内完成计算，其中 n 是数组的长度。

```java
import java.util.Deque;
import java.util.LinkedList;

public class MemoryUsageAnalysis {
    public static void main(String[] args) {
        int[] memoryUsage = {80, 100, 70, 90, 60, 85, 75, 95, 110};
        int k = 4;
        int[] result = calculateFluctuationRange(memoryUsage, k);
        for (int r : result) {
            System.out.print(r + " ");
        }
    }

    public static int[] calculateFluctuationRange(int[] memoryUsage, int k) {
        int n = memoryUsage.length;
        if (n == 0 || k > n) return new int[0];

        int[] result = new int[n - k + 1];
        Deque<Integer> maxDeque = new LinkedList<>();
        Deque<Integer> minDeque = new LinkedList<>();

        for (int i = 0; i < n; i++) {
            // Remove indices that are out of the current window
            while (!maxDeque.isEmpty() && maxDeque.peekFirst() <= i - k) {
                maxDeque.pollFirst();
            }
            while (!minDeque.isEmpty() && minDeque.peekFirst() <= i - k) {
                minDeque.pollFirst();
            }

            // Maintain the decreasing order for maxDeque
            while (!maxDeque.isEmpty() && memoryUsage[maxDeque.peekLast()] <= memoryUsage[i]) {
                maxDeque.pollLast();
            }
            // Maintain the increasing order for minDeque
            while (!minDeque.isEmpty() && memoryUsage[minDeque.peekLast()] >= memoryUsage[i]) {
                minDeque.pollLast();
            }

            maxDeque.offerLast(i);
            minDeque.offerLast(i);

            // Calculate the result for the current window
            if (i >= k - 1) {
                int max = memoryUsage[maxDeque.peekFirst()];
                int min = memoryUsage[minDeque.peekFirst()];
                result[i - k + 1] = max - min;
            }
        }
        return result;
    }
}
```

初始化：
- maxDeque 用于维护当前窗口的最大值的索引。
- minDeque 用于维护当前窗口的最小值的索引。

遍历数组：
- 对于每个元素，首先移除已经超出当前窗口范围的索引。
- 维护 maxDeque 使其保持递减顺序，维护 minDeque 使其保持递增顺序。
- 将当前索引添加到 maxDeque 和 minDeque 中。
- 当 当前索引已经达到至少 k 的窗口大小时，计算当前窗口的最大值和最小值的差值，并将结果存入 result 数组。

输出结果：
- 遍历完成后，result 数组中保存了每个连续 k 秒内的内存使用量的波动范围。


### 第三道编程题：回溯
#### 题目描述
Vivo为回馈粉丝，进行礼品派发活动，不同礼品价格不同，为公平起见，需要将全部的礼品公平的分配到粉丝手中，且每位粉丝拿到的礼品总价格相同。请帮忙确认以下的礼品数量和价格是否可以满足公平的分配原则，可以则返回true，否则返回false。

例如：

价格分别为 5, 4, 1, 3, 2, 3, 2 的礼物是否可以公平分配到4名粉丝手？答案是可以的，按(5)、(2,3)、(2,3)、(1,4)组合，第一位粉丝分配到价格为5的礼物，第二位分配到价格为2和3的礼物，第三位分配到价格为2和3的礼物，第四位分配到价格为1和4的礼物，全部礼物分配完，最后返回true，代表可以公平分配。

程序需要输入的参数是价格数组和粉丝人数（数组元素和粉丝人数均小于1000）


示例：
输入：
```
[5,4,1,3,2,3,2],4
```

输出：
```
true
```

#### 思路

1. 检查是否能分配：
	- 计算所有礼品价格的总和 S。
	- 如果总和 S 不能被粉丝人数 k 整除，则不能公平分配，返回 false。
	- 计算每个粉丝应该得到的礼品总价格，即 targetSum = S / k。
2. 使用回溯算法：
	- 尝试将礼品分配到不同的粉丝，确保每个粉丝的礼品总价格都等于 targetSum。
	- 使用回溯法遍历所有可能的分配方式，确保每个粉丝都能够获得符合条件的礼品组合。
3. 优化：
	- 对礼品价格进行排序，以提高效率，先处理大价格的礼品可以减少回溯的深度。

#### 答案
```java
import java.util.Arrays;

public class FairDistribution {
    public static void main(String[] args) {
        int[] prices = {5, 4, 1, 3, 2, 3, 2};
        int k = 4;
        System.out.println(canDistribute(prices, k));  // 输出: true
    }

    public static boolean canDistribute(int[] prices, int k) {
        int total = Arrays.stream(prices).sum();
        if (total % k != 0) {
            return false;
        }
        int targetSum = total / k;
        int n = prices.length;
        
        // 先排序以优化回溯性能
        Arrays.sort(prices);
        // 如果最大值大于目标总和，不能分配
        if (prices[n - 1] > targetSum) {
            return false;
        }
        
        boolean[] used = new boolean[n];
        return canDistributeHelper(prices, used, 0, k, 0, targetSum);
    }

    private static boolean canDistributeHelper(int[] prices, boolean[] used, int start, int k, int currentSum, int targetSum) {
        if (k == 0) {
            return true;
        }
        if (currentSum == targetSum) {
            // 当前组分配完，开始下一个组
            return canDistributeHelper(prices, used, 0, k - 1, 0, targetSum);
        }

        for (int i = start; i < prices.length; i++) {
            if (!used[i] && currentSum + prices[i] <= targetSum) {
                used[i] = true;
                if (canDistributeHelper(prices, used, i + 1, k, currentSum + prices[i], targetSum)) {
                    return true;
                }
                used[i] = false;
            }
        }
        return false;
    }
}
```