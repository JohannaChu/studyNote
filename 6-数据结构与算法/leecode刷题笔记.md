

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
```js
var search = function(nums, target) {
let left = 0;
let right = nums.length - 1;
while(left <= right){   
   let middle = Math.floor((right - left)/2) + left
   if(nums[middle] > target){
       right = middle -1;
   }else if(nums[middle] < target){
       left = middle +1;
   }else{
       return middle
   }
}
return -1

};
```





### 移除元素

>给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
>
>不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
>
>元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
>
>```
>输入：nums = [3,2,2,3], val = 3
>输出：2, nums = [2,2]
>解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
>```
>

移除元素用到的做法是快慢指针，其中快指针表示新数组的元素，慢指针是新数组的下标

- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

```js
//时间复杂度O(n),空间复杂度O(1)，类似js数组delete方法接口的时间复杂度也是O(n)
var removeElement = function(nums, val) {
    let slow = 0; //slow是慢指针
    for(let i = 0;i < nums.length; i++){ //这里的i其实代表快指针front
        if(nums[i] !== val){
            nums[slow] = nums[i]
            slow++; //这句和上面的代码也可以整合写成nums[slow++] = nums[i]
        }
    }
    return slow
};
```

### 有序数组的平方

>使用双指针的方法，一般的while里面的条件和两个指针的位置有关
>
>给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序
>
>```
>输入：nums = [-4,-1,0,3,10]
>输出：[0,1,9,16,100]
>解释：平方后，数组变为 [16,1,0,9,100]
>排序后，数组变为 [0,1,9,16,100]
>```

```js
//双指针方法
var sortedSquares = function(nums) {
    let n = nums.length;
    let res = new Array(n).fill(0); //新数组
    let i = 0, j = n - 1, k = n - 1;  //i相当于左指针，j相当于右指针，k是新数组的指针
    while (i <= j) { //一定要等于，否则会错过等于时的元素
        let left = nums[i] * nums[i],
            right = nums[j] * nums[j];
        if (left < right) {
            res[k] = right;
            k--; //res[k] = right;和k--; 可以结合写成res[k--] = right;
            j--;
        } else {
            res[k] = left;
            k--;
            i++;
        }
    }
    return res;
};



//api方法：注意，这里使用map方法当前所选元素是必填项，例如 nums=[1,2,3] nums.map(parseInt)=[1,NaN,NaN]
//平方可以用Math.pow(x,2)  sort在排序时如果没有具体传入排序规则默认是按照ASCII顺序排序，即使是数组中是数字，也会转化成字符串按照ASCII排序，因此排序出来的结果不是正确结果
// (sort排序方法：https://m.php.cn/article/480049.html)
var sortedSquares = function(nums) {
    let newMums = nums.map((e) => e*e)
    function f(a,b){ //从小到大，从大到小则是-(a-b)
        return (a-b)
    }
    return newMums.sort(f)
};
```

### 长度最小的子数组

>给定一个含有 n 个正整数的数组和一个正整数 target 。
>
>找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。
>
>```
>输入：target = 7, nums = [2,3,1,2,4,3]
>输出：2
>解释：子数组 [4,3] 是该条件下的长度最小的子数组。
>
>输入：target = 11, nums = [1,1,1,1,1,1,1,1]
>输出：0
>```

```js
var minSubArrayLen = function(target, nums) {
    let i=sum=sublength=0; //i为起始位置，j为终止位置
    //result初始值一定是最大值，而最大值是length+1而不是length，这里的result可以等于Infinity
    let result = nums.length+1  
    for(let j=0;j<nums.length;j++){
        sum += nums[j]
        while(sum>=target){
            sublength = j-i+1   //相当于一个中间缓存值
            result=sublength<result? sublength:result
            sum -= nums[i]
            i++ 
        }
    }
    return result==nums.length+1? 0:result 
    //如果result为Infinity则这里也要判断是否等于Infinity，相当于没有进入过第二个while里面
};

```





## 热题

### 34. 在排序数组中查找元素的第一个和最后一个位置

