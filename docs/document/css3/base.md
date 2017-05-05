## 换行问题

1. word-break:break-all;只对英文起作用，以字母作为换行依据
2. word-wrap:break-word; 只对英文起作用，以单词作为换行依据
3. white-space:pre-wrap; 只对中文起作用，强制换行
4. white-space:nowrap; 强制不换行，都起作用
5. white-space:nowrap; overflow:hidden; text-overflow:ellipsis;不换行，超出部分隐藏且以省略号形式出现（部分浏览器支持） 

代码：
```css
.p1{ word-break:break-all; width:150px;}/*只对英文起作用，以字母作为换行依据*/
.p2{ word-wrap:break-word; width:150px;}/*--只对英文起作用，以单词作为换行依据*/
.p3{white-space:pre-wrap; width:150px;}/*只对中文起作用，强制换行*/
.p4{white-space:nowrap; width:10px;}/*强制不换行，都起作用*/
.p5{white-space:nowrap; overflow:hidden; text-overflow:ellipsis; width:100px;}／/*不换行，超出部分隐藏且以省略号形式出现*/
```

注意，一定要指定容器的宽度，不然的话是没有用的。

注意word-break 是IE5+专有属性

语法：
```css
word-break : normal | break-all | keep-all
```

参数：

normal : 　依照亚洲语言和非亚洲语言的文本规则，允许在字内换行

break-all : 　该行为与亚洲语言的normal相同。也允许非亚洲语言文本行的任意字内断开。该值适合包含一些非亚洲文本的亚洲文本

keep-all : 　与所有非亚洲语言的normal相同。对于中文，韩文，日文，不允许字断开。适合包含少量亚洲文本的非亚洲文本

说明：

设置或检索对象内文本的字内换行行为。尤其在出现多种语言时。

对于中文，应该使用break-all 。

注意，兼容火狐浏览器强制英文换行：

```html
<div style="word-wrap:break-word; white-space:normal; word-break:break-all; width:150px;">英文内容</div>
```