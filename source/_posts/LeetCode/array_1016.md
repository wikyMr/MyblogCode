---
title: leetCode之数组专题
tags: [leetCode]
---
## 数组专题之合并两个排序数组：合并两个已排序的数组
思路：从后往前放置两个数组的最右边的最大值，最后检查被放入数组是否为空，
依次放入放入数组（容易知道这些值都是比放入数组最左的数字小的数）
```	
public static void merge(int[] nums1, int m, int[] nums2, int n) {
		if (m == 0) {
			for (int j = 0; j < nums2.length; j++) {
				nums1[j] = nums2[j];
			}
		}
		if (m > 0 && n > 0) {
			int i = m - 1;
			int j = n - 1;
			int k = m + n - 1;

			while (i >= 0 && j >= 0) {
				if (nums1[i] > nums2[j]) {
					nums1[k--] = nums1[i--];
				} else {
					nums1[k--] = nums2[j--];
				}
			}

			while (j >= 0) {
				nums1[k--] = nums2[j--];
			}
		}
	}
```
<!-- more -->
### 这道题比较的奇葩：统计两个数字相差k的数字对的数目，要求每个数字对是唯一的，且i，j与j，i算一对；
```
	public static int findPairs(int[] nums, int k) {
	 if(nums.length<0||k>1000||k<0) return 0;
	 int count=0;
	 Map<Integer, Integer> numTimes=new HashMap<Integer, Integer>();
	 //每个数字出现的频次
	 for(int i=0;i<nums.length;i++){
		 numTimes.put(nums[i], numTimes.getOrDefault(nums[i], 0)+1);
	 }
	 //对每个数字，分两种情况：
	 //1.k=0,统计数字频率大于等于2的数字即可；
	 //2.k>0，统计所有数字有多少数字加上k的值存在于该数据结构里面即可
	 for(Entry<Integer, Integer> itEntry: numTimes.entrySet()){
		 if(k==0){
			 //test_case:3 3 3 
			 //1 pair;
			//3 3  4 4 =2pair
			 if(itEntry.getValue()>=2){
				 count++;
			 }
		 }else{//test_case:3 1 4 1  5 =2pair(k=2)
			 if(numTimes.containsKey(itEntry.getKey()+k)){
				 count++;
			 }
		 }
	 }
	return count; 
	}
```
### 所有数中的三个数相乘积最大
要知道，最大的数只可能是三个最大的数相乘或者最小的两个数相乘再乘以最大数；
思路：找出最小的两个数以及最大的三个数；
找三个最小数，初始化：max1=a[0],max2=a[0],max3=a[0];
那么分三种情况：
1.值大于最大，依次更新次次大，次大，最大；
2.值大于次大，更新次次大，次大;
3.值大于次次大，更新次次大即可；
同理，找最小也是这个原理
```
	public static int maximumProduct(int[] nums) {
        int max1=Integer.MIN_VALUE,max2=Integer.MIN_VALUE,max3=Integer.MIN_VALUE;
		int min1=Integer.MAX_VALUE,min2=Integer.MAX_VALUE;
		for(int i=0;i<nums.length;i++){
			int n=nums[i];
			
			if(n<min1){
				min2=min1;
				min1=n;
			}else if(n<min2){
				min2=n;
			}
			
           if(n>max1){
        	   max3=max2;
        	   max2=max1;
        	   max1=n;
           }else if(n>max2){
        	   max3=max2;
        	   max2=n;
           }else if(n>max3){
        	   max3=n;
           }
			
        }
		int t1=max1*max2*max3;
		int t2=min1*min2*max1;
		return t1>t2 ?t1 :t2;
		// 改进：return Math.max(max1*max2*max3,min1*min2*max1);
    }
```
