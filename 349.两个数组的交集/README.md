# 两个数组的交集
给定两个数组，编写一个函数来计算它们的交集。

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]

## set做法
用set存储nums1，并且完成去重，然后遍历nums2，即可取交集，时间复杂度和空间复杂度都不错，98% 80%
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        //一个set就可以暴力解决，思路也比较简单
        Set<Integer> set=new HashSet<>();
        List<Integer> list=new ArrayList<>();
        for (int k : nums1) {
            set.add(k);
        }
        for (int j : nums2) {
            if (set.contains(j)) {
                list.add(j);
                set.remove(j);
            }
        }
        int[] nums=new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            nums[i]=list.get(i);
        }
        return nums;
    }
}
```

官方题解用了两个set，思路其实完全一样，但是空间复杂度要差一些。97% 58%
```java
class Solution {
  public int[] set_intersection(HashSet<Integer> set1, HashSet<Integer> set2) {
    int [] output = new int[set1.size()];
    int idx = 0;
    for (Integer s : set1)
      if (set2.contains(s)) output[idx++] = s;

    return Arrays.copyOf(output, idx);
  }

  public int[] intersection(int[] nums1, int[] nums2) {
    HashSet<Integer> set1 = new HashSet<Integer>();
    for (Integer n : nums1) set1.add(n);
    HashSet<Integer> set2 = new HashSet<Integer>();
    for (Integer n : nums2) set2.add(n);

    if (set1.size() < set2.size()) return set_intersection(set1, set2);
    else return set_intersection(set2, set1);
  }
}
```

## 双指针
需要先给两个数组排序，然后移动指针
```java
public int[] intersection(int[] nums1, int[] nums2) {
  Set<Integer> set = new HashSet<>();
  Arrays.sort(nums1);
  Arrays.sort(nums2);
  int i = 0, j = 0;
  while (i < nums1.length && j < nums2.length) {
    if (nums1[i] == nums2[j]) {
      set.add(nums1[i]);
      i++;
      j++;
    } else if (nums1[i] < nums2[j]) {
      i++;
    } else if (nums1[i] > nums2[j]) {
      j++;
    }
  }
  int[] res = new int[set.size()];
  int index = 0;
  for (int num : set) {
    res[index++] = num;
  }
  return res;
}
```