>给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。
>
>如果数组中不存在目标值 target，返回 [-1, -1]。你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。
>
>```
>输入：nums = [5,7,7,8,8,10], target = 8
>输出：[3,4]
>
>输入：nums = [5,7,7,8,8,10], target = 6
>输出：[-1,-1]
>```

```js
api方法 //indexOf的方法，不止可以用于string，也可以用于数组
var searchRange = function(nums, target){
    if(nums.length <= 0){
        return [-1, -1];
    }
   let first = nums.indexOf(target);
   let last = nums.lastIndexOf(target);
   return [first,last];
}
```



### 283. 移动零

>给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
>```
>输入: nums = [0,1,0,3,12]
>输出: [1,3,12,0,0]
>
>输入: nums = [0]
>输出: [0]
>```

```js
//直接移动非零值，剩下的全部置为0
var moveZeroes = function(nums) {
    let slow = 0
    for(let i=0;i<nums.length;i++){
        if(nums[i] !== 0){
            nums[slow] = nums[i]
            slow++
        }
    }
     for(let j=slow;j<nums.length;j++){
         nums[j] = 0
    }
    return nums
};

//也可以一次排序，原理是中间交换
var moveZeroes = function(nums) {
    let slow = 0,tem=0;
    for(let i=0;i<nums.length;i++){
        if(nums[i] !== 0){
            tem = nums[slow]
            nums[slow] = nums[i]
            nums[i] = tem
            slow++
        }
    }
    return nums
};
如果看不懂可以看：https://www.bilibili.com/video/BV1EB4y1Q7qV?spm_id_from=333.337.search-card.all.click
里面包含了两种方法
```



## 练习题

### 904. 水果成篮

> 你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 fruits 表示，其中 fruits[i] 是第 i 棵树上的水果 种类 。
>
> 你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：
>
> 你只有 两个 篮子，并且每个篮子只能装 单一类型 的水果。每个篮子能够装的水果总量没有限制。
> 你可以选择任意一棵树开始采摘，你必须从 每棵 树（包括开始采摘的树）上 恰好摘一个水果 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
> 一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。
> 给你一个整数数组 fruits ，返回你可以收集的水果的 最大 数目
>
> ```
> 输入：fruits = [1,2,1]
> 输出：3
> 解释：可以采摘全部 3 棵树。
> 
> 输入：fruits = [0,1,2,2]
> 输出：3
> 解释：可以采摘 [1,2,2] 这三棵树。
> 如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。
> ```
>
> 教程：
>
> https://www.bilibili.com/video/BV1d7411Q7vx?spm_id_from=333.337.search-card.all.click&vd_source=62f15965c1a32c0c4f9349c8ea68315b

这道题用的是滑动窗口配合在map存储的最后key和value判断是否大于两个种类的水果（key代表2个不重复时出现的树对应的值，value代表不重复时最后出现（因为是在不断更新的）对应的下标）

```js
var totalFruit = function(fruits) {
    const map=new Map()
    let max = 1; //当只有一棵树的时候最大为1
    let j=0; //起始位置
    for(let i=0;i<fruits.length;i++){ //i代表终止位置
        map.set(fruits[i],i); //存入map中并且key为数值，value为实时更新最后出现的下标值
        if(map.size>2){
            let minIndex = Infinity;
            //移除较小的那个数值
            for(let [fruits,index] of map){
                if(index<minIndex){
                    minIndex = index
                }
            }
            map.delete(fruits[minIndex])
            j=minIndex+1
        }
        max = Math.max(max,i-j+1) //每次都比较哪个比较大
    }
    return max
};
```



## 总结

![数组总结](D:\notesfor\6-数据结构与算法\asseet\数组总结.png)



# 链表

## 基础习题锻炼

### 移除链表

>给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点**

```js
var removeElements = function(head, val) {
    //定义一个虚拟头节点
    var dummyHead = new ListNode(0,head)
    var tempNode = dummyHead 
    //定义一个临时指针来遍历，如果直接使用头节点遍历的话头节点不断改变最后无法获得原本的头节点
    //头节点指向dummyHead而不是dummyHead.next的原因在于如果指向的是next则在删除时无法获得上一个链表的值，无法删除
    while(tempNode.next){ //指向实际上的头节点
        if(tempNode.next.val === val){ //是next.val因为只有next.val才知道上一个链表的位置
            tempNode.next = tempNode.next.next
            continue //这里一定要有continue,否则会报错；因为不执行tempNode = tempNode.next这句
        }
        tempNode = tempNode.next
    }
    return dummyHead.next
};
```



