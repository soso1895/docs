*斜体文本*  
_斜体文本_  
**粗体文本**  
__粗体文本__  
***粗斜体文本***  
___粗斜体文本___

---
创建脚注格式类似这样 [^RUNOOB]。

[^RUNOOB]: 菜鸟教程 -- 学的不仅是技术，更是梦想！！！
---
1. 第一项
2. 第二项
3. 第三项

---

1. 第一项：
    - 第一项嵌套的第一个元素
    - 第一项嵌套的第二个元素
2. 第二项：
    - 第二项嵌套的第一个元素
    - 第二项嵌套的第二个元素
---
>区块引用
>>菜鸟教程
>>>学的不仅是技术更是梦想
---
> ## 区块中使用列表
  > 1. 第一项
  > 2. 第二项
  >> + 第一项
  >> + 第二项
  >> + 第三项 

---
* 第一项
    > 菜鸟教程
    > 学的不仅是技术更是梦想
* 第二项


---
这是一个链接 [菜鸟教程](https://www.runoob.com)

<https://www.runoob.com>

链接也可以用变量来代替，文档末尾附带变量地址：
这个链接用 1 作为网址变量 [Google][1]
这个链接用 runoob 作为网址变量 [Runoob][runoob]
然后在文档的结尾为变量赋值（网址）

  [1]: http://www.google.com/
  [runoob]: http://www.runoob.com/
---
![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png)

![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png "RUNOOB")

这个链接用 1 作为网址变量 [RUNOOB][2].
然后在文档的结尾位变量赋值（网址）

[2]: http://static.runoob.com/images/runoob-logo.png

<img src="http://static.runoob.com/images/runoob-logo.png" width="50%">

---
|  表头   | 表头  |
|  ----  | ----  |
| 单元格  | 单元格 |
| 单元格  | 单元格 |

| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |

***
## 支持的 HTML 元素
不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。

目前支持的 HTML 元素有：\<kbd> \<b> \<i> \<em> \<sup> \<sub> \<br>等 ，如：

使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑

---
## 转义
Markdown 使用了很多特殊符号来表示特定的意义，如果需要显示特定的符号则需要使用转义字符，Markdown 使用反斜杠转义特殊字符：

**文本加粗** 
\*\* 正常显示星号 \*\*

\   反斜线
\`   反引号
\*   星号
\_   下划线
{}  花括号
[]  方括号
()  小括号
\#   井字号
\+   加号
\-   减号
.   英文句点
!   感叹号

---
`create database hero;`

```
function fun(){
		echo "这是一句非常牛逼的代码";
}
fun();
```
---
## 流程图
```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
```
---
---
```
	function fun(){
			echo "这是一句非常牛逼的代码";
	}
	fun();
```