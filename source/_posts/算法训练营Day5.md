---
title: 算法训练营Day5
date: 2025-06-02 22:51:35
tags:
---
# 242. 有效的字母异位词

[242. 有效的字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-anagram/description/)

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }
        std::map<char, size_t> ms{};
        std::map<char, size_t> mt{};
        for (size_t i = 0; i < s.size(); i++)
        {
            ++ms[s[i]];
            ++mt[t[i]];
        }
        return ms == mt;
    }
};
```

时间复杂度：O(n)
空间复杂度：O(1)

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }
        std::array<ssize_t, 26> count{};
        for (size_t i = 0; i < s.size(); i++)
        {
            ++count[s[i] - 'a'];
            --count[t[i] - 'a'];
        }
        for (auto &&i : count) {
            if (i != 0) {
                return false;
            }
        }
        return true;
    }
};
```

时间复杂度：O(n)
空间复杂度：O(1)

# 349. 两个数组的交集

[349. 两个数组的交集 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays/)

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        std::unordered_set<int> us1{}, us2{};
        for (auto&& i : nums1) {
            us1.insert(i);
        }
        for (auto&& i : nums2) {
            us2.insert(i);
        }

        const std::unordered_set<int>& small =
            (us1.size() < us2.size()) ? us1 : us2;
        const std::unordered_set<int>& large =
            (us1.size() < us2.size()) ? us2 : us1;
        std::vector<int> intersection;
        for (const int& val : small) {
            if (large.find(val) != large.end()) {
                intersection.push_back(val);
            }
        }
        return intersection;
    }
};
```

时间复杂度：O(m + n)
空间复杂度：O(m + n)

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        std::sort(nums1.begin(), nums1.end());
        std::sort(nums2.begin(), nums2.end());

        std::vector<int> intersection;
        size_t i{}, j{};
        while (i < nums1.size() && j < nums2.size()) {
            if (nums1[i] == nums2[j]) {
                if (!intersection.size() || nums1[i] != intersection.back()) {
                    intersection.push_back(nums1[i]);
                }
                ++i;
                ++j;
            } else if (nums1[i] < nums2[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return intersection;
    }
};
```

时间复杂度：O(m log m + n log n)
空间复杂度：O(log m + log n)

# 202. 快乐数

[202. 快乐数 - 力扣（LeetCode）](https://leetcode.cn/problems/happy-number/)

```cpp
class Solution {
public:
    size_t get_next(int n) {
        size_t output{};
        while (n) {
            output += (n % 10) * (n % 10);
            n /= 10;
        }
        return output;
    }
    bool isHappy(int n) {
        std::unordered_set<size_t> us{};
        while (n != 1) {
            auto ret = us.insert(n);
            if (!ret.second) {
                return false;
            }
            n = get_next(n);
        }
        return true;
    }
};
```

时间复杂度：O(log n)
空间复杂度：O(log n)

```cpp
class Solution {
public:
    size_t get_next(int n) {
        size_t output{};
        while (n) {
            output += (n % 10) * (n % 10);
            n /= 10;
        }
        return output;
    }
    bool isHappy(int n) {
        int slow{n};
        int fast{n};
        while (fast != 1) {
            slow = get_next(slow);
            fast = get_next(get_next(fast));
            if (fast == slow) {
                break;
            }
        }
        return fast == 1;
    }
};
```

时间复杂度：O(log n)
空间复杂度：O(1)

# 1. 两数之和

[1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/description/)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size() - 1; ++i) {
            for (int j = i + 1; j < nums.size(); ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```

时间复杂度：O(n^2)
空间复杂度：O(1)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map<int, int> um{};
        for (int i = 0; i < nums.size(); ++i) {
            if (um.find(target - nums[i]) != um.end()) {
                return {um[target - nums[i]], i};
            }
            um[nums[i]] = i;
        }
        return {};
    }
};
```

时间复杂度：O(n)
空间复杂度：O(n)