### 设计链表

>设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。
>
>在链表类中实现这些功能：
>
>get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
>addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
>addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
>addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
>deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

```js
// //使用class关键字定义一个类，本质上是一个function，定义了同一组对象（又称为实例）共有的属性和方法

// //constructor方法是类的默认方法，通过new生成实例对象的时候就会被调用，一个类必须有constructor方法，
// //如果没有显式定义，一个空的constructor就会被创建
// //一个类只有一个constructor方法，里面专门用于创建和初始化一个由class创建的对象

// //类里面共有的属性和方法必须要添加this使用
// //constructor 里面的 this 指向的是创建的实例对象
class LinkNode {
    //定义节点
    constructor(val, next) {
        this.val = val;
        this.next = next;
    }
}

/**
 * Initialize your data structure here.
 * 单链表 储存头尾节点 和 节点数量
 */
var MyLinkedList = function() {
    this.size = 0;
    this.tail = null;
    this.head = null;
};

/**
 * Get the value of the index-th node in the linked list. If the index is invalid, return -1. 
 * @param {number} index
 * @return {number}
 */
//定义一个getNode可以方便下面获取index下的节点
MyLinkedList.prototype.getNode = function(index) {
    if(index < 0 || index >= this.size) return null;
    // 创建虚拟头节点
    let cur = new LinkNode(0, this.head);
    // 0 -> head
    while(index>=0) { //这里要有等于0的情况
        cur = cur.next;
        index--;
    }
    return cur;
};
MyLinkedList.prototype.get = function(index) {
    if(index < 0 || index >= this.size) return -1;
    // 获取当前节点
    return this.getNode(index).val;
};

/**
 * Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtHead = function(val) { //这里的第一个元素表示头节点
    const node = new LinkNode(val, this.head);
    this.head = node; //新节点作为新的头节点
    this.size++; //节点数加1
    if(!this.tail) {
        this.tail = node;
    }
};

/**
 * Append a node of value val to the last element of the linked list. 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtTail = function(val) {
    const node = new LinkNode(val,null)
    this.size++;
    if(this.tail){
        this.tail.next = node; //交换位置
        this.tail = node;
        return;
    }
    this.tail = node;
    this.head = node;
};


/**
 * Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. 
 * @param {number} index 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtIndex = function(index, val) {
    // 当index > size,索引越界，不能插入节点
    if(index > this.size) return;
     // 当index <= 0,新插入的节点为头节点
    if(index <= 0) {
        this.addAtHead(val);
        return;
    }
    if(index === this.size) {
        this.addAtTail(val);
        return;
    }
    // 获取目标节点的上一个的节点
    const node = this.getNode(index - 1);
    node.next = new LinkNode(val, node.next);
    this.size++;
};

/**
 * Delete the index-th node in the linked list, if the index is valid. 
 * @param {number} index
 * @return {void}
 */
MyLinkedList.prototype.deleteAtIndex = function(index) {
    //判断index是否在存在的节点之内
    if(index < 0 || index >= this.size) return;
    if(index === 0) {
        this.head = this.head.next;
        // 如果删除的这个节点同时是尾节点，要处理尾节点
        if(index === this.size - 1){
            this.tail = this.head
        }
        this.size--;
        return;
    }
    // 获取目标节点的上一个的节点
    const node = this.getNode(index - 1);    
    node.next = node.next.next;
    // 处理尾节点
    if(index === this.size - 1) {
        this.tail = node;
    }
    this.size--;
};
```



### 反转链表

>给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表
>
>```
>输入：head = [1,2,3,4,5]
>输出：[5,4,3,2,1]
>```

