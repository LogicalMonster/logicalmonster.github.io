---
title: 算法训练营Day2
date: 2025-05-30 00:09:15
tags:
---
# 209. 长度最小的子数组

[209. 长度最小的子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-size-subarray-sum/solutions/305704/chang-du-zui-xiao-de-zi-shu-zu-by-leetcode-solutio/)

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }
        size_t left{};
        size_t right{};
        int total{};
        set<size_t> ans_set{};
        while (right < nums.size()) {
            total += nums[right];
            while (total >= target) {
                ans_set.insert(right - left + 1);
                total -= nums[left];
                ++left;
            }
            ++right;
        }
        return ans_set.empty() ? 0 : *ans_set.lower_bound(0);
    }
};
```

时间复杂度：
O(n log n)

时间复杂度：
O(n)

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }
        size_t left{};
        int total{};
        size_t min_len{SIZE_MAX};
        for (size_t right = 0; right < nums.size(); ++right) {
            total += nums[right];
            while (total >= target) {
                min_len = min(min_len, right - left + 1);
                total -= nums[left++];
            }
        }
        return (min_len == SIZE_MAX) ? 0 : min_len;
    }
};
```

时间复杂度：
O(n)

时间复杂度：
O(1)

# 59. 螺旋矩阵 II

[59. 螺旋矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix-ii/)

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> output(n, vector<int>(n));
        if (n == 0) {
            return output;
        }
        size_t left{};
        size_t right{static_cast<size_t>(n)};
        size_t idx{};
        while (left < right - 1) {
            for (size_t i = 0; i < right - 1 - left; ++i) {
                output[left][left + i] = idx + i + 1;
                output[left + i][right - 1] = right - left - 1 + idx + i + 1;
                output[right - 1][right - 1 - i] =
                    2 * (right - left - 1) + idx + i + 1;
                output[right - 1 - i][left] =
                    3 * (right - left - 1) + idx + i + 1;
            }
            idx += 4 * (right - 1 - left);
            ++left;
            --right;
        }
        if (n % 2 != 0) {
            output[left][left] = idx + 1;
        }
        return output;
    }
};
```

时间复杂度：
O(n^2)

空间复杂度：
O(n^2)

# 随想录58. 区间和

[58. 区间和（第九期模拟笔试）](https://kamacoder.com/problempage.php?pid=1070)

```cpp
#include <iostream>
#include <vector>

int main() {
    size_t n{};
    std::cin >> n;
    std::vector<int> vec(n);
    std::vector<int> p(n);
    int presum{};
    for (size_t i = 0; i < n; ++i) {
        std::cin >> vec[i];
        presum += vec[i];
        p[i] = presum;
    }

    size_t a{};
    size_t b{};
    while (std::cin >> a >> b) {
        int sum{};
        if (a == 0) {
            sum = p[b];
        } else {
            sum = p[b] - p[a - 1];
        }
        std::cout << sum << "\n";
    }
}
```

时间复杂度：
O(n + q)

空间复杂度：
O(n)
# 随想录44. 开发商购买土地

[44. 开发商购买土地（第五期模拟笔试）](https://kamacoder.com/problempage.php?pid=1044)

```cpp
#include <iostream>
#include <vector>

int main() {
    size_t n{};
    size_t m{};
    std::cin >> n >> m;
    std::vector<std::vector<size_t>> mat(n, std::vector<size_t>(m));
    for (size_t i = 0; i < n; ++i) {
        for (size_t j = 0; j < m; ++j) {
            std::cin >> mat[i][j];
        }
    }
    size_t output{SIZE_MAX};
    for (size_t s = 1; s < m; ++s) {
        size_t a{};
        size_t b{};
        for (size_t i = 0; i < n; ++i) {
            for (size_t j = 0; j < s; ++j) {
                a += mat[i][j];
            }
            for (size_t j = s; j < m; ++j) {
                b += mat[i][j];
            }
        }
        output = std::min(output, (a > b ? a - b : b - a));
    }
    for (size_t s = 1; s < n; ++s) {
        size_t a{};
        size_t b{};
        for (size_t j = 0; j < m; ++j) {
            for (size_t i = 0; i < s; ++i) {
                a += mat[i][j];
            }
            for (size_t i = s; i < n; ++i) {
                b += mat[i][j];
            }
        }
        output = std::min(output, (a > b ? a - b : b - a));
    }
    std::cout << output;
}
```

时间复杂度：O(n * m^2 + n^2 * m)
空间复杂度：O(n * m)

二维前缀和解法：

```cpp
#include <iostream>
#include <vector>

int main() {
    size_t n{};
    size_t m{};
    std::cin >> n >> m;
    std::vector<std::vector<int>> mat(n, std::vector<int>(m));
    std::vector<std::vector<int>> p(n, std::vector<int>(m));
    for (size_t i = 0; i < n; i++)
    {
        for (size_t j = 0; j < m; j++)
        {
            size_t a{};
            std::cin >> a;
            p[i][j] += a;
            if (i != 0) {
                p[i][j] += p[i - 1][j];
            }
            if (j != 0) {
                p[i][j] += p[i][j - 1];
            }
            if (i != 0 && j != 0) {
                p[i][j] -= p[i - 1][j - 1];
            }
        }
    }
    size_t output{SIZE_MAX};
    for (size_t i = 0; i < n - 1; ++i) {
        size_t a{p[n - 1][m - 1] > 2 * p[i][m - 1] ? p[n - 1][m - 1] - 2 * p[i][m - 1] : 2 * p[i][m - 1] - p[n - 1][m - 1]};
        output = std::min(output, a);
    }
    for (size_t j = 0; j < m - 1; ++j) {
        size_t a{p[n - 1][m - 1] > 2 * p[n - 1][j] ? p[n - 1][m - 1] - 2 * p[n - 1][j] : 2 * p[n - 1][j] - p[n - 1][m - 1]};
        output = std::min(output, a);
    }
    std::cout << output;
}
```

时间复杂度：O(n * m)
空间复杂度：O(n * m)
