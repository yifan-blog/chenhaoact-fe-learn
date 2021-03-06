# CSS选择器
## 一 常用选择器
E,F
**多元素选择器（逗号隔开）**，同时匹配所有E元素或F元素，E和F之间用逗号分隔

E F
**后代元素选择器（空格隔开）**，匹配所有属于E元素后代的F元素，E和F之间用空格分隔

E > F
子元素选择器，匹配所有E元素的子元素F

前面两个不要搞混了。

### :nth-child(n) 选择器，选择特定序号的字元素
选择父元素的第n个子元素，前面一般只能加标签名（如：p,div等）

```
p:nth-child(2){

}
```

**使用公式 (an + b)，选定特定序号的一系列子元素，a表示周期的长度，n 是计数器（从 0 开始），b 是偏移值**。

如：指定下标是 3 的倍数的所有 p 元素的背景色：



```
p:nth-child(3n+0)
{

}
```




## 二 选择器优先级
### 要点
CSS选择器权重：

通配选择符（*） 0,0,0,0
标签/伪元素选择器 0,0,0,1
类/属性/伪类选择器 0,0,1,0
ID选择器 0,1,0,0
important权值最高 1,0,0,0

**规则：选择器的权值加到一起，大的优先；如权值相同，后定义的优先 。**



参考：
CSS选择器、优先级与匹配原理
http://www.cnblogs.com/aaronjs/p/3156809.html

## 三 CSS选择器执行效率
如果非常在意页面性能那千万别用CSS3新增的选择器。在所有浏览器中，用 class 和 id 来渲染，比那些使用同胞，后代选择器，子选择器（sibling, descendant and child selectors）对页面性能的改善更值得关注。

### CSS选择器的效率从高到低排序：

1.id选择器（#myid）2.类选择器（.myclassname）3.标签选择器（div,h1,p）4.相邻选择器（h1+p）5.子选择器（ul < li）6.后代选择器（li a）7.通配符选择器（*）8.属性选择器（a[rel="external"]）9.伪类选择器（a:hover,li:nth-child）

上面九种选择器中**ID选择器的效率是最高，伪类选择器的效率最低**

详细参考：

CSS 优化、提高性能的方法有哪些？ -知乎
https://www.zhihu.com/question/19886806

## 参考

### 已学习
CSS选择器笔记-阮一峰
http://www.ruanyifeng.com/blog/2009/03/css_selectors.html

CSS选择器、优先级与匹配原理
http://www.cnblogs.com/aaronjs/p/3156809.html

### 待学习