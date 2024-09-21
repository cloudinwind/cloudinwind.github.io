## OPPO 
直接笔试，2个小时 20道单选+3道编程题

### 第一题：贪心

#### 题目描述

小红有一个长度为 n 的数组{a_1, a_2, ......, a_n} ，小红需要执行 k 次操作，操作内容如下：
- 选择一个数组元素 a_i，令 a_i = a_i 按位异或 1
小红想让数组元素之和最大，请你输出这个值。

输入描述:
第一行输入两个整数 n, k 代表数组中的元素数量、操作数量。
第二行输入 n 个整数 代表数组元素。
其中 1<=n<=100000, 1<=k<=1000000000
数组中的每个元素 a_i 满足1<= a_i<=1000000000

输出描述:
在一行上输出一个整数，代表最大的数组元素之和。

示例：
输入：
5 3
1 2 3 4 5

输出
16

#### 思路

这个问题的目标是通过执行最多 kkk 次按位异或操作，让数组的元素之和尽可能大。

按位异或操作 `a_i = a_i 异或 1` 的作用是：如果 `a_i` 是偶数，则将其变为奇数；如果 `a_i` 是奇数，则将其变为偶数。因此：

- 对偶数执行异或操作可以让它增加 1（变成更大的奇数）。
- 对奇数执行异或操作会让它减少 1（变成更小的偶数）。

优化思路：

1. **贪心策略**：
    
    - 如果 a_i​ 是偶数，我们希望对其执行异或操作，这样可以让它增加 1。
    - 如果 a_i​ 是奇数，我们尽量不对其执行异或操作，因为这样会减少它的值。
2. **优先处理偶数**：
    
    - 将数组中的偶数尽可能变为奇数，因为这样可以增加总和。
    - 如果操作次数 k 用完后还有剩余，剩下的操作需要交替对奇数和偶数进行反转。
3. **处理剩余操作**：
    
    - 如果 k 次操作后还有剩余，且剩下的次数是偶数次，那么不需要再改变数组，因为两次操作对同一个数会恢复原值。
    - 如果剩余次数是奇数次，则可以对任意一个元素进行一次异或操作，影响最小的选择是对某个奇数执行一次异或。

算法步骤：

1. 遍历数组，将偶数按位异或，使其变成奇数。
2. 计算剩余的操作次数。
3. 如果剩余操作是奇数次，则对影响最小的元素（奇数）执行一次异或。


#### 答案
```java
import java.util.Scanner;

public class MaximizeArraySum {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 输入 n 和 k
        int n = scanner.nextInt();
        long k = scanner.nextLong();
        
        // 输入数组
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = scanner.nextInt();
        }
        
        // 统计初始的数组总和
        long sum = 0;
        for (int num : arr) {
            sum += num;
        }

        // 遍历数组，处理偶数元素
        for (int i = 0; i < n && k > 0; i++) {
            if (arr[i] % 2 == 0) { // 如果是偶数，执行异或操作
                arr[i] = arr[i] ^ 1; // 使偶数变为奇数
                sum += 1; // 偶数异或后加1，所以总和增加1
                k--; // 执行了一次操作
            }
        }
        
        // 如果剩下的 k 还不为0且是奇数，需要再处理一次异或
        if (k > 0 && k % 2 == 1) {
            // 对最小的元素再执行一次异或操作，选择改变后最小影响的元素
            int minElement = Integer.MAX_VALUE;
            for (int num : arr) {
                minElement = Math.min(minElement, num);
            }
            // 异或操作会让这个最小的元素增减 1
            sum -= minElement; // 先减去当前的最小值
            sum += minElement ^ 1; // 再加上异或 1 后的值
        }
        
        // 输出最大的总和
        System.out.println(sum);
        
        scanner.close();
    }
}

```


>过了60%


#### 代码解析：

1. **输入处理**：我们读取数组的长度 n 和操作次数 k，并初始化数组。
2. **初始总和计算**：在不进行操作的情况下，先计算出数组的总和。
3. **处理偶数**：我们遍历数组，将偶数通过异或 1 变成奇数，并增加总和。
4. **处理剩余操作**：如果剩余的操作次数 kkk 是奇数，那么就选择数组中最小的元素执行一次异或操作，确保对总和的影响最小。
5. **输出结果**：最后输出最大化的数组总和。

复杂度分析：

- **时间复杂度**：O(n)，我们只需遍历数组一遍，来处理所有偶数并计算总和。
- **空间复杂度**：O(1)，只需常数空间来存储几个变量。


