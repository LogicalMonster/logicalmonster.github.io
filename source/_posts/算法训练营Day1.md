---
title: 算法训练营Day1
date: 2025-05-28 17:11:26
tags:
---
# 704. 二分查找

[704. 二分查找 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-search/description/)

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        size_t left{};
        size_t right{nums.size() - 1};
        while (left <= right) {
            size_t mid{left + (right - left) / 2};
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                if (mid == 0) {
                    break;
                }
                right = mid - 1;
            }
        }
        return -1;
    }
};
```

时间复杂度：
O(log n)
在 $k$ 次迭代后, 搜索空间变为 $n/2^k$, 直到 $n/2^k = 1$, 即$k = \log_2(n)$.

空间复杂度：
O(1)

# 27. 移除元素

[27. 移除元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-element/description/)

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k{};
        for (size_t i = 0; i < nums.size(); ) {
            if (nums[i] == val) {
                nums.erase(nums.begin() + i);
            } else {
                ++k;
                ++i;
            }
        }
        return k;
    }
};
```

时间复杂度：
O(n^2)

空间复杂度：
O(1)

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        size_t slow{};
        for (size_t fast = 0; fast < nums.size(); ++fast) {
            if (val != nums[fast]) {
                nums[slow++] = nums[fast];
            }
        }
        return slow;
    }
};
```

时间复杂度：
O(n)

空间复杂度：
O(1)

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if (nums.empty()) {
            return 0;
        }
        size_t left{};
        size_t right{nums.size() - 1};
        while (left <= right) {
            if (nums[left] == val) {
                nums[left] = nums[right];
                if (right == 0) {
                    break;
                }
                --right;
            } else {
                ++left;
            }
        }
        return left;
    }
};
```

时间复杂度：
O(n)

空间复杂度：
O(1)

# 977. 有序数组的平方

[977. 有序数组的平方 - 力扣（LeetCode）](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for (size_t i = 0; i < nums.size(); ++i) {
            nums[i] *= nums[i];
        }
        sort(nums.begin(), nums.end());
        return nums;
    }
};
```

时间复杂度：
O(nlog n)

空间复杂度：
O(1)

```cpp
class Solution {
public:
    vector<int> sortedSquares(const vector<int>& nums) {
        vector<int> ans{};
        if (nums.empty()) {
            return ans;
        }
        size_t nonnegative{};
        for (size_t i = 0; i < nums.size(); ++i) {
            if (nums[i] < 0) {
                ++nonnegative;
            } else {
                break;
            }
        }
        size_t i{nonnegative >= 1 ? nonnegative - 1 : 0};
        size_t j{nonnegative};
        while (nonnegative >= 1 && j < nums.size()) {
            if (nums[i] * nums[i] < nums[j] * nums[j]) {
                ans.push_back(nums[i] * nums[i]);
                if (i == 0) {
                    break;
                }
                --i;
            } else {
                ans.push_back(nums[j] * nums[j]);
                ++j;
            }
        }
        if (j == nums.size()) {
            while (true) {
                ans.push_back(nums[i] * nums[i]);
                if (i == 0) {
                    break;
                }
                --i;
            }
        } else if (i == 0) {
            while (j < nums.size()) {
                ans.push_back(nums[j] * nums[j]);
                ++j;
            }
        }
        return ans;
    }
};
```

时间复杂度：
O(n)

空间复杂度：
O(n)

```cpp
class Solution {
public:
    vector<int> sortedSquares(const vector<int>& nums) {
        vector<int> ans{};
        if (nums.empty()) {
            return ans;
        }
        size_t nonnegative{};
        for (size_t i = 0; i < nums.size(); ++i) {
            if (nums[i] < 0) {
                ++nonnegative;
            } else {
                break;
            }
        }
        size_t i{nonnegative};
        size_t j{nonnegative};
        while (i >= 1 || j < nums.size()) {
            if (j == nums.size()) {
                while (i >= 1) {
                    ans.push_back(nums[i - 1] * nums[i - 1]);
                    --i;
                }
            } else if (i == 0) {
                while (j < nums.size()) {
                    ans.push_back(nums[j] * nums[j]);
                    ++j;
                }
            } else if (nums[i - 1] * nums[i - 1] < nums[j] * nums[j]) {
                ans.push_back(nums[i - 1] * nums[i - 1]);
                --i;
            } else {
                ans.push_back(nums[j] * nums[j]);
                ++j;
            }
        }
        return ans;
    }
};
```

时间复杂度：
O(n)

空间复杂度：
O(n)

```cpp
class Solution {
public:
    vector<int> sortedSquares(const vector<int>& nums) {
        vector<int> ans(nums.size());
        for (int i = 0, j = nums.size() - 1, pos = nums.size() - 1; i <= j;) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                ans[pos] = nums[i] * nums[i];
                ++i;
            }
            else {
                ans[pos] = nums[j] * nums[j];
                --j;
            }
            --pos;
        }
        return ans;
    }
};
```

时间复杂度：
O(n)

空间复杂度：
O(n)
