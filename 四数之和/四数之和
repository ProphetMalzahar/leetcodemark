class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> lists=new ArrayList<>();
        for(int i=0;i<nums.length;i++)
        {
            if (i>0&&nums[i]==nums[i-1])
                continue;
            int newtarget=target-nums[i];//新的target
            for (int j = i+1; j < nums.length; j++) {
                if (j>i+1&&nums[j]==nums[j-1])
                    continue;
                int newtarget1=newtarget-nums[j];
                int start=j+1,end=nums.length-1;
                while (start<end)
                {
                    if (nums[start]+nums[end]==newtarget1)
                    {
                        lists.add(Arrays.asList(nums[i],nums[j],nums[start],nums[end]));
                        while (start<end&&nums[start]==nums[start+1])
                            start++;
                        while (start<end&&nums[end]==nums[end-1])
                            end--;
                        start++;
                        end--;
                    }
                    else if (nums[start]+nums[end]>newtarget1)
                        end--;
                    else 
                        start++;
                }
            }
        }
        return lists;
    }
}
