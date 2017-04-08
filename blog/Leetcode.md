## Leetcode 题目

> 本文将不断更新，主要内容为解决Leetcode中的算法问题，有比较难的题目可能会单独成文档，所有代码都将用Javascript编写并且Accepted.标题后面的百分号代表用JS写的代码中该答案比多少比例的答案快。

> 对于一些解法不高效的题，欢迎留言写下你的答案。

> 最后编辑时间： 7/4/2017

<h5><a id="problem1" href="https://leetcode.com/problems/two-sum/#/description">1.Two Sum </a> (96%) </h5>

```javascript
      var twoSum=function(nums,target){
      var arr=new Array();

      for(var i=0;i<nums.length;i++){
        var j=target-nums[i];
        if(typeof arr[j]!='undefined'){
            return [arr[j],i];
        }else{
            arr[nums[i]]=i;
        }
      }
    };
```
这里arr数组起到了记录其中一个数的位置和值的作用，每当i探索新的数，如果该数无法和arr中的数相加成target，则将该数放进arr，直到找到能和arr中数组相加等于target的数。巧妙之处在于每次将i找到的数放进arr时，该数的值在arr中充当了位置，该数的位置在arr中充当了值，这种方法使用两个arr达成了在数组中寻找是否存在一个数的效果，而无需使用map。


