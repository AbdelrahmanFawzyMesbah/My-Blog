---

layout: post
title: 13/1/2023 Merge sorted array leetcode

---

Hi i started my day by solving easy leetcode problem with a friend

[Merge Sorted Array - LeetCode](https://leetcode.com/problems/merge-sorted-array/description/)

you are given 2 non decreasing arrays `nums1` and n`ums2` and want to merge them into a single array `nums1`

The final sorted array should not be returned by the function, but instead be *stored inside the array `nums1`

My solution was to add nums2 to nums1 then sort xD

My solution

```cpp
class Solution {
public:
 void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
 if(m==0&&n==1)
 nums1[0]=nums2[0];

for(int i = m+n-1,j=n-1;i>=m-1&&j>=0;i--,j--)
 {
 nums1[i]=nums2[j];

}
 sort(nums1.begin(),nums1.end());
 }
};
```

![](https://i.postimg.cc/N0cbdJYT/image.png)

it beats 74.47% codes from other people

my friend came with this solution

```cpp
class Solution {
public:
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
    for (int i = m - 1, j = n - 1; j + i + 1 >= 0 ;) {
        if (j < 0 || nums1[i] >= nums2[j]) {
            nums1[i + j + 1] = nums1[i];
            i--;
        }
        else  {
            nums1[i + j + 1] = nums2[j];
            j--;
        }
    }
}
};
```

But he gets error from leetcode but when he test his code in Local compilier and test it with some test cases he gets the desired result

so i thinking why his code was wrong

my thinking process was it give error because some index became negative so i was thinking that m=n=0 case but it will not run the loop becosue of condition j+i+1>=0

-1 -1+1>=0 will be wrong

hmmm i got an idea if

`nums1=[1,2,3,0,0]
m=3
nums2[-10,10]
n=2`

and `cout<<i<<" "<<i<<endl; `

give

`i j
 2 1
 2 0
 1 0
 0 0
-1 0`

so when i =-1 and try condition of loop 0+-1+1>=0 give true

so when code go to this line

```cpp
 if (j < 0 || nums1[i] >= nums2[j]) {
```

i =-1 so nums1[-1] give this error so if condition should changed to

```cpp
 if (i>=0 (j < 0 || nums1[i] >= nums2[j])) {
```

![](https://i.postimg.cc/LsKQr4kJ/image.png)