### 第二题
#### 题目描述
小红有一个长度为  n 的数组  {a_1, a_2, ..., a_n}。小红可以进行若干次操作，每次操作可以选择一个数p (1<=p<=n-1) ，将数组分成 [1, p] 和 [p+1,n] 两部分，然后分别对这两部分进行反转。
例如，对于数组 [1, 2, 3, 4, 5]，如果 p=2，则操作后的数组为 [2, 1, 5, 4, 3]。
小红想对数组进行若干次操作，然后截取一个非空子数组，使得截取的子数组元素和最大，问最大的元素和是多少。


输入描述
第一行输入一个整数  n (1<=n<=100000)代表数组中的元素数量。
第二行输入 n 个整数 a_1, a_2, ..., a_n (1<=a_i<=1000000000)表示数组元素。

输出描述：
在一行上输出一个整数，代表最大的元素和

示例：
输入：
5
3 4 -5 -1 2
输出：
9


#### 思路

为了充分利用反转操作的潜力，我们需要考虑以下两点：

1. **不进行任何反转的最大子数组和**：即直接使用 Kadane's Algorithm 得到当前数组的最大子数组和。
2. **一次反转后的最大子数组和**：通过反转操作，我们可以优化子数组的和。特别是考虑“前缀”和“后缀”的组合，反转会将前缀和后缀互换，因此我们需要计算可能通过反转获得的更优子数组和。

解决方案：

1. **正向 Kadane's Algorithm**：计算数组的最大子数组和，代表不做任何反转的情况。
2. **反向 Kadane's Algorithm**：计算数组从后往前的最大子数组和，代表进行一次反转后，可以将一些负数移动到后面或前面以优化结果。

我们需要计算出以下几个部分：

- 原始数组中正向的最大子数组和。
- 反向时的前缀和后缀的组合，找出反转后可以获得的最大子数组和。

#### 答案
```java
import java.util.Scanner;

public class MaxSubarraySumWithReverse {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 读取输入的 n
        int n = scanner.nextInt();
        
        // 读取数组
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = scanner.nextInt();
        }
        
        // 计算最大子数组和（不做反转）
        long maxNormal = kadaneMaxSubarraySum(arr);
        
        // 计算可能通过反转获得的最大子数组和
        long maxWithReverse = maxSubarraySumWithReverse(arr);
        
        // 输出最大值
        System.out.println(Math.max(maxNormal, maxWithReverse));
        
        scanner.close();
    }
    
    // Kadane's Algorithm 实现，计算最大子数组和
    public static long kadaneMaxSubarraySum(int[] arr) {
        long currentMax = arr[0];
        long globalMax = arr[0];
        
        for (int i = 1; i < arr.length; i++) {
            currentMax = Math.max(arr[i], currentMax + arr[i]);
            globalMax = Math.max(globalMax, currentMax);
        }
        
        return globalMax;
    }
    
    // 计算通过一次反转获得的最大子数组和
    public static long maxSubarraySumWithReverse(int[] arr) {
        int n = arr.length;
        
        // 前缀最大和
        long[] prefixMax = new long[n];
        long currentPrefixSum = 0;
        long maxPrefix = Long.MIN_VALUE;
        
        for (int i = 0; i < n; i++) {
            currentPrefixSum += arr[i];
            maxPrefix = Math.max(maxPrefix, currentPrefixSum);
            prefixMax[i] = maxPrefix;
        }
        
        // 后缀最大和
        long[] suffixMax = new long[n];
        long currentSuffixSum = 0;
        long maxSuffix = Long.MIN_VALUE;
        
        for (int i = n - 1; i >= 0; i--) {
            currentSuffixSum += arr[i];
            maxSuffix = Math.max(maxSuffix, currentSuffixSum);
            suffixMax[i] = maxSuffix;
        }
        
        // 寻找通过一次反转可以得到的最大子数组和
        long maxSumWithReverse = Long.MIN_VALUE;
        
        for (int p = 0; p < n - 1; p++) {
            maxSumWithReverse = Math.max(maxSumWithReverse, prefixMax[p] + suffixMax[p + 1]);
        }
        
        return maxSumWithReverse;
    }
}
```

#### 代码解释：

1. **Kadane's Algorithm**：用于计算数组的最大子数组和，这部分不考虑反转，直接应用 Kadane 算法。
    
2. **maxSubarraySumWithReverse 函数**：
    
    - 计算数组的前缀和后缀最大值。
    - `prefixMax[i]` 表示从数组的开头到第 `i` 个元素的最大子数组和。
    - `suffixMax[i]` 表示从数组的第 `i` 个元素到数组末尾的最大子数组和。
    - 在反转时，我们选择一个位置 `p`，将数组分成前缀 `[1, p]` 和后缀 `[p+1, n]`，分别反转这两部分。为了获得最优解，我们找出某个位置 `p` 使得 `prefixMax[p] + suffixMax[p + 1]` 最大。
