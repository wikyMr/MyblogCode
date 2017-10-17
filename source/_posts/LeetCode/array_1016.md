---
title: leetCode之数组专题
tags: [leetCode]
---
##数组专题之合并两个排序数组：合并两个已排序的数组
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

###这道题比较的奇葩：统计两个数字相差k的数字对的数目，要求每个数字对是唯一的，且下标为i，j与j，i算一对；
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
###所有数中的三个数相乘积最大
要知道，最大的数只可能是三个最大的数相乘或者最小的两个数相乘再乘以最大数；
思路：找出最小的两个数以及最大的三个数；
1.排序后取相应的值即可。时间NO(LGN),排序需要O(logn)空间;
2.
**
找三个最小数，初始化：max1=a[0],max2=a[0],max3=a[0];
那么分三种情况：
1.值大于最大，依次更新次次大，次大，最大；
2.值大于次大，更新次次大，次大;
3.值大于次次大，更新次次大即可；
**
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
##查找数组里0到n的丢失元素
##思路1：
交换nums[i]与nums[nums[i]]直到i==nums[i],当数字为n时，说明之前的其他元素已归位，退出循环;依次检查nums[i]与i是否相等；
##思路2：
很简单，它们的和为n*(n+1)/2，那么就是把数组的值加起来的和与n*(n+1)/2的差；
##思路3：
注：0^n=n;
a^b^b=a;
那么数组的所有数与下标i异或会得到两种情况：
1.a^b^b^a=0;=>缺少的是n
2.a^b^b^c=a^c=i^n,缺少的是i;
那么只要把结果^n即可得到所要的结果；
```
int result=0;
        for(int i=0;i<nums.length;i++){
			while(nums[i]!=i){
				int m=nums[i];
				if(m==nums.length){
					break;
				}
				int t=nums[m];
				nums[m]=nums[i];
				nums[i]=t;
			}
		}
		
		for(int i=0;i<nums.length;i++){
			if(nums[i]!=i){
		       return i;
			}
		}
        
		return nums.length;
```

###找数组里面的连续的k个数值的和最大
思路：
思路1.循环1->n-k,更新sum=a[i]+...+a[i+k],缺点就是重复计算很多；
```
 public static double findMaxAverage(int[] nums, int k) {
		 int n=nums.length;
		 if(k>n||k==0)return 0.0;
		 int max=Integer.MIN_VALUE;
		 int i=0;
	        for(i=0;i<nums.length-k+1;i++){
	        	int t=0;
	        	for(int j=0;j<k;j++){
	        		t+=nums[i+j];
	        	}
	        	if(t>max){
	        		max=t;
	        	}
	        }
	        System.out.println(i);
	        return max*1.0/k*1.0;
	    }
```
思路2.
在基于1的基础,sum其实可以这样得来：
1.初始化sum=a[0]+...+a[k-1]，就是前k个数；
2.那么第1到第k+1个数的和可以等于sum+=a[k+1]-a[0],等价于在原来的和里面加上a[k+1]-a[0]的差值就可以得到;
```
public static double findMaxAverage(int[] nums, int k) {
		 int n=nums.length;
		 if(k>n||k==0)return 0.0;
		 int sum=0;
		 for(int i=0;i<k;i++){
			 sum+=nums[i];
		 }
		 int max=sum;
		// System.out.println(sum);
	     for(int i=1;i<nums.length-k+1;i++){
	          sum+=(nums[i+k-1]-nums[i-1]);	
	          //System.out.println(sum);
	          max=Math.max(sum, max);
	     }
	        
	        return max*1.0/k*1.0;
	    }
```
思路3：
用一个长度为n的数组保存前i个数的和=sum[i-1]+nums[i]，相距为k的数字和为sum[i+k-1]-sum[i]即可得到；
```
public static double findMaxAverage2(int[] nums ,int k){
		 int[] sum=new int[nums.length];
		 sum[0]=nums[0];
		 for(int i=1;i<nums.length;i++){
			 sum[i]=sum[i-1]+nums[i];
		 }
		 int res=sum[k-1];
		 for(int i=k;i<sum.length;i++){
			 res=Math.max(res, sum[i]-sum[i-k]);
		 }
		 return res;
	 }
```
