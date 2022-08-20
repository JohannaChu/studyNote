# 数组

## 基础习题锻炼

### 二分查找

>给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。
>
>例如：左闭右闭区间
>
>```
>输入: nums = [-1,0,3,5,9,12], target = 9
>输出: 4
>解释: 9 出现在 nums 中并且下标为 4
>```
>
>```js
>var search = function(nums, target) {
>    let left = 0;
>    let right = nums.length - 1;
>    while(left <= right){   
>        let middle = Math.floor((right - left)/2) + left
>        if(nums[middle] > target){
>            right = middle -1;
>        }else if(nums[middle] < target){
>            left = middle +1;
>        }else{
>            return middle
>        }
>    }
>    return -1
>    
>};
>```

## 热题

### 1. 



### 2. 两数之和

>给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。
>
>例如：
>
>```
>输入：nums = [2,7,11,15], target = 9
>输出：[0,1]
>解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1]
>```

自己写的暴力解法：双层循环，复杂度为O(n^2)

```js
var twoSum = function(nums, target) {
    for(let i=0;i<nums.length;i++){
        for(let j=i+1;j<nums.length;j++){
            if(nums[i]+nums[j] === target){
                return [i,j]
            }
        }
    }
};
twoSum([2,7,11,15],9)
```

优化解法：利用HashMap，复杂度为O(n)，在循环的时候进行查找将HashMap的Key定位nums[i]，Value定义为i

```js
var twoSum = function(nums, target) {
	const map = new Map();
    for(let i=0;i<nums.length;i++){
        const diff = target - nums[i]; //计算差值
        if(!map.has(diff)){ //如果差值不在map中则意味着另一个元素还未进入map
            map.set(nums[i],i)  //把目前的key和value值录入，其中key是nums[i],value是i
    }else {
        return [map.get((diff)),i];  //如果差值已经存在则直接输出目前的一个i和存在map里面的diff的key
    }
   }
};
twoSum([2,7,11,15],9)
```

一些函数知识点：

- has(key):has()函数返回一个布尔值,表示某个键(key)是否在当前的Map实例中;
- set(key,value):set()函数设置键名key所对应的键值为value,该函数的返回值是当前的Map实例;
- get(key):get()函数返回key对应的键值,如果找不到对应的key,则返回undefined.



# 字符串

## 1. 无重复字符的最长子串

>给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度
>
>例如：
>
>```
>输入: s = "abcabcbb"
>输出: 3 
>解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
>
>输入: s = "bbbbb"
>输出: 1
>解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
>```

解法：使用滑动窗口和双指针，在滑动窗口滑动时相当于把所有子串提取出并且比较是否有重复项，时间复杂度是O(n)

讲解视频：https://www.bilibili.com/video/BV113411v7Ak?spm_id_from=333.337.search-card.all.click&vd_source=62f15965c1a32c0c4f9349c8ea68315b

```js
var lengthOfLongestSubstring = function(s) {
    let set = new Set();
    let left=right=length=maxlength=0;

    while(right<s.length){
        if(!set.has(s[right])){
            set.add(s[right]);
            length++;
            if(length>maxlength){
                maxlength = length;
            }
            right++;
        }else{
            while(set.has(s[right])){
                set.delete(s[left]);
                left++;
                length--;
            }
            set.add(s[right]);
            length++;
            right++;
        }
    }
    return maxlength;
};
```

## 2. 最长回文子串

>给你一个字符串 `s`，找到 `s` 中最长的回文子串。
>
>例如：
>
>```
>输入：s = "babad"
>输出："bab"
>解释："aba" 同样是符合题意的答案。
>
>输入：s = "cbbd"
>输出："bb"
>```

解法：简单快速的解法是中心扩散法，也可以使用动态规划；动态规划适用范围更广

```js
//中心扩散法
var longestPalindrome = function(s) {
    let max = ''

    for(let i=0; i< s.length; i++) {
        // 分奇偶， 一次遍历，每个字符位置都可能存在奇数或偶数回文
        helper(i, i)
        helper(i, i+1)
    }

    function helper(l, r) {
        // 定义左右双指针，满足条件由中心外扩
        while(l>=0 && r< s.length && s[l] === s[r]) {
            l--
            r++
        }
        // 拿到回文字符， 注意 上面while满足条件后多执行了一次，所以需要l+1, r+1-1=r
        const maxStr = s.slice(l + 1, r);
        // 取最大长度的回文字符
        if (maxStr.length > max.length) max = maxStr
    }
    return max //做完所有function之后以及for循环后才输出最后的max
};
```

动态规划方法：注意就按照以下五步方法解决动态规划问题

- dp数组的定义和下标表示的意思
- 递推公式
- dp数组如何初始化，初始化也需要注意（应该初始化为0还是1）
- 遍历顺序，比较考究，例如是先遍历背包还是物品
- （若出现问题）打印dp数组（打印dp数组，检查是否有问题，检验1 2 3 4）

```js
var longestPalindrome = function (s) {
    const n = s.length;
    const dp = new Array(n).fill(0).map(() => new Array(n).fill(0));
    let ans = s[0]; //因为最后输出的回文子串最小也是一个数字（当只有一个/两个字母时）
 
    //先确定对角线为1，1表示true，都是属于回文子串
    for(let i=0;i<n;i++){
        dp[i][i] = 1
    }

    //遍历顺序 一般都是两个for循环，因为j>i，所以二维数组只有上半部分有可能有值
    for(let j=1;j<n;j++){ 
        for(i=j-1;i >= 0; i--){
            //两种情况：只有两个且两个相同 / 大于两个并且外层相同，里层的也是回文子串
            if(s[i] === s[j] && (j-i === 1 || dp[i+1][j-1])) {
                dp[i][j] = 1 //记录其为回文子串
                if( ans.length < j-i+1){
                    ans = s.slice(i,j+1)
                }
            }
        }
    }
    return ans
}
```