3. **比较最大值**：我们取不反转时的最大子数组和 `maxNormal` 和反转后可能获得的最大子数组和 `maxWithReverse` 中的较大值，作为最终的结果。


### 第三题：
#### 题目描述
小红有两个长度为 n 的数组 {a_1, a_2, ..., a_n} 和 {b_1, b_2, ..., b_n} ，定义数组  a和数组 b 的距离为 (a_1-b_1)^2 + (a_2 - b_2)^2 + ... + (a_i - b_i)^2 + ... + (a_n - b_n)^2。
小红最多可以进行 k 次操作，每次操作可以选择数组 a 或数组 b 中的一个元素，将其加一或减一。请问小红最多可以将两个数组的距离缩小到多少。

输入描述：
第一行输入两个整数 n,k (1<=n<=100000, 0<=k<=1000000000) 分别代表数组中的元素数量、最多可以进行的操作次数。
第二行输入  n 个整数a_1, a_2, ..., a_n(1<=a_i<=1000000000)  表示数组 a。
第三行输入  n 个整数b_1, b_2, ..., b_n(1<=b_i<=1000000000)  表示数组 b。

输出描述：
输出一个整数，表示最多可以将两个数组的距离缩小到多少。由于答案可能很大，输出答案对 1000000000+7 取模的结果。

示例：
输入：
5 3
1 2 3 4 5
5 4 3 2 1
输出：
21

#### 思路

1. **初始距离计算**：首先计算初始的距离，即对每一对元素 (ai,bi)(a_i, b_i)(ai​,bi​) 计算 (ai−bi)2(a_i - b_i)^2(ai​−bi​)2 的和。
    
2. **优先减少差距大的项**：我们希望尽量减小那些差值绝对值最大的项的平方值，因此每次操作我们需要选择绝对差值最大的元素进行调整。
    
3. **调整后的差值**：每次对差值最大的项进行调整，将其减少（加1或减1），然后重新计算这个位置的平方差。
    
4. **贪心策略**：每次找到当前最大差值的项，进行一次操作，使得距离尽可能减少。可以使用最大堆（优先队列）来追踪当前最大差值的位置和值。
    
5. **取模计算**：最后结果对 10^9 + 7 取模。
    

具体步骤：

1. 初始化计算每一对元素 a_i​ 和 b_i​ 的差值平方，并计算出总距离。
2. 将每一对元素的差值按绝对值大小放入优先队列。
3. 每次从队列中取出当前绝对差值最大的项，执行一次操作（将其增大或减小1，取决于差值的符号），并更新优先队列中的该项。
4. 重复该操作最多执行 `k` 次。
5. 最后计算出更新后的距离，并对 10^9 + 7 取模。

#### 代码
```java
import java.util.PriorityQueue;
import java.util.Scanner;

public class MinimizeArrayDistance {
    // 模数
    static final long MOD = 1000000000 + 7;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 输入 n 和 k
        int n = scanner.nextInt();
        long k = scanner.nextLong();
        
        // 输入数组 a 和 b
        int[] a = new int[n];
        int[] b = new int[n];
        for (int i = 0; i < n; i++) {
            a[i] = scanner.nextInt();
        }
        for (int i = 0; i < n; i++) {
            b[i] = scanner.nextInt();
        }
        
        // 优先队列，存储绝对差值，最大堆
        PriorityQueue<Integer> pq = new PriorityQueue<>((x, y) -> Integer.compare(Math.abs(y), Math.abs(x)));
        
        // 初始计算距离并将差值放入优先队列
        long initialDistance = 0;
        for (int i = 0; i < n; i++) {
            int diff = a[i] - b[i];
            pq.offer(diff);
            initialDistance += (long) diff * diff;
        }
        
        // 贪心减少距离
        while (k > 0 && !pq.isEmpty()) {
            int diff = pq.poll();
            int newDiff;
            
            if (diff > 0) {
                newDiff = diff - 1;
            } else if (diff < 0) {
                newDiff = diff + 1;
            } else {
                newDiff = 0; // 如果差值为 0，不能再减少了
            }
            
            // 更新总距离
            initialDistance -= (long) diff * diff;
            initialDistance += (long) newDiff * newDiff;
            pq.offer(newDiff);
            
            k--;
        }
        
        // 输出结果，对 MOD 取模
        System.out.println(initialDistance % MOD);
        
        scanner.close();
    }
}
```

并没有完全A