```js
var reverseList = function(head) {
    if(!head || !head.next) return head;
    var cur = head; //双指针
    var pre = null; //pre指向虚拟头，即head的前一个，即null
    var temp = null //定义一个临时链表指向cur.next
    while(cur){ //里面的条件相当于当cur指向null的时候
        temp = cur.next //得先保存cur的下一个节点位置以便cur移动时可以赋值
        cur.next = pre  //反转
        pre = cur //往前走
        cur = temp
    }
    return pre
};
```



### 两两交换链表中的节点

>给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
>
>```
>输入：head = [1,2,3,4]
>输出：[2,1,4,3]
>```

```js
var swapPairs = function(head) {
    var dummpHead = new ListNode(0,head)
    var cur = dummpHead
    while(cur.next && cur.next.next){
        //这里代表偶数时cur的下一个为null时跳出循环；或者奇数时cur.next.next为null时退出循环
        var temp = cur.next  //这句和下一句一定要放在循环里面，因为temp和temp1是每次都会改变的
        var temp1 = cur.next.next.next
        cur.next = cur.next.next;
        cur.next.next = temp;
        temp.next = temp1;  //这句也可改为cur.next.next.next = temp1
        cur = cur.next.next;
    }
    return dummpHead.next
};
```



### 删除链表的倒数第n个节点

>给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。
>
>```
>输入：head = [1,2,3,4,5], n = 2
>输出：[1,2,3,5]
>
>输入：head = [1], n = 1
>输出：[]
>```

```js
var removeNthFromEnd = function(head, n) {
    var dummyHead = new ListNode(0,head);
    var fast = dummyHead;
    var slow = dummyHead; //快慢指针
    while(n+1){
        fast = fast.next;
        n--;
    }
    while(fast){ 
        //当快指针等于null时，也就是指向最后尾节点null(null相当于0,也就是false，会自动跳出循环)
        //其他不为0的时候为true，会执行while里面的内容
         fast = fast.next;
         slow = slow.next;
    }
    //删除节点
    slow.next= slow.next.next
    return dummyHead.next
};
```



### 环形链表

>给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
>
>如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
>
>```
>输入：head = [3,2,0,-4], pos = 1
>输出：返回索引为 1 的链表节点
>解释：链表中有一个环，其尾部连接到第二个节点。
>
>输入：head = [1,2], pos = 0
>输出：返回索引为 0 的链表节点
>解释：链表中有一个环，其尾部连接到第一个节点。
>```

```js
var detectCycle = function(head) {
    var fast = slow = head
    while(fast && fast.next){ 
        //判断fast节点以及其下一位是否为null
        //如果没有fast.next则当其为null时执行fast.next.next会报错
        fast = fast.next.next;
        slow = slow.next;
        if(fast === slow){
            var index1 = fast;
            var index2 = head;
            while(index1 !== index2){
                index1 = index1.next;
                index2 = index2.next
            }
            return index1;
        }
    }
    return null;
};
```



## 总结

### 链表的基础知识

- 链表的种类主要为：单链表，双链表，循环链表
- 链表的存储方式：链表的节点在内存中是分散存储的，通过指针连在一起

![链表总结](D:\notesfor\6-数据结构与算法\asseet\链表总结.png)



# 哈希表

> **当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**

## 基础习题锻炼

### 有效的字母异位词

>给定两个字符串 `s` 和 `t` ，编写一个函数来判断 `t` 是否是 `s` 的字母异位词
>
>```
>输入: s = "anagram", t = "nagaram"
>输出: true
>
>输入: s = "rat", t = "car"
>输出: false
>```

```js
//方法一
var isAnagram = function(s,t){
    function getLocation(str){
        let arr = Array(26).fill(0);
        let base = 'a'.charCodeAt();
        for(let i=0;i<str.length;i++){
            ascii = str[i].charCodeAt() - base;
            arr[ascii]++;
        }
        return arr.join('')
    }
    return getLocation(s) === getLocation(t)
}
//方法二
var isAnagram = function(s, t) {
    if(s.length !== t.length) return false;
    const resSet = new Array(26).fill(0);
    const base = "a".charCodeAt();
    for(const i of s) {
        resSet[i.charCodeAt() - base]++;
    }
    for(const i of t) {
        if(!resSet[i.charCodeAt() - base]) return false;
        resSet[i.charCodeAt() - base]--;
    }
    return true;
};
```



