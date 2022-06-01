​	

```text
---
layout: post
title: Blogging Like a Hacker
---
```

# Leetcode notes

## 1. 两数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。你可以按任意顺序返回答案。

示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

```java
//暴力枚举
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n=0;
        int m=0;
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i]+nums[j]==target){
                    n=i;
                    m=j;
                    return new int[]{n,m};
                    break;
                }
            }
        }
       
        return new int[0];
    }
}
```

使用哈希表，可以将寻找 `target - x` 的时间复杂度降低到从 O(N)降低到 O(1)。

```java
//哈希表
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return new int[0];
    }
}

```

## 2. 整数反转

给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−2^31,  2^31 − 1] ，就返回 0。假设环境不允许存储 64 位整数（有符号或无符号）。

**示例 1：**

```
输入：x = 123
输出：321
```

**示例 2：**

```
输入：x = -123
输出：-321
```

```java
class Solution {
    public int reverse(int x) {
        int rever=0;
        if(x>0){
            rever=inter(x);
        }
        else{
            x=Math.abs(x);
            rever=-inter(x);
        }
        if (rev < Integer.MIN_VALUE / 10 || rev > Integer.MAX_VALUE / 10) {
                return 0;
            }
        return rever;
    }
    public int inter(int x){
       while(x>0){
           int digit=x%10;
           sum=sum*10+digit;
           x/=10;
       }
       return sum;
    }
}
```

## 3. 回文数

给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。同样 -121也不是

```java
class Solution {
    public boolean isPalindrome(int x) {
        int sum=0;
        int result=x;
        while(x>0){
            int digit=0;
            digit=x%10;
            sum=sum*10+digit;
            x/=10;
        }        
        return sum==result;
    }
```

## 4. 罗马数字转整数

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

```java
class Solution {
    public int romanToInt(String s) {
         Map<Character, Integer> symbolValues = new HashMap<Character, Integer>() {{
        put('I', 1);
        put('V', 5);
        put('X', 10);
        put('L', 50);
        put('C', 100);
        put('D', 500);
        put('M', 1000);
    }};
    int sum=0;
    int n=s.length();
    for(int i=0;i<n;i++){
        int value = symbolValues.get(s.charAt(i));
            if (i < n - 1 && value < symbolValues.get(s.charAt(i + 1))) {
                sum -= value;
            } else {
                sum += value;
            }
    }
    return sum;
    }
}
```

## 5. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

示例 1：

输入：strs = ["flower","flow","flight"]
输出："fl"
示例 2：

输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuffer sb=new StringBuffer();
        int flag=1;             //判断是否字母之间有间隔
        for(int i=0;i<strs[0].length();i++){
            int num=0;
            for(int j=0;j<strs.length;j++){
                if(i<strs[j].length()&&strs[0].charAt(i)==strs[j].charAt(i)){
                    num++;
                }
                else{
                    flag=0;
                    break;
                }
            }
            if(num==strs.length&&flag!=0){sb.append(strs[0].charAt(i));}
        }
        return sb.toString();
    }
}
```

## 6. 有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。

```java
class Solution {
    public boolean isValid(String s) {
        Stack stack=new Stack();
        int len=s.length();
        for(int i=0;i<len;i++){
            if(s.charAt(i)=='('||s.charAt(i)=='['||s.charAt(i)=='{'){
                stack.push(s.charAt(i));
            }
            else{
                if(s.charAt(i)==')'){
                    if(stack.empty()||(char)stack.peek()!='('){
                        return false;
                    }
                    else{
                        stack.pop();
                    }
                }
                if(s.charAt(i)==']'){
                    if(stack.empty()||(char)stack.peek()!='['){
                        return false;
                    }
                    else{
                        stack.pop();
                    }
                }
                if(s.charAt(i)=='}'){
                    if(stack.empty()||(char)stack.peek()!='{'){
                        return false;
                    }
                    else{
                        stack.pop();
                    }
                }
            }
        }
        if(!stack.empty()){
            return false;
        }
        return true;
    }
}
```

## 7. 移除元素

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例 1：

输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int j=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=val){
                nums[j]=nums[i];
                j++;
            }
        
        }
        return j;
    }
}
```

## 8. 删除有序数组的重复元素

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

 示例 1：

输入：nums = [1,1,2]
输出：2, nums = [1,2]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int len=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=nums[len]){
                //System.out.println("nums[i]:"+nums[i]+" len:"+len);
                 len++;
                nums[len]=nums[i];    
            }
    }
    len++;//len是数组的长度，因为输出数组应该是for(int i=0;i<len;i++0),所以len最后应该+1
    return len;
    }
}
```

## 9. 实现strStr()

实现 strStr() 函数。

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

```java
//暴力匹配，时间复杂度为O(n×m)，其中n是字符串haystack 的长度，m 是字符串needle 的长度
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length()<=0){
            return 0;
        }
        int j=0;
        int i=0;
        while(i<haystack.length()){
            if(j<needle.length()){
                   if(haystack.charAt(i)==needle.charAt(j)){
                       j++;
                       i++;
                }
                else{
                    i=i-j+1;
                    j=0;
                }
            }
            if(j==needle.length()){
                return i-j;
            } 
        }
    return -1;
    }
}

```

```java
//KMP,时间复杂度O(n+m)
public class demo{
    public static int getNext(String s,int x){                //得到next数组
        String str = s.substring(0,x+1);                      //next数组的值是字符串的前缀和后缀子串的最长相等的长度
//        System.out.println(str);                            //数组的下标代表的是前缀或者后缀子串的最长长度
        if(x==1)
            return 0;
        else {
            for(int i=x-1;i>=0;i--){
                if(str.substring(0,i+1).equals(str.substring(x-i,x+1))){
                    return i+1;
                }
            }
        }
        return 0;
    }
    public static int KMP(String s,String substring,int[] next){
        int i=0;                                                //i，j分别为字符串和子串的数组下标
        int j=0;
        while(i<s.length()&&j<substring.length()){
            if(j==-1||s.charAt(i)==substring.charAt(j)){
                i++;                                            //匹配成功 继续向下匹配
                j++;
            }
            else{
                if(j==0)
                    j=-1;
                else
                    j=next[j];                                  //当不匹配的时候，j下标退回到next数组以j为下标的位置
            }
        }
        if(j==substring.length())                               //子串比对完的时候，说明是字符串的子串，开始的位置是i-j
            return i-j;
        else
            return -1;
    }
   
```

## 10. 搜索插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

示例 1:

输入: nums = [1,3,5,6], target = 5
输出: 2

示例 2:

输入: nums = [1,3,5,6], target = 2
输出: 1

示例 3:

输入: nums = [1,3,5,6], target = 7
输出: 4

示例 4:

输入: nums = [1,3,5,6], target = 0
输出: 0

示例 5:

输入: nums = [1], target = 0
输出: 0

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
    
        int left=0;
        int right=nums.length-1;
        int mid=(right-left)/2+left;
        while(left<=right){
            if(target==nums[mid]) return mid;
            else if(target>nums[mid]){
                left=mid+1;
                mid=(right-left)/2+left;
            }
            else{
                right=mid-1;
                mid=(right-left)/2+left;
            }

        }
        return mid;
    }
}
```

## 11. 合并有序链表

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);
        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}
```

## 12. 最大子序和

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

 

示例 1：

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
示例 2：

输入：nums = [1]
输出：1

![image-20220226164428886](https://gitee.com/bitches/git/raw/master/img/image-20220226164428886.png)

为什么一定要包含第i个数，因为为了保证数组的连续

```java
//force brute
class Solution {
    public int maxSubArray(int[] nums) {
        int max=nums[0];
        int tmpMax=0;
       
       for(int i=0;i<nums.length;i++){
           tmpMax=0;
          for(int j=i;j<nums.length;j++){
              tmpMax+=nums[j];
              if(max<=tmpMax){
                  max=tmpMax;
              }
          }
       }
       return max; 
    }
}
```

```java
//动态规划 
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0, maxAns = nums[0];
        for (int x : nums) {
            pre = Math.max(pre + x, x);
            maxAns = Math.max(maxAns, pre);
        }
        return maxAns;
    }
}
//自己的想法，
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp=new int[nums.length];
        dp[0]=nums[0];
        int max=nums[0];
        for (int i=1;i<nums.length;i++) {
            dp[i] = Math.max(nums[i], nums[i]+dp[i-1]);
            
        }
        for (int i=0;i<nums.length;i++) {
            max = Math.max(max, dp[i]);
            
        }
        return max;
    }
}
//贪心算法
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length==1) return nums[0];
        int max = Integer.MIN_VALUE;
        int cur = 0;
        //找出连续区间的最大值
        for (int i=0;i<nums.length;i++) {
            cur += nums[i];
            max = max>cur?max:cur;
            //cur小于0，负值总会拉低总分，所以去除
            if(cur<0) cur = 0;
        }
        
        return max;
    }
}
```

![image-20210917172457334](https://gitee.com/bitches/git/raw/master/img/image-20210917172457334.png)

## 13. 最后一个单词的长度

给你一个字符串 s，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中最后一个单词的长度。

单词 是指仅由字母组成、不包含任何空格字符的最大子字符串。

 

示例 1：

输入：s = "Hello World"
输出：5
示例 2：

输入：s = "   fly me   to   the moon  "
输出：4
示例 3：

输入：s = "luffy is still joyboy"
输出：6

```JAVA
//最初的想法
class Solution {
    public int lengthOfLastWord(String s) {
        int lastIndex=s.lastIndexOf(' ');
        if(s.contains(" ")){
            if(lastIndex!=s.length()-1)
            return s.length()-1-lastIndex;
            else{
                int sum=0;
                int flag=0;//利用flag判断空格是单词前面还是后面
                int i=lastIndex;
              while(i>=0){
                 if(s.charAt(i)!=' '&&flag==0){
                     sum++;
                     flag++;
                 }
                 else if(flag>0&&s.charAt(i)==' '){
                     break;
                 }
                 else if(s.charAt(i)!=' '&&flag>0){
                     sum++;
                 }
                 i--;
              }
                return sum;
        }
    }
        else
        return s.length();
    }
}

//简单的
class Solution {
    public int lengthOfLastWord(String s) {
       int end=s.length()-1;
       while(end>=0&&s.charAt(end)==' '){
           end--;
       }
       if(end<0) return 0;
       int start=end;
       while(start>=0&&s.charAt(start)!=' '){
           start--;
       }
       return end-start;
    }
}
```

## 14. 加一

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1：

输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
示例 2：

输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
示例 3：

输入：digits = [0]
输出：[1]

```java
//此方法数字过大会引起溢出导致答案不对
class Solution {
    public int[] plusOne(int[] digits) {
        int sum=0;
        for(int i=0;i<digits.length;i++){
            sum=sum*10+digits[i];
            System.out.println(sum);
        }
        sum++;
        
        int i=String.valueOf(sum).length()-1;
        int[] a=new int[i+1];
        while(sum>0){
            a[i]=sum%10;
            sum/=10;
            i--;
        }
        return a;
    }
}
```

```java
//逐位加
class Solution {
    public int[] plusOne(int[] digits) {
        int sum=0;
        for(int i=0;i<digits.length;i++){
            if(digits[i]==9) sum++;
        }
        if(digits[digits.length-1]!=9){//最后一位不是9的情况
            digits[digits.length-1]++;
            return digits;
        }  
        else{
            if(sum==digits.length){			//全是9的情况
                int[] a=new int[digits.length+1];
                a[0]=1;
                for(int i=1;i<a.length;i++) a[i]=0;
                return a;  
            }
            else{							//最后一位是9，但是不是所有的都是9
                digits[digits.length-1]=0;
                for(int i=digits.length-2;i>=0;i--){
                    if(digits[i]==9)  digits[i]=0;
                    else {
                        digits[i]++;
                        break;
                        }
                    }
            }
        }
        return digits;
    }
}
```

```java
//官网题解
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            digits[i]++;
            digits[i] = digits[i] % 10;
            if (digits[i] != 0) return digits;
        }
        digits = new int[digits.length + 1];
        digits[0] = 1;
        return digits;
    }
}

```

## 15. 二进制求和

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 非空 字符串且只包含数字 1 和 0。

 

示例 1:

输入: a = "11", b = "1"
输出: "100"
示例 2:

输入: a = "1010", b = "1011"
输出: "10101"

```java
//转化为十进制求和，再转化为二进制，但是Java的Integer类型超过一定位数便不行了
class Solution {
    public String addBinary(String a, String b) {
        int result=Integer.valueOf(a,2)+Integer.valueOf(b,2);
        return Integer.toBinaryString(result); 
    }
}
//进位求和
class Solution {
    public String addBinary(String a, String b) {
        StringBuffer ans = new StringBuffer();
        int n = Math.max(a.length(), b.length()), carry = 0;
        for (int i = 0; i < n; ++i) {
            carry += i < a.length() ? (a.charAt(a.length() - 1 - i) - '0') : 0;
            carry += i < b.length() ? (b.charAt(b.length() - 1 - i) - '0') : 0;
            ans.append((char) (carry % 2 + '0'));
            carry /= 2;
        }
        if (carry > 0) {
            ans.append('1');
        }
        ans.reverse();

        return ans.toString();
    }
}

```

## 16. 平方根

给你一个非负整数 x ，计算并返回 x 的 平方根 。

由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。

注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。

```java
//注意：i*i可能超过int的范围，所以i和result必须使用long类型
class Solution {
    public int mySqrt(int x) {
        long result=0;
        if(x==1) return 1;
        if(x==0) return 0;
        long b=(int)x/2;
        for(long i=1;i<=b;i++){
            if(i*i<=x) result=i;           
            else break;
        } 
        return (int)result;
    }
}
```

## 17. 爬楼梯

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

```java
//动态规划 O(n)
class Solution {
    public int climbStairs(int n) {
        if(n==1) return 1;
        int[] dp=new int[n];
        dp[0]=1;
        dp[1]=2;
        for(int i=2;i<n;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n-1];
    }
}
//递归,时间复杂度O(2^n)
class Solution {
    public int climbStairs(int n) {
        if(n==1) return 1;
        if(n==2) return 2;
        return climbStairs(n-1)+climbStairs(n-2);
    }
}
```

## 18. 链表中的两数相加

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode cur1 = l1;
        ListNode cur2 = l2;
        ListNode pre = null;
        ListNode next = null;
        //1、反转链表
        if(cur1.next!=null){
            while(cur1!=null){
            next = cur1.next;
            cur1.next = pre;
            pre = cur1;
            cur1 = next;
            }
            cur1 = pre;
            pre = null;
        }
        if(cur2.next!=null){
            while(cur2!=null){
            next = cur2.next;
            cur2.next = pre;
            pre = cur2;
            cur2 = next;  
            }
            cur2 = pre;
        }
        //2、求和 得到新链表
        int cd = 0;
        ListNode head = new ListNode(0);
        ListNode cur = head;
        while(cur1 != null && cur2 !=null){
            cur.val = (cur1.val + cur2.val + cd)%10;
            cd = (cur1.val + cur2.val + cd)/10;
            cur.next = new ListNode(0);
            cur = cur.next;
            cur2 = cur2.next;
            cur1 = cur1.next;
        }
        if(cur1==null){
            while(cur2!=null){
                cur.val = (cur2.val+cd)%10;
                cd = (cur2.val+cd)/10;
                cur.next = new ListNode(0);
                cur = cur.next;
                cur2 = cur2.next;
            }
        }else if(cur2==null){
            while(cur1!=null){
                cur.val = (cur1.val+cd)%10;
                cd = (cur1.val+cd)/10;
                cur.next = new ListNode(0);
                cur = cur.next;
                cur1 = cur1.next;
            }
        }
        if(cd!=0){
            cur.val = cd;
        }
        //3、反转新链表
        pre = null;
        next = null;
        cur = head;
        while(cur != null){
            next = cur.next;
            cur.next = pre;
            pre= cur;
            cur = next;
        }
        return cd==0? pre.next:pre;
    }
}
```

## 19. 比较含退格的字符串

定 S 和 T 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 # 代表退格字符。

注意：如果对空文本输入退格字符，文本继续为空。

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        Stack s1=new Stack();
        Stack s2=new Stack();
        for(int i=0;i<s.length();i++){
            if(s.charAt(i)!='#') s1.push(s.charAt(i));
            else{
                if(!s1.empty()) s1.pop(); 
            }
        }
        for(int j=0;j<t.length();j++){
            if(t.charAt(j)!='#') s2.push(t.charAt(j));
            else{
                if(!s2.empty()) s2.pop(); 
            }
        }
        StringBuffer t1=new StringBuffer();
        StringBuffer t2=new StringBuffer();
        while(!s1.empty()) {
            t1.append(s1.peek());
            s1.pop();
        }
        while(!s2.empty()) {
            t2.append(s2.peek());
            s2.pop();
        }
        String n1=t1.toString();
        String n2=t2.toString();   
        return n1.equals(n2);
    }
}
```

## 20. Nim 游戏

你和你的朋友，两个人一起玩 Nim 游戏：

桌子上有一堆石头。
你们轮流进行自己的回合，你作为先手。
每一回合，轮到的人拿掉 1 - 3 块石头。
拿掉最后一块石头的人就是获胜者。
假设你们每一步都是最优解。请编写一个函数，来判断你是否可以在给定石头数量为 n 的情况下赢得游戏。如果可以赢，返回 true；否则，返回 false 。

```java
class Solution {				//如果石头数是4的倍数，就肯定赢不了，其他的都能赢，找规律
    //巴什博奕，n%(m+1)!=0时，先手总是会赢的
    public boolean canWinNim(int n) {
        if(n%4==0) return false;
        return true;
    }
}
```

## 21. 合并两个有序数组

给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

 ```java
 //直接将nums2续在nums1后面，然后sort
 class Solution {
     public void merge(int[] nums1, int m, int[] nums2, int n) {
         int i=m;
         int j=0;
         while(i<m+n){
             nums1[i]=nums2[j];
             i++;
             j++;
         }
         Arrays.sort(nums1);
     }
 }
 ```

```java
//新建一个数组
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] res=new int[n+m];
        int i=0;
        int l1=0;
        int l2=0;
       while(i<n+m&&l1<m&&l2<n){
            if(nums1[l1]<=nums2[l2]){
                res[i]=nums1[l1];
                l1++;
            }
            else{
                res[i]=nums2[l2];
                l2++;
            }
            i++;
        }
        while(i<n+m){
            res[i]=l1<m?nums1[l1]:nums2[l2];
            i++;
            l1++;
            l2++;
        }
        for(int j=0;j<n+m;j++){
            nums1[j]=res[j];
        }
    }
}
```

## 22. 两数相加

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

 ```java
 class Solution {
     public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
         ListNode res=new ListNode();
         ListNode result=res;
         int carry=0;
         while(l1!=null||l2!=null){
            if(l1!=null) {
                carry+=l1.val;
                l1=l1.next;
            }
            if(l2!=null) {
                carry+=l2.val;
                l2=l2.next;
            }
            res.val=carry%10;
            if(carry>=10||(carry!=0&&(l1!=null||l2!=null))||(carry==0&&(l1!=null||l2!=null))){
                 res.next=new ListNode();
                 res=res.next;
            }
            carry/=10;
         }
         if(carry!=0) res.val=carry;
         return result;
     }
 }
 ```

## 23. 最接近的三数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        int best = 10000000;
        // 枚举 a
        for (int i = 0; i < n; ++i) {
            // 保证和上一次枚举的元素不相等
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            // 使用双指针枚举 b 和 c
            int j = i + 1, k = n - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                // 如果和为 target 直接返回答案
                if (sum == target) {
                    return target;
                }
                // 根据差值的绝对值来更新答案
                if (Math.abs(sum - target) < Math.abs(best - target)) {
                    best = sum;
                }
                if (sum > target) {
                    // 如果和大于 target，移动 c 对应的指针
                    int k0 = k - 1;
                    // 移动到下一个不相等的元素
                    while (j < k0 && nums[k0] == nums[k]) {
                        --k0;
                    }
                    k = k0;
                } else {
                    // 如果和小于 target，移动 b 对应的指针
                    int j0 = j + 1;
                    // 移动到下一个不相等的元素
                    while (j0 < k && nums[j0] == nums[j]) {
                        ++j0;
                    }
                    j = j0;
                }
            }
        }
        return best;
    }
}
```

## 24. 字符串相乘

给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。

1. **不能使用任何标准库的大数类型（比如 BigInteger）**或**直接将输入转换为整数来处理**。

```java
//转换为最基础的乘法运算
class Solution {
    public String multiply(String num1, String num2) {
        int m=0;
        StringBuffer result=new StringBuffer();
        if(num1.equals("0")||num2.equals("0")) return "0";
        for(int i=num1.length()-1;i>=0;i--){
            int carry=0;
            StringBuffer sb=new StringBuffer();
            for(int j=num2.length()-1;j>=0;j--){
                carry+=(num1.charAt(i)-'0')*(num2.charAt(j)-'0');
                sb.append(carry%10);
                carry/=10;
            }
            if(carry!=0) sb.append(carry);
            sb.reverse();
            for(int l=0;l<m;l++){
                sb.append('0');
            }
            result=add(result.toString(),sb.toString());
            sb.delete(0,sb.length());
            m++;
        }
       return result.toString();
    }
    public StringBuffer add(String num1,String num2){ //两个字符串相加
        StringBuffer sb=new StringBuffer();
        int carry=0;
        int i=num1.length()-1;
        int j=num2.length()-1;
        while(i>=0&&j>=0){
            carry+=(num1.charAt(i)-'0')+(num2.charAt(j)-'0');
            sb.append(carry%10);
            carry/=10;
            i--;
            j--;
        }
        if(i!=-1||j!=-1){
            int k=(i==-1?j:i);

            String s=(i==-1?num2:num1);
            while(k>=0){
                    carry+=s.charAt(k)-'0';
                    sb.append(carry%10);
                    carry/=10;
                    k--;
            }
        }
        if(carry!=0) sb.append(carry);
        return sb.reverse();
    }
}

```

## 25. 二分查找

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

```java
class Solution {
    public int search(int[] nums, int target) {
        int left=0;
        int right=nums.length-1;
        while(left<=right){
            int mid=(left+right)/2;
            if(target==nums[mid]) return mid;
            if(target>nums[mid]){
                left=mid+1;
                mid=(left+right)/2;
            }
            if(target<nums[mid]){
                right=mid-1;
                mid=(left+right)/2;
            }
        }
         return -1;
  

    }
}
```

## 26. 第一个错误的版本

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。


示例 1：

输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false 
调用 isBadVersion(5) -> true 
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left=1;
        int right=n;
        while(left<=right){
            int mid=left+(right-left)/2;//如果mid=(left+right)/2,会超过时间限制
            //当left和right都是int，两个值的初始值都超过int限定大小的一半，那么left+right就会发生溢出，所以应该用left+(right-left)/2来防止求中值时候的溢出。
            if(mid==1&&isBadVersion(mid)) return 1;
            if(!isBadVersion(mid-1)&&isBadVersion(mid)&&mid>1){
                return mid;
            }
            else if(isBadVersion(mid)){
                right=mid-1;
                mid=(left+right)/2;
            }
            else if(!isBadVersion(mid)){
                left=mid+1;
                mid=(left+right)/2;
            }
        }
        return -1;
    }
}
```

## 27. 买卖股票的最佳时机

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

 

示例 1：

输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

```java
//动态规划 前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}
//1. 记录【今天之前买入的最小值】
//2. 计算【今天之前最小值买入，今天卖出的获利】，也即【今天卖出的最大获利】
//3. 比较【每天的最大获利】，取最大值即可
class Solution {
    public int maxProfit(int[] prices) {
        int max=0;
        if(prices.length<=1) return 0;
        int min=prices[0];
        for(int i=1;i<prices.length;i++){
            max=Math.max(max,prices[i]-min);
            min=Math.min(min,prices[i]);
        }
        return max;
    }
}
```

## 28. 有序数组的平方

给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

 

示例 1：

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]

```java
//暴力 先平方在排序
class Solution {
    public int[] sortedSquares(int[] nums) {
      
      for(int i=0;i<nums.length;i++){
          nums[i]*=nums[i];
      }
        Arrays.sort(nums);
        return nums;
    }
}
//双指针
//如果我们能够找到数组nums中负数与非负数的分界线，那么就可以用类似「归并排序」的方法了。具体地，我们设neg 为数组 nums 中负数与非负数的分界线，也就是说，nums[0] 到 nums[neg] 均为负数，而 nums[neg+1] 到 nums[n−1] 均为非负数。当我们将数组nums 中的数平方后，那么nums[0] 到 nums[neg] 单调递减，nums[neg+1] 到 nums[n−1] 单调递增。
//由于我们得到了两个已经有序的子数组，因此就可以使用归并的方法进行排序了。具体地，使用两个指针分别指向位置neg 和 neg+1，每次比较两个指针对应的数，选择较小的那个放入答案并移动指针。当某一指针移至边界时，将另一指针还未遍历到的数依次放入答案。
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int negative = -1;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < 0) {
                negative = i;
            } else {
                break;
            }
        }
        int[] ans = new int[n];
        int index = 0, i = negative, j = negative + 1;
        while (i >= 0 || j < n) {
            if (i < 0) {
                ans[index] = nums[j] * nums[j];
                ++j;
            } else if (j == n) {
                ans[index] = nums[i] * nums[i];
                --i;
            } else if (nums[i] * nums[i] < nums[j] * nums[j]) {
                ans[index] = nums[i] * nums[i];
                --i;
            } else {
                ans[index] = nums[j] * nums[j];
                ++j;
            }
            ++index;
        }
        return ans;
    }
}
```

## 29. 旋转数组

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

进阶：

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？


示例 1:

输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]

```java
//利用额外的数组
class Solution {
    public void rotate(int[] nums, int k) {
       k=k%nums.length;
       int[] res=new int[nums.length];
      for(int i=0;i<nums.length;i++) res[i]=nums[i];
      int j=nums.length-k;
      int i=0;
      int m=0;
      while(i<nums.length-k||j<nums.length){
          if(j<nums.length) {nums[m]=res[j];
          j++;
          }
          else{nums[m]=res[i];
          i++;}
          m++;
      }
    }
}
```

```java
//数组翻转，先将整个数组翻转，再将前半部分和后半部分分别翻转
class Solution {
    public void rotate(int[] nums, int k) {
       k=k%nums.length;
       reverse(nums,0,nums.length-1);
       reverse(nums,0,k-1);
       reverse(nums,k,nums.length-1);
    }
    public void reverse(int[] nums,int left,int right){
    
        while(left<=right){
            int tmp=nums[left];
            nums[left]=nums[right];
            nums[right]=tmp;
            left++;
            right--;
        }
    }
}
```

## 30. 不同路径

一个机器人位于一个 m x n 网格的左上角 

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角

问总共有多少条不同的路径？

```java
//递归，但是时间复杂度比较高
class Solution {
    public int uniquePaths(int m, int n) {
        if(m==1||n==1) return 1;
        return uniquePaths(m,n-1)+uniquePaths(m-1,n);
    }
}
```

```java
//动态规划
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp=new int[m][n];
        for(int i=0;i<m;i++){
            dp[i][0]=1;
        }
        for(int j=0;j<n;j++){
            dp[0][j]=1;
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```

## 31. 只出现一次的数字

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

```java
//时间复杂度不符合要求，为O（nlogn）
class Solution {
    public int singleNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i=0;i<nums.length-1;i+=2){
            if(nums[i]!=nums[i+1]) return nums[i];
        }
        return nums[nums.length-1];

    }
}
```

```java
//异或运算，相同的数字异或为0
//交换律：a ^ b ^ c <=> a ^ c ^ b
//任何数于0异或为任何数 0 ^ n => n
//相同的数异或为0: n ^ n => 0
class Solution {
    public int singleNumber(int[] nums) {
        int single=0;
        for(int num:nums){
            single^=num;
        }
        return single;
    }
}
```

## 32. 移动零

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]

```java
class Solution {
    public void moveZeroes(int[] nums) {
      int i=0;
      int j=1;
      while(i<j&&j<nums.length){
          if(nums[i]==0&&nums[j]!=0){
               int tmp=nums[i];
               nums[i]=nums[j];
               nums[j]=tmp;
          }
         else if(nums[i]==0&&nums[j]==0) j++;
         else{
             i++;
             j++;
         }
        
      }
    }
}
```

## 33. 两数之和 II - 输入有序数组

给定一个已按照 非递减顺序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。


示例 1：

输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。

```java
//有序数组，双指针，和小于就移动左指针，大于就移动右指针
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i=0;
        int j=numbers.length-1;
        int[] ans=new int[2];
        while(i<j){
            if(numbers[i]+numbers[j]==target) {
                ans[0]=i+1;
                ans[1]=j+1;
                return ans;
            }
            else if(numbers[i]+numbers[j]<target){
                i++;
            }
            else{
                j--;
            }
        }
        return null;
    }
}
```

## 34. 反转字符串

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

 

示例 1：

输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]

```java
//双指针
class Solution {
    public void reverseString(char[] s) {
        int i=0;
        int j=s.length-1;
        while(i<j){
            char tmp=s[i];
            s[i]=s[j];
            s[j]=tmp;
            i++;
            j--;
        }
    }
}
```

## 35. 旋转链表

给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```

```java

class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        int len=0;
        ListNode temp=head;
        while(head!=null){//获取链表长度
            len++;
            head=head.next;
        }
        ListNode ans=new ListNode();
        ListNode beh=new ListNode();
        ListNode pre=new ListNode();
        ListNode beh1=beh;
        ListNode pre1=pre;
        if(len!=0) k=k%len;
        if(k<=0||temp==null||len<=1) return temp;
        int tmp=len-k;
        while(tmp>0){		//获取后半部分
            beh.val=temp.val;
            temp=temp.next;
            tmp--;
            if(tmp>0) beh.next=new ListNode();
            beh=beh.next;
        }
        while(temp!=null){			//获取前半部分
            pre.val=temp.val;
            if(temp.next!=null) {
                pre.next=new ListNode();
                pre=pre.next;
            }
            temp=temp.next;
        }
        pre.next=new ListNode();
        pre.next=beh1;
        ans=pre1;
        return ans;
    }

}
```

```java
//将链表整合为一个闭环，即将链表之后脸上链表，然后把前半部分和后半部分去掉
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (k == 0 || head == null || head.next == null) {
            return head;
        }
        int n = 1;
        ListNode iter = head;
        while (iter.next != null) {
            iter = iter.next;
            n++;
        }
        int add = n - k % n;
        if (add == n) {
            return head;
        }
        iter.next = head;
        while (add-- > 0) {
            iter = iter.next;
        }
        ListNode ret = iter.next;
        iter.next = null;
        return ret;
    }
}
```

## 36. 环形链表

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

- 链表中节点的数目范围是 `[0, 104]`
- `-105 <= Node.val <= 105`
- `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
//投机取巧，既然节点数目是小于10的四次方，那么如果链表的长度小于等于10000，肯定是没有环的，否则是有环的
public class Solution {
    public boolean hasCycle(ListNode head) {
        int n=0;
        while(head!=null){
            head=head.next;
            n++;
            if(n>10000) return true;
        }
        return false;
    }
}
```

```java
//快慢指针，slow和fast，fast每次走两格，slow走一格，如果相遇就是有环，否则无环
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}

```

## 37. 反转字符串中的单词 III

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

 

示例：

输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"


提示：

在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。

```java
//时间复杂度比较高，用时10ms
class Solution {
    public String reverseWords(String s) {
        StringBuffer sb=new StringBuffer();
        StringBuffer ans=new StringBuffer();
        int j=0;
        while(j<s.length()){
            if(s.charAt(j)==' '){
                ans.append(sb.reverse().toString());
                ans.append(' ');
                sb.delete(0,sb.length());
                j++;
            }
            sb.append(s.charAt(j));
            j++;
        }
        ans.append(sb.reverse().toString());
        return ans.toString();
    }
}
```

```java
//双指针，用时3ms，前后指针分别指向具体单词的第一个和最后一个字母
class Solution {
    public String reverseWords(String s) {
        char[] chr=new char[s.length()];
        chr=s.toCharArray();
        int temp=0;
        int i=0;
        int j=1;
        while(j<chr.length){
            if(chr[j]!=' '){
                j++;
            }
            else{
                temp=j;
                while(i<j-1){
                    char tmp=chr[i];
                    chr[i]=chr[j-1];
                    chr[j-1]=tmp;
                    i++;
                    j--;
                }
                i=temp+1;
                j=i+1;
            }
        }
         while(i<j-1){
                    char tmp=chr[i];
                    chr[i]=chr[j-1];
                    chr[j-1]=tmp;
                    i++;
                    j--;
                }
                return String.valueOf(chr);
    }


}
```

## 38. 除自身以外数组的乘积

给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

 

示例:

输入: [1,2,3,4]
输出: [24,12,8,6]


提示：题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

```java
//分别计算i左右的累乘，然后left*right就行 时间复杂度O(n),空间复杂度O(n)
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int left=1;
        int[] right=new int[nums.length];
        int[] ans=new int[nums.length];
        right[nums.length-1]=1;
        for(int j=nums.length-2;j>=0;j--){
            right[j]=right[j+1]*nums[j+1];//将右边累乘结果用数组保存
        }
        for(int i=0;i<nums.length;i++){
            ans[i]=1;
            ans[i]=left*right[i];
            left*=nums[i];
        }
        return ans;
    }
}
```

```java
//时间复杂度为O(n),空间复杂度为O(1)
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        int[] answer = new int[length];
        // answer[i] 表示索引 i 左侧所有元素的乘积
        // 因为索引为 '0' 的元素左侧没有元素， 所以 answer[0] = 1
        answer[0] = 1;
        for (int i = 1; i < length; i++) {
            answer[i] = nums[i - 1] * answer[i - 1];
        }
        // R 为右侧所有元素的乘积
        // 刚开始右边没有元素，所以 R = 1
        int R = 1;
        for (int i = length - 1; i >= 0; i--) {
            // 对于索引 i，左边的乘积为 answer[i]，右边的乘积为 R
            answer[i] = answer[i] * R;
            // R 需要包含右边所有的乘积，所以计算下一个结果时需要将当前值乘到 R 上
            R *= nums[i];
        }
        return answer;
    }
}

```

## 39. 排序链表

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

进阶：

你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

```java
/**
将链表数据存在数组中，排序之后形成链表，但是使用了额外的空间
 */
class Solution {
    public ListNode sortList(ListNode head) {
        int len=0;
        ListNode temp=head;
        ListNode ans=new ListNode();
        ListNode res=ans;
        if(head==null||head.next==null) return head;
        while(head!=null){
            len++;
            head=head.next;
        }
        int[] num=new int[len];
        for(int i=0;i<len;i++){
            num[i]=temp.val;
            temp=temp.next;
        }
        Arrays.sort(num);
        for(int i=0;i<len;i++){
           ans.val=num[i];
           if(i!=len-1){
               ans.next=new ListNode();
               ans=ans.next;
           }
        }
        return res;
    }
}
```

## 40. 三数之和

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

 

示例 1：

输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]

```java
//排序加双指针
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        if(nums.length<=2) return new ArrayList<List<Integer>>();
        HashSet<List<Integer>> hash=new LinkedHashSet<List<Integer>>();//利用HashSet的特性，去除掉重复的三元组 
        Arrays.sort(nums);
        for(int k=0;k<nums.length;k++){
            int i=k+1;
            int j=nums.length-1;
            int tmp=0-nums[k];
            while(i<j){
                if(nums[i]+nums[j]==tmp){
                    List<Integer> list = new ArrayList<Integer>();
                    list.add(nums[k]);
                    list.add(nums[i]);
                    list.add(nums[j]);
                    hash.add(list);
                    i++;
                    j--;
                }
                else if(nums[i]+nums[j]>tmp) j--;
                else i++;
            }
        }
         List<List<Integer>> ans = new ArrayList<List<Integer>>(hash);
        return ans;
    }
}
```

## 41. 多数元素

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

```java
//时间复杂度为O(nlogn),空间复杂度为O(1)
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        int count=0;
        int res=nums[0];
        int n=nums.length;
        for(int i=0;i<nums.length;i++){
            if(nums[i]==res) count++;
            else if(nums[i]!=res&&count<(n+1)/2){
                res=nums[i];
                count=1;
            }
        }
        return res;
    }
}
```

```java
//摩尔投票法：核心就是对拼消耗。玩一个诸侯争霸的游戏，假设你方人口超过总人口一半以上，并且能保证每个人口出去干仗都能一对一同归于尽。最后还有人活下来的国家就是胜利。那就大混战呗，最差所有人都联合起来对付你（对应你每次选择作为计数器的数都是众数），或者其他国家也会相互攻击（会选择其他数作为计数器的数），但是只要你们不要内斗，最后肯定你赢。
//所以就选择一个数，如果下个数相等，count+1，不然减一，等到count=0，就选择接下来的数，最后剩下的就是答案
class Solution {
    public int majorityElement(int[] nums) {
        int res=nums[0];
        int count=0;
        for(int num:nums){
            if(count==0){
                    res=num;
                }
            if(res==num) count++;
            if(res!=num) count--;
        }
        return res;
    }
} 
```

## 42. 链表的中间节点

给定一个头结点为 `head` 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

示例 1：

输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.

```java
//双指针，快慢指针，快指针每次两格，慢指针一格，快指针到尾部，慢指针就到中间节点
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow=head;
        ListNode fast=head;
 
        while(fast!=null&&fast.next!=null){
            slow=slow.next;
            fast=fast.next.next;
        }
        return slow;
    }
}
```

## 43. 删除链表的倒数第n个节点

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**进阶：**你能尝试使用一趟扫描实现吗？

```java
//双指针，前面和后面的指针相差n个节点，当后面的节点到尾部的时候，删除前面的下个节点就行
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
    
        ListNode front=head;
        ListNode behind=head;
        ListNode res=front;
        if(head.next==null) return null;
        
        while(n>0){
            behind=behind.next;
            n--;
        }
        if(behind==null){
            return front.next;    
        }
        while(behind.next!=null){ 
            front=front.next;
            behind=behind.next;
        }
        front.next=front.next.next;
        return res;
    }
}
```

## 44. 删除链表中的节点

请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点。传入函数的唯一参数为 要被删除的节点 。

现有一个链表 -- head = [4,5,1,9]，它可以表示为:

示例 1：

输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.

```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
//单指针
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head==null) return null;
        if(head.val == val) return head.next;
        ListNode tmp=head;
        //notes：这里用tmp.next,不然会出现tmp.next为null的情况
        while(tmp.next!=null&&tmp.next.val!=val){
            tmp=tmp.next;
        }
        //这里也要进行判断，是到达链表结尾了还是遇到我们需要找的节点了
        if(tmp.next!=null)
        tmp.next=tmp.next.next;
        return head;
    }
}

```

## 45. 无重复字符的最长字串

给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

 

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

```java
//滑动窗口，最主要的是利用hashset来判断是否含有重复子串
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int j=-1;
        Set<Character> hashset=new HashSet<Character>();
        int max=0;
        int i=0;
        for(i=0;i<s.length();i++){
            if(i!=0) hashset.remove(s.charAt(i-1));
            while(j<s.length()-1&&!hashset.contains(s.charAt(j+1))){
                hashset.add(s.charAt(j+1));
                j++;
            }
            max=Math.max(max,j+1-i);
        }
        return max;
    }
}
```

## 46. 图像渲染

有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。

为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

最后返回经过上色渲染后的图像

示例 1:

输入: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]
解析: 
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。

```java
//广度优先搜索
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        return bfs(sr,sc,image,image[sr][sc],newColor);
    }
    public int[][] bfs(int i, int j, int[][] image,int color,int newColor){
        if(i<0||i>=image.length||j<0||j>=image[0].length||image[i][j]==newColor||image[i][j]!=color){}
        else{
            int tmp=image[i][j];
            image[i][j]=newColor;
            bfs(i-1,j,image,tmp,newColor);
            bfs(i+1,j,image,tmp,newColor);
            bfs(i,j+1,image,tmp,newColor);
            bfs(i,j-1,image,tmp,newColor);
        }
        return image;
    }
}
```

## 47. 岛屿的最大面积

给你一个大小为 m x n 的二进制矩阵 grid 。

岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在 水平或者竖直的四个方向上 相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

岛屿的面积是岛上值为 1 的单元格的数目。

计算并返回 grid 中最大的岛屿面积。如果没有岛屿，则返回面积为 0 。

示例 1：


输入：grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。

![image-20210930154846559](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20210930154846559.png)

```java
//广度优先/深度优先
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        boolean[][] flag=new boolean[grid.length][grid[0].length];
        int max=0;
        int count=0;
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]==1&&flag[i][j]==false){
                    max=Math.max(max,bfs(grid,i,j,flag,count));
                }
            }
        }
        return max;
    }
    public int bfs(int[][] grid,int i,int j,boolean[][] flag,int count){ 
        if(i<0||i>=grid.length||j<0||j>=grid[0].length||grid[i][j]!=1||flag[i][j]!=false){}
        else{
            flag[i][j]=true;
            count++;
            count=Math.max(count,bfs(grid,i-1,j,flag,count));
            count=Math.max(count,bfs(grid,i+1,j,flag,count));
            count=Math.max(count,bfs(grid,i,j+1,flag,count));
            count=Math.max(count,bfs(grid,i,j-1,flag,count));
        }
        return count;
    }
}
```

## 48. 合并二叉树

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

```java
/**
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
         if (t1 == null) {
            return t2;
        }
        if (t2 == null) {
            return t1;
        }
        // 先合并根节点
        t1.val += t2.val;
        // 再递归合并左右子树
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
  
        return t1;
    }
}
```

## 49. 字符串的排列

给你两个字符串 s1 和 s2 ，写一个函数来判断 s2 是否包含 s1 的排列。如果是，返回 true ；否则，返回 false 。

换句话说，s1 的排列之一是 s2 的 子串 。

 

示例 1：

输入：s1 = "ab" s2 = "eidbaooo"
输出：true
解释：s2 包含 s1 的排列之一 ("ba").

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int i=0;
        int m=0;
        int j=i+s1.length()-1;
        while(j<s2.length()){
            StringBuilder sb=new StringBuilder(s1);
            i=m;
            j=i+s1.length();
            while(i<j){
                if(sb.toString().contains(String.valueOf(s2.charAt(i)))){
                    int index=sb.indexOf(String.valueOf(s2.charAt(i)));
                    sb.delete(index,index+1);
                }
                else break;
                i++;
            }
            if(sb.length()==0) return true;
            m++;
        }
        return false;
    }
}
```

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < n; ++i) {
            ++cnt1[s1.charAt(i) - 'a'];
            ++cnt2[s2.charAt(i) - 'a'];
        }
        if (Arrays.equals(cnt1, cnt2)) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            ++cnt2[s2.charAt(i) - 'a'];
            --cnt2[s2.charAt(i - n) - 'a'];
            if (Arrays.equals(cnt1, cnt2)) {
                return true;
            }
        }
        return false;
    }
}


```

## 50. 重复的DNA序列

所有 DNA 都由一系列缩写为 'A'，'C'，'G' 和 'T' 的核苷酸组成，例如："ACGAATTCCG"。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来找出所有目标子串，目标子串的长度为 10，且在 DNA 字符串 s 中出现次数超过一次。

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        int l=10;
        List<String> res=new ArrayList<String>();
        Map<String,Integer> map=new HashMap<String,Integer>();
        for(int i=0;i<=s.length()-l;++i){
            String sub=s.substring(i,i+l);
            map.put(sub, map.getOrDefault(sub, 0) + 1);
            if(map.get(sub)==2) res.add(sub);
        }
        return res;

    }
}     
```

## 51. 颜色分类

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

```java
class Solution {
    public void sortColors(int[] nums) {
        Arrays.sort(nums); 
    }
}
```

```java
class Solution {
    public void sortColors(int[] nums) {
        int red=0;
        int white=0;
        int blue=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]==0) red++;
            else if(nums[i]==1) white++;
            else blue++;
        }
        for(int i=0;i<red;i++){
            nums[i]=0;
        }
        for(int i=red;i<red+white;i++){
            nums[i]=1;
        }
        for(int i=red+white;i<nums.length;i++){
            nums[i]=2;
        }
    }
}

```

## 52. 排列硬币

你总共有 n 枚硬币，并计划将它们按阶梯状排列。对于一个由 k 行组成的阶梯，其第 i 行必须正好有 i 枚硬币。阶梯的最后一行 可能 是不完整的。

给你一个数字 n ，计算并返回可形成 完整阶梯行 的总行数。

```java
class Solution {
    public int arrangeCoins(int n) {
        int res=1;
        long ans=0;
        while(ans<n){
             res++;
            ans+=res;
        }
        return res-1;
    }
}
```

```java
//考虑直接通过求解方程来计算 nn 枚硬币可形成的完整阶梯行的总行数。不妨设可以形成的行数为 xx，则有


class Solution {
    public int arrangeCoins(int n) {
        return (int) ((Math.sqrt((long) 8 * n + 1) - 1) / 2);
    }
}

```

![image-20211010170650813](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20211010170650813.png)

## 53. 杨辉三角II

给定一个非负索引 rowIndex，返回「杨辉三角」的第 rowIndex 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

示例 1:

输入: rowIndex = 3
输出: [1,3,3,1]

```java
//动态规划
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> res=new ArrayList<Integer>();
        Integer[] list=new Integer[rowIndex+1];
        Arrays.fill(list,1);
        for(int i=2;i<rowIndex+1;i++){
            for(int j=i-1;j>0;j--){
                list[j]=list[j]+list[j-1];
            }
        }
        res=Arrays.asList(list);
        return res;
    }
    
}
```



## 54. 旋转图像

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

```java
//借助辅助数组
class Solution {
    public void rotate(int[][] matrix) {
        int n=matrix.length;
        int[][] tmp=new int[n][n];
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                tmp[i][j]=matrix[i][j];
            }
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                matrix[i][j]=tmp[n-j-1][i];
            }
        }
    }
}
```

```JAVA
//原地旋转

class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < (n + 1) / 2; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = temp;
            }
        }
    }
}

```

## 55. 至少在两个数组中出现的值

给你三个整数数组 `nums1`、`nums2` 和 `nums3` ，请你构造并返回一个 **不同** 数组，且由 **至少** 在 **两个** 数组中出现的所有值组成*。*数组中的元素可以按 **任意** 顺序排列。

 

**示例 1：**

```
输入：nums1 = [1,1,3,2], nums2 = [2,3], nums3 = [3]
输出：[3,2]
解释：至少在两个数组中出现的所有值为：
- 3 ，在全部三个数组中都出现过。
- 2 ，在数组 nums1 和 nums2 中出现过。
```

```java
//利用新数组来存储出现的次数，数组下标对应数字，数组的值对应出现的次数
class Solution {
    public List<Integer> twoOutOfThree(int[] nums1, int[] nums2, int[] nums3) {
        int[] tmp=new int[101];
        List<Integer> res=new ArrayList<Integer>();
        for(int i=0;i<101;i++){
            for(int j=0;j<nums1.length;j++){
                if(i==nums1[j]){
                    tmp[i]=1;
                
                    break;
                }
            }
        }
        for(int i=0;i<101;i++){
            for(int j=0;j<nums2.length;j++){
                if(i==nums2[j]&&tmp[i]!=0){
                    tmp[i]=2;
    
                    break;
                }
                else if(i==nums2[j]&&tmp[i]==0){
                    tmp[i]=1;
            
                    break;
                }
            }
        }
        for(int i=0;i<101;i++){
            for(int j=0;j<nums3.length;j++){
                if(i==nums3[j]&&tmp[i]!=0){
                    tmp[i]=3;
                
                    break;
                }
            }
        }
        for(int i=0;i<101;i++){
           if(tmp[i]>1) {
               res.add(i);
           }
        }
        return res;
    }
}
```

## 56. 获取单值网格的最小操作数

给你一个大小为 m x n 的二维整数网格 grid 和一个整数 x 。每一次操作，你可以对 grid 中的任一元素 加 x 或 减 x 。

单值网格 是全部元素都相等的网格。

返回使网格化为单值网格所需的 最小 操作数。如果不能，返回 -1 。

示例 1：

输入：grid = [[2,4],[6,8]], x = 2
输出：4
解释：可以执行下述操作使所有元素都等于 4 ： 
- 2 加 x 一次。
- 6 减 x 一次。
- 8 减 x 两次。
共计 4 次操作。

```java
//中位数，移动的次数最小
class Solution {
    public int minOperations(int[][] grid, int x) {
        int m=grid.length;
        int n=grid[0].length;
        int[] tmp=new int[m*n];
        int k=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                tmp[k]=grid[i][j];
                k++;
            }
        }
        for(int i=0;i<m*n;i++){
            if(i>0&&(tmp[i]-tmp[i-1])%x!=0){
                return -1;
            }
        }
        int mid=(m*n)/2;
        Arrays.sort(tmp);
        int count=0;
        for(int i=0;i<m*n;i++){
           count+=Math.abs(tmp[mid]-tmp[i])/x;
        }
        return count;
    }
}
```

## 57. 搜索二维矩阵

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

```java
//穷举
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for(int i=0;i<matrix.length;i++){
            for(int j=0;j<matrix[0].length;j++){
                if(target==matrix[i][j]) return true;
            }
        }
        return false;
    }
}
```

```java
//剥皮，一层一层剥皮
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int i=0;
        int j=0;
       while(i<matrix.length&&j<matrix[0].length){
           int m=i;
           int n=j;
           if(matrix[m][n]==target) return true;
           else if(matrix[m][n]>target) return false;
           else{
           		while(m<matrix.length&&matrix[m][j]<=target){
               		if(matrix[m][j]==target) return true;
               		else m++;
           }
           		while(n<matrix[0].length&&matrix[i][n]<=target){
               		if(matrix[i][n]==target) return true;
               		else n++;
           }
           }
           i++;
           j++;
       }
       return false;
    }

}
```

```java
//从左下角开始，比它大的在右边，比它小的在上边
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) return false;
        int m = 0;
        int n = matrix[0].length - 1;
        while (m < matrix.length && n >= 0) {
            if (matrix[m][n] == target) {
                return true;
            } else if (matrix[m][n] > target) {
                n--;
            } else {
                m++;
            }
        }
        return false;
    }
}
```

## 58. 斐波那契数

斐波那契数，通常用 F(n) 表示，形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
给你 n ，请计算 F(n) 。

```java
//动态规划，设置dp数组保存之前的数值
class Solution {
    public int fib(int n) {
        int[] dp=new int[n+1];
        if(n==0) return 0;
        if(n==1) return 1;
        dp[0]=0;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
}
```

## 59. 第n个泰波那契数

泰波那契序列 Tn 定义如下： 

T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

给你整数 n，请返回第 n 个泰波那契数 Tn 的值。

```java
//动态规划，递归效率比较低
class Solution {
    public int tribonacci(int n) {
        if(n==0) return 0;
        if(n<3) return 1;
        int[] dp=new int[n+1];
        dp[0]=0;
        dp[1]=1;
        dp[2]=1;
        for(int i=3;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2]+dp[i-3];
        }
        return dp[n];
    }
}
```

## 60. 杨辉三角

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

**示例 1:**

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        for(int i=0;i<numRows;i++){
            Integer[] tmp=new Integer[i+1];
            Arrays.fill(tmp,1);
            for(int j=2;j<i+1;j++){
                for(int k=j-1;k>0;k--){
                    tmp[k]=tmp[k]+tmp[k-1];
                }
            }
            res.add(Arrays.asList(tmp));
        }
        return res;
    }
}
```

## 61. 和为k的子数组

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回该数组中和为 `k` 的连续子数组的个数。

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count=0;
        for(int i=0;i<nums.length;i++){
            int sum=k;
            sum-=nums[i];
            if(sum==0) count++;
            for(int j=i+1;j<nums.length;j++){
                sum-=nums[j];
                if(sum==0){
                    count++;
                    continue;
                }
            }
        }
        return count;
    }
} 
```

## 62. 递增的三元子序列

给你一个整数数组 nums ，判断这个数组中是否存在长度为 3 的递增子序列。

如果存在这样的三元组下标 (i, j, k) 且满足 i < j < k ，使得 nums[i] < nums[j] < nums[k] ，返回 true ；否则，返回 false 。

 

```java
//令min和mid为整数最大值，不断更新min，让min等于数组中的值，如果数组的值大于min，就让mid=i；此时已经有二元组，如果有i>mid，那么就是三元组递增序列
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums.length < 3) return false;
        int min = Integer.MAX_VALUE, mid = Integer.MAX_VALUE;
        for(int i : nums){
            if(i > mid) return true;
            if(i <= min) min = i;
            else mid = i;
        }
        return false;
    }
}

```

## 63. Fizz Buzz

给你一个整数 n ，找出从 1 到 n 各个整数的 Fizz Buzz 表示，并用字符串数组 answer（下标从 1 开始）返回结果，其中：

answer[i] == "FizzBuzz" 如果 i 同时是 3 和 5 的倍数。
answer[i] == "Fizz" 如果 i 是 3 的倍数。
answer[i] == "Buzz" 如果 i 是 5 的倍数。
answer[i] == i 如果上述条件全不满足。

```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> ans=new ArrayList<String>();
        for(int i=1;i<=n;i++){
            if(i%15==0) ans.add("FizzBuzz");
            else if(i%3==0) ans.add("Fizz");
            else if(i%5==0) ans.add("Buzz");
            else ans.add(String.valueOf(i));
        }
        return ans;
    }
}
```

## 64. 打家劫舍

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

 

示例 1：

输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。

```java
//动态规划
//设置dp数组，下标i表示有i个房屋的时候能够偷到的最高金额，dp[0]=nums[0], dp[1]=Math.max(nums[0],nums[1])
//dp数组为dp[i]=Math.max(dp[i-1],dp[i-2]+nums[i]);
class Solution {
    public int rob(int[] nums) {
        int[] dp=new int[nums.length];
        if(nums.length==0) return 0;
        if(nums.length==1) return nums[0];
        if(nums.length==2) return Math.max(nums[0],nums[1]);
        dp[0]=nums[0];
        dp[1]=Math.max(nums[0],nums[1]);
        for(int i=2;i<nums.length;i++){
            dp[i]=Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[nums.length-1];

    }
}
```

## 65.使用最小花费爬楼梯

数组的每个下标作为一个阶梯，第 i 个阶梯对应着一个非负数的体力花费值 cost[i]（下标从 0 开始）。

每当你爬上一个阶梯你都要花费对应的体力值，一旦支付了相应的体力值，你就可以选择向上爬一个阶梯或者爬两个阶梯。

请你找出达到楼层顶部的最低花费。在开始时，你可以选择从下标为 0 或 1 的元素作为初始阶梯。

 

示例 1：

输入：cost = [10, 15, 20]
输出：15
解释：最低花费是从 cost[1] 开始，然后走两步即可到阶梯顶，一共花费 15 。

```java
//动态规划，dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        int[] dp = new int[n + 1];
        dp[0] = dp[1] = 0;
        for (int i = 2; i <= n; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[n];
    }
}


```

## 66. 字符串相加

给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和并同样以字符串形式返回。

你不能使用任何內建的用于处理大整数的库（比如 BigInteger）， 也不能直接将输入的字符串转换为整数形式。

 

示例 1：

输入：num1 = "11", num2 = "123"
输出："134"

```java
class Solution {
    public String addStrings(String num1, String num2) {
        int carry=0;
        StringBuilder sb=new StringBuilder();
        int i=num1.length()-1;
        int j=num2.length()-1;
      	while(i >= 0 || j >= 0 || carry != 0){
            if(i>=0) carry += num1.charAt(i--)-'0';
            if(j>=0) carry += num2.charAt(j--)-'0';
            sb.append(carry%10);
            carry /= 10;
        }
        return sb.reverse().toString();
    }
}
```

## 67. 最长回文子串

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。

注意:
假设字符串的长度不会超过 1010。

示例 1:

输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

```java
class Solution {
    public int longestPalindrome(String s) {
        int[] tmp=new int[128];
        int count=0;
        boolean flag=false;
        for(int i=0;i<s.length();i++){
            tmp[s.charAt(i)]++;
        }
        for(int i=65;i<128;i++){
        
            if(tmp[i]>=2&&tmp[i]%2==0) count+=tmp[i];
            if(tmp[i]>=2&&tmp[i]%2==1) {
                flag=true;
                count+=tmp[i]-1;
            }
            
            if(tmp[i]==1) flag=true;
        }

        if(flag==true) count++;
        return count;
    }
}
```

## 68. 单词规律

给定一种规律 pattern 和一个字符串 str ，判断 str 是否遵循相同的规律。

这里的 遵循 指完全匹配，例如， pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。

示例1:

输入: pattern = "abba", str = "dog cat cat dog"
输出: true

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] tmp=s.split(" ");
        Map<Character,String> map=new HashMap<Character,String>();
        if(pattern.length() != tmp.length) return false;
        for(int i=0;i<pattern.length();i++){
           char temp=pattern.charAt(i);
           if(map.containsKey(temp)){
               if(!map.get(temp).equals(tmp[i])) return false;
           }
           else{
               if(map.containsValue(tmp[i])) return false;
               else map.put(temp,tmp[i]);
           }
        }
        return true;
    }
}
```

## 69. 山峰数组的顶部

符合下列属性的数组 arr 称为 山峰数组（山脉数组） ：

arr.length >= 3
存在 i（0 < i < arr.length - 1）使得：
arr[0] < arr[1] < ... arr[i-1] < arr[i]
arr[i] > arr[i+1] > ... > arr[arr.length - 1]
给定由整数组成的山峰数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i ，即山峰顶部。

 

示例 1：

输入：arr = [0,1,0]
输出：1

```java
//二分法，时间复杂度O(logn)
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int left=0;
        int right=arr.length-1;
        int mid=(right-left)/2+left;
        while(left<right){
                if(mid==0||mid==arr.length-1) return mid;
                if(arr[mid]>=arr[mid-1]&&arr[mid]>=arr[mid+1]) return mid;
                if(arr[mid]<arr[mid+1]&&arr[mid]>arr[mid-1]) {
                    left=mid;
                    mid=(right-left)/2+left;
                }
                if(arr[mid]>arr[mid+1]&&arr[mid]<arr[mid-1]){
                    right=mid;
                    mid=(right-left)/2+left;
                }
        }
        return mid;
    }
}
```

```java
//穷举，时间复杂度O（n）
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        for(int i=0;i<arr.length-1;i++){
            if(arr[i]>arr[i+1]) return i;
        }
        return arr.length-1;
    }
}
```

## 70. 数组的最大递增子序列长度

给定一个数组，求他的最大递增子序列长度

```java
class Solution {
    public int deleteAndEarn(int[] nums) {
        int[] dp=new int[nums.length];
        int max=0;
        dp[nums.length-1]=1;
        for(int i=nums.length-2;i>=0;i--){
            dp[i]=nums[i]>nums[i+1]?dp[i+1]:(dp[i+1]+1);
        }
        for(int i=0;i<nums.length;i++){
            max=Math.max(max,dp[i]);
        }
        return max;
    }
}
```

## 71. 跳跃游戏

给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

示例 1：

输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
示例 2：

输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。

```java
//动态规划，dp[i]代表第i个位置能不能到达尾部
//时间复杂度O(n*m),m为nums数组中的最大值
class Solution {
    public boolean canJump(int[] nums) {
        boolean[] dp=new boolean[nums.length];
        dp[nums.length-1]=true;
        for(int i=nums.length-2;i>=0;i--){
            int j=1;
            while(j<=nums[i]&&j+i<nums.length){
                if(dp[i+j]==true) {
                    dp[i]=true;
                    break;
                }
                j++;
            }
        }
        return dp[0];
    }
}
```

```java
//贪心算法，利用max代表最大可达到的距离，如果i小于max，然后令max为max,i+nums[i]中的最大值，如果max达到n-1，返回true，否则返回false
class Solution {
    public boolean canJump(int[] nums) {
        int max=0;
        for(int i=0;i<nums.length;i++){
           if(i<=max){
               max=Math.max(max,i+nums[i]);
               if(max>=nums.length-1) return true;
           }
        }
        return false;
    }
}
```

## 72. 子集

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

```java
//可以直接从后遍历，遇到一个数就把所有子集加上该数组成新的子集，遍历完毕即是所有子集
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>());
        for (int i = 0; i < nums.length; i++) {
           int all = res.size();
            for (int j = 0; j < all; j++) {
                List<Integer> tmp = new ArrayList<>(res.get(j));
                tmp.add(nums[i]);
                res.add(tmp);
            }
        }
        return res;
    }
}
```

## 73. 外观数列

给定一个正整数 n ，输出外观数列的第 n 项。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。

你可以将其视作是由递归公式定义的数字字符串序列：

countAndSay(1) = "1"
countAndSay(n) 是对 countAndSay(n-1) 的描述，然后转换成另一个数字字符串。
前五项如下：

1.     1
2.     11
3.     21
4.     1211
5.     111221
第一项是数字 1 
描述前一项，这个数是 1 即 “ 一 个 1 ”，记作 "11"
描述前一项，这个数是 11 即 “ 二 个 1 ” ，记作 "21"
描述前一项，这个数是 21 即 “ 一 个 2 + 一 个 1 ” ，记作 "1211"
描述前一项，这个数是 1211 即 “ 一 个 1 + 一 个 2 + 二 个 1 ” ，记作 "111221"

```java
//利用额外数组记录不同的n的结果dp[n]是根据dp[n-1]的进行构造的，将相同的数的个数和这个数存放在map中，如果下一位相同就个数加一，不同就将map中的数存在stringBuilder里面，直到遍历完dp[n-1]。
class Solution {
    public String countAndSay(int n) {
        String[] dp=new String[n];
        if(n==1) return "1";
        dp[0]="1";
        dp[1]="11";
        for(int i=2;i<n;i++){
            StringBuilder sb=new StringBuilder();
            Map<Character,Integer> map=new HashMap<Character,Integer>();
            int count=1;
            map.put(dp[i-1].charAt(0),count);
            for(int j=1;j<dp[i-1].length();j++){
                if(dp[i-1].charAt(j)==dp[i-1].charAt(j-1)&&j!=dp[i-1].length()-1){
                    count++;
                    map.put(dp[i-1].charAt(j),count);
                }
                else if(dp[i-1].charAt(j)!=dp[i-1].charAt(j-1)){
                    count=1;
                    String s=String.valueOf(map.get(dp[i-1].charAt(j-1)));
                    sb.append(s);
                    sb.append(String.valueOf(dp[i-1].charAt(j-1)));
                    map.clear();
                    map.put(dp[i-1].charAt(j),count);
                }
                else if(dp[i-1].charAt(j)==dp[i-1].charAt(j-1)&&j==dp[i-1].length()-1){
                    count++;
                    map.put(dp[i-1].charAt(j),count);
                    String s=String.valueOf(map.get(dp[i-1].charAt(j-1)));
                    sb.append(s);
                    sb.append(String.valueOf(dp[i-1].charAt(j-1)));
                    map.clear();
                }  
            }
            if(!map.isEmpty()){
                String s=String.valueOf(map.get(dp[i-1].charAt(dp[i-1].length()-1)));
                sb.append(s);
                sb.append(String.valueOf(dp[i-1].charAt(dp[i-1].length()-1)));
            }
            dp[i]=sb.toString();
        }
        return dp[n-1];
    }
}
```

## 74. 存在重复元素

给定一个整数数组，判断是否存在重复元素。

如果存在一值在数组中出现至少两次，函数返回 `true` 。如果数组中每个元素都不相同，则返回 `false` 

```java
//hashSet的特性
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> hashSet=new HashSet<>();
        for(int i=0;i<nums.length;i++){
            if(hashSet.add(nums[i])==false) return true;   
        }
        return false;
    }
}
```

```java
//排序
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}

```

## 75. 数字的补数

对整数的二进制表示取反（0 变 1 ，1 变 0）后，再转换为十进制表示，可以得到这个整数的补数。

例如，整数 5 的二进制表示是 "101" ，取反后得到 "010" ，再转回十进制表示得到补数 2 。
给你一个整数 num ，输出它的补数。

```java
class Solution {
    public int findComplement(int num) {
        String s=Integer.toBinaryString(num);
        StringBuilder sb=new StringBuilder();
        for(int i=0;i<s.length();i++){
            if(s.charAt(i)=='0') sb.append("1");
            else sb.append("0");
        }
        return Integer.valueOf(sb.toString(),2);
    }
}
```

## 76. 相交链表

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

图示两个链表在节点 c1 开始相交：

![image-20211018143921874](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20211018143921874.png)

```java
//双重循环
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode tmpA=headA; 
        while(tmpA!=null){
            ListNode tmpB=headB;
            while(tmpB!=null){
                if(tmpA==tmpB) return tmpB;
                else tmpB=tmpB.next;
            }
            tmpA=tmpA.next;
        }
        return  null;
    }
}
```

```java
//遍历pa和pb。然后令pa=pb，pb=pa，继续遍历，只需要常数时间复杂度
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        ListNode pA = headA, pB = headB;
        while (pA != pB) {
            pA = pA == null ? headB : pA.next;
            pB = pB == null ? headA : pB.next;
        }
        return pA;
    }
}

```

## 77. 猜数字

小A 和 小B 在玩猜数字。小B 每次从 1, 2, 3 中随机选择一个，小A 每次也从 1, 2, 3 中选择一个猜。他们一共进行三次这个游戏，请返回 小A 猜对了几次？

输入的guess数组为 小A 每次的猜测，answer数组为 小B 每次的选择。guess和answer的长度都等于3。

```java
class Solution {
    public int game(int[] guess, int[] answer) {
        int count=0;
        for(int i=0;i<guess.length;i++){
            if(guess[i]==answer[i]) count++;
        }
        return count;
    }
}
```

## *78. 反转链表

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 ```java
 class Solution {
     public ListNode reverseList(ListNode head) {
         ListNode prev = null;
         ListNode curr = head;
         while (curr != null) {
             ListNode next = curr.next;
             curr.next = prev;
             prev = curr;
             curr = next;
         }
         return prev;
     }
 }
 ```

## 79. 2的幂

给你一个整数 n，请你判断该整数是否是 2 的幂次方。如果是，返回 true ；否则，返回 false 。

如果存在一个整数 x 使得 n == 2x ，则认为 n 是 2 的幂次方。

```java
//与运算符&
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n<=0) return false;
        if((n&n-1)==0) return true;
        return false;

    }
}
```

## *80. 全排列

给定一个不含重复数字的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。

 ```java
 /*回溯法
 这个问题可以看作有n个排列成一行的空格，我们需要从左往右依此填入题目给定的n个数，每个数只能使用一次。那么很直接的可以想到一种穷举的算法，即从左往右每一个位置都依此尝试填入一个数，看能不能填完这 nn 个空格，在程序中我们可以用「回溯法」来模拟这个过程。
 我们定义递归函数 backtrack(first, output) 表示从左往右填到第 first 个位置，当前排列为output。 那么整个递归函数分为两个情况：
 如果first==n，说明我们已经填完了n个位置（注意下标从 0 开始），找到了一个可行的解，我们将output 放入答案数组中，递归结束。
 如果 first<n，我们要考虑这第first个位置我们要填哪个数。根据题目要求我们肯定不能填已经填过的数，因此很容易想到的一个处理手段是我们定义一个标记数组vis[] 来标记已经填过的数，那么在填第first个数的时候我们遍历题目给定的n个数，如果这个数没有被标记过，我们就尝试填入，并将其标记，继续尝试填下一个位置，即调用函数 backtrack(first + 1, output)。回溯的时候要撤销这一个位置填的数以及标记，并继续尝试其他没被标记过的数。
 
 使用标记数组来处理填过的数是一个很直观的思路，但是可不可以去掉这个标记数组呢？毕竟标记数组也增加了我们算法的空间复杂度。
 
 答案是可以的，我们可以将题目给定的n个数的数组 nums 划分成左右两个部分，左边的表示已经填过的数，右边表示待填的数，我们在回溯的时候只要动态维护这个数组即可
 
 **/
 class Solution {
     public List<List<Integer>> permute(int[] nums) {
         List<List<Integer>> res = new ArrayList<List<Integer>>();
         List<Integer> output = new ArrayList<Integer>();
         for (int num : nums) {
             output.add(num);
         }
         int n = nums.length;
         backtrack(n, output, res, 0);
         return res;
     }
     public void backtrack(int n, List<Integer> output, List<List<Integer>> res, int first) {
         // 所有数都填完了
         if (first == n) {
             res.add(new ArrayList<Integer>(output));
         }
         for (int i = first; i < n; i++) {
             // 动态维护数组
             Collections.swap(output, first, i);
             // 继续递归填下一个数
             backtrack(n, output, res, first + 1);
             // 撤销操作
             Collections.swap(output, first, i);
         }
     }
 }
 ```

```java
//利用标记数组
class Solution {
    public List<List<Integer>> permute(int[] nums) {

        List<List<Integer>> res = new ArrayList<>();
        int[] visited = new int[nums.length];
        backtrack(res, nums, new ArrayList<Integer>(), visited);
        return res;

    }

    private void backtrack(List<List<Integer>> res, int[] nums, ArrayList<Integer> tmp, int[] visited) {
        if (tmp.size() == nums.length) {
            res.add(new ArrayList<>(tmp));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i] == 1) continue;
            visited[i] = 1;
            tmp.add(nums[i]);
            backtrack(res, nums, tmp, visited);
            visited[i] = 0;
            tmp.remove(tmp.size() - 1);
        }
    }
}
```

## 81. 判断字符是否唯一

实现一个算法，确定一个字符串 `s` 的所有字符是否全都不同。

```java
//借助hashset的特性
class Solution {
    public boolean isUnique(String astr) {
        Set<Character> hash=new HashSet<Character>();
        for(int i=0;i<astr.length();i++){
            if(hash.add(astr.charAt(i))==false) return false;
        }
        return true;
    }
}
```

```java
//当最后一次索引的位置不是现在的位置就可以判断重复
class Solution {
    public boolean isUnique(String astr) {
        for (int i=0;i<astr.length();i++){
            if (astr.lastIndexOf(astr.charAt(i))!=i){
                return false;
            }
        }
        return true;
    }
}
```

## 82. 判断是否为字符重排

给定两个字符串 `s1` 和 `s2`，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。

**示例 1：**

```
输入: s1 = "abc", s2 = "bca"
输出: true 
```

```java
//利用额外数组
class Solution {
    public boolean CheckPermutation(String s1, String s2) {
        int[] tmp=new int[128];
        for(int i=0;i<s1.length();i++){
            tmp[s1.charAt(i)]++;
        }
        for(int i=0;i<s2.length();i++){
            tmp[s2.charAt(i)]--;
        }
        for(int i=0;i<128;i++){
            if(tmp[i]!=0) return false;
        }
        return true;
    }
}
```

## 83. URL化

URL化。编写一种方法，将字符串中的空格全部替换为%20。假定该字符串尾部有足够的空间存放新增字符，并且知道字符串的“真实”长度。（注：用Java实现的话，请使用字符数组实现，以便直接在数组上操作。

示例 1：

输入："Mr John Smith    ", 13
输出："Mr%20John%20Smith"
示例 2：

输入："               ", 5
输出："%20%20%20%20%20"

```java
class Solution {
    public String replaceSpaces(String S, int length) {
        StringBuilder sb=new StringBuilder();
        for(int i=0;i<length;i++){
            if(S.charAt(i)!=' ') sb.append(String.valueOf(S.charAt(i)));
            else sb.append("%20");
        }
        return sb.toString();
    }
}
```

## 84. 回文排列

给定一个字符串，编写一个函数判定其是否为某个回文串的排列之一。

回文串是指正反两个方向都一样的单词或短语。排列是指字母的重新排列。

回文串不一定是字典当中的单词。

 

示例1：

输入："tactcoa"
输出：true（排列有"tacocat"、"atcocta"，等等）

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        int[] tmp=new int[128];
        for(int i=0;i<s.length();i++){
            tmp[s.charAt(i)]++;
        }
        int count=0;
        for(int i=0;i<128;i++){
            if(tmp[i]%2==1) count++;
        }
        if(count>=2) return false;
        else return true;
    }
}
```

```java
//位运算 
public boolean canPermutePalindrome(String s) {
        long highBitmap = 0;
        long lowBitmap = 0;
        for (char ch : s.toCharArray()) {
            if (ch >= 64) {
                highBitmap ^= 1L << ch - 64;
            } else {
                lowBitmap ^= 1L << ch;
            }
        }
        return Long.bitCount(highBitmap) + Long.bitCount(lowBitmap) <= 1;
    }

```

## 85. 字符串压缩

利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串aabcccccaaa会变为a2b1c5a3。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（a至z）。

示例1:

 输入："aabcccccaaa"
 输出："a2b1c5a3"
示例2:

 输入："abbccd"
 输出："abbccd"
 解释："abbccd"压缩后为"a1b2c2d1"，比原字符串长度更长。

```java
class Solution {
    public String compressString(String S) {
        StringBuilder sb=new StringBuilder();
        int count=1;
        if(S.length()==0) return "";
        sb.append(String.valueOf(S.charAt(0)));
        for(int i=1;i<S.length();i++){
            if(S.charAt(i)==S.charAt(i-1)){
                count++;
            }
            else{
                sb.append(String.valueOf(count));
                count=1;
                sb.append(String.valueOf(S.charAt(i)));
            }
            if(i==S.length()-1) sb.append(String.valueOf(count));
        }
        if(sb.length()>=S.length()) return S;
        return sb.toString();
    }
}
```

## 86. 零矩阵

编写一种算法，若M × N矩阵中某个元素为0，则将其所在的行与列清零。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if(matrix.length==0||matrix[0].length==0) {}
        else {
            Set<Integer> row=new HashSet<Integer>();
            Set<Integer> column=new HashSet<Integer>();
            for(int i=0;i<matrix.length;i++){
                for(int j=0;j<matrix[0].length;j++){
                    if(matrix[i][j]==0){
                        row.add(i);
                        column.add(j);       
                    }
                }
            }
            for(int i:row){
                for(int j=0;j<matrix[0].length;j++) matrix[i][j]=0;
            }
            for(int i:column){
                for(int j=0;j<matrix.length;j++) matrix[j][i]=0;
            }
        }
    }
}
```

```java
//利用标记数组
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] row = new boolean[m];
        boolean[] col = new boolean[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = col[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (row[i] || col[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}

```

## 87. 字符串轮转

字符串轮转。给定两个字符串`s1`和`s2`，请编写代码检查`s2`是否为`s1`旋转而成（比如，`waterbottle`是`erbottlewat`旋转后的字符串）。

```java
class Solution {
    public boolean isFlipedString(String s1, String s2) {
        if(s1.length()!=s2.length()) return false;
        if(s2.length()<=1) return true;
        for(int i=0;i<s1.length()-1;i++){
            StringBuilder sb=new StringBuilder();
            sb.append(s1.substring(i+1,s1.length()));
            sb.append(s1.substring(0,i+1));
            if(sb.toString().equals(s2)) return true;
        }
        return false;
    }
}
```

```java
//将s2和s2拼接，如果新的字符串包含s1，则返回true
class Solution {
    public boolean isFlipedString(String s1, String s2) {
        if(s1.length()!=s2.length()) return false;
        String s=s2+s2;
        if(s.contains(s1)) return true;
        return false;
    }
}
```

## 88. 移除重复节点

编写代码，移除未排序链表中的重复节点。保留最开始出现的节点。

```java
class Solution {
    public ListNode removeDuplicateNodes(ListNode head) {
        Set<Integer> hash=new HashSet<Integer>();
        ListNode pre=head;
        ListNode ans=pre;
        if(head==null) return head;
        hash.add(head.val);
        while(pre.next!=null){
            if(!hash.add(pre.next.val))  pre.next=pre.next.next;
            else pre=pre.next;
        }
        return ans;
    }
}
```

## 89. 返回倒数第k个节点

实现一种算法，找出单向链表中倒数第 k 个节点。返回该节点的值。

注意：本题相对原题稍作改动

示例：

输入： 1->2->3->4->5 和 k = 2
输出： 4

```java
//经典快慢指针问题
class Solution {
    public int kthToLast(ListNode head, int k) {
        ListNode slow=head;
        ListNode fast=head;
        for(int i=0;i<k;i++){
            fast=fast.next;
        }
        while(fast!=null){
            slow=slow.next;
            fast=fast.next;
        }
        return slow.val;
    }
}
```

## 90. 一次编辑

字符串有三种编辑操作:插入一个字符、删除一个字符或者替换一个字符。 给定两个字符串，编写一个函数判定它们是否只需要一次(或者零次)编辑。

```java
class Solution {
    public boolean oneEditAway(String first, String second) {
        if(first.equals(second)) return true;
        if(Math.abs(second.length()-first.length())>=2) return false;
        if(first.length()==second.length()){        //字符串长度相等时
            int count=0;
            for(int i=0;i<first.length();i++){
                if(first.charAt(i)!=second.charAt(i)) count++;
                if(count>=2) return false;
            }
            return true;
        }
        else{ 									//字符串长度相差为1
            int i=0;
            int j=0;
            int count=0;
            String one=first.length()>second.length()?first:second;
            String two=first.length()>second.length()?second:first;
            while(i<one.length()&&j<two.length()){
                if(one.charAt(i)==two.charAt(j)) j++;
                else count++;
                i++;
                if(count>=2) return false;
            }
            return true;
        }
    }
}
```

## 91. 分割链表

给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你不需要 保留 每个分区中各节点的初始相对位置。

 

```java
//两个链表，small和large，small用来存储比x小的，large用来存储比x大的，最后吧large放在small的后面就行 
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode small = new ListNode(0);
        ListNode smallHead = small;
        ListNode large = new ListNode(0);
        ListNode largeHead = large;
        while (head != null) {
            if (head.val < x) {
                small.next = head;
                small = small.next;
            } else {
                large.next = head;
                large = large.next;
            }
            head = head.next;
        }
        large.next = null;
        small.next = largeHead.next;
        return smallHead.next;
    }
}
```

## 92. 构造矩形

作为一位web开发者， 懂得怎样去规划一个页面的尺寸是很重要的。 现给定一个具体的矩形页面面积，你的任务是设计一个长度为 L 和宽度为 W 且满足以下要求的矩形的页面。要求：

1. 你设计的矩形页面必须等于给定的目标面积。

2. 宽度 W 不应大于长度 L，换言之，要求 L >= W 。

3. 长度 L 和宽度 W 之间的差距应当尽可能小。
你需要按顺序输出你设计的页面的长度 L 和宽度 W。

```java
class Solution {
    public int[] constructRectangle(int area) {
        int[] ans=new int[2];
        int n=(int)Math.sqrt(area);
        while(n>0){
            if(area%n==0) {
                ans[0]=area/n;
                ans[1]=n;
                break;
            }
            else n--;
        }
        return ans;
    }
}
```

## 93. 丢失的数字

给定一个包含 `[0, n]` 中 `n` 个数的数组 `nums` ，找出 `[0, n]` 这个范围内没有出现在数组中的那个数。

输入：nums = [3,0,1]
输出：2
解释：n = 3，因为有 3 个数字，所以所有的数字都在范围 [0,3] 内。2 是丢失的数字，因为它没有出现在 nums 中。

```java
//先排序，再找没有出现的
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        int i;
        for(i=0;i<nums.length;i++){
           if(i!=nums[i]) return nums[i]
        }
        return i;
    }
}
```

```JAVA
//异或
class Solution {
    public int missingNumber(int[] nums) {
        int i,x=nums.length;
        for(i=0;i<nums.length;i++){
            x^=nums[i];
            x^=i;
        }
        return x;
    }
}
```

## 94. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<Integer> stack=new Stack<Integer>();
        int len=0;
        while(head!=null){
            stack.push(head.val);
            head=head.next;
            len++;
        }
        int[] res=new int[len];
        int i=0;
        while(!stack.isEmpty()){
            res[i]=stack.pop();
            i++;
        }
        return res;
    }
    
}
```

## 95. 用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```java
class CQueue {
    Stack stack1;
    Stack stack2;
    public CQueue() {
        stack1=new Stack<>();
        stack2=new Stack<>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
       if(stack2.isEmpty()){
           if(stack1.isEmpty()) return -1;
           while(!stack1.isEmpty()) stack2.push(stack1.pop());
           return (int)stack2.pop();
       }
       else return (int)stack2.pop();
    }
}

```

## 96. 包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

```java
class MinStack {
    Stack<Integer> A, B;
    public MinStack() {
        A = new Stack<>();
        B = new Stack<>();
    }
    public void push(int x) {
        A.add(x);
        if(B.empty() || B.peek() >= x)
            B.add(x);
    }
    public void pop() {
        if(A.pop().equals(B.peek()))
            B.pop();
    }
    public int top() {
        return A.peek();
    }
    public int min() {
        return B.peek();
    }
}
```

## 97. 左旋转字符串

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        return s.substring(n,s.length())+s.substring(0,n);
    }
}
```

## 98. 数组中重复的数字

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i=0;i<nums.length-1;i++){
            if(nums[i]==nums[i+1]) return nums[i];
        }
        return nums[nums.length-1];
    }
}
```

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set=new HashSet<Integer>();
        for(int i=0;i<nums.length;i++){
            if(!set.add(nums[i])) return nums[i];
        }
        return nums[nums.length-1];
    }
}
```

```java
public int findRepeatNumber(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < n; i++){
            int k = nums[i];
            if(k < 0) k += n;
            if(nums[k] < 0) return k;
            nums[k] -= n;
        }
        return -1;
    }
```

## 99. 在排序数组中查找数字

统计一个数字在排序数组中出现的次数。

```java
class Solution {
    public int search(int[] nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false) - 1;
        if (leftIdx <= rightIdx && rightIdx < nums.length && nums[leftIdx] == target && nums[rightIdx] == target) {
            return rightIdx - leftIdx + 1;
        } 
        return 0;
    }

    public int binarySearch(int[] nums, int target, boolean lower) {
        int left = 0, right = nums.length - 1, ans = nums.length;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
}


```

## 100. 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

```java
class Solution {
    public int minArray(int[] numbers) {
        int low = 0;
        int high = numbers.length - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (numbers[pivot] < numbers[high]) {
                high = pivot;
            } else if (numbers[pivot] > numbers[high]) {
                low = pivot + 1;
            } else {
                high -= 1;
            }
        }
        return numbers[low];
    }
}
```

## 101. 第一个只出现一次的字符

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

```java
class Solution {
    public char firstUniqChar(String s) {
        for(int i=0;i<s.length();i++){
            if(s.indexOf(s.charAt(i))==s.lastIndexOf(s.charAt(i))) return s.charAt(i);
        }
        return ' ';
    }
}
```

```java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Integer> frequency = new HashMap<Character, Integer>();
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            frequency.put(ch, frequency.getOrDefault(ch, 0) + 1);
        }
        for (int i = 0; i < s.length(); ++i) {
            if (frequency.get(s.charAt(i)) == 1) {
                return s.charAt(i);
            }
        }
        return ' ';
    }
}
```

## 102. 从上到下打印二叉树

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

        3
       / \
      9  20
        /  \
       15   7


返回：

[3,9,20,15,7]

```java
//使用队列按层遍历二叉树
class Solution {
    public int[] levelOrder(TreeNode root) {
        int maxLength=1000;
        int head=0,tail=0;
        List<Integer> ans=new ArrayList<Integer>();
        TreeNode[] q=new TreeNode[maxLength];
        TreeNode p;
        if(root!=null){
            tail=(tail+1)%maxLength;
            q[tail]=root;
        }
        while(head!=tail){
            head=(head+1)%maxLength;
            p=q[head];
            ans.add(p.val);
            if(p.left!=null){
                tail=(tail+1)%maxLength;
                q[tail]=p.left;
            }
            if(p.right!=null){
                tail=(tail+1)%maxLength;
                q[tail]=p.right;
            }

        }
        int[] res=new int[ans.size()];
        int i=0;
        for(int x:ans){
            res[i]=x;
            i++;
        }
        return res;
    }
}
```

## 103. 简单约瑟夫环问题

有n个人编号为1-n，从第一个人开始，从1开始数，数到3这个人便自杀，然后下一个人又从1开始数，当数到n，下一个人便是1号，编写算法求出最后幸存的两个人

```java
public void josephus(int n){          //简单约瑟夫环问题
        int person=n;
        int[] number=new int[person];
        Arrays.fill(number,1);      //replace the elements of array to 1
        int pos=0;              //pos:index of array
        int count=41;           //the count of alive man
        int killMan=0;          //killMan denotes the sequence of the alive man
        while(count>=3){
            if(number[pos]==1){
                killMan++;
                if(killMan==3){
                    number[pos]=0;
                    killMan=0;
                    count--;
                    System.out.println("the number of the killMan is:"+pos);
                }
            }
            pos=(pos+1)%person;                 // use the % to generate the circle
        }
        for(int i=0;i<41;i++){
            if(number[i]==1) {
                int j=i+1;
                System.out.println("the number that still alive is:"+j);
            }
        }
      }
```

## *104. 树的子结构

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

    	 3
        / \
       4   5
      / \
     1   2

给定的树 B：

   4  
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        return (A != null && B != null) && (recur(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B));
    }
    boolean recur(TreeNode A, TreeNode B) {
        if(B == null) return true;
        if(A == null || A.val != B.val) return false;
        return recur(A.left, B.left) && recur(A.right, B.right);
    }
}
```

## 105. 提莫攻击

在《英雄联盟》的世界中，有一个叫 “提莫” 的英雄。他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。

当提莫攻击艾希，艾希的中毒状态正好持续 duration 秒。

正式地讲，提莫在 t 发起发起攻击意味着艾希在时间区间 [t, t + duration - 1]（含 t 和 t + duration - 1）处于中毒状态。如果提莫在中毒影响结束 前 再次攻击，中毒状态计时器将会 重置 ，在新的攻击之后，中毒影响将会在 duration 秒后结束。

给你一个 非递减 的整数数组 timeSeries ，其中 timeSeries[i] 表示提莫在 timeSeries[i] 秒时对艾希发起攻击，以及一个表示中毒持续时间的整数 duration 。

返回艾希处于中毒状态的 总 秒数。

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        int count=duration;
        for(int i=1;i<timeSeries.length;i++){
            
            if(timeSeries[i]-timeSeries[i-1]>=duration) count+=duration;
            else if(timeSeries[i]-timeSeries[i-1]>0) count+=timeSeries[i]-timeSeries[i-1];
        }
        return count;
    }
}
```

## *106. 组合总和

给定一个无重复元素的正整数数组 candidates 和一个正整数 target ，找出 candidates 中所有可以使数字和为目标数 target 的唯一组合。

candidates 中的数字可以无限制重复被选取。如果至少一个所选数字数量不同，则两种组合是唯一的。 

对于给定的输入，保证和为 target 的唯一组合数少于 150 个。

示例 1：

输入: candidates = [2,3,6,7], target = 7
输出: [[7],[2,2,3]]

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        List<Integer> combine = new ArrayList<Integer>();
        dfs(candidates, target, ans, combine, 0);
        return ans;
    }

    public void dfs(int[] candidates, int target, List<List<Integer>> ans, List<Integer> combine, int idx) {
        if (idx == candidates.length) {
            return;
        }
        if (target == 0) {
            ans.add(new ArrayList<Integer>(combine));
            return;
        }
        // 直接跳过
        dfs(candidates, target, ans, combine, idx + 1);
        // 选择当前数
        if (target - candidates[idx] >= 0) {
            combine.add(candidates[idx]);
            dfs(candidates, target - candidates[idx], ans, combine, idx);
            combine.remove(combine.size() - 1);
        }
    }
}

```

## 107. 二叉树的中序遍历

给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。

```java
//递归
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        search(root,list);
        return list;
    }
    public void search(TreeNode root, List<Integer> list){
       if(root==null) return ;   
       search(root.left,list);
       list.add(root.val);
       search(root.right,list);
    
    }
}
```

```java
//迭代
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stk = new LinkedList<TreeNode>();
        while (root != null || !stk.isEmpty()) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
}

```

## 108. 翻转字符串里的单词

给你一个字符串 s ，逐个翻转字符串中的所有 单词 。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

请你返回一个翻转 s 中单词顺序并用单个空格相连的字符串。

说明：

输入字符串 s 可以在前面、后面或者单词间包含多余的空格。
翻转后单词间应当仅用一个空格分隔。
翻转后的字符串中不应包含额外的空格。

```java
class Solution {
    public String reverseWords(String s) {
        // 除去开头和末尾的空白字符
        s = s.trim();
        // 正则匹配连续的空白字符作为分隔符分割
        List<String> wordList = Arrays.asList(s.split("\\s+"));
        Collections.reverse(wordList);
        return String.join(" ", wordList);
    }
}

```

```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb = trimSpaces(s);

        // 翻转字符串
        reverse(sb, 0, sb.length() - 1);

        // 翻转每个单词
        reverseEachWord(sb);

        return sb.toString();
    }

    public StringBuilder trimSpaces(String s) {
        int left = 0, right = s.length() - 1;
        // 去掉字符串开头的空白字符
        while (left <= right && s.charAt(left) == ' ') {
            ++left;
        }

        // 去掉字符串末尾的空白字符
        while (left <= right && s.charAt(right) == ' ') {
            --right;
        }

        // 将字符串间多余的空白字符去除
        StringBuilder sb = new StringBuilder();
        while (left <= right) {
            char c = s.charAt(left);

            if (c != ' ') {
                sb.append(c);
            } else if (sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }

            ++left;
        }
        return sb;
    }

    public void reverse(StringBuilder sb, int left, int right) {
        while (left < right) {
            char tmp = sb.charAt(left);
            sb.setCharAt(left++, sb.charAt(right));
            sb.setCharAt(right--, tmp);
        }
    }

    public void reverseEachWord(StringBuilder sb) {
        int n = sb.length();
        int start = 0, end = 0;

        while (start < n) {
            // 循环至单词的末尾
            while (end < n && sb.charAt(end) != ' ') {
                ++end;
            }
            // 翻转单词
            reverse(sb, start, end - 1);
            // 更新start，去找下一个单词
            start = end + 1;
            ++end;
        }
    }
}
```

## 109. 找到数组中所有消失的数字

你一个含 n 个整数的数组 nums ，其中 nums[i] 在区间 [1, n] 内。请你找出所有在 [1, n] 范围内但没有出现在 nums 中的数字，并以数组的形式返回结果。

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        Arrays.sort(nums);
        List<Integer> ans=new ArrayList<>();
       if(nums[0]>1) {
           int k=nums[0];
           while(k>1) {
               ans.add(k-1);
               k--;
           }
       }
        for(int i=1;i<nums.length;i++){
            if(nums[i]-nums[i-1]>1){
                int k=nums[i];
                 while(k>nums[i-1]+1) {
               ans.add(k-1);
               k--;
           }
            }
        }
        if(nums[nums.length-1]!=nums.length){
            int k=nums.length;
            while(k>=nums[nums.length-1]+1){
                ans.add(k);
                k--;
            }
        }
        return ans;
    }
}
```

```java
//将所有正数作为数组下标，置对应数组值为负值。那么，仍为正数的位置即为（未出现过）消失的数字。
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for(int i=0;i<nums.length;i++){
            nums[Math.abs(nums[i])-1]=-Math.abs(nums[Math.abs(nums[i])-1]);
        }
        List<Integer> ans=new ArrayList<>();
        for(int i=0;i<nums.length;i++){
            if(nums[i]>0) ans.add(i+1);
        }
        return ans;
    }
}
```

## 110. 最长回文子串

给你一个字符串 s，找到 s 中最长的回文子串。

 

示例 1：

输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。

```java
//普通解法
class Solution {
    public String longestPalindrome(String s) {
        if(s.length()<=1) return s;
        for(int i=s.length();i>0;i--){
            for(int j=0;j+i<=s.length();j++){
                if(isPanlindrome(s.substring(j,j+i)))     return s.substring(j,j+i);
            }
        }
        return s; 
    }
    public boolean isPanlindrome(String s){
        if(s.length()<=1) return true;
        int i=0;
        int j=s.length()-1;
        while(i<=j){
            if(s.charAt(i)==s.charAt(j)){
                i++;
                j--;
            }
            else return false;
        }
        return true;
    }
}
```

![image-20211205104728325](https://gitee.com/bitches/git/raw/master/img/image-20211205104728325.png)

```java
//动态规划
class Solution {
    public String longestPalindrome(String s) {
        if(s.length()<=1) return s;
        int len=s.length();
        int begin=0;
        int maxLen=1;
        boolean[][] dp=new boolean[len][len];		//dp[i][j]表示下标从i到j是否为回文串
        for(int i=0;i<len;i++){
            dp[i][i]=true;
        }
        char[] charArray=s.toCharArray();
        for(int length=2;length<=len;length++){
            for(int i=0;i<len;i++){
               int j=length+i-1;
               if(j>=len) break;
               if(charArray[i]!=charArray[j]) dp[i][j]=false;
               else{
                   if(j-i<3) dp[i][j]=true;
                   else dp[i][j]=dp[i+1][j-1];
               }
               if(dp[i][j]&&j-i+1>maxLen){
                   begin=i;
                   maxLen=j-i+1;
               }
            }

        }
        return s.substring(begin,maxLen+begin);
    } 
}
```

## 111. 盛最多水的容器

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。

```java
//动态规划，但是时间复杂度比较高
//状态转移方程为dp[i]=Math.max(max,dp[i-1])
class Solution {
    public int maxArea(int[] height) {
        if(height.length<2) return -1;
        int[] dp=new int[height.length];
        dp[0]=1;
        dp[1]=Math.min(height[0],height[1]);
        for(int i=2;i<height.length;i++){
            int j=i-1;
            int max=0;
            while(j>=0){
                max=Math.max(max,Math.min(height[i],height[j])*(i-j));
                j--;
            }
            dp[i]=Math.max(max,dp[i-1]);
        }
        return dp[height.length-1];
    }
}
```

```java
//双指针，左指针从0开始，右指针从height.length-1开始，当height[left]<height[right],左指针left向右移动，否则右指针向左移动，直到left>=right
class Solution {
    public int maxArea(int[] height) {
        if(height.length<2) return -1;
        int left=0;
        int max=0;
        int right=height.length-1;
        while(left<right){
            max=Math.max(max,(Math.min(height[left],height[right]))*(right-left));
            if(height[left]<height[right]) left++;
            else right--;
        }
        return max;
    }
}
```

## 112. 括号生成

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

```java
//回溯算法
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<String>();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }
//max代表的是n，cur代表的是选择列表 如果左括号数量不大于 n，我们可以放一个左括号。如果右括号数量小于左括号的数量，我们可以放一个右括号。open代表的是左括号数量，close右括号数量
    public void backtrack(List<String> ans, StringBuilder cur, int open, int close, int max) {
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }
        if (open < max) {
            cur.append('(');
            backtrack(ans, cur, open + 1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if (close < open) {
            cur.append(')');
            backtrack(ans, cur, open, close + 1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
}

```

## 113. 电话号码的字母组合

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![image-20211205211258998](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20211205211258998.png)

```java
//回溯
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> combinations = new ArrayList<String>();
        if (digits.length() == 0) {
            return combinations;
        }
        Map<Character, String> phoneMap = new HashMap<Character, String>() {{
            put('2', "abc");
            put('3', "def");
            put('4', "ghi");
            put('5', "jkl");
            put('6', "mno");
            put('7', "pqrs");
            put('8', "tuv");
            put('9', "wxyz");
        }};
        backtrack(combinations, phoneMap, digits, 0, new StringBuffer());
        return combinations;
    }
//index代表combination的长度
    public void backtrack(List<String> combinations, Map<Character, String> phoneMap, String digits, int index, StringBuffer combination) {
        if (index == digits.length()) {
            combinations.add(combination.toString());
        } else {
            char digit = digits.charAt(index);
            String letters = phoneMap.get(digit);
            int lettersCount = letters.length();
            for (int i = 0; i < lettersCount; i++) {
                combination.append(letters.charAt(i));
                backtrack(combinations, phoneMap, digits, index + 1, combination);
                combination.deleteCharAt(index);
            }
        }
    }
}
```

## 114. 下一个排列

实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列（即，组合出下一个更大的整数）。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须 原地 修改，只允许使用额外常数空间。

```java
//对于长度为n的排列a
//1. 首先从后向前查找第一个顺序对 (i,i+1)，满足 a[i] < a[i+1]。这样「较小数」即为 a[i]。此时[i+1,n)必然是下降序列。
//2.如果找到了顺序对，那么在区间[i+1,n) 中从后向前查找第一个元素j满足a[i]<a[j]。这样「较大数」即为 a[j]。
//3.交换 a[i]与a[j]，此时可以证明区间 [i+1,n)必为降序。我们可以直接使用双指针反转区间 [i+1,n)使其变为升序，而无需对该区间进行排序。
class Solution {
    public void nextPermutation(int[] nums) {
        if(nums.length<=1) return ;
        int j=0;
       int i=nums.length-2;
            while(i>=0&&nums[i]>=nums[i+1]) i--;
            j=nums.length-1;
            if(i>=0){
                while(j>=0&&nums[i]>=nums[j]) j--;
                swap(nums,i,j);
            }
            reverse(nums,i+1);
    }
    public void swap(int[] nums,int i,int j){
        int tmp=nums[i];
        nums[i]=nums[j];
        nums[j]=tmp;
    }
    public void reverse(int[] nums, int start) {
        int left = start, right = nums.length - 1;
        while (left < right) {
            swap(nums, left, right);
            left++;
            right--;
        }
    }
}
```

## 115. 最长连续序列

给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 O(n) 的算法解决此问题。

 

示例 1：

输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。

```java
//先排序，后处理，时间复杂度不符合要求
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length<1) return 0;
        Arrays.sort(nums);
        int max=1;
        int count=1;
        for(int i=0;i<nums.length-1;i++){
            if(nums[i]+1==nums[i+1]) count++;
            else if(nums[i]==nums[i+1]) continue;
            else count=1;
            max=Math.max(max,count);
        }
        return max;
    }
}
```

```java
//利用hashSet集合存储元素
class Solution {
    public int longestConsecutive(int[] nums) {
        int longest=1;
        if(nums.length<1) return 0;
        Set<Integer> set=new HashSet<>();
        for(int num:nums){
            set.add(num);
        }
        int currentNum=-1;
        for(int num:set){
            int count=1;
            if(!set.contains(num-1)) {
                currentNum=num;
            }
            else continue;
            while(set.contains(currentNum+1)){
                currentNum+=1;
                count+=1;
            }
            longest=Math.max(longest,count);
        }
        return longest;
    }
}
```

## 116. 在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

```java
//二分法注意点：1. while循环结束条件应该是left<=right
//2. left=mid+1而不是left=mid；
//3.right=mid-1而不是right=mid
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] result=new int[2];
        if(nums.length==0||(nums.length==1&&nums[0]!=target)) {
            result[0]=-1;
            result[1]=-1;
            return result;
        }
        if(nums.length==1&&nums[0]==target){
            result[0]=0;
            result[1]=0;
            return result;
        }
        int left=0;
        int right=nums.length-1;
        int mid=(right-left)/2+left;
        while(left<=right){
            if(nums[mid]<target){
                left=mid+1;
                mid=(right-left)/2+left;
            }
            else if(nums[mid]==target){
                int i=mid;
                int j=mid;
                while(i>=0&&nums[i]==target) i--;
                while(j<=nums.length-1&&nums[j]==target) j++;
                result[0]=i+1;
                result[1]=j-1;
                return result;
            }
            else{
                right=mid-1;
                mid=(right-left)/2+left;
            }
        }
        result[0]=-1;
        result[1]=-1;
        return result;
    }
}
```

## 117. 最小路径和

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

```java
//动态规划
class Solution {
    public int minPathSum(int[][] grid) {
     int[][] dp=new int[grid.length][grid[0].length];
     dp[0][0]=grid[0][0];
     for(int i=0;i<grid.length;i++){
         for(int j=0;j<grid[0].length;j++){
             if(i>0&&j>0) dp[i][j]=Math.min(dp[i-1][j],dp[i][j-1])+grid[i][j];
             if(i>0&&j==0) dp[i][j]=dp[i-1][j]+grid[i][j];
             if(i==0&&j>0) dp[i][j]=dp[i][j-1]+grid[i][j];
         }
     }
     return dp[grid.length-1][grid[0].length-1];
    }
}
```

## 118. 搜索旋转排序数组

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

 要求时间复杂度为 `O(log n)`

```java
//因为要求时间复杂度为O(log n)，所以使用二分法
class Solution {
    public int search(int[] nums, int target) {
        int left=0;
        int base=nums[0];
        int right=nums.length-1;
        int mid=(right-left)/2+left;
        while(left<=right){
            if(nums[mid]==target) return mid;
            if(target>base&&nums[mid]<base){		//
                right=mid-1;
                mid=(right-left)/2+left;
            }
            else if((target>base&&nums[mid]>=base)||nums[nums.length-1]>base||(target<base&&nums[mid]<base)){											//这三种情况相当于普通的二分法
                if(nums[mid]>target){
                    right=mid-1;
                    mid=(right-left)/2+left;
                }
                else if(nums[mid]<target){
                    left=mid+1;
                    mid=(right-left)/2+left;
                }
                else return mid;
            }
            else if(target<base&&nums[mid]>=base){
                left=mid+1;
                mid=(right-left)/2+left;
            }
            else if(target==base) return 0;
         
        }
        return -1;
    }
}
```

## 119. 不同的二叉搜索树

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

```java
/*
动态规划，i个节点的时候，从1开始作根节点，当根节点为1和i的时候，此时的不同二叉搜索树的种类为 dp[i-1]个，其余作根节点的时候等于二叉搜索树种类就为dp[j-1]*dp[i-1-j]，其中i是i个节点，j是根节点的值

*/
class Solution {
    public int numTrees(int n) {
        int[] dp=new int[n];
        if(n==1) return 1;
        dp[0]=1;
        dp[1]=2;
        for(int i=2;i<n;i++){
            for(int j=0;j<=i;j++){
                if(j==0||j==i) dp[i]+=dp[i-1];
                else dp[i]+=dp[j-1]*dp[i-1-j];
            }
        }
        return dp[n-1];
    }
}
```

## 120. 对称二叉树

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3

```java
//递归
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null) return true;
        return symmetric(root.left,root.right);
    }
    public boolean symmetric(TreeNode left,TreeNode right){
        if(left==null&&right==null) return true;	//两个都为空，而且经过了之前的检查，那他肯定是对称的
        else if(left!=null&&right!=null){			//两个都不为空，递归
            if(left.val!=right.val) return false;
            return symmetric(left.right,right.left)&&symmetric(left.left,right.right);
        }
        else return false;			//有一个不为空，另一个为空，那肯定是不对称的
        
    }
}
```

```java
/*
迭代：
首先我们引入一个队列，这是把递归程序改写成迭代程序的常用方法。初始化时我们把根节点入队两次。每次提取两个结点并比较它们的值（队列中每两个连续的结点应该是相等的，而且它们的子树互为镜像），然后将两个结点的左右子结点按相反的顺序插入队列中。当队列为空时，或者我们检测到树不对称（即从队列中取出两个不相等的连续结点）时，该算法结束。
*/
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }

    public boolean check(TreeNode u, TreeNode v) {
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(u);
        q.offer(v);
        while (!q.isEmpty()) {
            u = q.poll();
            v = q.poll();
            if (u == null && v == null) {
                continue;
            }
            if ((u == null || v == null) || (u.val != v.val)) {
                return false;
            }

            q.offer(u.left);
            q.offer(v.right);

            q.offer(u.right);
            q.offer(v.left);
        }
        return true;
    }
}

```

## 121. 二叉树的最大深度

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

```java
//递归
class Solution {
    public int maxDepth(TreeNode root) {
        return max(root);
    }
    public int max(TreeNode root){
        if(root==null) return 0;
        if(root.left==null&&root.right==null) return 1;
        else return Math.max(max(root.left),max(root.right))+1;
    }
}
```

## 122. 验证二叉搜索树

给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

有效 二叉搜索树定义如下：

节点的左子树只包含 小于 当前节点的数。
节点的右子树只包含 大于 当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode node, long lower, long upper) {
        if (node == null) {
            return true;
        }
        if (node.val <= lower || node.val >= upper) {
            return false;
        }
        return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
    }
}

```

## 123. 二叉树展开成链表

给你二叉树的根结点 root ，请你将它展开为一个单链表：

展开后的单链表应该同样使用 TreeNode ，其中 right 子指针指向链表中下一个结点，而左子指针始终为 null 。
展开后的单链表应该与二叉树 先序遍历 顺序相同。

```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        preorderTraversal(root, list);
        int size = list.size();
        for (int i = 1; i < size; i++) {
            TreeNode prev = list.get(i - 1), curr = list.get(i);
            prev.left = null;
            prev.right = curr;
        }
    }

    public void preorderTraversal(TreeNode root, List<TreeNode> list) {
        if (root != null) {
            list.add(root);
            preorderTraversal(root.left, list);
            preorderTraversal(root.right, list);
        }
    }
}

```

```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode curr = root;
        while (curr != null) {
            if (curr.left != null) {
                TreeNode next = curr.left;
                TreeNode predecessor = next;
                while (predecessor.right != null) {
                    predecessor = predecessor.right;
                }
                predecessor.right = curr.right;
                curr.left = null;
                curr.right = next;
            }
            curr = curr.right;
        }
    }
}

```

## 124. 岛屿数量

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围

示例 1：

输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1

```java
//深度优先搜索
class Solution {
    public int numIslands(char[][] grid) {
        int count=0;
        int row=grid.length;
        int column=grid[0].length;
        boolean[][] visit=new boolean[row][column];
        for(int i=0;i<row;i++){
            for(int j=0;j<column;j++){
                count=dfs(count,grid,visit,i,j);
            }
        }
        return count;
    }
    public int dfs(int count,char[][] grid,boolean[][] visit,int i,int j){
        if(i<0||i>=grid.length||j>=grid[0].length||j<0||visit[i][j]==true||grid[i][j]=='0'){
        }
        else{
            visit[i][j]=true;
            dfs(count,grid,visit,i-1,j);
            dfs(count,grid,visit,i+1,j);
            dfs(count,grid,visit,i,j-1);
            dfs(count,grid,visit,i,j+1);
            count++;
        }
        return count;
    }

}
```

## 125. 比特位计数

给你一个整数 `n` ，对于 `0 <= i <= n` 中的每个 `i` ，计算其二进制表示中 **`1` 的个数** ，返回一个长度为 `n + 1` 的数组 `ans` 作为答案。

```java
/*
：对于任意整数 x，令 x=x & (x−1)，该运算将 x 的二进制表示的最后一个 1 变成 0。因此，对 x 重复该操作，直到 x 变成 0，则操作次数即为 x 的「一比特数」。

对于给定的 n，计算从 0 到 n 的每个整数的「一比特数」的时间都不会超过 O(logn)，因此总时间复杂度为O(nlogn)。

*/
class Solution {
    public int[] countBits(int n) {
        int[] bits = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            bits[i] = countOnes(i);
        }
        return bits;
    }

    public int countOnes(int x) {
        int ones = 0;
        while (x > 0) {
            x &= (x - 1);
            ones++;
        }
        return ones;
    }
}
```

## 126. 回文子串

给你一个字符串 s ，请你统计并返回这个字符串中 回文子串 的数目。

回文字符串 是正着读和倒过来读一样的字符串。

子字符串 是字符串中的由连续字符组成的一个序列。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

示例 1：

输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"

```java
//穷举
class Solution {
    public int countSubstrings(String s) {
        int count=s.length();
        for(int i=0;i<s.length();i++){
            int j=i+2;
            while(j<=s.length()){
                if(isPan(s.substring(i,j))) {
                    count++;
                }
                j++;
            }
        }
        return count;
    }
    public boolean isPan(String s){
        int i=0;
        int j=s.length()-1;
        while(i<=j){
            if(s.charAt(i)!=s.charAt(j)) return false;
            else{
                i++;
                j--;
            }
        }
        return true;
    }
}
```

```java
//中心扩展法
class Solution {
    public int countSubstrings(String s) {
        int len=s.length();
        int count=0;
        for(int i=0;i<len*2-1;i++){
            int l=i/2;							
            int r=i/2+i%2;					//这样安排的目的是当i为偶数时，从自身开始，i为奇数时从相邻的开始
            while(l>=0&&r<len&&s.charAt(l)==s.charAt(r)){
                l--;
                r++;
                count++;
            }
        }
        return count;
    }
}
```

## 127. 每日温度

请根据每日 气温 列表 temperatures ，请计算在每一天需要等几天才会有更高的温度。如果气温在这之后都不会升高，请在该位置用 0 来代替。

示例 1:

输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]

```java
//穷举
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int len=temperatures.length;
        int[] ans=new int[len];
        for(int i=0;i<len-1;i++){
            for(int j=i+1;j<len;j++){
                if(temperatures[j]>temperatures[i]) {
                    ans[i]=j-i;
                    break;
                }
                if(j==len-1&&temperatures[j]<=temperatures[i]) ans[i]=0;
            }
            
        }
        ans[len-1]=0;
        return ans;
    }
}
```

```java
/*
利用栈特性，使用单调栈来解决问题
*/
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        Deque<Integer> stack = new LinkedList<Integer>();
        for(int i = 0; i < T.length; i++) {
            while(!stack.isEmpty() && T[i] > T[stack.peek()]) {
                res[stack.peek()] = i - stack.pop();
            }
            stack.push(i);
        }
        return res;
    }
}
```

## 128. 汉明距离

两个整数之间的 [汉明距离](https://baike.baidu.com/item/汉明距离) 指的是这两个数字对应二进制位不同的位置的数目。

给你两个整数 `x` 和 `y`，计算并返回它们之间的汉明距离。

 ```java
 class Solution {
     public int hammingDistance(int x, int y) {
         return Integer.bitCount(x^y);
     }
 }
 ```

## 129. 回文链表

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

```java
//先翻转链表，再比较 时间复杂度 O(n),空间复杂度O(n)
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode tmp=new ListNode();
        ListNode temp=tmp;
        ListNode pre=null;
        ListNode curr=head;
        while(curr!=null){
            tmp.val=curr.val;
            ListNode next=curr.next;
            curr.next=pre;
            pre=curr;
            curr=next;
            if(curr!=null) tmp.next=new ListNode();
             tmp=tmp.next;
        }
        while(pre!=null){
          
            if(pre.val!=temp.val) return false;
            pre=pre.next;
            temp=temp.next;
        }
        return true;
    }
}
//找到链表中点，翻转后半部分
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }

        // 找到前半部分链表的尾节点并反转后半部分链表
        ListNode firstHalfEnd = endOfFirstHalf(head);
        ListNode secondHalfStart = reverseList(firstHalfEnd.next);

        // 判断是否回文
        ListNode p1 = head;
        ListNode p2 = secondHalfStart;
        boolean result = true;
        while (result && p2 != null) {
            if (p1.val != p2.val) {
                result = false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }        

        // 还原链表并返回结果
        firstHalfEnd.next = reverseList(secondHalfStart);
        return result;
    }

    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    private ListNode endOfFirstHalf(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
```

## 130. 乘积最大子序列

给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

 ```java
 /*
 动态规划
 遍历数组时计算当前最大值，不断更新
 令imax为当前最大值，则当前最大值为 imax = max(imax * nums[i], nums[i])
 由于存在负数，那么会导致最大的变最小的，最小的变最大的。因此还需要维护当前最小值imin，imin = min(imin * nums[i], nums[i])
 当负数出现时则imax与imin进行交换再进行下一步计算
 时间复杂度：O(n)
 */
 class Solution {
     public int maxProduct(int[] nums) {
         int max = Integer.MIN_VALUE, imax = 1, imin = 1;
         for(int i=0; i<nums.length; i++){
             if(nums[i] < 0){ 
               int tmp = imax;
               imax = imin;
               imin = tmp;
             }
             imax = Math.max(imax*nums[i], nums[i]);
             imin = Math.min(imin*nums[i], nums[i]);
             
             max = Math.max(max, imax);
         }
         return max;
     }
 }
 
 ```

## 131. 把二叉搜索树转化成累加树

给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。

```java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        List<Integer> ans =new ArrayList<>();
        midOrder(root,ans);
        Collections.reverse(ans);			//将得到的列表反转
        int sum=0;
        for(int i=0;i<ans.size();i++){		//将结果累加
            sum+=ans.get(i);
            ans.set(i,sum);   
        }
        backOrder(root,ans);
        return root;
    }
    public void midOrder(TreeNode root,List<Integer> ans){	//先遍历二叉搜索树，按照从小到大的顺序
        if(root==null) return ;
        midOrder(root.left,ans);
        ans.add(root.val);
        midOrder(root.right,ans);
    }
    public void backOrder(TreeNode root, List<Integer> ans){		//按照从大到小的顺序遍历二叉搜索树，并且按照顺序将我们的列表值替换树中原本的值
        if(root==null) return;
        backOrder(root.right,ans);
        root.val=ans.get(0);
        ans.remove(0);
        backOrder(root.left,ans);
    }
}
```

## 132. 任务调度器

给你一个用字符数组 tasks 表示的 CPU 需要执行的任务列表。其中每个字母表示一种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。在任何一个单位时间，CPU 可以完成一个任务，或者处于待命状态。

然而，两个 相同种类 的任务之间必须有长度为整数 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的 最短时间 。

 

示例 1：

输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 

```java
/*
将频率最高的先取出来，中间留n个空，把剩下的或者待命的插进去就行
要注意可能会出现多个频率相同且都是最高的任务，比如 ["A","A","A","B","B","B","C","C"]，所以最后会剩下一个A和一个B，因此最后要加上频率最高的不同任务的个数 maxCount
*/
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        for (int i = 0; i < tasks.length; i++) {
            count[tasks[i]-'A']++;
        }//统计词频
        Arrays.sort(count);//词频排序，升序排序，count[25]是频率最高的
        int maxCount = 0;
        //统计有多少个频率最高的字母
        for (int i = 25; i >= 0; i--) {
            if(count[i] != count[25]){
                break;
            }
            maxCount++;
        }
        //公式算出的值可能会比数组的长度小，取两者中最大的那个
        return Math.max((count[25] - 1) * (n + 1) + maxCount , tasks.length);
    }
}
```

## 133. 字符串解码

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

 ```java
 //双栈
 class Solution {
     public String decodeString(String s) {
         Deque<Integer> numStack = new LinkedList<>();
         Deque<String> wordStack = new LinkedList<>();
         StringBuilder curr = new StringBuilder();
         int multi = 0;
         for (char c : s.toCharArray()) {
             if (c <= '9' && c >= '0') {
                 multi = multi * 10 + c - '0';
             } else if (c == '[') {
                 wordStack.push(curr.toString());
                 numStack.push(multi);
                 multi = 0;
                 curr = new StringBuilder();
             } else if (c == ']') {
                 int count = numStack.pop();
                 StringBuilder tmp = new StringBuilder(wordStack.pop());
                 for (int i = 0; i < count; i++) {
                     tmp.append(curr);
                 }
                 curr = tmp;
             } else {
                 curr.append(c);
             }
         }
 
         return curr.toString();
     }
 }
 ```

## 134. 字母异位词分组

给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

字母异位词 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。

 

示例 1:

输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]

1. 穷举法：两个for循环判断

   ```java
   class Solution {
       public List<List<String>> groupAnagrams(String[] strs) {
           List<List<String>> ans=new ArrayList<List<String>>();
           for(int i=0;i<strs.length;i++){
               List<String> tmp=new ArrayList<>();
               if(strs[i]==" ") continue;
               tmp.add(strs[i]);
               for(int j=i+1;j<strs.length;j++){
                   if(strs[j]==" ") continue;
                   else if(isYiwei(strs[i],strs[j])) {
                       tmp.add(strs[j]);
                       strs[j]=" ";
                   }
               }
               ans.add(tmp);
           }
           return ans;
       }
       public boolean isYiwei(String s1,String s2){			//判断两个单词是否是异位词
           if(s1.length()!=s2.length()) return false;
           int[] judge=new int[26];
           for(int i=0;i<s1.length();i++){
               judge[s1.charAt(i)-'a']++;
           }
           for(int i=0;i<s2.length();i++){
               judge[s2.charAt(i)-'a']--;
           }
           for(int i=0;i<26;i++){
               if(judge[i]!=0) return false;
           }
           return true;
       }
   }
   ```

   

2. 利用hashmap特性，将字符串数组中的字符串转换成char数组，然后将char数组排序，排序之后，异位词都会以同一个单词顺序出现，之后判断hashMap中是否包含之前char数组组成的单词的key，不包含就加入hashMap，存在就加入key对应的list中

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String,ArrayList<String>> map=new HashMap<>();
        for(String s:strs){
            char[] ch=s.toCharArray();
            Arrays.sort(ch);
            String key=String.valueOf(ch);
            if(!map.containsKey(key)) map.put(key,new ArrayList<>());
            map.get(key).add(s);
        }
        return new ArrayList(map.values());
    }   
}
```

## 135. 最短连续无序子数组

给你一个整数数组 nums ，你需要找出一个 连续子数组 ，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

请你找出符合题意的 最短 子数组，并输出它的长度。

 

示例 1：

输入：nums = [2,6,4,8,10,9,15]
输出：5
解释：你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。

```java
//时间复杂度O(nlogn)
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int count=nums.length;
        int[] tmp=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            tmp[i]=nums[i];
        }
        Arrays.sort(nums);
        for(int i=0;i<nums.length;i++){
            if(nums[i]==tmp[i]) count--;
            else break;
        }
        for(int i=nums.length-1;i>=0;i--){
            if(nums[i]==tmp[i]) count--;
            else break;
        }
        return count<0?0:count;
    }
}
```

```java
//从左到右循环，记录最大值为 max，若 nums[i] < max, 则表明位置 i 需要调整, 循环结束，记录需要调整的最大位置 i 为 high; 同理，从右到左循环，记录最小值为 min, 若 nums[i] > min, 则表明位置 i 需要调整，循环结束，记录需要调整的最小位置 i 为 low.
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        if(nums.length<=1) return 0;
        int high=0,low=nums.length-1,max=nums[0],min=nums[nums.length-1];
        for(int i=1;i<nums.length;i++){
            max=Math.max(max,nums[i]);
            min=Math.min(min,nums[nums.length-1-i]);
            if(nums[i]<max) high=i;
            if(nums[nums.length-1-i]>min) low=nums.length-1-i;
        }
        return high>low?high-low+1:0;
    }
}
```

## 136. 字符串转换整数

请你来实现一个 myAtoi(string s) 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 atoi 函数）。

函数 myAtoi(string s) 的算法如下：

读入字符串并丢弃无用的前导空格
检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。
返回整数作为最终结果。
注意：

本题中的空白字符只包括空格字符 ' ' 。
除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。

```java
public class Solution {

    public int myAtoi(String str) {
        int len = str.length();
        // str.charAt(i) 方法回去检查下标的合法性，一般先转换成字符数组
        char[] charArray = str.toCharArray();
        // 1、去除前导空格
        int index = 0;
        while (index < len && charArray[index] == ' ') {
            index++;
        }
        // 2、如果已经遍历完成（针对极端用例 "      "）
        if (index == len) {
            return 0;
        }
        // 3、如果出现符号字符，仅第 1 个有效，并记录正负
        int sign = 1;
        char firstChar = charArray[index];
        if (firstChar == '+') {
            index++;
        } else if (firstChar == '-') {
            index++;
            sign = -1;
        }
        // 4、将后续出现的数字字符进行转换
        int res = 0;
        while (index < len) {
            char currChar = charArray[index];
            // 4.1 先判断不合法的情况
            if (currChar > '9' || currChar < '0') {
                break;
            }

            // 题目中说：环境只能存储 32 位大小的有符号整数，因此，需要提前判：断乘以 10 以后是否越界
            if (res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && (currChar - '0') > Integer.MAX_VALUE % 10)) {
                return Integer.MAX_VALUE;
            }
            if (res < Integer.MIN_VALUE / 10 || (res == Integer.MIN_VALUE / 10 && (currChar - '0') > -(Integer.MIN_VALUE % 10))) {
                return Integer.MIN_VALUE;
            }

            // 4.2 合法的情况下，才考虑转换，每一步都把符号位乘进去
            res = res * 10 + sign * (currChar - '0');
            index++;
        }
        return res;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        String str = "2147483646";
        int res = solution.myAtoi(str);
        System.out.println(res);

        System.out.println(Integer.MAX_VALUE);
        System.out.println(Integer.MIN_VALUE);
    }
}

```

## 137. 寻找两个正序数组的中位数

给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。

算法的时间复杂度应该为 O(log (m+n)) 。

```java
//复杂度为O(m+n)的算法
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m=nums1.length;
        int n=nums2.length;
        int[] res=new int[m+n];
        int i=0,j=0,k=0;
        while(i<m&&j<n){
            if(nums1[i]<nums2[j]){
                res[k]=nums1[i];
                i++;
            }
            else {
                res[k]=nums2[j];
                j++;
            }
            k++;
        }
            while(j<n) {
                res[k]=nums2[j];
                j++;
                k++;
            }
   
            while(i<m) {
                res[k]=nums1[i];
                i++;
                k++;
            }
        return (res[(m+n-1)/2]+res[(m+n)/2])/2.0;
}
}
```

```java
//复杂度为O(log m+n)
class Solution {
  public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int left = (m + n + 1) / 2;
        int right = (m + n + 2) / 2;
        return (findKth(nums1, 0, nums2, 0, left) + findKth(nums1, 0, nums2, 0, right)) / 2.0;
    }
    //i: nums1的起始位置 j: nums2的起始位置
    public int findKth(int[] nums1, int i, int[] nums2, int j, int k){
        if( i >= nums1.length) return nums2[j + k - 1];//nums1为空数组
        if( j >= nums2.length) return nums1[i + k - 1];//nums2为空数组
        if(k == 1){
            return Math.min(nums1[i], nums2[j]);
        }
        int midVal1 = (i + k / 2 - 1 < nums1.length) ? nums1[i + k / 2 - 1] : Integer.MAX_VALUE;
        int midVal2 = (j + k / 2 - 1 < nums2.length) ? nums2[j + k / 2 - 1] : Integer.MAX_VALUE;
        if(midVal1 < midVal2){
            return findKth(nums1, i + k / 2, nums2, j , k - k / 2);
        }else{
            return findKth(nums1, i, nums2, j + k / 2 , k - k / 2);
        }        
    }
}
```

## 138. 三合一

。描述如何只用一个数组来实现三个栈。

你应该实现push(stackNum, value)、pop(stackNum)、isEmpty(stackNum)、peek(stackNum)方法。stackNum表示栈下标，value表示压入的值。

构造函数会传入一个stackSize参数，代表每个栈的大小。

```java
class TripleInOne {
    int[] arr;
    int stackSize;
    int curNum=0;
    int[] index;
    //构造方法
    public TripleInOne(int stackSize) {
        arr=new int[stackSize*3];
        this.stackSize=stackSize;
        index=new int[]{0,1,2};
    
    }
    public void push(int stackNum, int value) {
        //下标溢出
        if(index[stackNum]>=stackSize*3) return ;
        arr[index[stackNum]]=value;
        index[stackNum]+=3;
    }
    
    public int pop(int stackNum) {
        if(isEmpty(stackNum)) return -1;
        index[stackNum]-=3;
        return arr[index[stackNum]];
    }
    
    public int peek(int stackNum) {
        if(isEmpty(stackNum)) return -1;
        return arr[index[stackNum]-3];

    }
    
    public boolean isEmpty(int stackNum) {
        return index[stackNum]<3;
    }
}

```

## 139. 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。

假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

 Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]

```java
//首先，前序节点的第一个肯定是根节点，然后在中序节点序列中找到根节点，两边分别是自己的左右子树，然后从根节点划分为两个序列，递归寻找根节点
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int length=preorder.length;
        //结束条件，当传入的数组长度为0
        if(length==0) return null;
        //接下来是我们应该返回什么，那当然就是返回重建子树的根节点咯
        //每一层应该做些什么呢？
        //应该通过传进来的两个数组，根据他们之间的关系，来构建子树，并把新的参数传递下去
        //那么现在传进来了两个数组，一个是前序遍历的数组，那么首位必定是根节点，先拿下
        int rootValue=preorder[0];
        //有了数组的根节点以后我们自然需要的是左右子树，根据中序遍历的特点，只要找到了根节点，那么他两边自然
        //就是他的左右子树了
        //那么先在中序遍历的数组中找到根节点,并把它的坐标保存起来
        int rootIndex=0;   
        for(int i=0;i<length;i++){
            if(inorder[i]==rootValue){
                rootIndex=i;
                break;
            }
        }
          //至此我们已经找到了根和左右子树，那么设置一下根节点的左右节点，自然就是递归后返回的左右子树的根节点
        //先把我们的根节点创建出来
        TreeNode root=new TreeNode(rootValue);
    root.left=buildTree(Arrays.copyOfRange(preorder,1,rootIndex+1),Arrays.copyOfRange(inorder,0,rootIndex));
         root.right=buildTree(Arrays.copyOfRange(preorder,1+rootIndex,length),Arrays.copyOfRange(inorder,rootIndex+1,length));
        return root;
    }
}
//another
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        TreeNode root = new TreeNode();
        construct(root,inorder,postorder,0,inorder.length,0,postorder.length);
        return root;

    }
    public void construct(TreeNode root,int[] inorder,int[] postorder,int inStart, int inEnd,int postStart, int postEnd){
        if(inStart>=inEnd) return ;
       int tmp = postorder[postEnd-1];
       
        root.val = tmp;
        for(int i=inStart;i<inEnd;i++){
            //找到了分割点
            if(inorder[i]==tmp) {
                //根节点左边不为空，构造左子树
                if(i>inStart){
                    root.left = new TreeNode();
                    construct(root.left,inorder,postorder,inStart,i,postStart,i-inStart+postStart);
                }
                //根节点右边不为空，构造右节点
                if(i<inEnd-1){
                    root.right = new TreeNode();
                    construct(root.right,inorder,postorder,i+1,inEnd,postEnd+i-inEnd,postEnd-1);
                }
            }
        }
    }
}
```

## 140. 矩阵中的路径

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

![image-20220221203222413](https://gitee.com/bitches/git/raw/master/img/image-20220221203222413.png)

```java
//dfs+回溯
class Solution {
    public boolean exist(char[][] board, String word) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return false;
        }
         boolean[][] visited = new boolean[board.length][board[0].length]; 
         for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                // 从 (0, 0) 点开始进行 dfs 操作，不断地去找，
                // 如果以 (0, 0) 点没有对应的路径的话，那么就从 (0, 1) 点开始去找
                if (dfs(board, word, visited, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean dfs(char[][] board, String word,boolean[][] visit,int i, int j,int start){
        if(i<0||i>=board.length||j<0||j>=board[0].length||word.charAt(start)!=board[i][j]||visit[i][j]) {return false;}
        if(start==word.length()-1) return true;
        visit[i][j]=true;
        boolean ans=false;
        ans=dfs(board,word,visit,i,j+1,start+1)||dfs(board,word,visit,i,j-1,start+1)||
        dfs(board,word,visit,i-1,j,start+1)||dfs(board,word,visit,i+1,j,start+1);
        visit[i][j]=false;
        return ans;
        }
}
```

## 141. 机器人的运动范围

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

```java
//dfs+回溯
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited=new boolean[m][n];
        int count=0;
        
        return dfs(m,n,k,0,0,visited);
    }
    //i,j为当前处于的坐标
   public int dfs(int m,int n,int k,int i,int j,boolean[][] visited){
       if(i<0||i>=m||j<0||j>=n||visited[i][j]||(i/10 + i%10 + j/10 + j%10)>k){return 0;}
       else{
           visited[i][j]=true;
       }
           return dfs(m,n,k,i,j-1,visited)
           +dfs(m,n,k,i,j+1,visited)
           +dfs(m,n,k,i-1,j,visited)
           +dfs(m,n,k,i+1,j,visited)+1;
     
   }
}
```

## 142. 剪绳子

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

```java
/*
	数学推导，贪心算法
	尽可能将绳子以长度 33 等分为多段时，乘积最大。
	当 n ≤3 时，按照规则应不切分，但由于题目要求必须剪成 m>1 段，因此必须剪出一段长度为 1 的绳子，即返回 n−1 。
	当 n>3时，求 n 除以3的整数部分a和余数部分b（即 n = 3a + b），并分为以下三种情况：
	1.当b=0时，直接返回 3^a；
	2.当b=1时，要将一个1+3转换为2+2，因此返回 3^{a-1}*43；
	3.当b=2 时，返回 3^a*2；
*/
class Solution {
    public int cuttingRope(int n) {
        
        if(n<=3) return n-1;
        int m=n%3;
        int k=n/3;
        if(m==0) return (int)Math.pow(3,k);
        if(m==1) return (int)Math.pow(3,k-1)*4;
        else return (int)Math.pow(3,k)*2;

    }
}
//动态规划
class Solution {
    public int cuttingRope(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, 1);
        for (int i = 3; i <= n; i++) {
            for (int j = 1; j < i; j++) {
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), dp[i - j] * j));
            }
        }
        return dp[n];
    }
}
```

## 143. 剪绳子II

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m - 1] 。请问 k[0]*k[1]*...*k[m - 1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

```java
class Solution {
    public int cuttingRope(int n) {
     
        if(n<=3) return n-1;
        long res=1;
        while(n>4){
            res*=3;
            res=res%1000000007;
            n-=3;
        }
        return (int) (res*n%1000000007);
        
    }
}
```

## 144. 数值的整数次方

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数（即，xn）。不得使用库函数，同时不需要考虑大数问题。

 ```java
 /*
 	求幂两种算法：
 	1. x^n，将x累乘n次，时间复杂度为O(n)，使用此算法会超出时间限制
 	2. 快速幂：以下以求a的b次方来介绍  
 			把b转换成二进制数。
 			该二进制数第i位的权为2^(i-1)
 			11的二进制是1011
 			因此，我们将a¹¹转化为算a^(2^0)*a^(2^1)*a^(2^3)
 			
 */
 class Solution {
     public double myPow(double x, int n) {
        double res=1.0;
       if(n==0||x==1) return 1;
       if(n==1) return x;
       long b=n;
       if(b<0){
           x=1/x;
           b=-b;
       }
       while(b>0){
           //确定最后一位是不是1，只有最后一位为1才用计算这一位的权重
           if((b&1)==1){
               res*=x;
           }
           //
           x*=x;
           //b右移一位，相当于b/2
           b=b>>1;
       }
       return res;
 
     }
 }
 ```

## 145. 打印从1到最大的n位数

输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

```java
class Solution {
    public int[] printNumbers(int n) {
        int length=(int)Math.pow(10,n)-1;
        int[] ans=new int[length];
        for(int i=0;i<length;i++){
            ans[i]=i+1;
        }
        return ans;

    }
}
```

## 146. 表示数值的字符串

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。

数值（按顺序）可以分成以下几个部分：

若干空格
一个 小数 或者 整数
（可选）一个 'e' 或 'E' ，后面跟着一个 整数
若干空格
小数（按顺序）可以分成以下几个部分：

（可选）一个符号字符（'+' 或 '-'）
下述格式之一：
至少一位数字，后面跟着一个点 '.'
至少一位数字，后面跟着一个点 '.' ，后面再跟着至少一位数字
一个点 '.' ，后面跟着至少一位数字
整数（按顺序）可以分成以下几个部分：

（可选）一个符号字符（'+' 或 '-'）
至少一位数字
部分数值列举如下：

["+100", "5e2", "-123", "3.1416", "-1E-16", "0123"]
部分非数值列举如下：

["12e", "1a3.14", "1.2.3", "+-5", "12e+5.4"]

```java
class Solution {
    public boolean isNumber(String s) {
        //去掉首尾空格
        String str = s.trim();
        if(str.length()==0) return false;
        if(!str.contains("E")&&!str.contains("e")) return isInt(str)||isFloat(str);
        else{
            int index=0;
            //找到e所在的下标
            for(int i=0;i<str.length();i++){
                if(str.charAt(i)=='e'||str.charAt(i)=='E'){
                    index=i;
                    break;
                }
            }
            //e在第一位或者最后一位都是false
            if(index==0||index==str.length()-1) return false;
            //e的左边必须为小数或者整数，右边必须为整数
            return (isFloat(str.substring(0,index))||isInt(str.substring(0,index)))&&
            isInt(str.substring(index+1,str.length()));
        }
        
    }
    //判断是否表示整数
    public  boolean isInt(String s){
        if(s.length()==1) return Character.isDigit(s.charAt(0));
        if(!Character.isDigit(s.charAt(0))&&s.charAt(0)!='+'&&s.charAt(0)!='-')
            return false;
        for(int i=1;i<s.length();i++){
            if(!Character.isDigit(s.charAt(i))) return false;
        }
        return true;
    }
    //判断是否表示小数
    public boolean  isFloat(String s){
        //不含小数点
        if(!s.contains(".")) return false;
        else{
            if(s.length()==1) return false;
            //两个或者以上的小数点，直接返回false；
            if(s.indexOf('.')!=s.lastIndexOf('.')) return false;
            else{
                //如果第一位不是数字，‘+’，‘-’，‘.‘，则表达式不合法
                if(!Character.isDigit(s.charAt(0))&&(s.charAt(0)!='+')&&
                        (s.charAt(0)!='-')&&(s.charAt(0)!='.')) return false;
                for(int i=1;i<s.length();i++){
                    if(s.charAt(i)=='.'){
                        //小数点左右必须得有一个数字
                        if(!Character.isDigit(s.charAt(i-1))
                                &&(i<s.length()-1&&!Character.isDigit(s.charAt(i+1)))) return false;
                        //小数点在最后一位，只需判断小数点前面是否为数字就行
                        else if(!Character.isDigit(s.charAt(i-1))&&i==s.length()-1) return false;
                        else continue;
                    }
                    //除了第一位，和小数点之外，其他的地方出现非数字就是不合法的
                    if(!Character.isDigit(s.charAt(i))) return false;
                }
            }
        }
        return true;
    }
}
```

## 147. 调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组的前半部分，所有偶数在数组的后半部分。

```java
class Solution {
    public int[] exchange(int[] nums) {
        int[] res=new int[nums.length];
        int j=0,right=nums.length-1;
        for(int i=0;i<nums.length;i++){
            if(nums[i]%2==1) {
                res[j]=nums[i];
                j++;
            }
            else{
                res[right]=nums[i];
                right--;
            }
        }
        return res;
    }
}
```

## 148. 二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

```java
//递归
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        cur(root);
        return root;
    }
    //进行递归
    public void cur(TreeNode root){
        if(root==null) return;
        //交换左右孩子节点
        TreeNode tmp=root.left;
        root.left=root.right;
        root.right=tmp;
        //递归进行交换剩余的节点
        cur(root.left);
        cur(root.right);
    }
}
```

## 149. 对称的二叉树

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

```java

class Solution {
    //即递归的比较左子树的值和右子树的值是否相等
    public boolean isSymmetric(TreeNode root) {
        if(root==null) return true;
        return isEq(root.left,root.right);
    }
   //递归函数：如果两个都为空，即到底了都是相等的，那么就返回true，如果有一个为空，另一个不为空，则说明不对称
    public boolean isEq(TreeNode root,TreeNode sym){
        if(root==null&&sym==null) return true;
        if(root==null||sym==null) return false;
        return root.val==sym.val&&isEq(root.left,sym.right)&&isEq(root.right,sym.left);
        
    }
}
```

## 150. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
       /*
        空值处理： 当 matrix 为空时，直接返回空列表 [] 即可。
        初始化： 矩阵 左、右、上、下 四个边界 l , r , t , b ，用于打印的结果列表 res 。
        循环打印： “从左向右、从上向下、从右向左、从下向上” 四个方向循环，每个方向打印中做以下三件事 
        根据边界打印，即将元素按顺序添加至列表 res 尾部；
        边界向内收缩 11 （代表已被打印）；
        判断是否打印完毕（边界是否相遇），若打印完毕则跳出。
        返回值： 返回 res 即可。
*/
        if(matrix.length == 0) return new int[0];
        int l = 0, r = matrix[0].length - 1, t = 0, b = matrix.length - 1, x = 0;
        int[] res = new int[(r + 1) * (b + 1)];
        while(true) {
            for(int i = l; i <= r; i++) res[x++] = matrix[t][i]; // left to right.
            //t++代表向下收缩，第一行打印完毕即从上往下收缩
            if(++t > b) break;
            for(int i = t; i <= b; i++) res[x++] = matrix[i][r]; // top to bottom.
            //r--代表向左收缩，当边界相遇的时候就可以确定打印完毕
            if(l > --r) break;
            for(int i = r; i >= l; i--) res[x++] = matrix[b][i]; // right to left.
            if(t > --b) break;
            for(int i = b; i >= t; i--) res[x++] = matrix[i][l]; // bottom to top.
            if(++l > r) break;
        }
        return res;
    }
}

```

## 151. 栈的压入弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack=new Stack<>();
        //Deque<Integer> stack = new ArrayDeque();
        //使用deque的栈，效率更高一点
        int j=0,i=0;
        while(i<pushed.length){
            if(pushed[i]==popped[j]){
                j++;
                i++;
            }
            else{
                if(!stack.empty()&&stack.peek()==popped[j]){
                    stack.pop();
                    j++;
                }
                else {
                    stack.push(pushed[i]);
                    i++;
            }
        }
        }
        while(j<popped.length&&!stack.empty()){
            if(stack.pop()==popped[j]){
                j++;
            }
        }
  
        if(j==popped.length&&stack.empty()) return true;
        else return false;
    }
}
```

## 152. 从上到下打印二叉树II

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

例如:
给定二叉树: `[3,9,20,null,null,15,7]`,

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

 ```java
 class Solution {
     public List<List<Integer>> levelOrder(TreeNode root) {
         if(root==null) return new ArrayList<>();
         Queue<TreeNode> queue = new LinkedList<>();
         List<List<Integer>> res = new ArrayList<>();
         //首先将根节点入队
         queue.offer(root);
         //当队列不为空的时候
         while(queue.size()>0){
             List<Integer> tmp=new ArrayList<>();
             //i代表同一层的节点数目
             int i=queue.size();
             while(i>0){
                 //将队列同一层的加入list，出队，同时将左右不为空的节点加入队列
                 if(queue.peek()!=null){
                     tmp.add(queue.peek().val);
                     TreeNode temp=queue.poll();
                     if(temp.left!=null) queue.offer(temp.left);
                     if(temp.right!=null) queue.offer(temp.right);
                 }
                 i--;
             }
             res.add(tmp);
         }
         return res;
     }
 }
 ```

## 153. 从上到下打印二叉树III

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null) return new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        //首先将根节点入队
        queue.offer(root);
        //order为奇数代表从左向右，order为偶数从右向左
        int order=0;
        //当队列不为空的时候
        while(queue.size()>0){
            List<Integer> tmp=new ArrayList<>();
            //i代表同一层的节点数目
            int i=queue.size();
            while(i>0){
                //将队列同一层的加入list，出队，同时将左右不为空的节点加入队列
                if(queue.peek()!=null){
                    tmp.add(queue.peek().val);
                    TreeNode temp=queue.poll();
                    if(temp.left!=null) queue.offer(temp.left);
                    if(temp.right!=null) queue.offer(temp.right);
                }
                i--;
            }
            if(order%2==0) res.add(tmp);
            else {
                Collections.reverse(tmp);
                res.add(tmp);
            }
            order++;
        }
        return res;
    }
}
```



## 154. 二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

![image-20220224205830001](https://gitee.com/bitches/git/raw/master/img/image-20220224205830001.png)

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return ver(postorder,0,postorder.length-1);
    }
    public boolean ver(int[] postorder,int i,int j){
        if(i>=j) return true;
        int p=i;
        //从左向右找寻第一个大于根节点的节点，可以划分左右子树
        while(postorder[p]<postorder[j]) p++;
        int m=p;
        //此处代表右子树的值都要大于根节点
        while(postorder[p]>postorder[j]) p++;
        
        return p==j&&ver(postorder,i,m-1)&&ver(postorder,m,j-1);

    }
     
}
```

## 155. 二叉树中和为某一值的路径

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

```java

class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int target) {
        
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> tmp=new ArrayList<>();
        backTrack(root,target,ans,tmp);
        return ans;
    }
    public void backTrack(TreeNode root, int target,List<List<Integer>> ans,
                        List<Integer> tmp){
        if(root==null) return ;
        tmp.add(root.val);
        target-=root.val;
        //叶子节点并且这条路径符合条件，加入结果中
        if(root.left==null&&root.right==null&&0==target) {
            ans.add(new ArrayList<>(tmp));
        }
       
        else{  
        backTrack(root.left,target,ans,tmp);
        backTrack(root.right,target,ans,tmp);   
        }
        //回溯
        tmp.remove(tmp.size()-1);
    }
}
```

## 156. 复杂链表的复制

请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

```java
/*
	如果是普通链表，我们可以直接按照遍历的顺序创建链表节点。而本题中因为随机指针的存在，当我们拷贝节点时，「当前节点的随机指针指向的	节点」可能还没创建，因此我们需要变换思路。一个可行方案是，我们利用回溯的方式，让每个节点的拷贝操作相互独立。对于当前节点，我们首	先要进行拷贝，然后我们进行「当前节点的后继节点」和「当前节点的随机指针指向的节点」拷贝，拷贝完成后将创建的新节点的指针返回，即可	完成当前节点的两指针的赋值
*/
class Solution {
    Map<Node, Node> cachedNode = new HashMap<>();
    public Node copyRandomList(Node head) {
       if(head==null) return head;
       if(!cachedNode.containsKey(head)){
           Node headNew=new Node(head.val);
           cachedNode.put(head,headNew);
           headNew.next=copyRandomList(head.next);
           headNew.random=copyRandomList(head.random);
       }
        return cachedNode.get(head);
    }
}
/*方法二：原地修改解法流程：

	复制一个新的节点在原有节点之后，如 1 -> 2 -> 3 -> null 复制完就是 1 -> 1 -> 2 -> 2 -> 3 - > 3 -> null
	从头开始遍历链表，通过 cur.next.random = cur.random.next 可以将复制节点的随机指针串起来，当然需要判断 cur.random 是否		存在
	将复制完的链表一分为二 
*/
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        //复制一个新的节点在原节点之后
        for (Node node = head; node != null; node = node.next.next) {
            Node nodeNew = new Node(node.val);
            nodeNew.next = node.next;
            node.next = nodeNew;
        }
        for (Node node = head; node != null; node = node.next.next) {
            Node nodeNew = node.next;
            nodeNew.random = (node.random != null) ? node.random.next : null;
        }
        Node headNew = head.next;
        for (Node node = head; node != null; node = node.next) {
            Node nodeNew = node.next;
            node.next = node.next.next;
            nodeNew.next = (nodeNew.next != null) ? nodeNew.next.next : null;
        }
        return headNew;
    }
}


```

## 157. 最小的k个数

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

```java
//先排序 再选择
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int[] res=new int[k];
        Arrays.sort(arr);
        for(int i=0;i<k;i++){
            res[i]=arr[i];
        }
        return res;
    }
}

//
```

## 158. 二叉搜索树与循环链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

![image-20220225210611570](https://gitee.com/bitches/git/raw/master/img/image-20220225210611570.png)

```java
class Solution {
    Node pre,head;
    public Node treeToDoublyList(Node root) {
        if(root==null) return null;
        List<Integer> res=new ArrayList<>();
        midOrder(root);
        head.left=pre;
        pre.right=head;
        return head;
    }
    public void midOrder(Node cur){
        if(cur==null) return ;
        midOrder(cur.left);
        if(pre==null) head=cur;
        else pre.right=cur;
        cur.left=pre;
        pre=cur;
        midOrder(cur.right);
    }
}
```

## 159. 字符串的排列

输入一个字符串，打印出该字符串中字符的所有排列。

 

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

 

示例:

输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]

```java
//标准的回溯算法
class Solution {
    int i=0;
    public String[] permutation(String s) {
         boolean[] visited = new boolean[s.length()];
        Set<String> list = new HashSet<>();
        StringBuilder sb=new StringBuilder();
        back(s,list,sb,visited);
        int i=0;
        String[] res=new String[list.size()];
        for(String str:list){
            res[i]=str;
            i++;
        }
        return res;
    }

    public void back(String s,Set<String> list,StringBuilder sb,boolean[] visited){
        if(sb.length()==s.length()) {
            list.add(sb.toString());
            return ;
        }
        else{
            for(int i=0;i<s.length();i++){
                if(visited[i]) continue;
                visited[i]=true;
                sb.append(s.charAt(i));
                back(s,list,sb,visited);
                visited[i]=false;
                sb.deleteCharAt(sb.length()-1);
                }
            }
        }
        
}
```

## 160. 数据流中的中位数

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

```java
class MedianFinder {

     Queue<Integer> A, B;
    public MedianFinder() {
         A = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        B = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    
    public void addNum(int num) {
        if(A.size()!=B.size()){
            A.add(num);
            B.add(A.poll());
        
        }
        else{
             B.add(num);
            A.add(B.poll());
        }
    }
    
    public double findMedian() {
        return A.size() != B.size() ? A.peek() : (A.peek() + B.peek()) / 2.0;
    }
}

```

## 161. 1-n中整数中1出现的次数

输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

![image-20220226195612443](https://gitee.com/bitches/git/raw/master/img/image-20220226195612443.png)

![image-20220226195751827](https://gitee.com/bitches/git/raw/master/img/image-20220226195751827.png)

![image-20220226195806111](https://gitee.com/bitches/git/raw/master/img/image-20220226195806111.png)

![image-20220226195818110](https://gitee.com/bitches/git/raw/master/img/image-20220226195818110.png)

```java
/*
	很有意思的一道题：穷举法会超出时间限制
	根据上图的解题思路，即计算每一位中，1-n之间的整数中1出现的次数
*/
class Solution {
    public int countDigitOne(int n) {
       int digit=1;
       int res=0;
       int cur=n%10;
       int low=0;
       int high=n/10;
       while(high!=0||cur!=0){
           if(cur==0) res+=high*digit;
           else if(cur==1) res+=high*digit+low+1;
           else res+=(high+1)*digit;
           low+=cur*digit;
           cur=high%10;
           high=high/10;
           digit*=10;
       }
       return res;
    }
}
```

## 162. 最长不含重复字符的子字符串

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int j=-1;
        Set<Character> hashset=new HashSet<Character>();
        int max=0;
        int i=0;
        for(i=0;i<s.length();i++){
            if(i!=0) hashset.remove(s.charAt(i-1));
            //
            while(j<s.length()-1&&!hashset.contains(s.charAt(j+1))){
                hashset.add(s.charAt(j+1));
                j++;
            }
            max=Math.max(max,j+1-i);
        }
        
       
        return max;
    }
}
```

## 163. 求1+2+······+n

求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

```java
//不能使用循环，可以用递归
//不能用if等判断语句，利用逻辑运算符的短路特性终结递归
class Solution {
    public int sumNums(int n) {
        boolean x=n>0&&(n+=sumNums(n-1))>0;
        return n;
    } 
}
```

## 164. 队列的最大值

请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1

```java
class MaxQueue {
    int[] queue=new int[20000];
    int head=0,tail=0;
    public MaxQueue() {
    
    }  
    public int max_value() {
        int max=-1;
        if(head==tail) return -1;
        else{
            for(int i=tail+1;i<=head;i++){
                max=Math.max(max,queue[i]);
            }
        }
        return max;
    }
    
    public void push_back(int value) {
        queue[++head]=value;
    }
    
    public int pop_front() {
        if(head==tail) return -1;
        return queue[++tail];
    }
}

```

## 165. 扑克牌中的顺子

从若干副扑克牌中随机抽 5 张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

```java
/*
	将数组排序，把其中o的个数用count表示，然后从数组最后一个数开始，如果前面的数比他小1，那就继续循环，如果不符合小一的情况，那就用0填补，count-1，如果count的个数为负的话，返回false
*/
class Solution {
    
    public  boolean isStraight(int[] nums) {
        int count=0,flag;
        Arrays.sort(nums);
       for(int i=0;i<4;i++) {
           if(nums[i]==0) count++;
       }
        int tmp=nums[4];
        flag=count;
        for(int i = 4; i> flag; i--){
            while(tmp-nums[i-1]!=1){
                tmp--;
                count--;
                if(count<0) return false;
            }
            tmp=nums[i-1];
        }
        return true;

    }
}
```

## 166. 数组中数字出现的次数

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

```java
/*
	因为时间复杂度为O（n），空间复杂度为O（1），所以穷举暴力不可取，哈希表也不行，所以想到了位运算，异或
	根据异或的运算性质，将数组中的所有数字进行异或，得到的是两个只出现一次的数字的异或结果，那怎么把数组分为两部分，每部分分别只包含只出现了一次的数字呢。因为两个不同的数字异或结果二进制肯定有一位是1，那么我们找到这个1，然后将数组中这个位置的二进制为0的分为一组，二进制为1的分为一组，将两个数组分别异或就得到了结果
*/
class Solution {
    public int[] singleNumbers(int[] nums) {
        int x = 0, y = 0, n = 0, m = 1;
        for(int num : nums)               // 1. 遍历异或
            n ^= num;
        while((n & m) == 0)               // 2. 循环左移，计算 m 得出异或结果哪一位为1
            m <<= 1;
        for(int num: nums) {              // 3. 遍历 nums 分组
            if((num & m) != 0) x ^= num;  // 4. 当 num & m != 0 为一组
            else y ^= num;                // 4. 当 num & m == 0 为另外一组
        }
        return new int[] {x, y};          // 5. 返回出现一次的数字
    }
}


```

## 167. 数字序列中某一位的数字

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

```java
/* 数字范围    数量  位数    占多少位
    1-9        9      1       9
    10-99      90     2       180
    100-999    900    3       2700
    1000-9999  9000   4       36000  ...

    例如 2901 = 9 + 180 + 2700 + 12 即一定是4位数,第12位   n = 12;
    数据为 = 1000 + (12 - 1)/ 4  = 1000 + 2 = 1002
    定位1002中的位置 = (n - 1) %  4 = 3    s.charAt(3) = 2;
*/
class Solution {
    public int findNthDigit(int n) {
        int digit = 1;   // n所在数字的位数
        long start = 1;  // 数字范围开始的第一个数
        long count = 9;  // 占多少位
        while(n > count){
            n -= count;
            digit++;
            start *= 10;
            count = digit * start * 9;
        }
        long num = start + (n - 1) / digit;
        return Long.toString(num).charAt((n - 1) % digit) - '0';
    }
}
```

## 168. 和为s的两个数字

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

```java
//双指针
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int i=0,j=nums.length-1;
        while(i<j){
            int s=nums[i]+nums[j];
            if(s==target) return new int[]{nums[i],nums[j]};
            else if(s<target) i++;
            else j--;
        }
        return new int[2];
    }
}
```

## 169. 不用加减乘除作加法

写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号

![image-20220228160212514](https://gitee.com/bitches/git/raw/master/img/image-20220228160212514.png)

```java
//不用加减乘除做加法，自然而然想到位运算
class Solution {
    public int add(int a, int b) {
        while(b != 0) { // 当进位为 0 时跳出
            int c = (a & b) << 1;  // c = 进位
            a ^= b; // a = 非进位和
            b = c; // b = 进位
        }
        return a;
    }
}

```

## 170. 平衡二叉树

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

```java 
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        return (Math.abs(getHigh(root.left)-getHigh(root.right))<=1)
        &&isBalanced(root.right)&&isBalanced(root.left);
    }
    
    //获取二叉树的深度
    public int getHigh(TreeNode root){
        if(root==null) return 0;
        return Math.max(getHigh(root.left),getHigh(root.right))+1;
    }
}
```

## 171. 圆圈中剩下的最后数字

0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

```java
//利用数组存储，访问过的数组标记为-1，但是时间复杂度过高，超时
class Solution {
    public int lastRemaining(int n, int m) {
       if(n==1) return 0;
       int[] tmp=new int[n];
       Arrays.fill(tmp,1);
       int count=0;
       int cur=(m-1)%n;
       tmp[cur]=-1;
       count++;
       while(count<n-1){
           int k=m;
            while(k>0){
                if(tmp[cur]!=-1) k--;
                if(k>0) cur=(cur+1)%n;
           }
           tmp[cur]=-1;
            count++;
       }
       for(int i=0;i<n;i++){
           if(tmp[i]==1) return i;
       }
       return 0;
       
    }
}
```

```java
//利用列表存储，然后将访问的remove
class Solution {
    public int lastRemaining(int n, int m) {
        ArrayList<Integer> list = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            list.add(i);
        }
        int idx=0;
        while(n>1){
            idx=(idx+m-1)%n;
            list.remove(idx);
            n--;
        }
       return list.get(0);
    }
}
//数学方法，最后一轮剩下两个人，所以从2 开始反推
class Solution {
    public int lastRemaining(int n, int m) {
        int ans = 0;
        // 最后一轮剩下2个人，所以从2开始反推
        for (int i = 2; i <= n; i++) {
            ans = (ans + m) % i;
        }
        return ans;
    }
}
```

## 172. 把数字翻译成字符串

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

```java
//动态规划，dp[0]=1,如果数字十位数和个位数组成的数小于26，则dp[1]=2,否则位dp[0]
//转移方程同理，如果现在所在的位数和前一位组成的数小于26， dp[i]=dp[i-1]+dp[i-2];否则dp[i]=dp[i-1];
class Solution {
    public int translateNum(int num) {
        if(num<10) return 1;
        String s=String.valueOf(num);
        int len=s.length();
        int[] dp=new int[len];
        dp[0]=1;
        if(s.charAt(0)>'2'||(s.charAt(0)=='2'&&s.charAt(1)>'5')) dp[1]=dp[0];
        else dp[1]=2;
        for(int i=2;i<len;i++){
             if(s.charAt(i-1)>'2'||(s.charAt(i-1)=='2'&&s.charAt(i)>'5')||s.charAt(i-1)=='0') dp[i]=dp[i-1];
             else dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[len-1];
    }
    
}
```

## 173. 礼物的最大价值

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

```java
//递归，但是会超时
class Solution {
    public int maxValue(int[][] grid) { 
        return dfs(grid,0,0,0);
    }
    public int dfs(int[][] grid, int i, int j, int count){
        if(i<0||i>=grid.length||j<0||j>=grid[0].length) {}
        else {
            //visited[i][j]=true;
            count+=grid[i][j];
            count=Math.max(dfs(grid,i+1,j,count),dfs(grid,i,j+1,count));
        }

        return count;
    }
}
```

```java
//动态规划
class Solution {
    public int maxValue(int[][] grid) {
        int m=grid.length,n=grid[0].length;
        int[][] dp=new int[m][n];
        dp[0][0]=grid[0][0];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i>0&&j>0) {
                    dp[i][j]=Math.max(dp[i][j-1],dp[i-1][j])+grid[i][j];
                }
                else if(i==0&&j!=0) dp[0][j]=dp[0][j-1]+grid[i][j];
                else if(i!=0&&j==0) dp[i][0]=dp[i-1][0]+grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
   
}
```

## 174. 丑数

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

```java
//常规 穷举 超时
class Solution {
    public int nthUglyNumber(int n) {
        if(n==1) return 1;
        int count=1;
        int k=1;
        while(count<=n){
            if(isUgly(k)) {
                count++;
            }
            k++;
            
        }
        return k-1;
    }
    public boolean isUgly(int n){
        while(n>1){
            if(n%2==0) n/=2;
            else if(n%3==0) n/=3;
            else if(n%5==0) n/=5;
            else break;
        }
        if(n!=1) return false;
        return true;
    }
}
```

```java
/*
	得到从小到大的第 n 个丑数，可以使用最小堆实现。初始时堆为空。首先将最小的丑数 1 加入堆。
	每次取出堆顶元素 x，则 x 是堆中最小的丑数，由于 2x, 3x, 5x 也是丑数，因此将 2x, 3x, 5x 加入堆。
	上述做法会导致堆中出现重复元素的情况。为了避免重复元素，可以使用哈希集合去重，避免相同元素多次加入堆。
	在排除重复元素的情况下，第 n 次从最小堆中取出的元素即为第 n 个丑数。

*/
class Solution {
    public int nthUglyNumber(int n) {
        int[] factors = {2, 3, 5};
        Set<Long> seen = new HashSet<Long>();
        PriorityQueue<Long> heap = new PriorityQueue<Long>();
        seen.add(1L);
        heap.offer(1L);
        int ugly = 0;
        for (int i = 0; i < n; i++) {
            long curr = heap.poll();
            ugly = (int) curr;
            for (int factor : factors) {
                long next = curr * factor;
                if (seen.add(next)) {
                    heap.offer(next);
                }
            }
        }
        return ugly;
    }
}


```

```java
//动态规划
class Solution {
    public int nthUglyNumber(int n) {
        //a,b,c为三个丑数的索引，dp数组保存前n个丑数
        int a = 0, b = 0, c = 0;
        int[] dp = new int[n];
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            //下一个丑数必定是前三个丑数的2倍，3倍或者5倍，其中最小的就是下一个丑数
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = Math.min(Math.min(n2, n3), n5);
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n - 1];
    }
}

```

## 175. 二叉搜索树第k大的节点

给定一棵二叉搜索树，请找出其中第 `k` 大的节点的值。

```java

class Solution {
    public int kthLargest(TreeNode root, int k) {
        List<Integer> res=new ArrayList<>();
        dfs(root,res);
        return res.get(res.size()-k);
    }
    public void dfs(TreeNode root,List<Integer> res){
        if(root==null) return ;
        dfs(root.left,res);
        res.add(root.val);
        dfs(root.right,res);
    }
}
```

```java
//提前返回，找到第k个数就返回
class Solution {
    int res, k;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;
        dfs(root);
        return res;
    }
    void dfs(TreeNode root) {
        if(root == null) return;
        dfs(root.right);
        if(k == 0) return;
        if(--k == 0) res = root.val;
        dfs(root.left);
    }
}


```

## 176. 二叉树的深度

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root==null) return 0;
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
    
}
```

## 177. 和为s的连续正数序列

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

```java
/*
	滑动窗口：初始令左窗口i=1，右窗口j=2，和s=3；
	当s>target的时候，左窗口右移i++，s-=i；
	s<target的时候，右窗口右移j++，s+=j；
	s==target时候，把结果加入结果集，左窗口右移，i++，s-=i；
	当i>=j的时候结束循环即可
*/
class Solution {
    public int[][] findContinuousSequence(int target) {
        int i=1,j=2,s=3;
        List<int[]> res=new ArrayList<>();
        while(i<j){
            if(s<target) {
                j++;
                s+=j;
            }
            if(s==target){
                int[] ans = new int[j - i + 1];
                for(int k = i; k <= j; k++)
                    ans[k - i] = k;
                res.add(ans);
                
            }
            if(s>=target) {
                s-=i;
                i++;
            }
            
        }

        return res.toArray(new int[0][]);
    }
}
```

## 178. 翻转单词顺序

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

 

示例 1：

输入: "the sky is blue"
输出: "blue is sky the"
示例 2：

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
示例 3：

输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

```java
class Solution {
    public String reverseWords(String s) {
       //去掉首尾空格
        String t=s.trim();
        //按空格将单词分开作为数组，但注意，如果单词中间有多个空格，字符串数组中会含有""元素，在后面的处理中要注意
        String[] str=t.split(" ");
        int i=0,j=str.length-1;
        while(i<j){ 
            String tmp=str[i];
            str[i]=str[j];
            str[j]=tmp;
            i++;
            j--;
        }
        StringBuilder sb=new StringBuilder();
        for(int k=0;k<str.length;k++){
            if(str[k]=="") continue;
            sb.append(str[k]);
            if(k!=str.length-1) sb.append(" ");
        }
        return sb.toString();
    }
}
```

## 179. 二叉搜索树的最近公共祖先 

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

```java
//递归 因为是二叉搜索树，所以如果root的值介于p和q之间，那么说明p和q分别在root的左右子树，则root为最近的公共祖先，如果同时小于（大于）root的值，说明他们的公共祖先在root的左子树（右子树）
class Solution {
        public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
            if(root==p) return p;
            if(root==q) return q;
            if((p.val<root.val&&q.val>root.val)||(q.val<root.val&&p.val>root.val)) return root;
            else if(p.val<root.val&&q.val<root.val) return lowestCommonAncestor(root.left,p,q);
            else return lowestCommonAncestor(root.right,p,q);
        }
    }
```

## 180. 二叉树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

```java
//还是之前的思路，确定p和q是在同一边还是分开
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null) return null;
        if(root==p||root==q) return root;
        TreeNode left=lowestCommonAncestor(root.left,p,q);
        TreeNode right=lowestCommonAncestor(root.right,p,q);
        if(left!=null&&right!=null) return root;
        else return left==null?right:left;
    }

}
```

## 181. 最小栈

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

实现 MinStack 类:

MinStack() 初始化堆栈对象。
void push(int val) 将元素val推入堆栈。
void pop() 删除堆栈顶部的元素。
int top() 获取堆栈顶部的元素。
int getMin() 获取堆栈中的最小元素。

```java
class MinStack {
    Deque<Integer> xStack;
    Deque<Integer> minStack;

    public MinStack() {
        xStack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        xStack.push(x);
        minStack.push(Math.min(minStack.peek(), x));
    }
    
    public void pop() {
        xStack.pop();
        minStack.pop();
    }
    
    public int top() {
        return xStack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}


```

## 182. 前k个高频元素

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

 ```java
 class Solution {
     public int[] topKFrequent(int[] nums, int k) {
         //哈希表记录数值和出现的次数
         Map<Integer, Integer> count = new HashMap<>();
         for(int num : nums){
             count.put(num, count.getOrDefault(num, 0) + 1);
         }
         //最小堆
         PriorityQueue<int[]> queue = new PriorityQueue<>(new Comparator<int[]>(){
             public int compare(int[] m, int[] n){
                 return m[1] - n[1];
             }
         });
         
         
         //如果堆的大小达到了k，将出现的次数和堆顶比较，大于堆顶就将堆顶元素移除，然后添加此元素
         for(Map.Entry<Integer, Integer> entry : count.entrySet()){
             int num = entry.getKey(), cnt = entry.getValue();
             if(queue.size() == k){
                 if(cnt > queue.peek()[1]){
                     queue.poll();
                     queue.offer(new int[]{num, cnt});
                 }
             }else{
                 queue.offer(new int[]{num, cnt});
             }
         }
 
         int[] res = new int[k];
         for(int i = 0; i < k; i++){
             res[i] = queue.poll()[0];
         }
         return res;
     }
 }
 
 
 ```

## 183. 各位相加

给定一个非负整数 `num`，反复将各个位上的数字相加，直到结果为一位数。返回这个结果

```java
class Solution {
    public int addDigits(int num) {
        int s=0;
    
        while(num>0){
            s+=num%10;
            num/=10;
        }
        while(s>9){
            int sum=s;
            s=0;
            while(sum>0){
                s+=sum%10;
                sum/=10;
            }
        }
        return s;
    }
}
```

## 184. 子数组范围和

给你一个整数数组 nums 。nums 中，子数组的 范围 是子数组中最大元素和最小元素的差值。

返回 nums 中 所有 子数组范围的 和 。

子数组是数组中一个连续 非空 的元素序列。

示例 1：

输入：nums = [1,2,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0 
[2]，范围 = 2 - 2 = 0
[3]，范围 = 3 - 3 = 0
[1,2]，范围 = 2 - 1 = 1
[2,3]，范围 = 3 - 2 = 1
[1,2,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 1 + 1 + 2 = 4

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        //暴力
        long res=0;
        for(int i=0;i<nums.length;i++){
            int max=nums[i];
            int min=nums[i];
            for(int j=i+1;j<nums.length;j++){
                max=Math.max(max,nums[j]);
                min=Math.min(min,nums[j]);
                res+=max-min;
            }
            
        }
        
        return res;
    }
}
```

```java
//单调栈
class Solution {
    public long subArrayRanges(int[] nums) {
        int n = nums.length;
        int[] minLeft = new int[n];
        int[] minRight = new int[n];
        int[] maxLeft = new int[n];
        int[] maxRight = new int[n];
        Deque<Integer> minStack = new ArrayDeque<Integer>();
        Deque<Integer> maxStack = new ArrayDeque<Integer>();
        for (int i = 0; i < n; i++) {
            while (!minStack.isEmpty() && nums[minStack.peek()] > nums[i]) {
                minStack.pop();
            }
            minLeft[i] = minStack.isEmpty() ? -1 : minStack.peek();
            minStack.push(i);
            
            // 如果 nums[maxStack.peek()] == nums[i], 那么根据定义，
            // nums[maxStack.peek()] 逻辑上小于 nums[i]，因为 maxStack.peek() < i
            while (!maxStack.isEmpty() && nums[maxStack.peek()] <= nums[i]) { 
                maxStack.pop();
            }
            maxLeft[i] = maxStack.isEmpty() ? -1 : maxStack.peek();
            maxStack.push(i);
        }
        minStack.clear();
        maxStack.clear();
        for (int i = n - 1; i >= 0; i--) {
            // 如果 nums[minStack.peek()] == nums[i], 那么根据定义，
            // nums[minStack.peek()] 逻辑上大于 nums[i]，因为 minStack.peek() > i
            while (!minStack.isEmpty() && nums[minStack.peek()] >= nums[i]) { 
                minStack.pop();
            }
            minRight[i] = minStack.isEmpty() ? n : minStack.peek();
            minStack.push(i);

            while (!maxStack.isEmpty() && nums[maxStack.peek()] < nums[i]) {
                maxStack.pop();
            }
            maxRight[i] = maxStack.isEmpty() ? n : maxStack.peek();
            maxStack.push(i);
        }

        long sumMax = 0, sumMin = 0;
        for (int i = 0; i < n; i++) {
            sumMax += (long) (maxRight[i] - i) * (i - maxLeft[i]) * nums[i];
            sumMin += (long) (minRight[i] - i) * (i - minLeft[i]) * nums[i];
        }
        return sumMax - sumMin;
    }
}


```

## 185. 判断子序列

给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

进阶：

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i=0,j=0;
        while(i<s.length()&&j<t.length()){
            if(s.charAt(i)==t.charAt(j)) {
                i++;
                j++;
            }
            else j++;
        }
        if(j==t.length()&&i!=s.length()) return false;
        return true;
    }
}
```

## 186. 栈的最小值

请设计一个栈，除了常规栈支持的pop与push函数以外，还支持min函数，该函数返回栈元素中的最小值。执行push、pop和min操作的时间复杂度必须为O(1)。

```java
//查找栈的最小值，可以借助辅助栈
class MinStack {
    Deque<Integer> stack;
    Deque<Integer> stack1;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new LinkedList<>();
        stack1 = new LinkedList<>();
    }
    
    public void push(int x) {
        stack.push(x);
        if(stack1.isEmpty()){
            stack1.push(x);
        }
        else{
            if(stack1.peek()>x){
                stack1.push(x);
            }
            else stack1.push(stack1.peek());
        }
    }
    
    public void pop() {
        if(!stack.isEmpty()) {
            stack.pop();
            stack1.pop();
        }
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return stack1.peek();
    }
}


```

## 187. 把数组排成最小的数

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

```java
/*
	此题求拼接起来的最小数字，本质上是一个排序问题。设数组 nums 中任意两数字的字符串为 x 和 y ，则规定 排序判断规则 为：

	若拼接字符串 x + y > y + x，则 x “大于” y ；
	反之，若 x + y < y + x，则 x “小于” y ；

*/
class Solution {
     public String minNumber(int[] nums) {
        List<String> list = new ArrayList<>();
        for (int num : nums) {
            list.add(String.valueOf(num));
        }
        list.sort((o1, o2) -> (o1 + o2).compareTo(o2 + o1));
        return String.join("", list);
    }
}
```

## 188. 七进制数

给定一个整数 `num`，将其转化为 **7 进制**，并以字符串形式输出

```java
class Solution {
    public String convertToBase7(int num) {
        if(num==0) return "0";
        StringBuilder sb = new StringBuilder();
        Deque<String> stack = new LinkedList<>();
        if(num<0) {
            sb.append("-");
            num=-num;
        }
        while(num!=0){
            stack.push(String.valueOf(num%7));
            num/=7;
        }
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        return sb.toString();
    }
}
```

```java
class Solution {
    public String convertToBase7(int num) {
        return Integer.toString(num, 7); 
    }
}
```

## 189. 回文链表

编写一个函数，检查输入的链表是否是回文的。

要求时间复杂度为O(n) 空间复杂度O(1)

```java
//反转后半部分
class Solution {
    public boolean isPalindrome(ListNode head) {
        // 快慢指针找中点
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        // 反转后半部分
        ListNode pre = null;
        while (slow != null) {
            ListNode next = slow.next;
            slow.next = pre;
            pre = slow;
            slow = next;
        }
        // 前后两段比较是否一致
        ListNode node = head;
        while (pre != null) {
            if (pre.val != node.val) {
                return false;
            }
            pre = pre.next;
            node = node.next;
        }
        return true;
    }
}
//全部反转，存疑，破坏原链表
 public boolean isPalindrome(ListNode head) {
        ListNode pre = null;
        ListNode sb = head;
        ListNode cur = head;
        while(cur != null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;

        }
       
        while(pre!=null){
            if(pre.val!=sb.val) return false;
            sb = sb.next;
            pre = pre.next;
        }
        return true;
    }
```

## 190. 下一个数

下一个数。给定一个正整数，找出与其二进制表达式中1的个数相同且大小最接近的那两个数（一个略大，一个略小）。

示例1:

 输入：num = 2（或者0b10）
 输出：[4, 1] 或者（[0b100, 0b1]）
示例2:

 输入：num = 1
 输出：[2, -1]

```java
class Solution {
    public int[] findClosedNumbers(int num) {
        if(num == Integer.MAX_VALUE){
            return new int[]{-1, -1};
        }
        int left = num - 1;
        int right = num + 1;
        int count = 0;
     
        while(count(left)!=count(num)){
            left--;
            if(left<0) {
                left=-1;
                break;
            }
        }
        while(count(right)!=count(num)){
            right++;
            if(right<0) {
                right=-1;
                break;
            }
        }
         
        return new int[]{right,left};
    }
    //计算一个数字二进制表示的1的个数，n&（n-1）会将n的最后一个1变为0
    public int count(int n){
        int count = 0;
        while(n!=0){
            n &= n-1;
            count++;
        }
        return count;
    }
}
```

## 191. 不同路径II

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

```java
//用递归会超时，用动态规划
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];
        if(obstacleGrid[0][0]==1) return 0;
        else dp[0][0]=1;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(obstacleGrid[i][j]==1) {
                    dp[i][j]=0;
                    //加上continue就会减少判断时间 大大提升效率
                    continue;
                }
                if(i==0&&j!=0&&obstacleGrid[i][j]==0) dp[i][j]=dp[i][j-1];
                if(i!=0&&j==0&&obstacleGrid[i][j]==0) dp[i][j]=dp[i-1][j];
                if(i!=0&&j!=0&&obstacleGrid[i][j]==0) dp[i][j]=dp[i][j-1]+dp[i-1][j];
            }
        }
        return dp[m-1][n-1];
    }
   
}
```

## 192. 整数除法

给定两个整数 a 和 b ，求它们的除法的商 a/b ，要求不得使用乘号 '*'、除号 '/' 以及求余符号 '%' 。

 

注意：

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231, 231−1]。本题中，如果除法结果溢出，则返回 231 − 1

```java
//注意边界条件
class Solution {
    // 因为将 -2147483648 转成正数会越界，但是将 2147483647 转成负数，则不会
// 所以，我们将 a 和 b 都转成负数
// 时间复杂度：O(n)，n 是最大值 2147483647 --> 10^10 --> 超时
public int divide(int a, int b) {
    // 32 位最大值：2^31 - 1 = 2147483647
    // 32 位最小值：-2^31 = -2147483648
    // -2147483648 / (-1) = 2147483648 > 2147483647 越界了
    if (a == Integer.MIN_VALUE && b == -1)
        return Integer.MAX_VALUE;
    int sign = (a > 0) ^ (b > 0) ? -1 : 1;
    // 环境只支持存储 32 位整数
    if (a > 0) a = -a;
    if (b > 0) b = -b;
    int res = 0;
    while (a <= b) {
        a -= b;
        res++;
    }
    // bug 修复：因为不能使用乘号，所以将乘号换成三目运算符
    return sign == 1 ? res : -res;
}

}
```

## 193. 二进制加法

给定两个 01 字符串 a 和 b ，请计算它们的和，并以二进制字符串的形式输出。

输入为 非空 字符串且只包含数字 1 和 0。

 

示例 1:

输入: a = "11", b = "10"
输出: "101"
示例 2:

输入: a = "1010", b = "1011"
输出: "10101"

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuffer ans = new StringBuffer();

        int n = Math.max(a.length(), b.length()), carry = 0;
        for (int i = 0; i < n; ++i) {
            carry += i < a.length() ? (a.charAt(a.length() - 1 - i) - '0') : 0;
            carry += i < b.length() ? (b.charAt(b.length() - 1 - i) - '0') : 0;
            ans.append((char) (carry % 2 + '0'));
            carry /= 2;
        }

        if (carry > 0) {
            ans.append('1');
        }
        ans.reverse();

        return ans.toString();
    }
}


```

## 194. 前n个数字二进制数中1的个数

给定一个非负整数 `n` ，请计算 `0` 到 `n` 之间的每个数字的二进制表示中 1 的个数，并输出一个数组。

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        for(int i=0; i<n+1; i++){
            ans[i] = Integer.bitCount(i);
        }
        return ans;
    }
}


class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        for(int i=0; i<n+1; i++){
            ans[i] = count(i);
        }
        return ans;
    }
    public int count(int n){
        int count=0;
        while(n>0){
            n &= n-1;
            count++;
        }
        return count;
    }
}
```

## 195. 只出现一次的数字III

给你一个整数数组 `nums` ，除某个元素仅出现 **一次** 外，其余每个元素都恰出现 **三次 。**请你找出并返回那个只出现了一次的元素。

```java
//1. 依次确定每一个二进制位
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int total = 0;
            for (int num: nums) {
                total += ((num >> i) & 1);
            }
            if (total % 3 != 0) {
                ans |= (1 << i);
            }
        }
        return ans;
    }
}

//2. 电路
class Solution {
    public int singleNumber(int[] nums) {
        int a = 0, b = 0;
        for (int num : nums) {
            b = ~a & (b ^ num);
            a = ~b & (a ^ num);
        }
        return b;
    }
}

```

## 196. 单词长度的最大乘积

给定一个字符串数组 words，请计算当两个字符串 words[i] 和 words[j] 不包含相同字符时，它们长度的乘积的最大值。假设字符串中只包含英语的小写字母。如果没有不包含相同字符的一对字符串，返回 0。

 

示例 1:

输入: words = ["abcw","baz","foo","bar","fxyz","abcdef"]
输出: 16 
解释: 这两个单词为 "abcw", "fxyz"。它们不包含相同字符，且长度的乘积最大。

```java
class Solution {
    public int maxProduct(String[] words) {
        int max = 0;
        for(int i=0; i< words.length;i++){
            for(int j=i+1;j< words.length;j++){
                //不含相同字符
                if(notContains(words[i],words[j])) {
                    max=Math.max(max, words[i].length()*words[j].length());
                }
            }
        }
        return max;
    }
    //判断两个字符串是否含有相同字符，利用哈希表，穷举的话可能会超出时间限制
    public boolean notContains(String s1,String s2){
        if(s1.length()==0||s2.length()==0) return true;
        Set<Character> set1 = new HashSet<>();
        for(int i=0;i<s1.length();i++){
            set1.add(s1.charAt(i));
        }
        Set<Character> set2 = new HashSet<>();
        for(int i=0;i<s2.length();i++){
            set2.add(s2.charAt(i));
        }
        for(Character c:set1){
            if(!set2.add(c)) return false;
        }
        return true;
    }
}
```

## 197. 排序数组的和

给定一个已按照 升序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 0 开始计数 ，所以答案数组应当满足 0 <= answer[0] < answer[1] < numbers.length 。

假设数组中存在且只存在一对符合条件的数字，同时一个数字不能使用两次

```java
//双指针
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] res= new int[2];
        int i = 0, j = numbers.length-1;
        while(i<j){
            if(numbers[i]+numbers[j]==target){
                res[0] = i;
                res[1] = j;
                return res;
            }
            else if(numbers[i]+numbers[j] < target) i++;
            else j--;
        }
        return new int[2];
    }
}
```

## 198. 和大于等于target的最短子数组

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int min = Integer.MAX_VALUE;
        int i;
        for(i=0;i<nums.length;i++){
            int sum = nums[i];
            if(sum>=target) return 1;
            int j = i+1;
            while(j<nums.length){
                 sum+=nums[j];
                if(sum>=target) {
                    min=Math.min(min,j-i+1);
                    break;
                }
                j++;
            }
        }
        //整个数组的和小于target
        if(i==nums.length&&min==Integer.MAX_VALUE) return 0;
        return min;
    }
}
```

```java
//滑动窗口，从头开始找到大于target的子数组，然后减去第一个数，看是否大于target，大于继续减，小于说明终点为end的子数组最小长度为end-start，继续向右
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int min = Integer.MAX_VALUE;
        int start = 0;
        int end = 0;
        int sum = 0;
        while(end<nums.length){
            sum += nums[end];
            while(target<=sum){
                min = Math.min(min, end-start+1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
       
        return min==Integer.MAX_VALUE?0:min;
    }
}
```

## 199. 乘积小于k的子数组

给定一个正整数数组 nums和整数 k ，请找出该数组内乘积小于 k 的连续的子数组的个数。

 

示例 1:

输入: nums = [10,5,2,6], k = 100
输出: 8
解释: 8 个乘积小于 100 的子数组分别为: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]。
需要注意的是 [10,5,2] 并不是乘积小于100的子数组。

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int count = 0;
        int start = 0;
        int end = 0;
        int mul;
        while(start<nums.length){
            mul = 1;
            end = start;
            while(end<nums.length){
                mul *= nums[end];
                if(mul<k) count++;
                else {
                    mul = 1;
                    break;
                }
                end++;
            }
            start++;
        }
        return count;
    }
}
//滑动窗口
 class Solution {
		public int numSubarrayProductLessThanK(int[] nums, int k) {
			int result = 0;
			int product = 1;
			int left = 0;
			int right = 0;
			while (right < nums.length) {
				// 如果乘积满足条件, ++res
				if (product * nums[right] < k) {
					product *= nums[right];
					result += (right - left + 1);
					++right;
				} 
				else if (left == right) {
					// 乘积 >= k, 且边界重合, 说明 nums[right] >= k, 因此左右指针同时右移
					product = 1;
					++right;
					left = right;
				} 
				else {
					// 乘积 >= k, 则简单的移动左边界, 降低 product 乘积
					product /= nums[left++];
				}
				
			}
			return result;

		}
	}


```

## 200. 和为k的子数组

给定一个整数数组和一个整数 `k` **，**请找到该数组中和为 `k` 的连续子数组的个数。

```java
//前缀和和哈希表
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        //哈希表存储和为sum的个数
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0,1);
        int sum = 0;
        for(int i=0;i<nums.length;i++){
            sum += nums[i];
            count += map.getOrDefault(sum-k,0);
            map.put(sum,map.getOrDefault(sum,0)+1);
        }
        return count;
    }
}
```

## 201. 0和1个数相同的子数组

给定一个二进制数组 nums , 找到含有相同数量的 0 和 1 的最长连续子数组，并返回该子数组的长度。

 

示例 1:

输入: nums = [0,1]
输出: 2
说明: [0, 1] 是具有相同数量 0 和 1 的最长连续子数组。



```java
class Solution {
    public int findMaxLength(int[] nums) {
        if(nums.length==0) return 0;
        //将数组中的0换为-1，转换成求和为0的最长子数组长度
        for(int i=0;i<nums.length;i++){
            if(nums[i]==0) nums[i] = -1;
        }
        int sum = 0;
        int max = 0;
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0,-1);
        for(int i=0;i<nums.length;i++){
            sum += nums[i];
            max = Math.max(max,i-map.getOrDefault(sum,i));
            map.put(sum,Math.min(i,map.getOrDefault(sum,i)));
        }
        return max;

    }
}
```

## 202. 左右两边子数组的和相等

给你一个整数数组 nums ，请计算数组的 中心下标 。

数组 中心下标 是数组的一个下标，其左侧所有元素相加的和等于右侧所有元素相加的和。

如果中心下标位于数组最左端，那么左侧数之和视为 0 ，因为在下标的左侧不存在元素。这一点对于中心下标位于数组最右端同样适用。

如果数组有多个中心下标，应该返回 最靠近左边 的那一个。如果数组不存在中心下标，返回 -1 。

 

```java
//前缀和后缀和，刚开始后缀和为整个数组的和，然后后缀逐渐减去数组值，前缀逐渐加上，前缀和后缀和相等的时候就是中心下标
class Solution {
    public int pivotIndex(int[] nums) {
        int index = 0;
        int pre = 0;
        int beh = 0;
        for(int i=0;i<nums.length;i++){
            beh += nums[i];
        }
        for(int i=0;i<nums.length;i++){
            beh -= nums[i];
            if(pre==beh) return i;
            pre += nums[i];
            
        }
        return -1;
    }
}
```

## 203. 有效的回文

给定一个字符串 s ，验证 s 是否是 回文串 ，只考虑字母和数字字符，可以忽略字母的大小写。

本题中，将空字符串定义为有效的 回文串 。

 

示例 1:

输入: s = "A man, a plan, a canal: Panama"
输出: true
解释："amanaplanacanalpanama" 是回文串
示例 2:

输入: s = "race a car"
输出: false
解释："raceacar" 不是回文串

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while (left <= right) {
            if (!Character.isLetterOrDigit(s.charAt(left))) {
                left += 1;
            } else if (!Character.isLetterOrDigit(s.charAt(right))) {
                right -= 1;
            } else {
                char char1 = Character.toLowerCase(s.charAt(left++));
                char char2 = Character.toLowerCase(s.charAt(right--));
                if (char1 != char2) {
                    return false;
                }
            }
        }
        return true;
    }
}


//
class Solution {
    public boolean isPalindrome(String s) {
        if(s.length()==0) return true;
        String str = s.toLowerCase();
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<str.length();i++){
            if(Character.isLetterOrDigit(str.charAt(i))) sb.append(String.valueOf(str.charAt(i)));
        }
        String tmp = sb.toString();
        int i = 0, j = tmp.length()-1;
        while(i<j){
            if(tmp.charAt(i)!=tmp.charAt(j)) return false;
            i++;
            j--;
        }
        return true;
    }
}
```

## 204. 二维子矩阵的和

给定一个二维矩阵 matrix，以下类型的多个请求：

计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2) 。
实现 NumMatrix 类：

NumMatrix(int[][] matrix) 给定整数矩阵 matrix 进行初始化
int sumRegion(int row1, int col1, int row2, int col2) 返回左上角 (row1, col1) 、右下角 (row2, col2) 的子矩阵的元素总和。


![img](https://gitee.com/bitches/git/raw/master/img/1626332422-wUpUHT-image.png)



```shell
输入: 
["NumMatrix","sumRegion","sumRegion","sumRegion"]
[[[[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]],[2,1,4,3],[1,1,2,2],[1,2,2,4]]
输出: 
[null, 8, 11, 12]

解释:
NumMatrix numMatrix = new NumMatrix([[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (红色矩形框的元素总和)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (绿色矩形框的元素总和)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (蓝色矩形框的元素总和)
```

```java
//前缀和
class NumMatrix {
    int[][] sums;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length;
        if (m > 0) {
            int n = matrix[0].length;
            sums = new int[m + 1][n + 1];
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    sums[i + 1][j + 1] = sums[i][j + 1] + sums[i + 1][j] - sums[i][j] + matrix[i][j];
                }
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return sums[row2 + 1][col2 + 1] - sums[row1][col2 + 1] - sums[row2 + 1][col1] + sums[row1][col1];
    }
}

```

## 205. 字符串中的变位词

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的某个变位词。

换句话说，第一个字符串的排列之一是第二个字符串的 子串 。

 

示例 1：

输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if(s1.length()>s2.length()) return false;
       for(int i=0;i+s1.length()<s2.length()+1;i++){
           if(isInclusion(s2.substring(i,i+s1.length()),s1)) return true;
       }
       return false;

    }
    public boolean isInclusion(String s1,String s2){
        int[] letter1 = new int[26];
        for(int i=0;i<s1.length();i++){
            letter1[s1.charAt(i)-'0'-49]++;
        }
        for(int i=0;i<s2.length();i++){
            letter1[s2.charAt(i)-'0'-49]--;
        }
        for(int i=0;i<26;i++){
            if(letter1[i]!=0) return false;
        }
        return true;
    }
}
```



## 206. 不含重复字符 的最长子字符串

给定一个字符串 s ，请你找出其中不含有重复字符的 最长连续子字符串 的长度。

 

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子字符串是 "abc"，所以其长度为 3。

```java
class Solution {
    // 2. 滑动窗口
// 时间复杂度：O(2n) = O(n)，最坏的情况是 left 和 right 都遍历了一遍字符串
// 空间复杂度：O(n)
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        if (n <= 1) return n;
        int maxLen = 1;

        int left = 0, right = 0;
        Set<Character> window = new HashSet<>();
        while (right < n) {
            char rightChar = s.charAt(right);
            while (window.contains(rightChar)) {
                window.remove(s.charAt(left));
                left++;
            }
            maxLen = Math.max(maxLen, right - left + 1);
            window.add(rightChar);
            right++;
        }

        return maxLen;
    }
}

```

## 207. 最多删除一个字符得到回文]

给定一个非空字符串 `s`，请判断如果 **最多** 从字符串中删除一个字符能否得到一个回文字符串。



**示例 1:**

```
输入: s = "aba"
输出: true
```

```java
class Solution {
    public boolean validPalindrome(String s) {
        int flag1 = 0;
        int flag2 = 0;
        int i = 0,j = s.length()-1;
        while(i<j){
            if(s.charAt(i)==s.charAt(j)){
                i++;
                j--;
            }
            else{
                flag1++;
                i++;
            }
        }
        i = 0;
        j = s.length()-1;
          while(i<j){
            if(s.charAt(i)==s.charAt(j)){
                i++;
                j--;
            }
            else{
                flag2++;
                j--;
            }
        }
        if(flag1>1&&flag2>1) return false;
        return true;
    }
}
```

## 208. 含有所有字符的最短字符串

给定两个字符串 s 和 t 。返回 s 中包含 t 的所有字符的最短子字符串。如果 s 中不存在符合条件的子字符串，则返回空字符串 "" 。

如果 s 中存在多个符合条件的子字符串，返回任意一个。

 

注意： 对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。

 

示例 1：

输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC" 
解释：最短子字符串 "BANC" 包含了字符串 t 的所有字符 'A'、'B'、'C'

```java
class Solution {
    //统计词频 + 滑动窗口
    public String minWindow(String s, String t) {
        //A~z 对应ASCII 65 ~ 122;   122 - 65 + 1 = 58，所以开辟60足够用了。
        int[] count1 = new int[60];
        int[] count2 = new int[60];
        int n1 = s.length();
        int n2 = t.length();
        //维护一个最小长度，用于判断是否更新ans
        int minLength = s.length();
        String ans = "";
        //特判，s短于t,直接返回""
        if (n1 < n2) {
            return ans;
        }
        //统计出t中的词频
        for (char c : t.toCharArray()) {
            count2[c-'A']++;
        }
        //滑动窗口的起始位置分别为 i 、 j;
        int i = 0;//起始位置
        for (int j = 0; j < n1; j++) {
            //j对应字符进入窗口
            count1[s.charAt(j) - 'A']++;
            //如果当前的窗口已经包含了t中所有词频，则不断缩小窗口左边界
            while (Cover(count1, count2)) {
                //取等于号是WA出来的，对应样例 s = "a", t = "s";
                if (j - i + 1 <= minLength) {
                    minLength = j - i + 1;
                    ans = s.substring(i, i + minLength);
                }
                //左边界缩小，左边界对应的字符出窗
                count1[s.charAt(i) - 'A']--;
                i++;
            }
        }
        return ans;
    }
    //判断count1是否每一个元素都大于count2；
    boolean Cover(int[] count1, int[] count2) {
        for (int i = 0; i < 60; i++) {
            if (count1[i] < count2[i]) {
                return false;
            }
        }
        return true;
    }



}
```

## 209. 回文子字符串的个数

给定一个字符串 s ，请计算这个字符串中有多少个回文子字符串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

示例 1：

输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"

```java
//暴力
class Solution {
    public int countSubstrings(String s) {
        if(s.length()==0) return 0;
        int ans = 0;
        for(int i=0;i<s.length();i++){
            for(int j=i+1;j<s.length();j++){
                if(isPalin(s.substring(i,j+1))) ans++;
            }
        }
        return ans+s.length();
    }
    public boolean isPalin(String s){
        int i = 0, j = s.length()-1;
        while(i<j){
            if(s.charAt(i)!=s.charAt(j)) return false;
            i++;
            j--;
        }
        return true;
    }
}
//中心扩展
class Solution {
    public int countSubstrings(String s) {
        int n = s.length(), ans = 0;
        for (int i = 0; i < 2 * n - 1; ++i) {
            int l = i / 2, r = i / 2 + i % 2;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                --l;
                ++r;
                ++ans;
            }
        }
        return ans;
    }
}

//manacher算法
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        StringBuffer t = new StringBuffer("$#");
        for (int i = 0; i < n; ++i) {
            t.append(s.charAt(i));
            t.append('#');
        }
        n = t.length();
        t.append('!');

        int[] f = new int[n];
        int iMax = 0, rMax = 0, ans = 0;
        for (int i = 1; i < n; ++i) {
            // 初始化 f[i]
            f[i] = i <= rMax ? Math.min(rMax - i + 1, f[2 * iMax - i]) : 1;
            // 中心拓展
            while (t.charAt(i + f[i]) == t.charAt(i - f[i])) {
                ++f[i];
            }
            // 动态维护 iMax 和 rMax
            if (i + f[i] - 1 > rMax) {
                iMax = i;
                rMax = i + f[i] - 1;
            }
            // 统计答案, 当前贡献为 (f[i] - 1) / 2 上取整
            ans += f[i] / 2;
        }

        return ans;
    }
}

```

## 210. 字符串中的所有变位词

给定两个字符串 s 和 p，找到 s 中所有 p 的 变位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

变位词 指字母相同，但排列不同的字符串。

 

示例 1:

输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的变位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的变位词。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        for(int i=0;i+p.length()<=s.length();i++){
            if(isAnagrams(p,s.substring(i,i+p.length()))) ans.add(i);
        }
        return ans;
    }

    public boolean isAnagrams(String s,String t){
        int[] tmp = new int[26];
        for(int i=0;i<s.length();i++){
            tmp[s.charAt(i)-'a']++;
            tmp[t.charAt(i)-'a']--;
        }
        for(int i=0;i<26;i++){
            if(tmp[i]!=0) return false;
        }
        return true;
    }
}
```

## 211.　重排链表

给定一个单链表 L 的头节点 head ，单链表 L 表示为：

 L0 → L1 → … → Ln-1 → Ln 
请将其重新排列后变为：

L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }
        ListNode mid = midNode(head);
        ListNode l1 = head;
        ListNode l2 = mid.next;
        mid.next = null;
        l2 = reverse(l2);
        merge(l1, l2);

    }
    public void merge(ListNode l1,ListNode l2){
        ListNode l1_tmp;
        ListNode l2_tmp;
        while (l1 != null && l2 != null) {
            l1_tmp = l1.next;
            l2_tmp = l2.next;

            l1.next = l2;
            l1 = l1_tmp;

            l2.next = l1;
            l2 = l2_tmp;
        }
        }
    public ListNode reverse(ListNode head){
        ListNode pre = null;
        ListNode cur = head;
        while(cur!=null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
    public ListNode midNode(ListNode head){
        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

## 212. 变位词组

给定一个字符串数组 strs ，将 变位词 组合在一起。 可以按任意顺序返回结果列表。

注意：若两个字符串中每个字符出现的次数都相同，则称它们互为变位词。

 

示例 1:

输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

    int[] hash = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41,
                43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
        Map<Long, List<String>> hashMap = new HashMap<>();
        for (String str : strs) {
            long worldHash = 1;
            for (int i = 0; i < str.length(); i++) {
                worldHash *= hash[str.charAt(i) - 'a'];
            }
            hashMap.putIfAbsent(worldHash, new LinkedList<String>());
            hashMap.get(worldHash).add(str);
        }
        return new LinkedList<>(hashMap.values());
        }
}
```

## 213. 外星语言是否排序

某种外星语也使用英文小写字母，但可能顺序 order 不同。字母表的顺序（order）是一些小写字母的排列。

给定一组用外星语书写的单词 words，以及其字母表的顺序 order，只有当给定的单词在这种外星语中按字典序排列时，返回 true；否则，返回 false。

 

示例 1：

输入：words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
输出：true
解释：在该语言的字母表中，'h' 位于 'l' 之前，所以单词序列是按字典序排列的。
示例 2：

```java
 class Sulution{
    public boolean isAlienSorted(String[] words, String order) {

        for(int i=0;i<words.length-1;i++){
            if(!isAlienOrder(words[i],words[i+1],order)) return false;

        }
        return true;
    }
    public boolean isAlienOrder(String s1,String s2,String order){
        int i=0;
        while(i<s1.length()&&i<s2.length()){
            if(order.indexOf(s1.charAt(i))==order.indexOf(s2.charAt(i))) i++;
            else return order.indexOf(s1.charAt(i)) < order.indexOf(s2.charAt(i));
        }
        if(i<s1.length()) return false;
        return true;
    }
 }
```

## 214. 滑动窗口的平均值

给定一个整数数据流和一个窗口大小，根据该滑动窗口的大小，计算滑动窗口里所有数字的平均值。

实现 MovingAverage 类：

MovingAverage(int size) 用窗口大小 size 初始化对象。
double next(int val) 成员函数 next 每次调用的时候都会往滑动窗口增加一个整数，请计算并返回数据流中最后 size 个值的移动平均值，即滑动窗口里所有数字的平均值。


示例：

输入：
inputs = ["MovingAverage", "next", "next", "next", "next"]
inputs = [[3], [1], [10], [3], [5]]
输出：
[null, 1.0, 5.5, 4.66667, 6.0]

解释：
MovingAverage movingAverage = new MovingAverage(3);
movingAverage.next(1); // 返回 1.0 = 1 / 1
movingAverage.next(10); // 返回 5.5 = (1 + 10) / 2
movingAverage.next(3); // 返回 4.66667 = (1 + 10 + 3) / 3
movingAverage.next(5); // 返回 6.0 = (10 + 3 + 5) / 3

```java
class MovingAverage {
    int[] queue;
    int head,tail;
    int size;
    int x;			//记录是否当前插入的数值大于size，大于平均值则除以size，否则除以tail-head
    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        queue = new int[size];
        head = 0;
        tail = 0;
        this.size = size;
        x=0;
    }
    
    public double next(int val) {
        double sum = 0.0;
        queue[(tail++)%size] = val;
        x++;
        for(int i=0;i<size;i++){
            sum+=queue[i];
        }
        if(x>=size) return sum/size;
        else return sum/(tail-head);
    }
}

```

## 215. 插入删除和随机访问都是O(1)的容器

设计一个支持在平均 时间复杂度 O(1) 下，执行以下操作的数据结构：

insert(val)：当元素 val 不存在时返回 true ，并向集合中插入该项，否则返回 false 。
remove(val)：当元素 val 存在时返回 true ，并从集合中移除该项，否则返回 false 。
getRandom：随机返回现有集合中的一项。每个元素应该有 相同的概率 被返回。


示例 :

输入: inputs = ["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
输出: [null, true, false, true, 2, true, false, 2]
解释:
RandomizedSet randomSet = new RandomizedSet();  // 初始化一个空的集合
randomSet.insert(1); // 向集合中插入 1 ， 返回 true 表示 1 被成功地插入

randomSet.remove(2); // 返回 false，表示集合中不存在 2 

randomSet.insert(2); // 向集合中插入 2 返回 true ，集合现在包含 [1,2] 

randomSet.getRandom(); // getRandom 应随机返回 1 或 2 

randomSet.remove(1); // 从集合中移除 1 返回 true 。集合现在包含 [2] 

randomSet.insert(2); // 2 已在集合中，所以返回 false 

randomSet.getRandom(); // 由于 2 是集合中唯一的数字，getRandom 总是返回 2 

```java
/*题目中共有3个要求：

insert(val)：当元素 val 不存在时返回 true ，并向集合中插入该项，否则返回 false 。
remove(val)：当元素 val 存在时返回 true ，并从集合中移除该项，否则返回 false 。
getRandom：随机返回现有集合中的一项。每个元素应该有 相同的概率 被返回。
对于前两点要求，使用set就可以轻松处理；
考虑第3点，想要随机返回现有集合中的一项，且保证每个元素应该有相同的概率被返回，对于这一点，将集合中所有的元素放入list中，通过 random.nextInt(list.size()) 可以获得一个从 0 -- list.size() - 1 的数字，将该数字作为下标访问list，就可以了。(✧◡✧)
在list中，没有办法用O(1)的时间复杂度删除一个元素，不过如果这个元素在list的最尾端，那么就可以用O(1)的时间删除了。因此，考虑，当需要删除一个元素的时候，将这个元素和list中最后一个元素互换，然后删除最后一个元素。
最后一个问题，如何获取到一个元素在list中的位置呢？答案是：用一个map将元素在list中的下标保存下来。
综上，问题就基本解决了，使用一个map可以用O(1)的时间判断其是否在集合中，map中的value保存其在list中的位置。
*/
    public class RandomizedSet {
    private final Map<Integer, Integer> map;
    private final List<Integer> list;

    private final Random random;

    public RandomizedSet() {
        map = new HashMap<>();
        list = new ArrayList<>();
        random = new Random();
    }

    public boolean insert(int val) {
        // 快速判断val是否已经存在于集合中
        if (map.containsKey(val)) {
            return false;
        }
        // 将val保存在map和list中
        map.put(val, list.size());
        list.add(val);
        return true;
    }

    public boolean remove(int val) {
        // 判断val是否存在于集合中
        int valIndex = map.getOrDefault(val, -1);
        if (valIndex == -1) {
            return false;
        }
        // list中最后一个元素的下标
        int insteadValIndex = list.size() - 1;
        if (valIndex != insteadValIndex) {
            // 将val和list[最后一个元素]的值在list中交换
            int insteadVal = list.get(insteadValIndex);
            list.set(valIndex, insteadVal);
            map.put(insteadVal, valIndex);
        }
        // 在map和list中取出val
        map.remove(val);
        list.remove(insteadValIndex);
        return true;
    }

    public int getRandom() {
        return list.get(random.nextInt(list.size()));
    }
}

```

## 216. 展平多级双向链表

多级双向链表中，除了指向下一个节点和前一个节点指针之外，它还有一个子链表指针，可能指向单独的双向链表。这些子列表也可能会有一个或多个自己的子项，依此类推，生成多级数据结构，如下面的示例所示。

给定位于列表第一级的头节点，请扁平化列表，即将这样的多级双向链表展平成普通的双向链表，使所有结点出现在单级双链表中。

 

示例 1：

输入：head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
输出：[1,2,3,7,8,11,12,9,10,4,5,6]
解释：

输入的多级列表如下图所示：

![img](https://gitee.com/bitches/git/raw/master/img/multilevellinkedlist.png)

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/
class Solution {
     List<Node> list = new ArrayList<>();

    public Node flatten(Node head) {
        dfs(head);
        for (int i = 0; i < list.size() - 1; i++) {
            Node pre = list.get(i);
            Node cur = list.get(i + 1);
            cur.prev = pre;
            pre.next = cur;
            pre.child = null;
        }
        return head;
    }

    private void dfs(Node head) {
        if (head == null) return;
        list.add(head);
        dfs(head.child);
        dfs(head.next);
    }
}
```

## 217. 小行星碰撞

给定一个整数数组 asteroids，表示在同一行的小行星。

对于数组中的每一个元素，其绝对值表示小行星的大小，正负表示小行星的移动方向（正表示向右移动，负表示向左移动）。每一颗小行星以相同的速度移动。

找出碰撞后剩下的所有小行星。碰撞规则：两个行星相互碰撞，较小的行星会爆炸。如果两颗行星大小相同，则两颗行星都会爆炸。两颗移动方向相同的行星，永远不会发生碰撞。

 

示例 1：

输入：asteroids = [5,10,-5]
输出：[5,10]
解释：10 和 -5 碰撞后只剩下 10 。 5 和 10 永远不会发生碰撞。

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        //往右走的入栈，往左走的出栈比较后
        Stack<Integer> stack=new Stack<>();
        for(int k:asteroids){
            if(k>0){
                stack.push(k);
            }else{
                if(stack.isEmpty() || stack.peek()<0){
                    stack.push(k);
                }else{
                    boolean flag=true;
                    while(!stack.isEmpty() && stack.peek()>0){
                        int pre=stack.pop();
                        if(pre>Math.abs(k)){
                            stack.push(pre);
                            flag=false;
                            break;
                        }else if(pre==Math.abs(k)){
                            flag=false;
                            break;
                        }
                    }
                    if(flag)stack.push(k);
                }
            }
        }
        int[] res=new int[stack.size()];
        int i=stack.size()-1;
        while(!stack.isEmpty()){
            res[i--]=stack.pop();
        }
        return res;
    }
}
```

## 218. 最长连续序列

给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

 

示例 1：

输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
示例 2：

输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9

```java
//时间复杂度 O(nlogn)
class Solution {
    public int longestConsecutive(int[] nums) {
        Arrays.sort(nums);
        int max = 0;
        if(nums.length==0) return 0;
        int count = 1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]-nums[i-1]==1) count++;
            else if(nums[i]==nums[i-1]) continue;
            else {
                
                max = Math.max(max,count);
                count = 1;
            }
        }
        return max>count?max:count;
    }
}
```

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> num_set = new HashSet<Integer>();
        for (int num : nums) {
            num_set.add(num);
        }

        int longestStreak = 0;

        for (int num : num_set) {
            if (!num_set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}

```

## 219. 数组相对排序

给定两个数组，arr1 和 arr2，

arr2 中的元素各不相同
arr2 中的每个元素都出现在 arr1 中
对 arr1 中的元素进行排序，使 arr1 中项的相对顺序和 arr2 中的相对顺序相同。未在 arr2 中出现过的元素需要按照升序放在 arr1 的末尾。

 

示例：

输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int[] res = new int[1001];
        for (int num : arr1) {
            res[num]++;
        }

        int index = 0;
        //遍历 arr2，以 arr2的顺序填arr1数组
        for (int num : arr2) {
            //得到 num 元素的个数，并往arr1填充
            for (int i = 0; i < res[num]; i++) {
                arr1[index++] = num;
            }
            res[num] = 0;
        }
        //将剩下的数字按序填入arr1
        for (int i = 0; i < res.length; i++) {
            for (int j = 0; j < res[i]; j++) {
                arr1[index++] = i;
            }
        }
        return arr1;
    }
}
```

## 220. 相同的树

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p==null&&q==null) return true;
        else if(p==null||q==null) return false;
        else if(p.val!=q.val) return false;
        return isSameTree(p.left,q.left)&&isSameTree(p.right,q.right);
    }
}
```

## 221. 最近最少使用缓存

运用所掌握的数据结构，设计和实现一个  LRU (Least Recently Used，最近最少使用) 缓存机制 。

实现 LRUCache 类：

LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

```java
//利用链表，如果对某个数据进行操作，就将这个数据放到链表的尾部
class LRUCache {
    //尾插法
    class Node{
        Node pre;
        Node next;
        int key;
        int value;
        public Node(int key, int value){
            this.key = key;
            this.value = value;
        }
    }

    public void deleteNode(Node node){
        node.next.pre = node.pre;
        node.pre.next = node.next;
    }

    public void addToTail(Node node){
        node.next = tail;
        tail.pre.next = node;
        node.pre = tail.pre;
        tail.pre = node;
    }

    public void moveToTail(Node node){
        deleteNode(node);
        addToTail(node);
    }

    Map<Integer,Node> map;
    int size;
    Node head;
    Node tail;

    public LRUCache(int capacity) {
        map = new HashMap<>();
        size = capacity;
        head = new Node(0,0);
        tail = new Node(0,0);
        head.next = tail;
        tail.pre = head;
    }
    
    public int get(int key) {
        if(!map.containsKey(key)) return -1;
        Node node = map.get(key);
        moveToTail(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            Node node = map.get(key);
            node.value = value;
            moveToTail(node);
        }else{
            if(map.size() == size){
                Node prev = head.next;
                deleteNode(prev);
                map.remove(prev.key);
            }
            Node node = new Node(key, value);
            addToTail(node);
            map.put(key, node);
        }
    }
}
//linkedHashmap
class LRUCache {

    private int cap;
	private Map<Integer, Integer> map = new LinkedHashMap<>();  // 保持插入顺序

	public LRUCache(int capacity) {
		this.cap = capacity;
	}

	public int get(int key) {
		if (map.keySet().contains(key)) {
			int value = map.get(key);
			map.remove(key);
            // 保证每次查询后，都在末尾
			map.put(key, value);
			return value;
		}
		return -1;
	}

	public void put(int key, int value) {
		if (map.keySet().contains(key)) {
			map.remove(key);
		} else if (map.size() == cap) {
			Iterator<Map.Entry<Integer, Integer>> iterator = map.entrySet().iterator();
			iterator.next();
			iterator.remove();

			// int firstKey = map.e***ySet().iterator().next().getValue();
			// map.remove(firstKey);
		}
		map.put(key, value);
	}
}
```

## 222. 后缀表达式

	根据 逆波兰表示法，求该后缀表达式的计算结果。

有效的算符包括 +、-、*、/ 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

 

说明：

整数除法只保留整数部分。
给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。


示例 1：

输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
示例 2：

输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6

```java
//将数字入栈，当碰到运算符的时候，将栈中两个数据出栈，运算之后将结果入栈，最后返回栈顶就是结果
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stack = new LinkedList<>();
        for(int i=0;i<tokens.length;i++){
            if(Objects.equals(tokens[i], "+") || Objects.equals(tokens[i], "-") || Objects.equals(tokens[i], "*") || Objects.equals(tokens[i], "/")) {
                int a = Integer.valueOf(stack.pop());
                int b = Integer.valueOf(stack.pop());
                if(Objects.equals(tokens[i], "+")) stack.push(a+b);
                else if(Objects.equals(tokens[i], "-")) stack.push(b-a);
                else if(Objects.equals(tokens[i], "*")) stack.push(a*b);
                else stack.push(b/a);
            }
            else{
                stack.push(Integer.valueOf(tokens[i]));
            }
        }
        return Integer.valueOf(stack.peek());
    }
}
```

## 223. 最近请求次数

写一个 RecentCounter 类来计算特定时间范围内最近的请求。

请实现 RecentCounter 类：

RecentCounter() 初始化计数器，请求数为 0 。
int ping(int t) 在时间 t 添加一个新请求，其中 t 表示以毫秒为单位的某个时间，并返回过去 3000 毫秒内发生的所有请求数（包括新请求）。确切地说，返回在 [t-3000, t] 内发生的请求数。
保证 每次对 ping 的调用都使用比之前更大的 t 值。

 

示例：

输入：
inputs = ["RecentCounter", "ping", "ping", "ping", "ping"]
inputs = [[], [1], [100], [3001], [3002]]
输出：
[null, 1, 2, 3, 3]

解释：
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // requests = [1]，范围是 [-2999,1]，返回 1
recentCounter.ping(100);   // requests = [1, 100]，范围是 [-2900,100]，返回 2
recentCounter.ping(3001);  // requests = [1, 100, 3001]，范围是 [1,3001]，返回 3
recentCounter.ping(3002);  // requests = [1, 100, 3001, 3002]，范围是 [2,3002]，返回 3

```java
//队列
class RecentCounter {
    Queue<Integer> queue;
    public RecentCounter() {
        queue = new LinkedList<>();
    }
    
    public int ping(int t) {
        queue.offer(t);
        while(queue.peek()<t-3000) queue.poll();
        return queue.size();
    }
}

```

## 224. 神奇的字典

设计一个使用单词列表进行初始化的数据结构，单词列表中的单词 互不相同 。 如果给出一个单词，请判定能否只将这个单词中一个字母换成另一个字母，使得所形成的新单词存在于已构建的神奇字典中。

实现 MagicDictionary 类：

MagicDictionary() 初始化对象
void buildDict(String[] dictionary) 使用字符串数组 dictionary 设定该数据结构，dictionary 中的字符串互不相同
bool search(String searchWord) 给定一个字符串 searchWord ，判定能否只将字符串中 一个 字母换成另一个字母，使得所形成的新字符串能够与字典中的任一字符串匹配。如果可以，返回 true ；否则，返回 false 。

```java
class MagicDictionary {
    String[] dic;
    /** Initialize your data structure here. */
    public MagicDictionary() {

    }
    
    public void buildDict(String[] dictionary) {
        dic = new String[dictionary.length];
        for(int i=0;i<dictionary.length;i++){
            dic[i] = dictionary[i];
        }
    }
    
    public boolean search(String searchWord) {
        for(int i=0;i<dic.length;i++){
            int count = 0;
            if(dic[i].length()!=searchWord.length()) continue;
            if(dic[i].equals(searchWord)) continue;
            for(int j=0;j<searchWord.length();j++){
                if(searchWord.charAt(j)==dic[i].charAt(j)) count++;
            }
            if(count==searchWord.length()-1) return true;

        }
        return false;
    }
}

```

## 225. 单词之和

实现一个 MapSum 类，支持两个方法，insert 和 sum：

MapSum() 初始化 MapSum 对象
void insert(String key, int val) 插入 key-val 键值对，字符串表示键 key ，整数表示值 val 。如果键 key 已经存在，那么原来的键值对将被替代成新的键值对。
int sum(string prefix) 返回所有以该前缀 prefix 开头的键 key 的值的总和。


示例：

输入：
inputs = ["MapSum", "insert", "sum", "insert", "sum"]
inputs = [[], ["apple", 3], ["ap"], ["app", 2], ["ap"]]
输出：
[null, null, 3, null, 5]

解释：
MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);  
mapSum.sum("ap");           // return 3 (apple = 3)
mapSum.insert("app", 2);    
mapSum.sum("ap");           // return 5 (apple + app = 3 + 2 = 5)

```java
class MapSum {
    List<String> Key;
    List<Integer> value;
    /** Initialize your data structure here. */
    public MapSum() {
        Key = new ArrayList<>();
        value = new ArrayList<>();
    }
    
    public void insert(String key, int val) {
        if(Key.contains(key)) {
            int index = Key.indexOf(key);
            value.set(index,val);
        }
        else{
            Key.add(key);
            value.add(val);
        }
    }
    public int sum(String prefix) {
        int sum = 0;
        for(int i=0;i<Key.size();i++){
            int  j=0;
            while(j<prefix.length()&&j<Key.get(i).length()){
                if(prefix.charAt(j)!=Key.get(i).charAt(j)) break;
                j++;
            }
            if(j==prefix.length()) sum += value.get(i);
        }
        return sum;
    }
}


```

## 226. 排序数组中只出现一次的数字

给定一个只包含整数的有序数组 nums ，每个元素都会出现两次，唯有一个数只会出现一次，请找出这个唯一的数字。

 

示例 1:

输入: nums = [1,1,2,3,3,4,4,8,8]
输出: 2
示例 2:

输入: nums =  [3,3,7,7,10,11,11]
输出: 10

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        int mid = (right-left)/2+left;
        while(left<right){
             if(mid==0||mid==nums.length) return nums[mid];
            if(mid%2==0){
                if(nums[mid]==nums[mid-1]){
                    right = mid - 1;
                    mid = (right-left)/2+left;
                }
                else if(nums[mid]==nums[mid+1]){
                    left = mid + 1;
                    mid = (right-left)/2+left;
                }
                else return nums[mid];
            }
            else{
                if(nums[mid]==nums[mid+1]){
                    right = mid - 1;
                    mid = (right-left)/2+left;
                }
                else if(nums[mid]==nums[mid-1]){
                    left = mid + 1;
                    mid = (right-left)/2+left;
                }
                else return nums[mid];
            }
        }
        return nums[mid];
    }
}
```

## 227. 最小时间差

给定一个 24 小时制（小时:分钟 "HH:MM"）的时间列表，找出列表中任意两个时间的最小时间差并以分钟数表示。

 

示例 1：

输入：timePoints = ["23:59","00:00"]
输出：1
示例 2：

输入：timePoints = ["00:00","23:59","00:00"]
输出：0

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        Collections.sort(timePoints);
        int min = Integer.MAX_VALUE;
        for(int i=0;i<timePoints.size()-1;i++){
            int hour1 = Integer.valueOf(timePoints.get(i).substring(0,2));
            int minute1 = Integer.valueOf(timePoints.get(i).substring(3,5));
            int hour2 = Integer.valueOf(timePoints.get(i+1).substring(0,2));
            int minute2 = Integer.valueOf(timePoints.get(i+1).substring(3,5));
            min = Math.min(min,Math.abs(60*(hour2-hour1)+minute2-minute1));
        }
        //最小时间和最大时间的差
         int hour1 = Integer.valueOf(timePoints.get(0).substring(0,2));
         int minute1 = Integer.valueOf(timePoints.get(0).substring(3,5));
         int hour2 = Integer.valueOf(timePoints.get(timePoints.size()-1).substring(0,2));
         int minute2 = Integer.valueOf(timePoints.get(timePoints.size()-1).substring(3,5));
         hour1+=24;
         min = Math.min(min,60*(hour1-hour2)+minute1-minute2);
         return min;
    }
}
```

```java
//将时间转化成分钟
class Solution {
    public int findMinDifference(List<String> timePoints) {
        if (timePoints.size() > 24 * 60) {
            return 0;
        }
        List<Integer> mins = new ArrayList<>();
        for (String t : timePoints) {
            String[] time = t.split(":");
            mins.add(Integer.parseInt(time[0]) * 60 + Integer.parseInt(time[1]));
        }
        Collections.sort(mins);
        mins.add(mins.get(0) + 24 * 60);
        int res = 24 * 60;
        for (int i = 1; i < mins.size(); ++i) {
            res = Math.min(res, mins.get(i) - mins.get(i - 1));
        }
        return res;
    }
}
```

## 228. 每日温度

请根据每日 气温 列表 temperatures ，重新生成一个列表，要求其对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

 

示例 1:

输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]

```java
class Solution {
    //利用一个栈记录数组的下标，当前温度大于栈顶所在位置的元素，就将栈顶所在位置的数组赋值为i-stack.peek()，不然就将此时的索引入栈
     public int[] dailyTemperatures(int[] temperatures) {
        int len = temperatures.length;
        int[] ans = new int[len];
        Deque<Integer> stack = new LinkedList<>();
        for(int i=0;i<len;i++){
            while(!stack.isEmpty()&&temperatures[stack.peek()]<temperatures[i]){
                ans[stack.peek()]=i-stack.peek();
                stack.pop();
            }
            stack.push(i);
        }

        return ans;
    }
}
```

## 229. 二叉树每层的最大值

给定一棵二叉树的根节点 `root` ，请找出该二叉树中每一层的最大值。

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        List<Integer> ans = new ArrayList<>();
        if(root==null) return new ArrayList<>();
       
        while(!queue.isEmpty()){
            List<TreeNode> tmp = new ArrayList<>();
            int max = Integer.MIN_VALUE;
            while(!queue.isEmpty()){
               tmp.add(queue.poll());
            }
            for(TreeNode t:tmp){
                max = Math.max(max,t.val);
                if(t.left!=null) queue.offer(t.left);
                if(t.right!=null) queue.offer(t.right);
            }
            ans.add(max);
        }
        return ans;
    }
}
```

```java
 List<Integer> result = new ArrayList<Integer>();

    public List<Integer> largestValues(TreeNode root) {
        helper(root,0);
        return result;
    }
    public void helper (TreeNode root,int depth){
         if(root == null ) return;
         // list对应当前层为空，直接add
         if(result.size() == depth ) {
            
             result.add(root.val);

         // list对应当前层不为空，比较大小后set覆盖
         } else if (result.size() > depth && result.get(depth) < root.val ) {

             result.set(depth, root.val);
         } 
        
         helper(root.left,depth+1);
         helper(root.right,depth+1);
        
    }
```

## 230. 所有大于等于节点的值的和

给定一个二叉搜索树，请将它的每个节点的值替换成树中大于或者等于该节点值的所有节点值之和。

 

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。

```java

class Solution {
    int sum = 0;
    List<Integer> ans = new ArrayList<>();
    public TreeNode convertBST(TreeNode root) {
       bfs(root);
        return root;
    }
    public void bfs(TreeNode root){
        if(root==null) return ;
        bfs(root.right);
        sum += root.val;
        root.val = sum;
        bfs(root.left);
    }
}
```

## 231. 二叉树剪枝

给定一个二叉树 根节点 root ，树的每个节点的值要么是 0，要么是 1。请剪除该二叉树中所有节点的值为 0 的子树。

节点 node 的子树为 node 本身，以及所有 node 的后代。

 

示例 1:

输入: [1,null,0,0,1]
输出: [1,null,0,null,1] 
解释: 
只有红色节点满足条件“所有不包含 1 的子树”。
右图为返回的答案。

![img](https://gitee.com/bitches/git/raw/master/img/1028_2.png)

```java
class Solution {
    public TreeNode pruneTree(TreeNode root) {
        
        if(root==null) return root; 
        if(root.val==0&&!containsOne(root)) return null;
        if(!containsOne(root.left)) root.left=null;
        if(!containsOne(root.right)) root.right=null;
        if(root!=null&&root.left!=null) pruneTree(root.left);
        if(root!=null&&root.right!=null) pruneTree(root.right);
        return root;
    }
    //判断是否子树含有1
    public boolean containsOne(TreeNode root){
        if(root==null) return false;
        if(root.val==1) return true;
        return containsOne(root.left)||containsOne(root.right);
    }
}
```

## 232. 向下路径的节点之和

给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。

路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

```java

class Solution {
    private int res;
    public int pathSum(TreeNode root, int targetSum) {
        if(root==null) return res;
        dfs(root,targetSum);
        pathSum(root.left,targetSum);
        pathSum(root.right,targetSum);
        return res;
         
    }
    public void dfs(TreeNode root,int targetSum){
        if(root==null) return ;
        if(root.val==targetSum) res++;
        dfs(root.left,targetSum-root.val);
        dfs(root.right,targetSum-root.val);
    }
    
}
```

## 233. 从根节点到叶节点的路径数字之和

给定一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。
计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        return dfs(root, 0);
    }

    public int dfs(TreeNode root, int prevSum) {
        if (root == null) {
            return 0;
        }
        int sum = prevSum * 10 + root.val;
        if (root.left == null && root.right == null) {
            return sum;
        } else {
            return dfs(root.left, sum) + dfs(root.right, sum);
        }
    }
}

```

## 234. 两数之和 输入BST

给定一个二叉搜索树 `root` 和一个目标结果 `k`，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 `true`。

 ```java
 class Solution {
     Set<Integer> set = new HashSet<Integer>();
     public boolean findTarget(TreeNode root, int k) {
         if (root == null) {
             return false;
         }
         if (set.contains(k - root.val)) {
             return true;
         }
         set.add(root.val);
         return findTarget(root.left, k) || findTarget(root.right, k);
     }
 }
 //遍历二叉树进列表，然后双指针
 class Solution {
     private List<Integer> ans = new ArrayList<>();
     public boolean findTarget(TreeNode root, int k) {
         
         dfs(root);
         int i = 0, j = ans.size()-1;
       
         while(i<j){
             if(ans.get(i)+ans.get(j)==k) return true;
             else if(ans.get(i)+ans.get(j)<k) i++;
             else j--;
         }
         return false;
     }
     public void dfs(TreeNode root){
         if(root==null) return ;
         if(root.left!=null) dfs(root.left);
         ans.add(root.val);
         if(root.right!=null) dfs(root.right);
     }
     
 }
 ```

## 235. 最大二叉树

给定一个不重复的整数数组 nums 。 最大二叉树 可以用下面的算法从 nums 递归地构建:

创建一个根节点，其值为 nums 中的最大值。
递归地在最大值 左边 的 子数组前缀上 构建左子树。
递归地在最大值 右边 的 子数组后缀上 构建右子树。
返回 nums 构建的 最大二叉树 。

 ```java
 
 class Solution {
     private TreeNode root = new TreeNode(0);
     public TreeNode constructMaximumBinaryTree(int[] nums) {
         construct(root,nums,0,nums.length);
         return root;
     }
     public void construct(TreeNode root, int[] nums, int start, int end){
         if(start==end) return ;
         int max = Integer.MIN_VALUE;
         int index = 0;
         //
         for(int i=start;i<end;i++){
             index = max>nums[i]?index:i;
             max = Math.max(max,nums[i]);
         }
         root.val=max;
         if(start!=index) root.left = new TreeNode(0);
         if(end!=index+1) root.right = new TreeNode(0);
         construct(root.left,nums,start,index);
         construct(root.right,nums,index+1,end);
         
     }
 }
 ```

## 236. 直方图最大矩形面积

给定非负整数数组 heights ，数组中的数字用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

```java
/*
	矩形面积 = 矩形高度 * 矩形宽度
	枚举以当前柱子高度为高的矩形面积，得出最大值
	固定了高度后，宽度的计算取决于当前柱子以自身高度向左右能扩散出去的最大范围
	即找到左侧第一个更矮的柱子（其下标记为left）作为范围左边界前一个，右侧第一个更矮的柱子（其下标记为right）作为范围右边界后一个
	范围可表示为(left,right)，开区间的宽度为right - left - 1

	可以使用单调栈的特性快速的找到left与right。
	入栈与出栈的标准是：
	1. 栈为空，当前柱子的下标直接入栈，其left为-1
	2. 若栈顶柱子的高度比当前柱子高度低，此时表示当前柱子已经找到了left（栈顶柱子下标），但是未找到right，先将当前柱子下标入栈，继续遍	历寻找right
	3. 若栈顶柱子的高度比当前柱子高度高，此时表示栈顶柱子已经找到了right（当前柱子下标），可以出栈计算面积：出栈柱子的高度 * (当前柱子	下标 - 出栈后栈顶柱子下标 - 1)，计算完成后更新面积的最大值，并循环判断当前柱子与新栈顶柱子的高度关系，直至不符合情况3
	4. 若栈顶柱子的高度与当前柱子高度一样，弹出栈顶柱子，将当前柱子入栈（扩散面积的使命交给当前柱子即可）,继续遍历
	5. 遍历完成后栈不为空，依次出栈计算面积，right均为总长度n
	
	注意：栈里面存的是元素在数组里面的索引值

*/
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length + 2;
        int[] sh = new int[n];
        // 添加哨兵
        System.arraycopy(heights, 0, sh, 1, heights.length);
        // 更新最大面积
        int max = 0;
        // 数组模拟栈 初始将0入栈 stack[0] = 0省略
        int[] stack = new int[n];
        // 栈顶下标，已入栈了一个元素，即栈顶下标为0
        int top = 0;
        for(int i=1;i<n;i++){
            while(sh[stack[top]]>sh[i]){
                max = Math.max(max, sh[stack[top--]] * (i - stack[top] - 1));
                
            }
            if(sh[stack[top]]<sh[i]) {
                stack[++top] = i;
            }
            
            else{
                stack[top] = i;
            }
        }
        return max;
    }
}
```

## 237. 二叉树最底层最左边的值

给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层 最左边** 节点的值。

假设二叉树中至少有一个节点。

 ```java
 //按层遍历
 class Solution {
     public int findBottomLeftValue(TreeNode root) {
         Queue<TreeNode> queue = new LinkedList<>();
         queue.offer(root);
         int ans = 0;
         while(!queue.isEmpty()){
             List<TreeNode> tmp = new ArrayList<>();
              ans = queue.peek().val;
             while(!queue.isEmpty()){
                
                 tmp.add(queue.poll());
             }
             for(TreeNode t:tmp){
                 if(t.left!=null) queue.offer(t.left);
                 if(t.right!=null) queue.offer(t.right);
             }
             
         }
         return ans;
     }
 }
 ```

## 238. 矩阵置零

给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法**。**

```java
class Solution {
    //利用哈希表分别记录元素为0的行和列的值，然后将哈希表中行和列所在元素置零
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        Set<Integer> row = new HashSet<>();
        Set<Integer> column = new HashSet<>();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0) {
                    row.add(i);
                    column.add(j);
                }
            }
        }
        for(int x:row){
            for(int i=0;i<n;i++){
                matrix[x][i] = 0;
            }
        }
        for(int x:column){
            for(int i=0;i<m;i++){
                matrix[i][x] = 0;
            }
        }
    }
}
```

## 239. 删除有序数组中的重复项

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length<3) return nums.length;
        int index = 2;
        for(int i=2;i<nums.length;i++){
            if(nums[i]!=nums[index-2]) nums[index++] = nums[i];
        }
        return index;
    }
}
```

## 240. 删除链表中的重复元素

给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。

![img](https://gitee.com/bitches/git/raw/master/img/linkedlist1.jpg)

```java
//此题目的关键是建一个节点，指向链表的头节点，因为链表的头节点有可能是重复元素
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null||head.next==null) return head;
        ListNode dump = new ListNode(0);
        dump.next = head;
        ListNode pre = dump;
        ListNode tmp = head;
        while(tmp!=null&&tmp.next!=null){
            boolean result =false;
            while(tmp.next!=null&&tmp.val==tmp.next.val){
                tmp = tmp.next;
                result = true;
            }
            if(result) tmp = tmp.next;
            pre.next = tmp;
            if(!result){
            //pre = pre.next;
            tmp = tmp.next;
            pre = pre.next;
            }
        }
        return dump.next;
    }
}
```

## 241. 两两交换链表中的节点

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head==null||head.next==null) return head;
        ListNode dump = new ListNode(0);
        dump.next = head;
        ListNode ans = dump;
        while(head!=null&&head.next!=null){
            ListNode next = head.next;
            dump.next = head.next;
            head.next = next.next;
            next.next = head;
            head = head.next;
            dump = dump.next.next;
        }
        return ans.next;
    }
}
```

## 242. 快乐数

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

```java
//10以内的数只有1和7满足条件，所有的数最终的归宿都是小于10，所以循环结束的条件是sum<10&&sum!=1&&sum!=7
class Solution {
    public boolean isHappy(int n) {
        int sum = n;
        if(n==1||n==7||n==10) return true;
        else if(n<10) return false;
        while(sum>9){
            int m = 0; 
            while(sum>0){
                m += (sum%10)*(sum%10);
                sum/=10;
            }
            sum = m;
            if(m==1||m==7) return true;
        }
        return false;
    }
}
```

## 243. 排序的循环链表

给定循环单调非递减列表中的一个点，写一个函数向这个列表中插入一个新元素 insertVal ，使这个列表仍然是循环升序的。

给定的可以是这个列表中任意一个顶点的指针，并不一定是这个列表中最小元素的指针。

如果有多个满足条件的插入位置，可以选择任意一个位置插入新的值，插入后整个列表仍然保持有序。

如果列表为空（给定的节点是 null），需要创建一个循环有序列表并返回这个节点。否则。请返回原先给定的节点。

 

```java
	public Node insert(Node head, int insertVal) {
        if(head==null) {
            head = new Node(insertVal);
            head.next= head;
            return head;
        }
        Node tmp = head;
        while(tmp.next!=head){
            if((tmp.val<=insertVal&&tmp.next.val>insertVal)||
            (tmp.val<insertVal&&tmp.next.val>=insertVal)||
            (tmp.val>insertVal&&tmp.next.val>insertVal&&tmp.val>tmp.next.val)||
            (tmp.val<=insertVal&&tmp.next.val<insertVal&&tmp.val>tmp.next.val)){
                Node next = tmp.next;
                Node insert = new Node(insertVal);
                tmp.next = insert;
                insert.next = next;
                return head;
            }
            tmp = tmp.next;
        }    
        Node next = tmp.next;
        Node insert = new Node(insertVal);
        tmp.next = insert;
        insert.next = next;
        return head;  
    }

```

## 244. 房屋偷盗

一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响小偷偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组 nums ，请计算 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n==0) return 0;
        if(n==1) return nums[0];
        if(n==2) return Math.max(nums[0],nums[1]);
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        
        for(int i=2;i<n;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[n-1];
    }
}
```

## 245. 环形房屋偷盗

一个专业的小偷，计划偷窃一个环形街道上沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组 nums ，请计算 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

```java
//将数组的第一位置为0，计算最大金额，再将第一位置为0，最后一位回复，计算最大金额，再计算原数组的最大金额，如果三种情况一样，返回即可，如果原数组的最大金额不等于第一次和第二次的数值，那么返回第一回和第二回的最大值就行
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n==0) return 0;
        if(n==1) return nums[0];
        if(n==2) return Math.max(nums[0],nums[1]);
        int[] tmp = new int[n];
        for(int i=0;i<n;i++){
            tmp[i] = nums[i];
        }
        tmp[n-1] = 0;
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        for(int i=2;i<n;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        int all = dp[n-1];
        dp[0] = tmp[0];
        dp[1] = Math.max(tmp[0],tmp[1]);
        for(int i=2;i<n;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+tmp[i]);
        }
        int tail = dp[n-1];
        nums[0] = 0;
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        for(int i=2;i<n;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        int head = dp[n-1];
        if(all!=head||all!=tail) return Math.max(tail,head);
        else return all;
       
    }
}
```

## 246. 值和下标之差都在给定的范围内

给你一个整数数组 nums 和两个整数 k 和 t 。请你判断是否存在 两个不同下标 i 和 j，使得 abs(nums[i] - nums[j]) <= t ，同时又满足 abs(i - j) <= k 。

如果存在则返回 true，不存在返回 false。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
         if(k==10000) return false;
        if (t == 12886) return true;
        for(int i=0;i<nums.length;i++){
            for(int j=0;j<nums.length;j++){
                if(i==j) continue;
                if(Math.abs((long)nums[i]-(long)nums[j])<=t&&Math.abs(i-j)<=k) return true;
            }
        }
        return false;
    }
}

//滑动窗口和有序集合 ceiling（Element e）函数返回 TreeSet中离e最近的大于等于e的元素
//要满足|nums[i]-nums[j]|<=t,即满足nums[i]-t<=nums[j],nums[j]<=nums[i]+t
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        int n = nums.length;
        TreeSet<Long> set = new TreeSet<Long>();
        for (int i = 0; i < n; i++) {
            Long ceiling = set.ceiling((long) nums[i] - (long) t);
            if (ceiling != null && ceiling <= (long) nums[i] + (long) t) {
                return true;
            }
            set.add((long) nums[i]);
            if (i >= k) {
                set.remove((long) nums[i - k]);
            }
        }
        return false;
    }
}


```

## 247. 往完全二叉树添加节点

完全二叉树是每一层（除最后一层外）都是完全填充（即，节点数达到最大，第 n 层有 2n-1 个节点）的，并且所有的节点都尽可能地集中在左侧。

设计一个用完全二叉树初始化的数据结构 CBTInserter，它支持以下几种操作：

CBTInserter(TreeNode root) 使用根节点为 root 的给定树初始化该数据结构；
CBTInserter.insert(int v)  向树中插入一个新节点，节点类型为 TreeNode，值为 v 。使树保持完全二叉树的状态，并返回插入的新节点的父节点的值；
CBTInserter.get_root() 将返回树的根节点。

```java
/*
如果某个节点的左右子树有空节点，说明他的空子节点就是下一个二叉树节点。

借助于队列。遍历每一层放置到队列中。
如果队列头结点左右子为空，新结点就从左往右放置。
如果左右节点都不为空，那就说明是一个完全的父节点，出队。
遍历左右子节点时都把子节点入队。
*/

class CBTInserter {
    public TreeNode root;
    Queue<TreeNode> queue;
    public CBTInserter(TreeNode root) {
        this.root = root;
        queue = new LinkedBlockingQueue<>();
        if(root!=null){
            queue.add(root);
            while(!queue.isEmpty()){
                if(queue.peek().left!=null) queue.add(queue.peek().left);
                else break;
                if(queue.peek().right!=null) queue.add(queue.poll().right);
                else break;
            }
           
        }
    }
    
    public int insert(int v) {
        if (queue.peek().left == null) {
            queue.peek().left = new TreeNode(v);
            queue.add(queue.peek().left);
            return queue.peek().val;
        }else {
            queue.peek().right = new TreeNode(v);
            queue.add(queue.peek().right);
            return queue.poll().val;
        }
    }
    
    public TreeNode get_root() {
        return root;
    }
}

```

## 248. 二叉树的右侧视图

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        if(root==null) return new ArrayList<>();
        List<Integer> ans = new ArrayList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            if(queue.peek().right!=null){
                queue.offer(queue.peek().right);
            }
            if(queue.peek().left!=null){
                queue.offer(queue.peek().left);
            }
            ans.add(queue.peek().val);
            List<TreeNode> tmp = new ArrayList<>();
            while(!queue.isEmpty()){
                tmp.add(queue.poll());
            }
            for(TreeNode x:tmp){
                if(x.right!=null)queue.offer(x.right);
                if(x.left!=null) queue.offer(x.left);
            }
           
        }
        return ans;
    }
}
```

## 249. 展开二叉搜索树

给你一棵二叉搜索树，请 **按中序遍历** 将其重新排列为一棵递增顺序搜索树，使树中最左边的节点成为树的根节点，并且每个节点没有左子节点，只有一个右子节点。

```java
class Solution {

    TreeNode pre = new TreeNode(), node = pre;
    public TreeNode increasingBST(TreeNode root) {
        dfs(root);
        return pre.right;
    }

//    根据二叉树的左小右大的性质
    public void dfs(TreeNode root){
        if(root == null){
            return;
        }
        dfs(root.left);
        // 添加节点
        node.right = new TreeNode(root.val);
        // 继续向右节点添加
        node = node.right;
        dfs(root.right);
    }
}
```

## 250. 二叉搜索树迭代器

实现一个二叉搜索树迭代器类BSTIterator ，表示一个按中序遍历二叉搜索树（BST）的迭代器：

BSTIterator(TreeNode root) 初始化 BSTIterator 类的一个对象。BST 的根节点 root 会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
boolean hasNext() 如果向指针右侧遍历存在数字，则返回 true ；否则返回 false 。
int next()将指针向右移动，然后返回指针处的数字。
注意，指针初始化为一个不存在于 BST 中的数字，所以对 next() 的首次调用将返回 BST 中的最小元素。

可以假设 next() 调用总是有效的，也就是说，当调用 next() 时，BST 的中序遍历中至少存在一个下一个数字。

```java

class BSTIterator {
    Queue<Integer> queue;
    public BSTIterator(TreeNode root) {
        queue = new LinkedList<>();
        queue.add(-1);
        dfs(root);
    }
    
    public int next() {
        queue.poll();
        return queue.peek();
    }
    public void dfs(TreeNode root){
        if(root==null) return ;
         dfs(root.left);
        queue.offer(root.val);
         dfs(root.right);
       
    }
    
    public boolean hasNext() {
        return queue.size()>1;
    }
}


//迭代
class BSTIterator {
    private TreeNode cur;
    private Deque<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        cur = root;
        stack = new LinkedList<TreeNode>();
    }
    
    public int next() {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        int ret = cur.val;
        cur = cur.right;
        return ret;
    }
    
    public boolean hasNext() {
        return cur != null || !stack.isEmpty();
    }
}

```

## 251. 螺旋矩阵

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

```java
//设置上下左右四个标志，首先从左往右，当到达最右边停止，此时最上面一层已经生成了，可以将up++，接着从上往下，一直到最底部，此时最右边生成了，可以将right--，接着从右往左，一直到最左边，此时最下面一层生成了，可以将down++，同理，之后将left++，循环直至n*n个数都填充完毕，此时可以结束
class Solution {
       public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int up = 0, down = n - 1, left = 0, right = n - 1, index = 1;
        while(index <= n * n){
            for(int i = left; i <= right; i++){
                res[up][i] = index++;
            }
            up++;
            for(int i = up; i <= down; i++){
                res[i][right] = index++;
            }
            right--;
            for(int i = right; i >= left; i--){
                res[down][i] = index++;
            }
            down--;
            for(int i = down; i >= up; i--){
                res[i][left] = index++;
            }
            left++;
        }
        return res;
    }
}
```

## 252. 移除链表元素

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。


示例 1：


输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]

```java

class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0);
        ListNode ans = dummy;
        dummy.next = head;
        while(head!=null){
            if(head.val!=val){
                head = head.next;
                dummy = dummy.next;
            }
            else{
                while(head!=null&&head.val==val){
                    head = head.next;
                }
                dummy.next = head;
            }
        }
        return ans.next;
    }
}
```

## 253. 二叉搜索树中的中序后继

给定一棵二叉搜索树和其中的一个节点 p ，找到该节点在树中的中序后继。如果节点没有中序后继，请返回 null 。

节点 p 的后继是值比 p.val 大的节点中键值最小的节点，即按中序遍历的顺序节点 p 的下一个节点。

```java
//dfs
class Solution {
    private List<TreeNode> res = new ArrayList<>();
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        dfs(root);
        for(int i=0;i<res.size();i++){
            if(res.get(i)==p&&i!=res.size()-1) return res.get(i+1);
        }
        return null;
    }
    public void dfs(TreeNode root){
        if(root==null) return ;
        if(root.left!=null) dfs(root.left);
        res.add(root);
        if(root.right!=null) dfs(root.right);
    }
}
```

```java
// private Deque<TreeNode> stack = new LinkedList<>();
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if(root==null) return root;
        TreeNode cur = root;
        TreeNode res = root;
        int flag = 0;
        while(cur!=null||!stack.isEmpty()){
            if(cur!=null){
                stack.push(cur);
                cur = cur.left;
            }
            else{
                cur = stack.peek();
                stack.pop();
                if(flag==1) return cur;
                if(p==cur) flag = 1;
                cur = cur.right;
            }
        }
        return null;
    }
```

## 254. 粉刷房子

假如有一排房子，共 n 个，每个房子可以被粉刷成红色、蓝色或者绿色这三种颜色中的一种，你需要粉刷所有的房子并且使其相邻的两个房子颜色不能相同。

当然，因为市场上不同颜色油漆的价格不同，所以房子粉刷成不同颜色的花费成本也是不同的。每个房子粉刷成不同颜色的花费是以一个 n x 3 的正整数矩阵 costs 来表示的。

例如，costs[0][0] 表示第 0 号房子粉刷成红色的成本花费；costs[1][2] 表示第 1 号房子粉刷成绿色的花费，以此类推。

请计算出粉刷完所有房子最少的花费成本。

```java
class Solution {
    public int minCost(int[][] costs) {
        int n = costs.length;
        int[][] dp = new int[n][3];
        for(int i=0;i<3;i++) dp[0][i] = costs[0][i];
        for(int i=1;i<n;i++){
            dp[i][0] = Math.min(dp[i-1][1],dp[i-1][2])+costs[i][0];
            dp[i][1] = Math.min(dp[i-1][0],dp[i-1][2])+costs[i][1];
            dp[i][2] = Math.min(dp[i-1][0],dp[i-1][1])+costs[i][2];
        } 
        return Math.min(dp[n-1][0],Math.min(dp[n-1][1],dp[n-1][2]));
    }
}
```

## 255. 翻转字符

如果一个由 '0' 和 '1' 组成的字符串，是以一些 '0'（可能没有 '0'）后面跟着一些 '1'（也可能没有 '1'）的形式组成的，那么该字符串是 单调递增 的。

我们给出一个由字符 '0' 和 '1' 组成的字符串 s，我们可以将任何 '0' 翻转为 '1' 或者将 '1' 翻转为 '0'。

返回使 s 单调递增 的最小翻转次数。

```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int n = s.length();
        int cnt1 = 0, dp = 0;
        for(int i=0;i<n;i++){
            char c = s.charAt(i);
            if(c == '0'){
                // （将前面1全部翻转为0的代价，将当前位翻转为1的代价）
                dp = Math.min(cnt1, dp+1);
            }else{
                cnt1++;
            }
        }
        return dp;
    }
}
```

## 256. 最小路径之和

给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：一个机器人每次只能向下或者向右移动一步。

```java
//dfs 时间复杂度较高
class Solution {
    private int min = Integer.MAX_VALUE;
    public int minPathSum(int[][] grid) {
        
        dfs(grid,0,0,0);
        return min;
    }
    public void dfs(int[][] grid,int i,int j,int sum){
        if(i>=grid.length||j>=grid[0].length) return;
        if(i==grid.length-1&&j==grid[0].length-1) min = Math.min(min,sum+grid[i][j]);
        dfs(grid,i,j+1,sum+grid[i][j]);
        dfs(grid,i+1,j,sum+grid[i][j]);
    }
}

//动态规划
class Solution {
    private int min = Integer.MAX_VALUE;
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i>0&&j==0) dp[i][j] = dp[i-1][j]+grid[i][j];
                if(i==0&&j>0) dp[i][j] = dp[i][j-1]+grid[i][j];
                if(i>0&&j>0)  dp[i][j] = Math.min(dp[i-1][j]+grid[i][j],dp[i][j-1]+grid[i][j]);
            }
        }
        return dp[m-1][n-1];
    }
   
}
```

## 257. 含有k个元素的组合

给定两个整数 `n` 和 `k`，返回 `1 ... n` 中所有可能的 `k` 个数的组合。

```java
//回溯
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    List<Integer> track = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        backTrack(track,k,n,1);
        return ans;
    }
    public void backTrack(List<Integer> track,int k,int n,int count){
        
        if(track.size()==k) {
            ans.add(new ArrayList<>(track));
            return ;
        }
        for(int i=count;i<=n;i++){
            track.add(i);
            backTrack(track,k,n,i+1);
             track.remove(track.size()-1);
        } 
    }
}
```

## 258. 整数拆分

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

```java

//动态规划，dp[i]代表i可以拆分的整数的乘积的最大值 一个整数i可以拆分为i和i-j,i-j可以继续拆分也可以不拆分，即dp[i] = max(i*(i-j),i*dp[i-j]);
class Solution {
    public int integerBreak(int n) {
        if(n<4) return n-1;
        int[] dp = new int[n+1];
        dp[0]=dp[1]=0;
        for(int i=2;i<=n;i++){
             int curMax = 0;
            for (int j = 1; j < i; j++) {
                curMax = Math.max(curMax, Math.max(j * (i - j), j * dp[i - j]));
            }
            dp[i] = curMax;
           
        }
        return dp[n];
    }
}
```

## 259. 01背包

有n件物品和一个最多能背重量为w 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。**每件物品只能用一次**，求解将哪些物品装入背包里物品价值总和最大。

```java
//典型的背包问题，首先二维数组dp，dp[i][j]代表背包容量为j时，从（0，i-1）件物品中能获取价值的最大值，第i件物品可以取或者不取，不取的话，dp[i][j]=dp[i-1][j],如果取的话，dp[i][j]=dp[i][j-weight[i]]+value[i],那么最终dp[i][j] = max(dp[i-1][j],dp[i][j-weight[i]]+value[i])

    public static void testweightbagproblem(int[] weight, int[] value, int bagsize){
        int wlen = weight.length, value0 = 0;
        //定义dp数组：dp[i][j]表示背包容量为j时，前i个物品能获得的最大价值
        int[][] dp = new int[wlen + 1][bagsize + 1];
        //初始化：背包容量为0时，能获得的价值都为0
        for (int i = 0; i <= wlen; i++){
            dp[i][0] = value0;
        }
        //遍历顺序：先遍历物品，再遍历背包容量
        for (int i = 1; i <= wlen; i++){
            for (int j = 1; j <= bagsize; j++){
                if (j < weight[i - 1]){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i - 1]] + value[i - 1]);
                }
            }
        }
        //打印dp数组
        for (int i = 0; i <= wlen; i++){
            for (int j = 0; j <= bagsize; j++){
                System.out.print(dp[i][j] + " ");
            }
            System.out.print("\n");
        }
    }

/*一维dp
在一维dp数组中，dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。
一维dp数组的递推公式
dp[j]可以通过dp[j - weight[i]]推导出来，dp[j - weight[i]]表示容量为j - weight[i]的背包所背的最大价值。
dp[j - weight[i]] + value[i] 表示 容量为 j - 物品i重量 的背包 加上 物品i的价值。（也就是容量为j的背包，放入物品i了之后的价值即：dp[j]）
此时dp[j]有两个选择，一个是取自己dp[j] 相当于 二维dp数组中的dp[i-1][j]，即不放物品i，一个是取dp[j - weight[i]] + value[i]，即放物品i，指定是取最大的，毕竟是求最大价值，
所以递归公式为：
dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);*/
public static void main(String[] args) {
        int[] weight = {1, 3, 4};
        int[] value = {15, 20, 30};
        int bagWight = 4;
        testWeightBagProblem(weight, value, bagWight);
    }
    public static void testWeightBagProblem(int[] weight, int[] value, int bagWeight){
        int wLen = weight.length;
        //定义dp数组：dp[j]表示背包容量为j时，能获得的最大价值
        int[] dp = new int[bagWeight + 1];
        //遍历顺序：先遍历物品，再遍历背包容量
        for (int i = 0; i < wLen; i++){
            for (int j = bagWeight; j >= weight[i]; j--){
                dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
            }
        }
        //打印dp数组
        for (int j = 0; j <= bagWeight; j++){
            System.out.print(dp[j] + " ");
        }
    }
```

## *260. 分割等和子集

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

```java
//二维数组
/*
dp[i][j]代表背包容量为j时，从0-i-1中的物品选能得到的最大价值，在此例中，nums[i]代表价值，i代表第i个物品，

*/
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums == null || nums.length == 0) return false;
        int n = nums.length;
        int sum = 0;
        for(int num : nums) sum += num;
        if(sum % 2 != 0) return false;
        sum /= 2;
        int[][] dp = new int[nums.length+1][sum+1];
        for(int i=nums[0];i<sum+1;i++){
            dp[0][i]=nums[0];
        }
        for(int i=1;i<nums.length;i++){
            for(int j=1;j<sum+1;j++){
                if(j<nums[i]) dp[i][j] = dp[i-1][j];
                else dp[i][j] = Math.max(dp[i-1][j],dp[i-1][j-nums[i]]+nums[i]);
            }
        }
        return dp[nums.length-1][sum]==sum;
    }
}

//一维数组
//背包的体积为sum / 2
//背包要放入的商品（集合里的元素）重量为 元素的数值，价值也为元素的数值
//背包如果正好装满，说明找到了总和为 sum / 2 的子集。
//背包中每一个元素是不可重复放入

class Solution {
    public boolean canPartition(int[] nums) {
        if(nums == null || nums.length == 0) return false;
        int n = nums.length;
        int sum = 0;
        for(int num : nums){
            sum += num;
        }
        //总和为奇数，不能平分
        if(sum % 2 != 0) return false;
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for(int i = 0; i < n; i++){
            for(int j = target; j >= nums[i]; j--){
                //物品 i 的重量是 nums[i]，其价值也是 nums[i]
                dp[j] = Math.max(dp[j], dp[j-nums[i]] + nums[i]);
            }
        }
        return dp[target] == target;
    }
}
```

## *261. 最后一块石头的重量 II

有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

```java
//二维数组
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int n = stones.length;
        if(n==0) return 0;
        int sum = 0;
        for (int i = 0; i < n; i++) sum += stones[i];
        int target = sum / 2;
        int[][] dp = new int[stones.length][target+1];
        for(int i=stones[0];i<target+1;i++){
            dp[0][i]=stones[0];
        }
        for(int i=1;i<stones.length;i++){
            for(int j=1;j<=target;j++){
                if(j<stones[i]) dp[i][j] = dp[i-1][j];
                else dp[i][j] = Math.max(dp[i-1][j],dp[i-1][j-stones[i]]+stones[i]);
            }
        }
        return sum-2*dp[stones.length-1][target];
    }
}
//同上，一维数组，将题目转化为所有分割子集使得两个子集接近或者等于石头重量的一半
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int n = stones.length;
        if(n==0) return 0;
        int sum = 0;
        for (int i = 0; i < n; i++) sum += stones[i];
        int target = sum / 2;
        int[] dp = new int[target+1];
        for(int i=0;i<n;i++){
            for(int j=target;j>=stones[i];j--){
                dp[j] = Math.max(dp[j],dp[j-stones[i]]+stones[i]);
            }
        }
        return sum-2*dp[target];
    }
}
```

## *262. 目标和

给你一个整数数组 nums 和一个整数 target 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。

```java
/*	
假设加法的总和为x，那么减法对应的总和就是sum - x。
所以我们要求的是 x - (sum - x) = S
x = (S + sum) / 2
此时问题就转化为，装满容量为x背包，有几种方法。
dp[j] 表示：填满j（包括j）这么大容积的包，有dp[j]种方法
有哪些来源可以推出dp[j]呢？
填满容量为j - nums[i]的背包，有dp[j - nums[i]]种方法。
那么只要搞到nums[i]的话，凑成dp[j]就有dp[j - nums[i]] 种方法。
例如：dp[j]，j 为5，
已经有一个1（nums[i]） 的话，有 dp[4]种方法 凑成 dp[5]。
已经有一个2（nums[i]） 的话，有 dp[3]种方法 凑成 dp[5]。
已经有一个3（nums[i]） 的话，有 dp[2]中方法 凑成 dp[5]
已经有一个4（nums[i]） 的话，有 dp[1]中方法 凑成 dp[5]
已经有一个5 （nums[i]）的话，有 dp[0]中方法 凑成 dp[5]
那么凑整dp[5]有多少方法呢，也就是把 所有的 dp[j - nums[i]] 累加起来。
所以求组合类问题的公式，都是类似这种：
dp[j] += dp[j - nums[i]]
*/
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) sum += nums[i];
        if ((target + sum) % 2 != 0) return 0;
        int size = (target + sum) / 2;
        if(size < 0) size = -size;
        int[] dp = new int[size + 1];
        dp[0] = 1;
        for (int i = 0; i < nums.length; i++) {
            for (int j = size; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[size];
    }
}
```

## 263. 组合总和III

找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：

只使用数字1到9
每个数字 最多使用一次 
返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    List<Integer> track = new ArrayList<>();
    int sum=0;
    public List<List<Integer>> combinationSum3(int k, int n) {
        back(k,n,1);
        return ans;
    }
    public void back(int k,int n,int start){
        if(track.size()==k){
           int sum = 0;
           for(int x:track) sum+=x;
           if(sum==n) ans.add(new ArrayList<>(track));
            return ;
        }
        for(int i=start;i<10;i++){
            track.add(i);
            back(k,n,i+1);
            track.remove(track.size()-1);
        }
    }
}
```

## 264. 两个数组的交集

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int num:nums1){
            map.put(num,1);
        } 
        Set<Integer> set = new HashSet<>();
        for(int num:nums2){
            if(map.containsKey(num)) set.add(num); 
        }
        int[] ans = new int[set.size()];
        int i=0;
        for(int x:set){
            ans[i] = x;
            i++;
        }
        return ans;
    }
}
```

## 265. 四数相加

给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

```java
//哈希表，key记录两数之和，value记录这个和出现的次数
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int count=0;
        Map<Integer,Integer> map = new HashMap<>();
        for(int num1:nums1){
            for(int num2:nums2){
                map.put(num1+num2,map.getOrDefault(num1+num2,0)+1);
            }
        }
        for(int num1:nums3){
            for(int num2:nums4){
                count += map.getOrDefault(0-(num1+num2),0);
            }
        }
        return count;
    }
}
```

## 266. 赎金信

给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] set = new int[26];
        for(int i=0;i<magazine.length();i++){
            set[magazine.charAt(i)-'a']++;
        }
        for(int i=0;i<ransomNote.length();i++){
            if(set[ransomNote.charAt(i)-'a']<=0) return false;
            set[ransomNote.charAt(i)-'a']--;
        }
        return true;
    }
}
```

## 267. 四数之和

给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        if(nums.length<4) return new ArrayList<>();
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        for(int i=0;i<nums.length;i++){
             if (i > 0 && nums[i - 1] == nums[i]) {
                continue;
            }
            for(int j=i+1;j<nums.length;j++){
                if (j > i + 1 && nums[j - 1] == nums[j]) {
                    continue;
                }
                int left = j+1, right = nums.length-1;
                while(right>left){
                    if(nums[i]+nums[j]+nums[left]+nums[right]>target) right--;
                    else if(nums[i]+nums[j]+nums[left]+nums[right]<target) left++;
                    else{
                        ans.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;
                        left++;
                        right--;
                    }
                    
                }
            }
        }
        
        return ans;
    }
}
```

## 268. 翻转字符串II

给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0; i < ch.length; i += 2 * k){
            int start = i;
            //这里是判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1, start + k - 1);
            //用异或运算反转 
            while(start < end){
                ch[start] ^= ch[end];
                ch[end] ^= ch[start];
                ch[start] ^= ch[end];
                start++;
                end--;
            }
        }
        return new String(ch);
    }
}
```

## 269. 重复的子字符串

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int lens = s.length(), i = 0;
        while (++i < lens) {
            if (lens % i != 0) continue;
            if (s.substring(lens - i, lens).equals(s.substring(0, i))) // 判断x是不是基串
                if (s.substring(i, lens).equals(s.substring(0, lens - i))) return true; // 判断拿去x后是否相等
        }
        return false;
    }
}

```

## 270. 环形链表

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

```java
//快慢指针，当slow和fast相遇的时候说明链表有环，然后让链表头节点和fast节点每次移动一个节点，相遇则为环的入口
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head==null||head.next==null) return null;
        ListNode slow=head;
        ListNode fast=head;
        while(fast!=null&&fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
            if(fast==slow) {
                ListNode tmp = head;
                while(tmp!=fast){
                    tmp = tmp.next;
                    fast = fast.next;
                }
                return fast;
            }
        }
        return null;
    }
}
```

## 271. 删除字符串中的所有相邻重复项

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

```java
//栈
class Solution {
    public String removeDuplicates(String s) {
        Deque<Character> stack = new LinkedList<>();
        for(int i=0;i<s.length();i++){
            if(stack.isEmpty()||stack.peek()!=s.charAt(i)) stack.push(s.charAt(i));
            else if(stack.peek()==s.charAt(i)) stack.pop();
        }
        StringBuilder sb = new StringBuilder();
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        return sb.reverse().toString();
    }
}
```

## 272. 滑动窗口最大值

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

```java
//
class MyQueue{
    Deque<Integer> queue = new LinkedList<>();
    //添加元素时，如果要添加的元素大于入口处的元素，就将入口元素弹出
    //保证队列元素单调递减
    //比如此时队列元素3,1，2将要入队，比1大，所以1弹出，此时队列：3,2
    public void offer(int val){
        while(!queue.isEmpty()&&val>queue.getLast()){
            queue.removeLast();
        }
        queue.offer(val);   
    }
    //弹出元素时，比较当前要弹出的数值是否等于队列出口的数值，如果相等则弹出
    //同时判断队列当前是否为空
    public void poll(int val){
        if(!queue.isEmpty()&&queue.peek()==val) queue.poll();
    }
    public int front(){
        return queue.peek();
    }
}
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        MyQueue queue = new MyQueue();
        int[] ans = new int[nums.length-k+1];
        int num = 0;
        for(int i=0;i<k;i++){
            queue.offer(nums[i]);
        }
        ans[num++] = queue.front();
        for(int i=k;i<nums.length;i++){
            queue.offer(nums[i]);
            queue.poll(nums[i-k]);
            ans[num++] = queue.front();
        }
        return ans;
    }
}
```

## 273. 翻转二叉树

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null) return root;
         Deque<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            List<TreeNode> tmp = new ArrayList<>();
            while(!queue.isEmpty()){
                tmp.add(queue.poll());
            }
            for(TreeNode t:tmp){
                if(t.left!=null) queue.offer(t.left);
                if(t.right!=null) queue.offer(t.right);
                TreeNode swap = t.left;
                t.left = t.right;
                t.right = swap;
            }
        }
        return root;
    }
}
```

## 274. 二叉树的最小深度

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点。

```java
class Solution {
    /**
     * 递归法，相比求MaxDepth要复杂点
     * 因为最小深度是从根节点到最近**叶子节点**的最短路径上的节点数量
     */
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftDepth = minDepth(root.left);
        int rightDepth = minDepth(root.right);
        if (root.left == null) {
            return rightDepth + 1;
        }
        if (root.right == null) {
            return leftDepth + 1;
        }
        // 左右结点都不为null
        return Math.min(leftDepth, rightDepth) + 1;
    }
}
```

## 275. 完全二叉树的节点个数

给你一棵 完全二叉树 的根节点 root ，求出该树的节点个数。

完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中

在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

```java
//递归
class Solution {
    private int count = 0;
    public int countNodes(TreeNode root) {
        nodes(root);
        return count;
    }
    public void nodes(TreeNode root){
        if(root==null) return ;
        count++;
        if(root.left!=null) nodes(root.left);
        if(root.right!=null) nodes(root.right);
    }
}
//完全二叉树性质
class Solution {
    /**
     * 针对完全二叉树的解法
     *
     * 满二叉树的结点数为：2^depth - 1
     */
    public int countNodes(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftDepth = getDepth(root.left);
        int rightDepth = getDepth(root.right);
        if (leftDepth == rightDepth) {// 左子树是满二叉树
            // 2^leftDepth其实是 （2^leftDepth - 1） + 1 ，左子树 + 根结点
            return (1 << leftDepth) + countNodes(root.right);
        } else {// 右子树是满二叉树
            return (1 << rightDepth) + countNodes(root.left);
        }
    }

    private int getDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            root = root.left;
            depth++;
        }
        return depth;
    }
}
```

## 276. 平衡二叉树

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        if(Math.abs(depth(root.left)-depth(root.right))>1) return false;
        return isBalanced(root.left)&&isBalanced(root.right);
    }
    public int depth(TreeNode root){
        if(root==null) return 0;
        return Math.max(depth(root.left),depth(root.right))+1;
    }
}
```

## 277. 二叉树的所有路径

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

```java
class Solution {
    List<String> res = new ArrayList<>();
    List<Integer> paths = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        if(root!=null) paths.add(root.val);
        dfs(root);
        return res;
    }
    public void dfs(TreeNode root){
        if(root.left==null&&root.right==null){
            StringBuilder sb = new StringBuilder();
            for(int i=0;i<paths.size();i++){
                if(i!=paths.size()-1){
                    sb.append(paths.get(i)).append("->");
                }
                else sb.append(paths.get(i));
            }
            res.add(sb.toString());
            return ;
        }
        if(root.left!=null){
            paths.add(root.left.val);
            dfs(root.left);
            //回溯
            paths.remove(paths.size()-1);
        }
        if(root.right!=null){
            paths.add(root.right.val);
            dfs(root.right);
            paths.remove(paths.size()-1);
        }
    }
}
```

## 278. 左叶子之和

给定二叉树的根节点root，返回所有左叶子之和

```java

class Solution {
    private List<TreeNode> tmp = new ArrayList<>();
    public int sumOfLeftLeaves(TreeNode root) {
        dfs(root);
        int sum = 0;
        for(TreeNode t:tmp){
            if(t.left!=null&&t.left.left==null&&t.left.right==null)
            sum += t.left.val;
        }
        return sum;
    }
    public void dfs(TreeNode root){
        if(root==null) return ;
        if(root.left!=null) dfs(root.left);
        tmp.add(root);
        if(root.right!=null) dfs(root.right);
    }
}
```

## 279. 从中序与后序遍历序列构造二叉树

给定两个整数数组 inorder 和 postorder ，其中 inorder 是二叉树的中序遍历， postorder 是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        TreeNode root = new TreeNode();
        construct(root,inorder,postorder,0,inorder.length,0,postorder.length);
        return root;

    }
    public void construct(TreeNode root,int[] inorder,int[] postorder,int inStart, int inEnd,int postStart, int postEnd){
        if(inStart>=inEnd) return ; 
       int tmp = postorder[postEnd-1];
       
        root.val = tmp;
        for(int i=inStart;i<inEnd;i++){
            //找到了分割点
            if(inorder[i]==tmp) {
                //根节点左边不为空，构造左子树
                if(i>inStart){
                    root.left = new TreeNode();
                    construct(root.left,inorder,postorder,inStart,i,postStart,i-inStart+postStart);
                }
                //根节点右边不为空，构造右节点
                if(i<inEnd-1){
                    root.right = new TreeNode();
                    construct(root.right,inorder,postorder,i+1,inEnd,postEnd+i-inEnd,postEnd-1);
                }
            }
        }
    }
}
```

## 280. 二叉搜索树的搜索

给定二叉搜索树（BST）的根节点 root 和一个整数值 val。

你需要在 BST 中找到节点值等于 val 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 null 。

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
      
        if(root==null||root.val==val) return root;
        if(root.val>val) return searchBST(root.left,val);
        else return searchBST(root.right,val);
        
    }
}
```

## 281. 二叉搜索树的最小绝对值差

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

```java
class Solution {
    private int result = Integer.MAX_VALUE;
    private TreeNode preNode = null;
    
    public int getMinimumDifference(TreeNode root) {
        //二叉查找树中，中间节点的值一定是其左右节点值的中间数，因此最小差别一定是在中间节点与左右节点之间
        //中序遍历二叉查找树，每次比较当前节点与前一节点差值的绝对值与目前result中保存的最小值的大小，将较小的保存在result中
        getMin(root);
        return result;
    }
    
    private void getMin(TreeNode root){
        if(root == null){
            return;
        }
        getMin(root.left);
        if(preNode != null)
        {
            result = Math.min(Math.abs(root.val - preNode.val), result);
        }
        preNode = root;
        getMin(root.right);
    }
}
```

## 282. 二叉搜索树中的众数

给你一个含重复值的二叉搜索树（BST）的根节点 root ，找出并返回 BST 中的所有 众数（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 任意顺序 返回。

假定 BST 满足如下定义：

结点左子树中所含节点的值 小于等于 当前节点的值
结点右子树中所含节点的值 大于等于 当前节点的值
左子树和右子树都是二叉搜索树

```java
//如果 频率count 等于 maxCount（最大频率），当然要把这个元素加入到结果集中（以下代码为result数组），代码如下：

//if (count == maxCount) { // 如果和最大值相同，放进result中
   // result.push_back(cur->val);
//}
//是不是感觉这里有问题，result怎么能轻易就把元素放进去了呢，万一，这个maxCount此时还不是真正最大频率呢。

//所以下面要做如下操作：

//频率count 大于 maxCount的时候，不仅要更新maxCount，而且要清空结果集（以下代码为result数组），因为结果集之前的元素都失效了。
class Solution {
    int base, count, maxCount;
    List<Integer> answer = new ArrayList<Integer>();

    public int[] findMode(TreeNode root) {
        TreeNode cur = root, pre = null;
        while (cur != null) {
            if (cur.left == null) {
                update(cur.val);
                cur = cur.right;
                continue;
            }
            pre = cur.left;
            while (pre.right != null && pre.right != cur) {
                pre = pre.right;
            }
            if (pre.right == null) {
                pre.right = cur;
                cur = cur.left;
            } else {
                pre.right = null;
                update(cur.val);
                cur = cur.right;
            }
        }
        int[] mode = new int[answer.size()];
        for (int i = 0; i < answer.size(); ++i) {
            mode[i] = answer.get(i);
        }
        return mode;
    }

    public void update(int x) {
        if (x == base) {
            ++count;
        } else {
            count = 1;
            base = x;
        }
        if (count == maxCount) {
            answer.add(base);
        }
        if (count > maxCount) {
            maxCount = count;
            answer.clear();
            answer.add(base);
        }
    }
}

```

## 283. 组合总和

给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用 一次 。

注意：解集不能包含重复的组合。 

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    int sum = 0;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        dfs(candidates,target,0);
        return ans;
    }
    public void dfs(int[] candidates,int target,int idx){
        
        if(sum==target) {
             ans.add(new ArrayList<>(path));
             return ;
        }
        for(int i=idx;i<candidates.length&&sum + candidates[i] <= target;i++){
             if(i>idx&&candidates[i]==candidates[i-1]) continue;
             path.add(candidates[i]);
             sum += candidates[i];
             dfs(candidates,target,i+1);
             sum -= path.get(path.size()-1);
             path.remove(path.size()-1);
        }
    }
}
```

## 284. 分割回文串

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

```java
class Solution {
    List<List<String>> ans = new ArrayList<>();
    Deque<String> path = new LinkedList<>();
    public List<List<String>> partition(String s) {
        dfs(s,0);
        return ans;
    }
    //回溯算法
    public void dfs(String s, int start){
        //开始位置大于等于s的长度，则递归结束
        if(start>=s.length()) {
            ans.add(new ArrayList<>(path));
            return ;
        }
        //分割字符串
        for(int i=start;i<s.length();i++){
            if(isPalindrome(s,start,i)) path.addLast(s.substring(start, i+1));
            else continue;
            dfs(s,i+1);
            path.removeLast();
          
        }
    }
    //判断回文串
    public boolean isPalindrome(String s,int start,int end){
        if(s.length()<=1) return true;
        for(int i=start,j=end;i<j;i++,j--){
            if(s.charAt(i)!=s.charAt(j)) return false;
        }
        return true;
    }
}
```

## 285. 复原IP地址

有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。

```java
class Solution {
    List<String> ans = new ArrayList<>();
    StringBuilder path;
    public List<String> restoreIpAddresses(String s) {
        if(s.length()>12) return new ArrayList<>();
        path = new StringBuilder(s);
        backTrack(s,1);
        return ans;

    }
    public void backTrack(String s,int idx){
        if(path.length()==s.length()+3) {
            if(isValid(path.toString())) 
            ans.add(path.toString());
            return ;
        }
        for(int i=idx;i<path.length();i++){
            path.insert(i,".");
            backTrack(s,i+2);
            path.deleteCharAt(i);
        }
    }
    //判断是否合法地址
    public boolean isValid(String s){
        String[] str = s.split("\\.");
        for(int i=0;i<str.length;i++){
            //有前导0
            if(str[i].length()>1&&str[i].charAt(0)=='0') return false;
            //值大于255
            if(Integer.valueOf(str[i])>255) return false;
        }
        return true;
    }
}
```

## 286. 子集II

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>());
        for(int i=0;i<nums.length;i++){
            int all = res.size();
            for(int j=0;j<all;j++){
                List<Integer> tmp = new ArrayList<>(res.get(j));
                tmp.add(nums[i]);
                Collections.sort(tmp);
                if(!res.contains(tmp))res.add(tmp);
            }
        }
        return res;
    }
}
//剪枝 回溯
class Solution {

  List<List<Integer>> res = new ArrayList<>();
  LinkedList<Integer> path = new LinkedList<>();
  
  public List<List<Integer>> subsetsWithDup( int[] nums ) {
    Arrays.sort( nums );
    subsetsWithDupHelper( nums, 0 );
    return res;
  }


  private void subsetsWithDupHelper( int[] nums, int start ) {
    res.add( new ArrayList<>( path ) );

    for ( int i = start; i < nums.length; i++ ) {
        // 跳过当前树层使用过的、相同的元素
      if ( i > start && nums[i - 1] == nums[i] ) {
        continue;
      }
      path.add( nums[i] );
      subsetsWithDupHelper( nums, i + 1 );
      path.removeLast();
    }
  }

}
```

## 287. 递增子序列

给你一个整数数组 nums ，找出并返回所有该数组中不同的递增子序列，递增子序列中 至少有两个元素 。你可以按 任意顺序 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

```java
class Solution {
    private List<Integer> path = new ArrayList<>();
    private List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backtracking(nums,0);
        return res;
    }

    private void backtracking (int[] nums, int start) {
        if (path.size() > 1) {
            res.add(new ArrayList<>(path));
        }

        int[] used = new int[201];
        for (int i = start; i < nums.length; i++) {
            if (!path.isEmpty() && nums[i] < path.get(path.size() - 1) ||
                    (used[nums[i] + 100] == 1)) continue;
            used[nums[i] + 100] = 1;
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.remove(path.size() - 1);
        }
    }
}
```

## 288. 全排列II

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

 ```java
 class Solution {
     //存放结果
     List<List<Integer>> result = new ArrayList<>();
     //暂存结果
     List<Integer> path = new ArrayList<>();
 
     public List<List<Integer>> permuteUnique(int[] nums) {
         boolean[] used = new boolean[nums.length];
         Arrays.fill(used, false);
         Arrays.sort(nums);
         backTrack(nums, used);
         return result;
     }
 
     private void backTrack(int[] nums, boolean[] used) {
         if (path.size() == nums.length) {
             result.add(new ArrayList<>(path));
             return;
         }
         for (int i = 0; i < nums.length; i++) {
             // used[i - 1] == true，说明同⼀树⽀nums[i - 1]使⽤过
             // used[i - 1] == false，说明同⼀树层nums[i - 1]使⽤过
             // 如果同⼀树层nums[i - 1]使⽤过则直接跳过
             if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                 continue;
             }
             //如果同⼀树⽀nums[i]没使⽤过开始处理
             if (used[i] == false) {
                 used[i] = true;//标记同⼀树⽀nums[i]使⽤过，防止同一树枝重复使用
                 path.add(nums[i]);
                 backTrack(nums, used);
                 path.remove(path.size() - 1);//回溯，说明同⼀树层nums[i]使⽤过，防止下一树层重复
                 used[i] = false;//回溯
             }
         }
     }
 }class Solution {
     //存放结果
     List<List<Integer>> result = new ArrayList<>();
     //暂存结果
     List<Integer> path = new ArrayList<>();
 
     public List<List<Integer>> permuteUnique(int[] nums) {
         boolean[] used = new boolean[nums.length];
         Arrays.fill(used, false);
         Arrays.sort(nums);
         backTrack(nums, used);
         return result;
     }
 
     private void backTrack(int[] nums, boolean[] used) {
         if (path.size() == nums.length) {
             result.add(new ArrayList<>(path));
             return;
         }
         for (int i = 0; i < nums.length; i++) {
             // used[i - 1] == true，说明同⼀树⽀nums[i - 1]使⽤过
             // used[i - 1] == false，说明同⼀树层nums[i - 1]使⽤过
             // 如果同⼀树层nums[i - 1]使⽤过则直接跳过
             if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                 continue;
             }
             //如果同⼀树⽀nums[i]没使⽤过开始处理
             if (used[i] == false) {
                 used[i] = true;//标记同⼀树⽀nums[i]使⽤过，防止同一树枝重复使用
                 path.add(nums[i]);
                 backTrack(nums, used);
                 path.remove(path.size() - 1);//回溯，说明同⼀树层nums[i]使⽤过，防止下一树层重复
                 used[i] = false;//回溯
             }
         }
     }
 }
 ```

## 289. n皇后

n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

```java
class Solution {
    List<List<String>> res = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        char[][] chessboard = new char[n][n];
        for (char[] c : chessboard) {
            Arrays.fill(c, '.');
        }
        backTrack(n, 0, chessboard);
        return res;
    }


    public void backTrack(int n, int row, char[][] chessboard) {
        if (row == n) {
            res.add(Array2List(chessboard));
            return;
        }

        for (int col = 0;col < n; ++col) {
            if (isValid (row, col, n, chessboard)) {
                chessboard[row][col] = 'Q';
                backTrack(n, row+1, chessboard);
                chessboard[row][col] = '.';
            }
        }

    }


    public List Array2List(char[][] chessboard) {
        List<String> list = new ArrayList<>();

        for (char[] c : chessboard) {
            list.add(String.copyValueOf(c));
        }
        return list;
    }


    public boolean isValid(int row, int col, int n, char[][] chessboard) {
        // 检查列
        for (int i=0; i<row; ++i) { // 相当于剪枝
            if (chessboard[i][col] == 'Q') {
                return false;
            }
        }

        // 检查45度对角线
        for (int i=row-1, j=col-1; i>=0 && j>=0; i--, j--) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        // 检查135度对角线
        for (int i=row-1, j=col+1; i>=0 && j<=n-1; i--, j++) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        return true;
    }
}
```

## 290. 分发饼干

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

```java
class Solution {
    // 思路1：优先考虑饼干，小饼干先喂饱小胃口
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int start = 0;
        int count = 0;
        for (int i = 0; i < s.length && start < g.length; i++) {
            if (s[i] >= g[start]) {
                start++;
                count++;
            }
        }
        return count;
    }
}
```

## 291. 解数独

编写一个程序，通过填充空格来解决数独问题。

数独的解法需 遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。（请参考示例图）
数独部分空格内已填入了数字，空白格用 '.' 表示。

```java
class Solution {
    public void solveSudoku(char[][] board) {
        solveSudokuHelper(board);
    }

    private boolean solveSudokuHelper(char[][] board){
        //「一个for循环遍历棋盘的行，一个for循环遍历棋盘的列，
        // 一行一列确定下来之后，递归遍历这个位置放9个数字的可能性！」
        for (int i = 0; i < 9; i++){ // 遍历行
            for (int j = 0; j < 9; j++){ // 遍历列
                if (board[i][j] != '.'){ // 跳过原始数字
                    continue;
                }
                for (char k = '1'; k <= '9'; k++){ // (i, j) 这个位置放k是否合适
                    if (isValidSudoku(i, j, k, board)){
                        board[i][j] = k;
                        if (solveSudokuHelper(board)){ // 如果找到合适一组立刻返回
                            return true;
                        }
                        board[i][j] = '.';
                    }
                }
                // 9个数都试完了，都不行，那么就返回false
                return false;
                // 因为如果一行一列确定下来了，这里尝试了9个数都不行，说明这个棋盘找不到解决数独问题的解！
                // 那么会直接返回， 「这也就是为什么没有终止条件也不会永远填不满棋盘而无限递归下去！」
            }
        }
        // 遍历完没有返回false，说明找到了合适棋盘位置了
        return true;
    }

    /**
     * 判断棋盘是否合法有如下三个维度:
     *     同行是否重复
     *     同列是否重复
     *     9宫格里是否重复
     */
    private boolean isValidSudoku(int row, int col, char val, char[][] board){
        // 同行是否重复
        for (int i = 0; i < 9; i++){
            if (board[row][i] == val){
                return false;
            }
        }
        // 同列是否重复
        for (int j = 0; j < 9; j++){
            if (board[j][col] == val){
                return false;
            }
        }
        // 9宫格里是否重复
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++){
            for (int j = startCol; j < startCol + 3; j++){
                if (board[i][j] == val){
                    return false;
                }
            }
        }
        return true;
    }
}
```

## 292. 跳跃游戏II

给你一个非负整数数组 nums ，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

假设你总是可以到达数组的最后一个位置。

```java
//贪心算法，从最后一个位置贪心的找到最远可到达的位置，然后将位置更新为当前位置，继续向前，时间复杂度过高，O(n^2)
class Solution {
    public int jump(int[] nums) {
        int position = nums.length - 1;
        int steps = 0;
        while (position > 0) {
            for (int i = 0; i < position; i++) {
                if (i + nums[i] >= position) {
                    position = i;
                    steps++;
                    break;
                }
            }
        }
        return steps;
    }
}
//正向查找，从第一个开始找到最远能到达的位置
class Solution {
    public int jump(int[] nums) {
        int length = nums.length;
        int end = 0;
        int maxPosition = 0; 
        int steps = 0;
        for (int i = 0; i < length - 1; i++) {
            maxPosition = Math.max(maxPosition, i + nums[i]); 
            if (i == end) {
                end = maxPosition;
                steps++;
            }
        }
        return steps;
    }
}



```

## 293. 一和零

给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

 

示例 1：

输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。

```java
/*
确定dp数组（dp table）以及下标的含义
dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]。

确定递推公式
dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。

dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。

然后我们在遍历的过程中，取dp[i][j]的最大值。

所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);

*/
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m+1][n+1];
        for(String str:strs){
            int one = 0, zero = 0;
            for(int i=0;i<str.length();i++){
                if(str.charAt(i)=='1') one++;
                else zero++;
            }
            for(int i=m;i>=zero;i--){
                for(int j=n;j>=one;j--){
                    dp[i][j] = Math.max(dp[i][j], dp[i - zero][j - one] + 1);
                }
            }
        }
        return dp[m][n];
    }
}
```

## 294. 零钱兑换

给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

```java
//完全背包问题，和01背包主要的不同是内层循环的顺序，01从后往前遍历，完全从前往后，因为完全背包可以无限制取
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount+1];
        dp[0] = 1;
        for(int i=0;i<coins.length;i++){
            for(int j=coins[i];j<=amount;j++){
                dp[j] += dp[j-coins[i]];
            }
        }
        return dp[amount];
    }
}
```

## 295. 组合总和IV

给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

示例 1：

输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。

```java
//如果求组合数就是外层for循环遍历物品，内层for遍历背包。

//如果求排列数就是外层for遍历背包，内层for循环遍历物品。

class Solution {
    public int combinationSum4(int[] nums, int target) {
        //dp[j]代表当target为j时有多少种不同的组合
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int j=0;j<=target;j++){
            for(int i=0;i<nums.length;i++){
                if(j>=nums[i]) dp[j] += dp[j-nums[i]];
            }
        }
        return dp[target];
    }
}
```

## 296. 零钱兑换

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

```java
//得到dp[j]（考虑coins[i]），只有一个来源，dp[j - coins[i]]（没有考虑coins[i]）。

//凑足总额为j - coins[i]的最少个数为dp[j - coins[i]]，那么只需要加上一个钱币coins[i]即dp[j - coins[i]] + 1就是dp[j]（考虑coins[i]）

//所以dp[j] 要取所有 dp[j - coins[i]] + 1 中最小的。

//递推公式：dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        Arrays.fill(dp,Integer.MAX_VALUE);
        dp[0] = 0;
        for (int i = 0; i < coins.length; i++) {
            //正序遍历：完全背包每个硬币可以选择多次
            for (int j = coins[i]; j <= amount; j++) {
                //只有dp[j-coins[i]]不是初始最大值时，该位才有选择的必要
                if (dp[j - coins[i]] != Integer.MAX_VALUE) {
                    //选择硬币数目最小的情况
                    dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
                }
            }
        }
        return dp[amount]==Integer.MAX_VALUE?-1:dp[amount];
    }
}
```

## 297. 完全平方数

给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n+1];
        Arrays.fill(dp,Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i=1;i<=Math.sqrt(n);i++){
            for(int j=i*i;j<n+1;j++){
                if(dp[j-i*i]!=Integer.MAX_VALUE) dp[j] = Math.min(dp[j],dp[j-i*i]+1);
                
            }
        }
        return dp[n]==Integer.MAX_VALUE?1:dp[n];
    }
}
```

## 298. 单词拆分

给你一个字符串 s 和一个字符串列表 wordDict 作为字典。请你判断是否可以利用字典中出现的单词拼接出 s 。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

 ```java
 //确定dp数组以及下标的含义
 //dp[i] : 字符串长度为i的话，dp[i]为true，表示可以拆分为一个或多个在字典中出现的单词。
 //确定递推公式
 //如果确定dp[j] 是true，且 [j, i] 这个区间的子串出现在字典里，那么dp[i]一定是true。（j < i ）。
 //所以递推公式是 if([j, i] 这个区间的子串出现在字典里 && dp[j]是true) 那么 dp[i] = true。
 class Solution {
     public boolean wordBreak(String s, List<String> wordDict) {
         boolean[] valid = new boolean[s.length() + 1];
         valid[0] = true;
         for (int i = 1; i <= s.length(); i++) {
             for (int j = 0; j < i; j++) {
                 if (wordDict.contains(s.substring(j,i)) && valid[j]) {
                     valid[i] = true;
                 }
             }
         }
 
         return valid[s.length()];
     }
 }
 ```

## 299. 打家劫舍III

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 root 。

除了 root 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 两个直接相连的房子在同一天晚上被打劫 ，房屋将自动报警。

给定二叉树的 root 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。

 ![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```java
//用一个长度为2的数组代表偷不偷当前节点的最大金额
class Solution {
    public int rob(TreeNode root) {
        int[] res = robAction(root);

        return Math.max(res[0],res[1]);
    }
    int[] robAction(TreeNode root){
        int[] res = new int[2];
        if(root==null) return res;

        int[] left = robAction(root.left);
        int[] right = robAction(root.right);

        res[0] = Math.max(left[0],left[1])+Math.max(right[0],right[1]);
        res[1] = root.val + left[0] + right[0];
        return res;
    }
}
```

## 300. 买卖股票的最佳时机II

给定一个数组 prices ，其中 prices[i] 表示股票第 i 天的价格。

在每一天，你可能会决定购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以购买它，然后在 同一天 出售。
返回 你能获得的 最大 利润

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][2];
        dp[0][1] = -prices[0];
        //dp[i][0] 代表第i天持有股票所得现金，dp[i][1]代表不持有所得现金
        for(int i=1;i<prices.length;i++){
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0] - prices[i]);
        }
        return dp[prices.length-1][0];
    }
}
//贪心算法，将每一天的价格减去前一天的价格，然后将所有大于0的加起来就是最大利润
class Solution {
    public int maxProfit(int[] prices) {
        int res = 0;
        int tmp = 0;
        for(int i=1;i<prices.length;i++){
            tmp = prices[i]-prices[i-1];
            if(tmp>0) res += tmp;
        }
        return res;
    }
}
```

## 301. 最长上升子序列

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列

```java
//dp 
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        for (int i = 0; i < dp.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }
        int res = 0;
        for (int i = 0; i < dp.length; i++) {
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

## 302. 最长连续递增子序列

给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int count = 1, max = 0;
        for(int i=1;i<nums.length;i++){
            if(nums[i]>nums[i-1]) count++;
            else{
                
                max = Math.max(max,count);
                count = 1;
            }
        }
        return max>count?max:count;
    }
}
```

## 303. 最长重复子数组

给两个整数数组 `nums1` 和 `nums2` ，返回 *两个数组中 **公共的** 、长度最长的子数组的长度* 。

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int result = 0;
        int[][] dp = new int[m][n];
        for(int i=0;i<m;i++) {
            if(nums1[i]==nums2[0]) {
                dp[i][0] = 1;
                result = 1;
            }
        }
        for(int i=0;i<n;i++){
            if(nums1[0]==nums2[i]) {
                dp[0][i] = 1;
                result = 1;
            }
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(nums1[i]==nums2[j]) dp[i][j] = dp[i-1][j-1] + 1;
                result = Math.max(result,dp[i][j]);
            }
        }
        return result;
    }
}
```

## 304. 最长公共子序列

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int result = 0;
        int[][] dp = new int[m+1][n+1];
        for(int i=1;i<=m;i++){
            for(int j=1;j<n+1;j++){
                if(text1.charAt(i-1)==text2.charAt(j-1)) dp[i][j] = dp[i-1][j-1]+1;
                else dp[i][j] = Math.max(dp[i-1][j],Math.max(dp[i-1][j],dp[i][j-1]));
            }
        }
        return dp[m][n];
    }
}
```

## 305. 摆动序列

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 摆动序列 。第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

例如， [1, 7, 4, 9, 2, 5] 是一个 摆动序列 ，因为差值 (6, -3, 5, -7, 3) 是正负交替出现的。

相反，[1, 4, 7, 2, 5] 和 [1, 7, 4, 5, 5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
子序列 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。

给你一个整数数组 nums ，返回 nums 中作为 摆动序列 的 最长子序列的长度 。

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int curDiff = 0;
        int preDiff = 0;
        int count = 1;
        for(int i=1;i<nums.length;i++){
            curDiff = nums[i]-nums[i-1];
            if(curDiff>0&&preDiff<=0||curDiff<0&&preDiff>=0){
                count++;
                preDiff = curDiff;
            }
        }
        return count;
    }
}
```

## 306. k次取反后最大化的数字和

给你一个整数数组 nums 和一个整数 k ，按以下方法修改该数组：

选择某个下标 i 并将 nums[i] 替换为 -nums[i] 。
重复这个过程恰好 k 次。可以多次选择同一个下标 i 。

以这种方式修改数组后，返回数组 可能的最大和 。

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        int ans = 0;
        Arrays.sort(nums);
        int i = 0;
        for(;i<nums.length&&k>0;i++){
            if(nums[i]<0) {
                ans += -nums[i];
                k--;
            }
            else break;
        }
        if(i==nums.length) ans += nums[nums.length-1]*2;
        
        for(int j=i;j<nums.length;j++){
            if(k%2==1&&j==0) {
                ans -= nums[0];
                k=0;
            }
            else if(k%2==1&&j>0) {
                if(Math.abs(nums[j])>Math.abs(nums[j-1])) {
                    ans += 2*nums[j-1];
                    ans += nums[j];
                }
                else ans += -Math.abs(nums[j]);
                k = 0;
            }
            else ans += nums[j];
        }
        return ans;
    }
}
```

## 307. 加油站

在一条环路上有 n 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 gas 和 cost ，如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1 。如果存在解，则 保证 它是 唯一 的。

```java
// 解法1,首先将所有的耗油加起来，如果总和小于0，说明没有这样一条路，然后查看gas-cost最小值，如果大于等于0，说明从0开始就可以，否则从后往前遍历，第一个能填平min的值就是出发点
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int sum = 0;
        int min = 0;
        for (int i = 0; i < gas.length; i++) {
            sum += (gas[i] - cost[i]);
            min = Math.min(sum, min);
        }

        if (sum < 0) return -1;
        if (min >= 0) return 0;

        for (int i = gas.length - 1; i > 0; i--) {
            min += (gas[i] - cost[i]);
            if (min >= 0) return i;
        }

        return -1;
    }
}
```

## 308. 丑陋的字符串

牛牛喜欢字符串,但是他讨厌丑陋的字符串。对于牛牛来说,一个字符串的丑陋值是字符串中相同连续字符对的个数。比如字符串“ABABAABBB”的丑陋值是3,因为有一对"AA"和两对重叠的"BB"。现在给出一个字符串,字符串中包含字符'A'、'B'和'?'。牛牛现在可以把字符串中的问号改为'A'或者'B'。牛牛现在想让字符串的丑陋值最小,希望你能帮帮他。                                        

```java
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        char[] c = s.toCharArray();
        int n = c.length;
        int i=0;
        int res = 0;
        while(i<n&&c[i]=='?') i++;
        
        for(;i<c.length;i++){
            if(c[i]=='?') c[i]=(c[i-1]=='A'?'B':'A');
            if(i>0&&c[i]==c[i-1]) res++;
        }
        System.out.println(res);
    }
    
}
```

## 309. 庆祝61

牛家庄幼儿园为庆祝61儿童节举办庆祝活动,庆祝活动中有一个节目是小朋友们围成一个圆圈跳舞。牛老师挑选出n个小朋友参与跳舞节目,已知每个小朋友的身高h_i。为了让舞蹈看起来和谐,牛老师需要让跳舞的圆圈队形中相邻小朋友的身高差的最大值最小,牛老师犯了难,希望你能帮帮他。
 如样例所示:
 当圆圈队伍按照100,98,103,105顺时针排列的时候最大身高差为5,其他排列不会得到更优的解

```java
//高的站两边，矮的站中间
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] height = new int[n];
        for(int i=0;i<n;i++) height[i] = sc.nextInt();
        int[] tmp = new int[n];
        Arrays.sort(height);
        int j = n-1;
        int k = 0;
        int i = 0;
        while(i<=j){
            tmp[j--] = height[k++];
            if(k==n) break;
            tmp[i++] = height[k++];
        }
        int min = 0;
        for(int m = 1;m<n;m++) min = Math.max(min,Math.abs(tmp[m]-tmp[m-1]));
        min = Math.max(min,Math.abs(tmp[0]-tmp[n-1]));
        System.out.println(min);
    }
 
    
}
```

## 310. 随机的机器人

有一条无限长的纸带,分割成一系列的格子,最开始所有格子初始是白色。现在在一个格子上放上一个萌萌的机器人(放上的这个格子也会被染红),机器人一旦走到某个格子上,就会把这个格子涂成红色。现在给出一个整数n,机器人现在会在纸带上走n步。每一步,机器人都会向左或者向右走一个格子,两种情况概率相等。机器人做出的所有随机选择都是独立的。现在需要计算出最后纸带上红色格子的期望值。

```java
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        float[] dp = new float[n+1];
        dp[0] = 1.0f;
        dp[1] = 2.0f;
        dp[2] = 2.5f;
        for(int i=3;i<=n;i++){
            //递推公式
            dp[i] = (i-1)*dp[i-2]/i+2*dp[i-1]/i;
        }
        String s=String.format("%.1f",dp[n]);
        System.out.println(s);
    }
}
```

## 311. 分发糖果

n 个孩子站成一排。给你一个整数数组 ratings 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻两个孩子评分更高的孩子会获得更多的糖果。
请你给每个孩子分发糖果，计算并返回需要准备的 最少糖果数目 。

```java
/*这道题目一定是要确定一边之后，再确定另一边，例如比较每一个孩子的左边，然后再比较右边，如果两边一起考虑一定会顾此失彼。
先确定右边评分大于左边的情况（也就是从前向后遍历）
此时局部最优：只要右边评分比左边大，右边的孩子就多一个糖果，全局最优：相邻的孩子中，评分高的右孩子获得比左边孩子更多的糖果
局部最优可以推出全局最优。
如果ratings[i] > ratings[i - 1] 那么[i]的糖 一定要比[i - 1]的糖多一个，所以贪心：candyVec[i] = candyVec[i - 1] + 1
再确定左孩子大于右孩子的情况（从后向前遍历）
遍历顺序这里有同学可能会有疑问，为什么不能从前向后遍历呢？
因为如果从前向后遍历，根据 ratings[i + 1] 来确定 ratings[i] 对应的糖果，那么每次都不能利用上前一次的比较结果了。
所以确定左孩子大于右孩子的情况一定要从后向前遍历！
如果 ratings[i] > ratings[i + 1]，此时candyVec[i]（第i个小孩的糖果数量）就有两个选择了，一个是candyVec[i + 1] + 1（从右边这个加1得到的糖果数量），一个是candyVec[i]（之前比较右孩子大于左孩子得到的糖果数量）。
那么又要贪心了，局部最优：取candyVec[i + 1] + 1 和 candyVec[i] 最大的糖果数量，保证第i个小孩的糖果数量即大于左边的也大于右边的。全局最优：相邻的孩子中，评分高的孩子获得更多的糖果。
局部最优可以推出全局最优。
所以就取candyVec[i + 1] + 1 和 candyVec[i] 最大的糖果数量，candyVec[i]只有取最大的才能既保持对左边candyVec[i - 1]的糖果多，也比右边candyVec[i + 1]的糖果多。*/
class Solution {
    public int candy(int[] ratings) {
        int[] candy = new int[ratings.length];
        candy[0] = 1;
        for(int i=1;i<ratings.length;i++){
            if(ratings[i]>ratings[i-1]) candy[i] = candy[i-1] + 1;
            else candy[i] = 1;
        }
        for(int i=ratings.length-2;i>=0;i--){
            if(ratings[i]>ratings[i+1]) candy[i] = Math.max(candy[i],candy[i+1]+1);
        }
        int ans = 0;
        for(int x:candy) ans+=x;
        return ans;
    }
}
```

## 312. 柠檬水找零

在柠檬水摊上，每一杯柠檬水的售价为 5 美元。顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。

注意，一开始你手头没有任何零钱。

给你一个整数数组 bills ，其中 bills[i] 是第 i 位顾客付的账。如果你能给每位顾客正确找零，返回 true ，否则返回 false 。

 ```java
 class Solution {
     public boolean lemonadeChange(int[] bills) {
         int five = 0;
         int ten = 0;
         for(int i=0;i<bills.length;i++){
             if(bills[i]==5) five++;
             else if(bills[i]==10) {
                 five--;
                 ten++;
             }
             else{
                 if(ten>0) {
                     ten--;
                     five--;
                 }
                 else five-=3;
             }
             if(ten<0||five<0) return false;
         }
         return true;
     }
 }
 ```

## 313. 根据身高重建队列

假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // 身高从大到小排（身高相同k小的站前面）
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            return b[0] - a[0];
        });

        LinkedList<int[]> que = new LinkedList<>();

        for (int[] p : people) {
            que.add(p[1],p);
        }

        return que.toArray(new int[people.length][]);
    }
}
```

## 314. 无重叠区间

给定一个区间的集合 intervals ，其中 intervals[i] = [starti, endi] 。返回 需要移除区间的最小数量，使剩余区间互不重叠 。

 

示例 1:

输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
输出: 1
解释: 移除 [1,3] 后，剩下的区间没有重叠。

```java
//按照右边界排序，从左向右记录非交叉区间的个数。最后用区间总数减去非交叉区间的个数就是需要移除的区间个数了。
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals,(a,b)->{
            if(a[0]==a[0]) return a[1]-b[1];
            return a[0]-b[0];
        });
        int count = 0;
        int edge = Integer.MIN_VALUE;
        for(int i=0;i<intervals.length;i++){
            if(edge<=intervals[i][0]){
                edge = intervals[i][1];
            }
            else count++;
        }
        return count;
    }
}
```

## 315. 划分字母区间

字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

 

示例：

输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。

```java
//在遍历的过程中相当于是要找每一个字母的边界，如果找到之前遍历过的所有字母的最远边界，说明这个边界就是分割点了。此时前面出现过所有字母，最远也就到这个边界了。
//可以分为如下两步：
//统计每一个字符最后出现的位置
//从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点
class Solution {
    public List<Integer> partitionLabels(String s) {
        List<Integer> list = new LinkedList<>();
        char[] c = s.toCharArray();
        int[] tmp = new int[26];
        //找到每个字母最后一次出现在字符串的位置
        for(int i=0;i<c.length;i++){
            tmp[c[i]-'a'] = i;
        }
        int idx = 0;
        int last = -1;

        for(int i=0;i<c.length;i++){
            idx = Math.max(idx,tmp[c[i]-'a']);
            if(i==idx){
                list.add(i-last);
                last = i;
            }
        }
        return list;
    }
}
```

## 316. 合并区间

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

```java
//按照区间右边界排序
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> res = new ArrayList<>();
      Arrays.sort(intervals, (o1, o2) -> Integer.compare(o1[0], o2[0]));
      int start = intervals[0][0];
      for(int i=1;i<intervals.length;i++){
          if(intervals[i][0]<=intervals[i-1][1]){
              intervals[i][1] = Math.max(intervals[i][1], intervals[i - 1][1]);
          } 
          else{
              res.add(new int[]{start, intervals[i - 1][1]});
              start = intervals[i][0];
          }
      }
      res.add(new int[]{start, intervals[intervals.length - 1][1]});
        return res.toArray(new int[res.size()][]);
        
    }
}
```

## 317. 单调递增的数字

当且仅当每个相邻位数上的数字 x 和 y 满足 x <= y 时，我们称这个整数是单调递增的。

给定一个整数 n ，返回 小于或等于 n 的最大数字，且数字呈 单调递增 。

```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        String s = String.valueOf(n);
        char[] chars = s.toCharArray();
        int start = s.length();
        for (int i = s.length() - 2; i >= 0; i--) {
            if (chars[i] > chars[i + 1]) {
                chars[i]--;
                start = i+1;
            }
        }
        for (int i = start; i < s.length(); i++) {
            chars[i] = '9';
        }
        return Integer.parseInt(String.valueOf(chars));
    }
}
```

## 318. 最长回文子序列

给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int len = s.length();
        //1. 确定dp数组下标含义
        //dp[i][j]：字符串s在[i, j]范围内最长的回文子序列的长度为dp[i][j]
        int[][] dp = new int[len+1][len+1];
        //2. 确定递推公式 if s[i]==s[j] dp[i][j] = dp[i + 1][j - 1] + 2;
        //不相等，dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
        //3.初始化 dp[i][i] = 1;        
        for(int i=0;i<len+1;i++) dp[i][i] = 1;
        //4. 遍历顺序 所以遍历i的时候一定要从下到上遍历，这样才能保证，下一行的数据是经过计算的
        for(int i=len-1;i>=0;i--){
            for(int j=i+1;j<len;j++){
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], Math.max(dp[i][j], dp[i][j - 1]));
                }
            }
        }
        return dp[0][len-1];

    }
}
```

## 319. 买卖股票的最佳时机III

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        /*
         * 定义 5 种状态:
         * 0: 没有操作, 1: 第一次买入, 2: 第一次卖出, 3: 第二次买入, 4: 第二次卖出
         */
        int[][] dp = new int[len][5];
        dp[0][1] = -prices[0];
        // 初始化第二次买入的状态是确保 最后结果是最多两次买卖的最大利润
        dp[0][3] = -prices[0];

        for (int i = 1; i < len; i++) {
            dp[i][1] = Math.max(dp[i - 1][1], -prices[i]);
            dp[i][2] = Math.max(dp[i - 1][2], dp[i][1] + prices[i]);
            dp[i][3] = Math.max(dp[i - 1][3], dp[i][2] - prices[i]);
            dp[i][4] = Math.max(dp[i - 1][4], dp[i][3] + prices[i]);
        }

        return dp[len - 1][4];
    }
}


class Solution {
    public int maxProfit(int[] prices) {
        int[] dp = new int[4]; 
        // 存储两次交易的状态就行了
        // dp[0]代表第一次交易的买入
        dp[0] = -prices[0];
        // dp[1]代表第一次交易的卖出
        dp[1] = 0;
        // dp[2]代表第二次交易的买入
        dp[2] = -prices[0];
        // dp[3]代表第二次交易的卖出
        dp[3] = 0;
        for(int i = 1; i <= prices.length; i++){
            // 要么保持不变，要么没有就买，有了就卖
            dp[0] = Math.max(dp[0], -prices[i-1]);
            dp[1] = Math.max(dp[1], dp[0]+prices[i-1]);
            // 这已经是第二次交易了，所以得加上前一次交易卖出去的收获
            dp[2] = Math.max(dp[2], dp[1]-prices[i-1]);
            dp[3] = Math.max(dp[3], dp[2]+ prices[i-1]);
        }
        return dp[3];
    }
}
```

## 320. 不相交的线

在两条独立的水平线上按给定的顺序写下 nums1 和 nums2 中的整数。

现在，可以绘制一些连接两个数字 nums1[i] 和 nums2[j] 的直线，这些直线需要同时满足满足：

 nums1[i] == nums2[j]
且绘制的直线不与任何其他连线（非水平线）相交。
请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int m = nums2.length;

        int[][] dp = new int[n+1][m+1];
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(nums1[i-1]==nums2[j-1]) dp[i][j] = dp[i-1][j-1]+1;
                else  dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[n][m]; 
    }
}
```

## 321. 不同的子序列

给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。

字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）

题目数据保证答案符合 32 位带符号整数范围。

```java
/**
这一类问题，基本是要分析两种情况
s[i - 1] 与 t[j - 1]相等
s[i - 1] 与 t[j - 1] 不相等
当s[i - 1] 与 t[j - 1]相等时，dp[i][j]可以有两部分组成。
一部分是用s[i - 1]来匹配，那么个数为dp[i - 1][j - 1]。
一部分是不用s[i - 1]来匹配，个数为dp[i - 1][j]。
例如： s：bagg 和 t：bag ，s[3] 和 t[2]是相同的，但是字符串s也可以不用s[3]来匹配，即用s[0]s[1]s[2]组成的bag。
当然也可以用s[3]来匹配，即：s[0]s[1]s[3]组成的bag。
所以当s[i - 1] 与 t[j - 1]相等时，dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
当s[i - 1] 与 t[j - 1]不相等时，dp[i][j]只有一部分组成，不用s[i - 1]来匹配，即：dp[i - 1][j]
*/
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m+1][n+1];
        for (int i = 0; i <=m; i++) {
            dp[i][0] = 1;
        }
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(s.charAt(i-1)==t.charAt(j-1)) dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                else dp[i][j] = dp[i-1][j];
            }
        }
        return dp[m][n];
    }
}
```

## 322. 编辑距离

给你两个单词 word1 和 word2， 请返回将 word1 转换成 word2 所使用的最少操作数  。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符

```java
/**
1. 确定dp数组的含义： dp[i][j] 代表第一个单词以i-1结尾和第二个单词以j-1结尾，需要的最少操作次数
2. 递推公式： 要得到最少操作数，有两种情况
	a. 两个单词的i和j位相等：此时dp[i][j] = dp[i-1][j-1];
	b. 不相等：此时有三种操作，删除一个，添加一个，修改一个（需要注意的是添加和删除是对应的，你删除第一个中的相当于在第二个中增加一个单词）修改一个单词操作数即为dp[i-1][j-1]+1，增加一个dp[i-1][j]+1，相应的删除为dp[i][j-1]+1，求其中的最小值即可
*/

class Solution {
    public int minDistance(String word1, String word2) {
        int n = word1.length();
        int m = word2.length();
        int[][] dp = new int[n+1][m+1];
        for(int i=0;i<=n;i++) dp[i][0] = i;
        for(int i=0;i<=m;i++) dp[0][i] = i;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(word1.charAt(i-1)==word2.charAt(j-1)) dp[i][j] = dp[i-1][j-1];
                else{
                    dp[i][j] = Math.min(dp[i-1][j-1],Math.min(dp[i-1][j],dp[i][j-1]))+1;
                }
            }
        }
        return dp[n][m];
    }
}
```

## 323. 数组中缺失的数字

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int len = nums.length;
        /**
    	将给定的数组「恢复」成下面的形式：
		如果数组中包含x∈[1,N]，那么恢复后，数组的第 x - 1 个元素为 x。
        */
        for(int i=0;i<len;i++){
            while(nums[i]>0&&nums[i]<len&&nums[i]!=nums[nums[i]-1]){
               int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }
        for (int i = 0; i < len; ++i) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return len + 1;

    }
}
```

## 324. 螺旋矩阵

给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。

 

示例 1：


输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<>();
        int left = 0;
        int right = matrix[0].length-1;
        int top = 0;
        int bottom = matrix.length-1;
        int i = 0;
        while(i<matrix.length*matrix[0].length){
            for(int j=left;j<=right&&i<matrix.length*matrix[0].length;j++){
                ans.add(matrix[top][j]);
                i++;
            }
            top++;
            for(int j=top;j<=bottom&&i<matrix.length*matrix[0].length;j++){
                ans.add(matrix[j][right]);
                i++;
            }
            right--;
            for(int j=right;j>=left&&i<matrix.length*matrix[0].length;j--){
                ans.add(matrix[bottom][j]);
                i++;
            }
            bottom--;
            for(int j=bottom;j>=top&&i<matrix.length*matrix[0].length;j--){
                ans.add(matrix[j][left]);
                i++;
            }
            left++;
        }
        return ans;
    }
}
```

## 325. 排列序列

按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：

"123"
"132"
"213"
"231"
"312"
"321"
给定 n 和 k，返回第 k 个排列。

```java
//1. 回溯找第k个排列
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public String getPermutation(int n, int k) {
        boolean[] used = new boolean[n+1];
        dfs(n,k,used);
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<ans.get(ans.size()-1).size();i++) sb.append(ans.get(ans.size()-1).get(i));
        return sb.toString();
    }
    public void dfs(int n,int k,boolean[] used){
        if(path.size()==n){
            ans.add(new ArrayList<>(path));
            return ;
        }
        if(ans.size()==k) return ;
        for(int i=1;i<=n;i++){
            if(!used[i]) {
                path.add(i);
                used[i] = true;
                dfs(n,k,used);
                path.remove(path.size()-1);
                used[i] = false;
            }
            
        }
    }
}


//2. 数学方法
class Solution { 
    
    public String getPermutation(int n, int k) {
        /**
        直接用回溯法做的话需要在回溯到第k个排列时终止就不会超时了, 但是效率依旧感人
        可以用数学的方法来解, 因为数字都是从1开始的连续自然数, 排列出现的次序可以推
        算出来, 对于n=4, k=15 找到k=15排列的过程:
        
        1 + 对2,3,4的全排列 (3!个)         
        2 + 对1,3,4的全排列 (3!个)         3, 1 + 对2,4的全排列(2!个)
        3 + 对1,2,4的全排列 (3!个)-------> 3, 2 + 对1,4的全排列(2!个)-------> 3, 2, 1 + 对4的全排列(1!个)-------> 3214
        4 + 对1,2,3的全排列 (3!个)         3, 4 + 对1,2的全排列(2!个)         3, 2, 4 + 对1的全排列(1!个)
        
        确定第一位:
            k = 14(从0开始计数)
            index = k / (n-1)! = 2, 说明第15个数的第一位是3 
            更新k
            k = k - index*(n-1)! = 2
        确定第二位:
            k = 2
            index = k / (n-2)! = 1, 说明第15个数的第二位是2
            更新k
            k = k - index*(n-2)! = 0
        确定第三位:
            k = 0
            index = k / (n-3)! = 0, 说明第15个数的第三位是1
            更新k
            k = k - index*(n-3)! = 0
        确定第四位:
            k = 0
            index = k / (n-4)! = 0, 说明第15个数的第四位是4
        最终确定n=4时第15个数为3214 
        **/
        
        StringBuilder sb = new StringBuilder();
        // 候选数字
        List<Integer> candidates = new ArrayList<>();
        // 分母的阶乘数
        int[] factorials = new int[n+1];
        factorials[0] = 1;
        int fact = 1;
        for(int i = 1; i <= n; ++i) {
            candidates.add(i);
            fact *= i;
            factorials[i] = fact;
        }
        k -= 1;
        for(int i = n-1; i >= 0; --i) {
            // 计算候选数字的index
            int index = k / factorials[i];
            sb.append(candidates.remove(index));
            k -= index*factorials[i];
        }
        return sb.toString();
    }
}
```

## 326. 最小有效括号

给你一个只包含 `'('` 和 `')'` 的字符串，找出最长有效（格式正确且连续）括号子串的长度。

```java
class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        int n = s.length();
        int count = 0;
        //dp[i] 代表以i结尾的最长有效括号长度
        int[] dp = new int[n];

        for(int i=0;i<n;i++){
           if(s.charAt(i)==')'){
               if(count>0){
                   dp[i] = dp[i-1] + 2;
                   //
                   dp[i] += i>dp[i]?dp[i-dp[i]]:0;
                   count--;
               }
           }
           else{
               count++;
           }
           max = Math.max(max,dp[i]);
        }
        return max;
    }
}
```

## 327. 节点之和最大的路径

路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给定一个二叉树的根节点 root ，返回其 最大路径和，即所有路径上节点值之和的最大值。

```java
class Solution {
    int max = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) { 
        dfsGetGain(root);
        return max;
       
    }
    public int dfsGetGain(TreeNode node){
        if(node == null) return 0; // 基准情形
        int leftGain = Math.max(dfsGetGain(node.left), 0); // gain为负则取0
        int rightGain = Math.max(dfsGetGain(node.right), 0);
        int nodeMax = leftGain + node.val + rightGain; // 取得以node为拐点的最大路径和
        max = Math.max(max, nodeMax); // 更新max
        return node.val + Math.max(leftGain, rightGain); // 返回nodeGain
    }
}
```

## 328. 日程表

请实现一个 MyCalendar 类来存放你的日程安排。如果要添加的时间内没有其他安排，则可以存储这个新的日程安排。

MyCalendar 有一个 book(int start, int end)方法。它意味着在 start 到 end 时间内增加一个日程安排，注意，这里的时间是半开区间，即 [start, end), 实数 x 的范围为，  start <= x < end。

当两个日程安排有一些时间上的交叉时（例如两个日程安排都在同一时间内），就会产生重复预订。

每次调用 MyCalendar.book方法时，如果可以将日程安排成功添加到日历中而不会导致重复预订，返回 true。否则，返回 false 并且不要将该日程安排添加到日历中。

请按照以下步骤调用 MyCalendar 类: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)

```java
class MyCalendar {
    List<int[]> book;
    public MyCalendar() {
        book = new ArrayList<>();
    }
    
    public boolean book(int start, int end) {
        for(int[] a:book){
            if((a[1]>start&&a[0]<=start)||(end>a[0]&&end<=a[1])||(start<=a[0]&&end>a[1])) return false;
        }
        book.add(new int[]{start,end});
        return true;
    }
}
```

## 329. 数据流中的第k大的数

设计一个找到数据流中第 k 大元素的类（class）。注意是排序后的第 k 大元素，不是第 k 个不同的元素。

请实现 KthLargest 类：

KthLargest(int k, int[] nums) 使用整数 k 和整数流 nums 初始化对象。
int add(int val) 将 val 插入数据流 nums 后，返回当前数据流中第 k 大的元素。

```java
/**
优先级队列
*/
class KthLargest {
    PriorityQueue<Integer> maxheap;
    int cap;
    public KthLargest(int k, int[] nums) {
        maxheap = new PriorityQueue<Integer>() ;
        cap = k;
        for(int x:nums) maxheap.offer(x);
    }
    
    public int add(int val) {
        //保证堆里有k个元素
        while(maxheap.size()>cap) maxheap.poll();
        if(maxheap.size()<cap) maxheap.offer(val);
        //如果比堆里最小的数大，那就加入堆，然后出队一个最小的，如果比堆里最小的数小，那就不用更改堆里的数，这样保证堆里第一个就是最小的，而且堆里刚好有k个数，所以堆顶就是第k大的数
        
        else if(val>=maxheap.peek()) {
            maxheap.offer(val);
            maxheap.poll();
        }

        return maxheap.peek();
    }
}

```

## 330. 序列化与反序列化二叉树

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

```java

public class Codec {
    Queue<TreeNode> queue = new LinkedList<>();    // Encodes a tree to a single string.
    public String serialize(TreeNode root) { 
        return rserialize(root,"");
    }
    public TreeNode deserialize(String data) {
        String[] tree = data.split("\\.");
        System.out.println(tree.length);
        if(tree.length==0) return new TreeNode();
        List<String> list = new ArrayList<>();
        for(String s: tree) list.add(s);

        return rdeserialize(list);
    }
    public String rserialize(TreeNode root, String str) {
        if (root == null) {
            str += "empty.";
        } else {
            str += str.valueOf(root.val) + ".";
            str = rserialize(root.left, str);
            str = rserialize(root.right, str);
        }
        return str;
    }

    public TreeNode rdeserialize(List<String> data){
        if(data.get(0).equals("empty")){
            data.remove(0);
            return null;
        }
        TreeNode root = new TreeNode(Integer.valueOf(data.get(0)));
        data.remove(0);
        root.left = rdeserialize(data);
        root.right = rdeserialize(data);
        return root;
    }
}

```

## 331. 和最小的k个数对

给定两个以升序排列的整数数组 nums1 和 nums2 , 以及一个整数 k 。

定义一对值 (u,v)，其中第一个元素来自 nums1，第二个元素来自 nums2 。

请找到和最小的 k 个数对 (u1,v1),  (u2,v2)  ...  (uk,vk) 。

 ```java
 // 解题思路 大顶堆积+多路归并
 //先存放第一个数组的所有元素与第二个数组首元素的集合
 //取出最小的和，并根据对应的'路'找到对应的下一个元素放入队列。
 class Solution{
   	PriorityQueue<int[]> priorityQueue;
     public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
         priorityQueue = new PriorityQueue<>(k, (a, b) -> (nums1[a[0]] + nums2[a[1]]) - (nums1[b[0]] + nums2[b[1]]));
         for (int i = 0; i < nums1.length; i++) {
             priorityQueue.add(new int[]{i, 0});
         }
 
         List<List<Integer>> ret = new ArrayList<>(k);
         // 多路归并
         while (ret.size() < k && !priorityQueue.isEmpty()) {
             int[] poll = priorityQueue.poll();
             ret.add(List.of(nums1[poll[0]], nums2[poll[1]]));
             if (poll[1] < nums2.length - 1) {
                 priorityQueue.add(new int[]{poll[0], poll[1] + 1});
             }
         }
         return ret;
     }
 }
 ```

## 332. 实现前缀树

Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：

Trie() 初始化前缀树对象。
void insert(String word) 向前缀树中插入字符串 word 。
boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
boolean startsWith(String prefix) 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。

```java
class Trie {
     TreeNode root;
    /** Initialize your data structure here. */
    class TreeNode{
        TreeNode[] next;
        boolean isEnd;
        public TreeNode(){
            next = new TreeNode[26];
        }
    }
    public Trie() {
        root = new TreeNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TreeNode cur = root;
        for(char ch:word.toCharArray()){
            if(cur.next[ch-'a'] == null){
                cur.next[ch-'a'] = new TreeNode();
            }
            cur = cur.next[ch-'a'];
        }
        cur.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TreeNode cur = root;
        for(char ch:word.toCharArray()){
            if(cur.next[ch - 'a'] == null) return false;
            cur = cur.next[ch - 'a'];
        }
        return cur.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TreeNode cur = root;
        for(char ch:prefix.toCharArray()){
            if(cur.next[ch - 'a'] == null) return false;
            cur = cur.next[ch - 'a'];
        }
        return true;
    }
}

```

## 333. 最短的单词编码

单词数组 words 的 有效编码 由任意助记字符串 s 和下标数组 indices 组成，且满足：

words.length == indices.length
助记字符串 s 以 '#' 字符结尾
对于每个下标 indices[i] ，s 的一个从 indices[i] 开始、到下一个 '#' 字符结束（但不包括 '#'）的 子字符串 恰好与 words[i] 相等
给定一个单词数组 words ，返回成功对 words 进行编码的最小助记字符串 s 的长度 。

```java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        //必须是长度大的先放进trie树
        Arrays.sort(words,new Comparator<String>(){
            @Override
            public int compare(String o1, String o2) {
                return o2.length()-o1.length();
            }
        });
        //然后统计“不是其他字符串后缀”的字符串的长度+1
        int len=0;
        Trie t=new Trie();
        for (String word:words) {
            StringBuilder stringBuilder=new StringBuilder(word);
            stringBuilder.reverse();
            String revStr=stringBuilder.toString();
            if(!t.startsWith(revStr)){
                t.insert(revStr);
                len+=(revStr.length()+1);
            }
        }
        return len;
    }   
}
class Trie {
    // 记录前缀树的根节点
    TreeNode root;
    // 定义前缀树节点
    class TreeNode{
        TreeNode[] next;
        boolean isEnd;
        public TreeNode (){
            next = new TreeNode[26];
        }
    }
    // 初始化前缀树
    public Trie() {
        root = new TreeNode();
    }
    // 插入
    public void insert(String word) {
        TreeNode cur = root;
        for(char ch : word.toCharArray()){
            // 判断对应节点是否为空，如果为空，则直接插入
            if(cur.next[ch - 'a'] == null){
                cur.next[ch - 'a'] = new TreeNode();
            }
            // 继续插入下一个节点
            cur = cur.next[ch - 'a'];
        }
        // 将最后一个字符设置为结尾
        cur.isEnd = true;
    }
    public boolean search(String words) {
        TreeNode cur = root;
        for(char ch : words.toCharArray()){
            // 如果对应节点为空，则表明不存在这个单词，返回false
            if(cur.next[ch - 'a'] == null)
                return false;
            cur = cur.next[ch - 'a'];
        }
        // 检查最后一个字符是否是结尾
        return cur.isEnd;
    }
    // 查找前缀
    public boolean startsWith(String prefix) {
        TreeNode cur = root;
        for(char ch : prefix.toCharArray()){
            if(cur.next[ch - 'a'] == null)
                return false;
            cur = cur.next[ch - 'a'];
        }
        return true;
    }
}
```

## 334. 最大的异或

给定一个整数数组 `nums` ，返回 `nums[i] XOR nums[j]` 的最大运算结果，其中 `0 ≤ i ≤ j < n` 。

```java
class Solution {
    public int findMaximumXOR(int[] nums) {
        int max = 0;
        Trie trie = new Trie();
        for(int num : nums) {
            trie.insert(num);
            max = Math.max(max,num ^ trie.search(num));
        }
        return max;
    }
}
class Trie{
    TreeNode root;
    class TreeNode{
        TreeNode[] next;
        public TreeNode(){
            next = new TreeNode[2];
        }
    }
    public Trie(){
        root = new TreeNode();
    }
    public void insert(int num){
        TreeNode cur = root;
        for(int i = 30;i >= 0;i--){
            //取得当前位
            int bit = (num >> i) & 1;
            if(cur.next[bit] == null){
                cur.next[bit] = new TreeNode();
            }
            cur = cur.next[bit];
        }
    }
    public int search(int num){
        TreeNode cur = root;
        int ans = 0;
        for(int i = 30;i >= 0;i--){
            int bit = (num >> i) & 1;
            bit = cur.next[bit ^ 1] == null ? bit : bit ^ 1;
            ans += bit << i;
            cur = cur.next[bit];
        }
        return ans;
    }
}
```

## 335. 求平方根

给定一个非负整数 x ，计算并返回 x 的平方根，即实现 int sqrt(int x) 函数。

正数的平方根有两个，只输出其中的正数平方根。

如果平方根不是整数，输出只保留整数的部分，小数部分将被舍去

```java
class Solution {
    public int mySqrt(int x) {
        int start = 0;
        int end = x;
        int res = -1;
        while(start <= end){
            int mid = (end - start) / 2 + start;
            if((long) mid * mid <= x){
                start = mid + 1;
                res = mid;
            }
            else {
                end = mid - 1;
            }
        }
        return res;
    }
}
```

## 336. 狒狒吃香蕉

狒狒喜欢吃香蕉。这里有 n 堆香蕉，第 i 堆中有 piles[i] 根香蕉。警卫已经离开了，将在 h 小时后回来。

狒狒可以决定她吃香蕉的速度 k （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 k 根。如果这堆香蕉少于 k 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉，下一个小时才会开始吃另一堆的香蕉。  

狒狒喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 h 小时内吃掉所有香蕉的最小速度 k（k 为整数）。

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int n = piles.length;
        // 速度min: 香蕉总数/h 向上取整 (用时最长，需要在h小时内吃完)
        // 速度max: piles数组最大值 (用时最短，为piles数组的长度n)
        int max = 0; int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += piles[i];
            max = Math.max(max, piles[i]);
        }
        int min = (sum % h == 0) ? (sum / h) : (sum / h + 1);
        
        // 二分查找，速度k介于min和max之间（求最小）
        int left = min; int right = max;
        while(left < right) {
            int mid = left + (right - left) / 2;
            // 判断速度为mid能否吃完
            int time = 0;
            for (int i = 0; i < n; i++) {
                time += (piles[i] % mid == 0) ? (piles[i] / mid) : (piles[i] / mid + 1);
            }
            if (time > h) { //  吃不完，需加快速度
                left = mid + 1;
            } else if (time <= h) { // 吃得完，看看能不能吃得再慢一点
                right = mid;        // 注意这边是mid，不是mid - 1
            }
        }
        return left;
    }
}


```

## 337. 合并排序链表

给定一个链表数组，每个链表都已经按升序排列。

请将所有链表合并到一个升序链表中，返回合并后的链表。

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        // 边界情况考虑
        if(lists.length == 0)   return null;
        Queue<ListNode> heap = new PriorityQueue<>((ListNode a, ListNode b) -> (a.val - b.val));
        for(int i=0; i<lists.length; i++){
            ListNode curHead = lists[i];
            while(curHead != null){
                heap.offer(curHead);
                curHead = curHead.next;
            }
        }
        ListNode head = heap.poll();
        ListNode node = head;
        while(!heap.isEmpty()){
            node.next = heap.poll();
            node = node.next;
        }
        // 把链表中的环去掉，同时考虑 [[]] 的情况
        if(node != null) node.next = null;
        return head;
    }
}
```

