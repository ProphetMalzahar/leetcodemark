和349的区别在于，对于输入：nums1 = [1,2,2,1], nums2 = [2,2]，349输出：[2]，350则输出：[2,2]


和三数之和的做法非常相似，先做排序，然后双指针。
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        List<Integer> list=new ArrayList<>();
        for (int i = 0,j=0 ; i < nums1.length&&j< nums2.length;) {
            if (nums1[i]<nums2[j]){
                i++;
            }else if (nums1[i]>nums2[j]){
                j++;
            }else {
                list.add(nums1[i]);
                i++;
                j++;
            }
        }
        int[] nums=new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            nums[i]= list.get(i);
        }
        return nums;
    }
}
```