### 两个数组的交集

>给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序**
>
>```
>输入：nums1 = [1,2,2,1], nums2 = [2,2]
>输出：[2]
>
>输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
>输出：[9,4]
>解释：[4,9] 也是可通过的
>```

```js
//使用set做去重操作
var intersection = function(nums1, nums2) {
    var set = new Set(nums1)
    var res = new Set()
    for(let i=0;i<nums2.length;i++){
        if(set.has(nums2[i])){
            res.add(nums2[i])
        }
    }
    return [...res];  //这里不能直接return res,因为无法return一个set，只能把它转成数组输出
};
```



### 快乐数

>编写一个算法来判断一个数 n 是不是快乐数。
>
>「快乐数」 定义为：
>
>对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
>然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
>如果这个过程 结果为 1，那么这个数就是快乐数。
>如果 n 是 快乐数 就返回 true ；不是，则返回 false 。
>
>```
>输入：n = 19
>输出：true
>解释：
>1^2 + 9^2 = 82
>8^2 + 2^2 = 68
>6^2 + 8^2 = 100
>1^2 + 0^2 + 0^2 = 1
>```

```js
function happyNums(n){
    var sum = 0;
    var arr = n.toString().split('');
    for(let i=0;i<arr.length;i++){
        sum += arr[i] * arr[i]
    }
    return sum
}
var isHappy = function(n) {
    let set = new Set()
    while(n !== 1 && !set.has(n)){
        set.add(n)
        n = happyNums(n)
    }
    return n === 1;
};
```



### 两数之和

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



### 四数相加

>给你四个整数数组 nums1、nums2、nums3 、 nums4 ，数组长度都是 n ，请计算多少个元组 (i, j, k, l) 能满足：
>- 0 <= i, j, k, l < n
>- nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
>
>```
>输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
>输出：2
>解释：
>两个元组如下：
>1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
>2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
>```

```js
//自己的写法：
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    var map = new Map();
    for(let i=0;i<nums1.length;i++){
        for(let j=0;j<nums2.length;j++){
            var sum = nums1[i] + nums2[j]
            var count = 1;
            if(!map.has(sum)){ //如果元素不存在的时候
                map.set(sum,count)
            }else{
                var temp = map.get(sum)
                temp = temp + 1;
                map.set(sum,temp)
            }
        }
    }

    var sum = 0;
    for(let i=0;i<nums3.length;i++){
        for(let j=0;j<nums4.length;j++){
            var diff =  0 - (nums3[i] + nums4[j])
            if(map.has(diff)){ //如果里面有存在的话
                sum = sum + map.get(diff)
            }
        }
    }
    return sum
};

//更简洁的写法：
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    const twoSumMap = new Map();
    let count = 0;

    for(const n1 of nums1) {
        for(const n2 of nums2) {
            const sum = n1 + n2;
            twoSumMap.set(sum, (twoSumMap.get(sum) || 0) + 1)
        }
    }

    for(const n3 of nums3) {
        for(const n4 of nums4) {
            const sum = n3 + n4;
            count += (twoSumMap.get(0 - sum) || 0)
        }
    }

    return count;
};
```



### 三数之和

>给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。
>注意：答案中不可以包含重复的三元组。
>
>```
>输入：nums = [-1,0,1,2,-1,-4]
>输出：[[-1,-1,2],[-1,0,1]]
>
>输入：nums = [0,1,1]
>输出：[]
>```

难点在于去重，去重就不适用于哈希表的方法，因此在这里使用双指针的方法

