2022/8/30

（1）给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

 

示例 1：

输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

示例 2：

输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。

我的题解：

双重循环暴力解法

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        int len = prices.length;
        for(int i = 0;i<len-1;i++){
            int j ;
            for(j = i+1;j<len;j++){
                int profitNew = prices[j]-prices[i];
                if(profit<profitNew){
                    profit = profitNew;
                }
            }
        }
        return profit;
    }
}
```

时间复杂度过高，超出时间限制

正确题解（一）

动态规划：最大利润 = 第i天的价格 - 前（i-1）天的最低价格 与 前i天的最大利润 取大

​				   最低价 = 第i天的价格 与 前i-1天的最低价格 取小

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int minPrice = prices[0];
        int len = prices.length;
        for(int i = 1;i<len;i++){
            maxProfit = Math.max(maxProfit,prices[i]-minPrice);
            minPrice = Math.min(minPrice,prices[i]);
        }
        return maxProfit;
    }
}
```



2022/8/31

（1）给定一个包含大写字母和小写字母的字符串 s ，返回 通过这些字母构造成的 最长的回文串 。

在构造过程中，请注意 区分大小写 。比如 "Aa" 不能当做一个回文字符串。

示例 1:

输入:s = "abccccdd"
输出:7
解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

示例 2:

输入:s = "a"
输入:1

我的题解：

```java
class Solution {
    public int longestPalindrome(String s) {
        //创建一个hashmap存储字符的出现次数
        Map<Character,Integer> hm= new HashMap<Character,Integer>();	
        char now;
        int len=0;
        int singleVal = 0;
        //遍历数组存值
        for(int i = 0;i<s.length();i++){
            now = s.charAt(i);
            if(!hm.containsKey(now)){
                hm.put(now,1);
            }else{
                hm.put(now,hm.get(now)+1);
            }
        }
        //遍历haspMap取value
        for(Integer value:hm.values()){
       		if(value%2==0){
            len+=value;
        	}else{
                //**重点** 将单数数量的值改为偶数数量
            	value = value/2*2;
            	len+=value;
                //若出现单数数量的值，则必可以在回文串中心放入一个
            	singleVal = 1;
        	}
    	}
    	return len = len+singleVal;
	}
}
```



简洁解法（一）

对于每个字符 ch，假设它出现了 v 次，我们可以使用该字符 v / 2 * 2 次，在回文串的左侧和右侧分别放置 v / 2 个字符 ch，其中 / 为整数除法。例如若 "a" 出现了 5 次，那么我们可以使用 "a" 的次数为 4，回文串的左右两侧分别放置 2 个 "a"。

如果有任何一个字符 ch 的出现次数 v 为奇数（即 v % 2 == 1），那么可以将这个字符作为回文中心，注意只能最多有一个字符作为回文中心。在代码中，我们用 ans 存储回文串的长度，由于在遍历字符时，ans 每次会增加 v / 2 * 2，因此 ans 一直为偶数。但在发现了第一个出现次数为奇数的字符后，我们将 ans 增加 1，这样 ans 变为奇数，在后面发现其它出现奇数次的字符时，我们就不改变 ans 的值了。

```java
class Solution {
    public int longestPalindrome(String s) {
        int count[] = new int[128];
        for (char c : s.toCharArray()) {
            count[c]++;
        }
        int ans = 0;
        for (int v : count) {
            ans += v / 2 * 2;
            if (v % 2 == 1 && ans % 2 == 0) {
                ans++;
            }
        }
        return ans;
    }
}
```

