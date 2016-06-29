---
layout: post
title: swift实现一个时间复杂度为0(N)的斐波那契数列
date: 2016-06-28 10:23:24.000000000 +09:00
---

#### 斐波那契数的定义([摘自维基百科](https://zh.wikipedia.org/wiki/%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97))
```
费波那契数列（意大利语：Successione di Fibonacci），又译费波拿契数、斐波那契数列、费氏数列、黄金分割数列。

在数学上，费波那契数列是以递归的方法来定义：
F(0) = 0
F(1) = 1
F(N) = F(N-1) + F(N-2)

用文字来说，就是费波那契数列由0和1开始，之后的费波那契系数就由之前的两数相加。首几个费波那契系数是[1]：

0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233……

特别指出：0不是第一项，而是第零项。
```

####递归实现斐波那契数列

{% highlight swift %}
func fib(n:Int) -> Int
{
    if n<2
    {
        return n
    }
    
    return fib(n-2) + fib(n-1)
}
{% endhighlight %}


要计算第n个斐波那契数，需要计算出第n-1和第n-2个斐波那契数。而计算第n-1个斐波那契数的时候又要计算第n-2和n-3个。


```
		         fib(n)
		        /      \
	    fib(n-1)	    fib(n-2)
        /    \         /     \
fib(n-2)  fib(n-3) fib(n-3) fib(n-4)
  
```
使用递归的算法计算了大量重复的节点，而且重复的节点随着n的增大急剧增加。计算一个新的fib(n+1)会将已经算好的fib(n)重新计算一次。效率非常低。这个算法的时间复杂度是O(1.6ⁿ)。


![](https://raw.githubusercontent.com/imphila/imphila.github.io/master/assets/blogresource/fib20.png)

可以看到计算fib(20)执行了10946+10945次，速度非常慢，计算fib(30)我已经没有耐心等结果了。那么，怎么来优化呢？


####使用字典来实现
优化，首先想到的就是避免重复的计算。我们可以使用字典来保存已经算好的值。从小到大计算。看了代码就很容易理解了。
{% highlight swift %}
func fibSample(n: Int)->Int
{
    var dict = [Int:Int]()
    dict[0] = 0
    dict[1] = 1
    for n in 2...n
    {
        dict[n] = dict[n-1]! + dict[n-2]!
    }
    return dict[n]!
}
{% endhighlight %}

![](https://raw.githubusercontent.com/imphila/imphila.github.io/master/assets/blogresource/fibSample50.png)

这个算法的时间复杂度为0(N)。

计算fib(50)只需要执行49次。无需等待，爽。