```js
var sortFunction = function(a,b){
    return a-b
}
var threeSum = function(nums) {
    nums.sort(sortFunction) //先排序
    var res = []
    for(let i=0;i<nums.length;i++){
        if(i>0 && nums[i] > 0) return res; //和前一个做对比去重，不能和后面一个(对nums[a]去重)
        if(nums[i] == nums[i-1]) continue; //若第一个大于0则不可能相加等于0，跳出循环
        var left = i + 1;
        var right = nums.length - 1;
        while(left < right){
            var sum = nums[i] + nums[left] + nums[right];
            if(sum > 0){
                right--;
            }else if(sum < 0){
                left++;
            }else{
                res.push([nums[i],nums[left],nums[right]]);
                while(left < right && nums[left] == nums[left+1]){
                    left++; //对nums[b]去重
                }
                while(left < right && nums[right] == nums[right-1]){
                    right--; //对nums[c]去重
                }
                left++;
                right--;
            }
        }
    }
    return res 
};
```



### 四数之和

>

```js
//相比三数之和多了一层嵌套
var fourSum = function(nums, target) {
    const len = nums.length;
    if(len < 4) return []; //没有四个数不符合题意直接返回空数组
    nums.sort((a, b) => a - b);
    const res = [];
    for(let i = 0; i < len; i++) {
        // 一级去重,去重i
        if(i > 0 && nums[i] === nums[i - 1]) continue;
        for(let j = i + 1; j < len; j++) {
            // 二级去重,去重j
            if(j > i + 1 && nums[j] === nums[j - 1]) continue;
            let l = j + 1, r = len - 1;
            while(l < r) {  //这里的逻辑和三数之和的逻辑一样,只是写的更简洁
                const sum = nums[i] + nums[j] + nums[l] + nums[r];
                if(sum < target) { l++; continue}
                if(sum > target) { r--; continue}
                res.push([nums[i], nums[j], nums[l], nums[r]]);
                while(l < r && nums[l] === nums[++l]);
                while(l < r && nums[r] === nums[--r]);
            }
        } 
    }
    return res;
};
```



## 热题

### 49. 字母异位词分组

>给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
>字母异位词 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。
>
>```
>输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
>输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
>
>输入: strs = [""]
>输出: [[""]]
>```

```js
var groupAnagrams = function(strs) {
    var map = new Map();
    for(let i=0;i<strs.length;i++){
        //对每一个数组内的值排序（因为sort方法是数组的方法，因此要先转成数组再排序再转成字符串）
        let key = strs[i].split('').sort().join(''); 
        if(map.get(key)){  //map中是否有值，其中key为排序后的字符串
            var temp = map.get(key);  //临时数组
            temp.push(strs[i]);
            map.set(key,temp);  //相同key下赋值后一个会覆盖前一个
        }else{
            map.set(key,[strs[i]]);
        }
    }
    return [...map.values()];  //map.values获取的值是类数组，需要转化为数组才能输出
    //Array.from(map.values())
};
```



### 438. 找到字符串中所有字母异位符

>给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。异位词 指由相同字母重排列形成的字符串（包括相同的字符串)
>
>```
>输入: s = "cbaebabacd", p = "abc"
>输出: [0,6]
>解释:
>起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
>起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
>
>输入: s = "abab", p = "ab"
>输出: [0,1,2]
>解释:
>起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
>起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
>起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
>```

```js
```



# 字符串

## 基础习题锻炼

### 反转字符串

>编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。
>不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题
>
>```
>输入：s = ["h","e","l","l","o"]
>输出：["o","l","l","e","h"]
>```
>
>api方法可以使用reverse，但是这里要求的是O(1)

```js
var reverseString = function(s) {
    //Do not return anything, modify s in-place instead.
    reverse(s)
};

var reverse = function(s) {
    var start = 0;
    var end = s.length - 1;
    while (start < end) {
        temp = s[start];
        s[start] = s[end];
        s[end] = temp;
        start++;
        end--;
    }
};

//reverse也可以写成下面更简洁的形式
var reverse = function(s) {
    let l = -1, r = s.length;
    while(++l < --r) [s[l], s[r]] = [s[r], s[l]];
};
```



### 反转字符串II

>给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。
>
>如果剩余字符少于 k 个，则将剩余字符全部反转。
>如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。
>
>```
>输入：s = "abcdefg", k = 2
>输出："bacdfeg"
>```
>解决方法：对for里面的条件进行处理，不要用常规的i++

