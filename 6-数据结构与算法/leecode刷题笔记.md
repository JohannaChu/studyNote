# 字符串

## 1. 两数之和

>给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。
>
>例如：输入：nums = [2,7,11,15], target = 9
>输出：[0,1]
>解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1]

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