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
1. 统计每个数字出现的频次;
2.对每个数字，分两种情况：
1.k=0,统计数字频率大于等于2的数字即可；
2.k>0，统计所有数字有多少数字加上k的值存在于该数据结构里面即可
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
### 有序数组寻找插入的位置：
思路：数组有序，那么二分查找是最快的，这实际上就是二分查找的变形：
```
if(target<nums[0]) return 0;
		 if(target>nums[nums.length-1]) return nums.length;
	        int low=0,high=nums.length-1;
	        while(low<=high){
	        	if(low==high) return low;
	        	int middle=(low+high)/2;
	        	if(nums[middle]==target) return middle;
	        	if(target>nums[middle]){
	        		if(target<nums[middle+1]){
	        			return middle+1;
	        		}else if(target>nums[middle+1]){
	        			low=middle+1;
	        		}else{
	        			return middle+1;
	        		}
	        	}else if(target<nums[middle]){
	        		if(target<nums[middle-1]){
	        				high=middle-1;
	        		}else if(target>nums[middle-1]){
	        			return middle;
	        		}else{
	        			return middle-1;
	        		}
	        	}
	        }
	        return -1;
	```
解法二：
```
int low=0,high=nums.length-1;
        while(low<=high){
            int middle=(low+high)/2;
            if(target==nums[middle]) return middle;
            if(target<nums[middle]){
                high=middle-1;
            }else{
                low=middle+1;
            }
        }
        return low;
```
## 查找出现频率最高的数字的首尾两次出现差值为最小的子数组的长度

思路：统计每个数字的频次，找到最大频次的数字数组，再依次找到最左和最右的下标的差值，找到差值的最小值；
时间复杂度O(n*m)，n为数组大小，m为频率最高的数字数组的大小，最坏情况会去到O(n*n)；
```
 Map<Integer,Integer> numtimes=new HashMap<Integer,Integer>();
	        for(int i=0;i<nums.length;i++){
	            numtimes.put(nums[i],numtimes.getOrDefault(nums[i],0)+1);
	        }
	        int max=Integer.MIN_VALUE;
	        for(Map.Entry<Integer,Integer> node: numtimes.entrySet()){
	        	//System.out.println(node.getKey()+":"+node.getValue());
	            if(node.getValue()>max){
	                max=node.getValue();
	            }
	        }
	        //System.out.println(max);
	        List<Integer> maxlist=new ArrayList<Integer>();
	        for(Map.Entry<Integer,Integer> node: numtimes.entrySet()){
	            if(node.getValue()==max){
	                maxlist.add(node.getKey());
	            }
	        }
	        //System.out.println(maxlist);
	        int minlen=Integer.MAX_VALUE;
	        int front=0,last=0;
	        for(Integer t: maxlist){
	        	for(int i=0;i<nums.length;i++){
	        		if(nums[i]==t){
	        			front=i;
	        			break;
	        		}
	        	}
	        	for(int j=nums.length-1;j>0;j--){
	        		if(nums[j]==t){
	        			last=j;
	        			break;
	        		}
	        	}
	        	minlen=Math.min(minlen,last-front+1);
	        }
	       return minlen; 
```
用O(3*n)的空间保存每个数字的最左出现和最右出现的下标，以及每个数字的出现频次，然后根据最大频次出现的数字找最小的子数组长度（最左出现和最右出现的下标的差值）;
```
Map<Integer,Integer> left=new HashMap<Integer,Integer>();
	        Map<Integer,Integer> right=new HashMap<Integer,Integer>();
	        Map<Integer,Integer> counts=new HashMap<Integer,Integer>();
	        for(int i=0;i<nums.length;i++){
	            int n=nums[i];
	            if(left.get(n)==null) left.put(n,i);
	            right.put(n,i);
	            counts.put(n,counts.getOrDefault(n,0)+1);
	        }
	        //System.out.println(counts);
	        int maxcnt=Integer.MIN_VALUE;
	        int minlen=Integer.MAX_VALUE;
	        for(Map.Entry<Integer,Integer> ct: counts.entrySet()){
	        	if(ct.getValue()>maxcnt){
	        		maxcnt=ct.getValue();
	        		minlen=right.get(ct.getKey())-left.get(ct.getKey())+1;
	        	}else if(ct.getValue()==maxcnt){
	        		minlen=Math.min(minlen,right.get(ct.getKey())-left.get(ct.getKey())+1);
	        	}  
	        }
	        return minlen;
```