```js
var reverseStr = function(s, k) {
    const len = s.length;
    let resArr = s.split(""); 
    for(let i = 0; i < len; i += 2 * k) {
        let l = i - 1, r = i + k > len ? len : i + k;
        while(++l < --r) [resArr[l], resArr[r]] = [resArr[r], resArr[l]];
    }
    return resArr.join("");
};
```



### 替换空格

>请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"
>
>```
>输入：s = "We are happy."
>输出："We%20are%20happy."
>```

```js
//api方法：
1.replace&replaceAll(/ /g代表全部都替换，如果只是' '就只会替换一次)
var replaceSpace = function(s) {
    return s.replace(/ /g,'%20')
};

2. split('') + join('%20')

3. 双指针方法
 var replaceSpace = function(s) {
   // 字符串转为数组
  const strArr = Array.from(s);
  let count = 0;

  // 计算空格数量
  for(let i = 0; i < strArr.length; i++) {
    if (strArr[i] === ' ') {
      count++;
    }
  }

  let left = strArr.length - 1;
  let right = strArr.length + count * 2 - 1;

  while(left >= 0) {
    if (strArr[left] === ' ') {
      strArr[right--] = '0';
      strArr[right--] = '2';
      strArr[right--] = '%';
      left--;
    } else {
      strArr[right--] = strArr[left--];
    }
  }

  // 数组转字符串
  return strArr.join('');
}; 
```



### 左旋转字符串

>字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"
>
>```
>输入: s = "abcdefg", k = 2
>输出: "cdefgab"
>```

```js
var reverseLeftWords = function (s, n) {
    /** Utils */
    function reverseWords(strArr, start, end) {
        let temp;
        while (start < end) {
            temp = strArr[start];
            strArr[start] = strArr[end];
            strArr[end] = temp;
            start++;
            end--;
        }
    }
    /** Main code */
    let strArr = s.split('');
    let length = strArr.length;
    reverseWords(strArr, 0, n - 1);
    reverseWords(strArr, n, length - 1);
    reverseWords(strArr, 0, length - 1);
    return strArr.join('');
    
    /** Main code也可以是这样 */
    let strArr = s.split('');
    let length = strArr.length;
    reverseWords(strArr, 0, length - 1);
    reverseWords(strArr, 0, length - n - 1);
    reverseWords(strArr, length - n, length - 1);
    return strArr.join('');
};
```



## 热题

### 3. 无重复字符的最长子串

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

### 5. 最长回文子串

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



# 栈与队列

>栈和队列在js中其实都是用数组代表，在进入的时候都是push，只是在出来的时候队列是先进先出，用的是shift方法（删除数组第一个元素，返回删除值）；栈用的是pop（删除数组最后一个元素，返回删除值）
>

## 基础习题锻炼

### 用栈模拟队列

>#### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

```js
var MyQueue = function() {
    this.stackIn = [];
    this.stackOut = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function(x) {
    this.stackIn.push(x)
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function() {
    var size = this.stackOut.length
    if(size){
        return this.stackOut.pop()
    }
    while(this.stackIn.length){
        this.stackOut.push(this.stackIn.pop())
    }
    return this.stackOut.pop()
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function() {
    const x = this.pop()
    this.stackOut.push(x)
    return x
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function() {
    return !this.stackIn.length && !this.stackOut.length
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.empty()
 */
```

### 用队列模拟栈

>#### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

### 有效括号

>给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
>
>有效字符串需满足：
>
>左括号必须用相同类型的右括号闭合。
>左括号必须以正确的顺序闭合
>
>```
>输入：s = "()"
>输出：true
>
>输入：s = "([)]"
>输出：false
>```

```js
var isValid = function(s) {
    if(s.length%2 !==0) return false;
    const stack = [];
    for(let i=0;i<s.length;i++){
        if(s[i] == '('){
            stack.push(')')
        }else if(s[i] == '{'){
            stack.push('}')
        }else if(s[i] == '['){
            stack.push(']')
        }else{  //处理两种情况：不匹配以及栈为空的时候还有未匹配项
            if(stack.length==0 || s[i] !== stack.pop()) return false
        }
    } //最后判断栈是否为空，即匹配完了但是还有剩余的是多出来的情况
    return stack.length == 0
};
```