##### [2.Add Two Numbers](https://leetcode.com/problems/add-two-numbers/#/description)(71%)
> [解法思路](https://segmentfault.com/a/1190000002986101)

```javascript
function ListNode(val) {
    this.val = val;
    this.next = null;
}


var addTwoNumbers=function(l1,l2){
        if(l1===null) return l2;
        if(l2===null) return l1;

        return helper(l1,l2,0);
  };

  var helper=function(l1,l2,temp){
      if(l1===null&&l2===null){
          if(temp==0){
              return null;
          }else{
              return new ListNode(temp);
          }
      }
      if(l1===null&&l2!==null){
          l1=new ListNode(0);
      }
      if(l1!==null&&l2===null){
          l2=new ListNode(0);
      }
      var sum=l1.val+l2.val+temp;
      var l3=new ListNode(sum%10);
      l3.next=helper(l1.next,l2.next,parseInt(sum/10));
      return l3;
  };
```

##### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/#/description)

>我的答案 （容易理解但效率不高） （15%）

```javascript
var lengthOfLongestSubstring = function(s) {
    if(s.length<2) return s.length;

    var maxCount=0;
    var sub=[];

    for(var i=0;i<s.length;i++){
        var judge=isExist(sub,s[i]);
        if(judge==-1){
            sub.push(s[i]);
        }else{
            i=i-sub.length+judge;
            var sub=[];
        }

        if(sub.length>maxCount){
            maxCount=sub.length;
        }
    }
    return maxCount;
};

var isExist=function(arr,num){
    for(var i=0;i<arr.length;i++){
        if(num==arr[i]){
            return i;
        }
    }
    return -1;
};
```

> 由第一题延伸出的另一种思路,由该题高分java答案改编而来。（48%）

```javascript
var lengthOfLongestSubstring = function(s) {
  if(s.length<2) return s.length;

    var max=0;
    var sub=[];

    for(var i=0,j=0;i<s.length;i++){
        j= typeof sub[s.charAt(i)] != 'undefined'?Math.max(j,sub[s.charAt(i)]+1):j ;
        sub[s.charAt(i)]=i;
        max=Math.max(max,i-j+1);
    }
    return max;
};
```
个人觉得这个答案很不错了，但我不知道为什么还是只超过了46%。还可以改进的地方是这块：
```javascript
       j=typeof sub[s.charAt(i)] != 'undefined'?Math.max(j,sub[s.charAt(i)]+1):j ;
```
如果能把这行代码直接表示成一个j=多少的表达式，应该速度能更快一些。但我不知道如何直接处理这里的'undefined'，能够达到以下效果：当在sub数组中找不到``s.charAt(i)``的值时，能直接用 -1 来取代返回的 'undefined'，如果能将返回值直接转成-1 ,那么j就会直接取j的值，我们也不用先判断再赋值了。

*******

29/3/2017更新：重新安排一下做题顺序，现在开始会分类做，而不是原计划的按照题号做，先把Array部分做完

### Tag: Array

##### [1.Two Sum  ](#problem1) (96%)
##### [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/#/description)  (88%) (该题前几天在同学的阿里面试中被问到)
```javascript
  var findMedianSortedArrays = function(nums1,nums2) {
    var median=0;
    var m=nums1.length; //m,n代表数组长度
    var n=nums2.length;
    var i=0;

    if(isNull(nums1)){
        return findMedianOneArray(nums2);
    }
    if(isNull(nums2)){
        return findMedianOneArray(nums1);
    }

    if((m+n)%2==0){
        while(i<(m+n)/2-1){
          if(isNull(nums1)){
            nums2.shift();
          }else if(isNull(nums2)){
            nums1.shift()
          }else {
            nums1[0]<nums2[0]? nums1.shift():nums2.shift() ;
          }
          i++;
        }

        var left,right;

        if(isNull(nums1)){
          left=nums2.shift();
        }else if(isNull(nums2)){
          left=nums1.shift();
        }else {
          left=nums1[0]<nums2[0]? nums1.shift():nums2.shift() ;
        }

        if(isNull(nums1)){
          right=nums2.shift();
        }else if(isNull(nums2)){
          right=nums1.shift();
        }else {
          right=nums1[0]<nums2[0]? nums1.shift():nums2.shift() ;
        }

        median=(left+right)/2;
    }else{
        while(i<(m+n-1)/2){
          if(isNull(nums1)){
            nums2.shift();
          }else if(isNull(nums2)){
            nums1.shift();
          }else {
            nums1[0]<nums2[0]? nums1.shift():nums2.shift();
          }
          i++;
        }

        if(isNull(nums1)){
          median=nums2.shift();
        }else if(isNull(nums2)){
          median=nums1.shift();
        }else {
          median=nums1[0]<nums2[0]? nums1.shift():nums2.shift() ;
        }
    }

    return median;
};

//在一个数组内寻找中位数
var findMedianOneArray=function(num){
    var l=num.length;
    if(l==0){
        return null;
    }
    if(l%2==0){
        return (num[l/2-1]+num[l/2])/2;
    }else{
        return num[(l-1)/2];
    }
};

//判断数组是否为空
var isNull=function(num){
  if(num.length>0){
    return false;
  }else {
    return true;
  }
};
```
上面的代码看起来比较复杂是因为加入了很多判断，需要判断很多特殊情况，比如数组为空的时候，实际上思路是很清晰的。因为要获得两个数组合并以后的中位数，无论合并以后排序如何，中位数的位置是不变的。当两个数组长度相加为奇数时，中位数永远位于``(m+n+1)/2``，两个数组长度相加为偶数时，中位数永远是位于``(m+n)/2``和``((m+n)/2)+1``两者的平均数。所以每次都比较两个数组首位的大小，把小的去掉，然后再继续比较，一直循环到中位数所在的位置，此时把中位数取出来即可。代码写的有点杂乱，有时间会把写法改一下，思路本身的速度已经不错了。


##### [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/#/description)  (74%)
```javascript
  var maxArea = function(height) {
    var left=0;
    var right=height.length-1;
    var maxSize=0;

    while(left<right){
        var size=height[left]<height[right]? height[left]*(right-left) : height[right]*(right-left) ;
        if(size>maxSize){
            maxSize=size;
        }

        if(height[left]<height[right]){
            left++;
        }else{
            right--;
        }
    }
    return maxSize;
  };
```
这个解法思路比较清晰，在数组的首尾分别安插一个指针然后往中间移直到遇到为止。移动过程中只需要明确一点:参照木桶原理，**当容器左边的高度小于右边的高度时，右边的位置往左移动所产生的面积将永远小于移动之前的面积；同理，当容器右边的高度小于左边的高度时，右边的位置不动，左边的位置往右移动所产生的面积将永远小于移动之前的面积**。只要理解这一点，就理解了这道题。


##### [15. 3Sum](https://leetcode.com/problems/3sum/#/description) (68%)
> 答案改编自高分java答案

```javascript
var threeSum = function(nums) {

    nums.sort(function(a,b){
         return a-b;
     });
//将算法从小到大排序
    var result=new Array();

    for(var i=0;i<nums.length-2;i++){             //1
       if(i==0||(i>0&&nums[i]!=nums[i-1])){
           var left=i+1;                      //2
           var right=nums.length-1;
           var sum=0-nums[i];
           while(left<right){
               if(nums[left]+nums[right]==sum){
                   result.push([nums[i],nums[left],nums[right]]);

                   while(left<right&&nums[left]==nums[left+1]) {  //3
                       left++;
                   }
                   while(left<right&&nums[right]==nums[right-1]){ //4
                       right--;
                   }
                   left++;  
                   right--;  //5
               }else if(nums[left]+nums[right]<sum){
                   left++;            //6
               }else{
                   right--;           //7
               }
           }
       }
    }

    return result;
 }
```
**首先记住数组是排了序的！！**
1. 因为nums[i]是三个数中最左边也是最小的数，所以最多只取到倒数第三位.
2. left和right分别代表除了i以外另外两个数的位置，nums[left]<nums[right].
3. 如果left右边的第一个数和现在的数相等，则直接往右跳过。
4. 与上同理
5. 现在left和right的位置已经满足 ``nums[left]+nums[right]=sum`` 的情况，在接下来移动后获得的数和现在不同的情况下，left往右跳一个位置或者right往左跳一个位置后``nums[left]+nums[right]``必定不可能依旧等于sum，因为left右边的数必定更大，而right左边的数必定比现在更小.所以,要left和right一起同时移动来寻找下一个可能的结果.
6. nums[left]+nums[right]<sum 等价于 nums[left]+nums[right]+nums[i]<0 ，i不变的情况下，需要将left往右移来寻找更大的nums[left],这样才可能找到一个数使nums[left]+nums[right]+nums[i]=0
7. 与上同理

##### [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/#/description) (90%)
```javascript
var threeSumClosest = function(nums, target) {
      nums.sort(function(a,b){
          return a-b;
      })

      var result;
      var minDiff=10000;

      for(var i=0;i<nums.length-2;i++){
              var left=i+1;
              var right=nums.length-1;
              while(left<right){
                  var sum=nums[i]+nums[left]+nums[right];
                  if(Math.abs(target-sum)<minDiff){
                      result=sum;
                      minDiff=Math.abs(target-sum);

                      if(minDiff==0){
                          return result;
                      }
                  }

                  target<sum? right--:left++;
              }
      }
      return result;
  };
```
这道题和上一题有异曲同工之妙，最关键的思路几乎是一模一样的，都是先确定一个数，然后再按照2sum的方法来探索剩下两个数。可以用这道题来检测一下自己对上一题的思路是否真正理解。如果对于这道题还是有不明白的地方可以留言。


##### [18. 4Sum](https://leetcode.com/problems/4sum/#/description) (63%)

```javascript
var fourSum = function(nums, target) {
      nums.sort(function(a,b){
        return a-b;
      })
      var result=new Array();

      for(var i=0;i<nums.length-3;i++){
          if(i>0&&nums[i]==nums[i-1]) continue;
          for(var j=i+1;j<nums.length-2;j++){
              if(j>i+1&&nums[j]==nums[j-1]) continue;
              var left=j+1;
              var right=nums.length-1;
              var sum=target-nums[i]-nums[j];
              while(left<right){
                 if(nums[left]+nums[right]==sum){
                     result.push([nums[i],nums[j],nums[left],nums[right]]);

                     while(left<right&&nums[left]==nums[left+1]) {  
                         left++;
                     }
                     while(left<right&&nums[right]==nums[right-1]){
                         right--;
                     }
                     left++;  
                     right--;  
                 }else if(nums[left]+nums[right]<sum){
                     left++;            
                 }else{
                     right--;           
                 }
              }
          }
      }

      return result;
  };
```
这道题的解法也是跟随了3sum的解法，建议上面3题一起看。

##### [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/#/description)(33%）
```javascript
  var removeDuplicates = function(nums) {
    for(var i=0;i<nums.length;i++){
        if(nums[i]==nums[i+1]){
            nums.splice(i,1);
            i--;
        }
    }
  };
```

##### [27. Remove Element](https://leetcode.com/problems/remove-element/#/description) (87%)
```javascript
  var removeElement = function(nums, target) {
    for(var i=0;i<nums.length;i++){
        if(nums[i]==target){
            nums[i]=nums[0];
            nums.shift();
            i--;
        }
    }
  };
```
这里我没用splice，因为效率很低。这里当发现有一个数字和target相同时,直接把第一个数字的值覆盖这个与target相同的值，然后将第一个数字删掉。

##### [34. Search for a Range](https://leetcode.com/problems/search-for-a-range/#/description) (61%)
```javascript
var searchRange = function(nums, target) {
    var left=0;
    var right=nums.length-1;
    var i=-1;
    var j=-1;

    while(left<=right&&(i==-1||j==-1)){
        nums[left]==target?i=left:left++;
        if(i!=-1){
            j=i+1;
            while(nums[j]==target){
                j++;
            }
        }
    }
    return j==-1?[i,j]:[i,j-1];
};
```
因为已经排了序，找到左边那个以后就直接从左边的位置往后找就能找到右边的target的位置了。理论上用二叉树做更快，但不知道为什么，用二叉树试了几次都超时了，如果有网友能有js的二叉树解法，欢迎留言。


## 链接
* [Leetcode算法题](https://leetcode.com/problemset/algorithms/)
