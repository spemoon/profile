---
title: IE与FF脚本兼容性问题
category:
- javascript
layout: blog-post
---

###getYear()方法###

{% highlight lua linenos %}
var year = new Date().getYear();
document.write(year);
{% endhighlight %}

在IE中得到的日期是"2010"，在Firefox中看到的日期是"110"，主要是因为在 Firefox 里面 getYear 返回的是 "当前年份-1900" 的值。</p>

#####【兼容处理】#####

加上对年份的判断，如：

{% highlight lua linenos %}
var year= new Date().getYear();
year = (year&lt;1900?(1900+year):year);
document.write(year);
{% endhighlight %}

也可以通过 getFullYear getUTCFullYear 去调用:

{% highlight lua linenos %}
var year = new Date().getFullYear();
document.write(year);
{% endhighlight %}

###eval()函数###

在IE中，可以使用eval("idName")或getElementById("idName")来取得id为idName的HTML对象；Firefox下只能使用getElementById("idName")来取得id为idName的HTML对象。

#####【兼容处理】#####

统一用getElementById("idName")来取得id为idName的HTML对象。

###const声明###

在 IE 中不能使用 const 关键字。如：

{% highlight lua %}
const constVar = 32;
{% endhighlight %}

在IE中这是语法错误。

#####【兼容处理】#####

不使用 const ，以 var 代替。

###var###

{% highlight lua linenos %}
echo=function(str){
   document.write(str);
}
{% endhighlight %}

这个函数在IE上运行正常，Firefox下却报错了。

#####【兼容处理】#####

而在echo前加上var就正常了，这个就是我们提到var的目的。

###CSS的"float"属性###

Javascript访问一个给定CSS 值的最基本句法是：object.style.property，但部分CSS属性跟Javascript中的保留字命名相同，如"float"，"for"，"class"等，不同浏览器写法不同。

在IE中这样写：

{% highlight lua linenos %}
document.getElementById("header").style.styleFloat = "left";
{% endhighlight %}

在FF中这样写：

{% highlight lua linenos %}
document.getElementById("header").style.cssFloat = "left";
{% endhighlight %}

#####【兼容处理】#####

{% highlight lua linenos %}
if(document.all){
　　document.getElementById("header").style.styleFloat = "left";
}
else{
　　document.getElementById("header").style.cssFloat = "left";
}
{% endhighlight %}

###访问&lt;label&gt;标签中的"for"###

和"float"属性一样，同样需要使用不现的句法区分来访问&lt;label&gt;标签中的"for"。

在IE中这样写：

{% highlight lua linenos %}
var myObject = document.getElementById("myLabel");
var myAttribute = myObject.getAttribute("htmlFor");
{% endhighlight %}

在FF中这样写：

{% highlight lua linenos %}
var myObject = document.getElementById("myLabel");
var myAttribute = myObject.getAttribute("for");
{% endhighlight %}

#####【兼容处理】#####

解决的方法也是先 判断浏览器类型。

###访问和设置class属性###

同样由于class是Javascript保留字的原因，这两种浏览器使用不同的 JavaScript 方法来获取这个属性。

IE8.0之前的所有IE版本的写法：

{% highlight lua linenos %}
var myObject = document.getElementById("header");
var myAttribute = myObject.getAttribute("className");
{% endhighlight %}

另外，在使用setAttribute()设置Class属性的时候，两种浏览器也存在同样的差异。

{% highlight lua %}
setAttribute("className",value);
{% endhighlight %}

这种写法适用于IE8.0之前的所有IE版本，注意：IE8.0也不支持"className"属性了。

{% highlight lua %}
setAttribute("class",value);适用于IE8.0 以及 firefox。
{% endhighlight %}

#####【兼容处理】#####

方法一，两种都写上：

{% highlight lua linenos %}
var myObject = document.getElementById("header");
myObject.setAttribute("class","classValue");
myObject.setAttribute("className","classValue");
 //设置header的class为classValue
{% endhighlight %}

方法二，IE和FF都支持object.className，所以可以这样写：

{% highlight lua linenos %}
var myObject = document.getElementById("header");
myObject.className="classValue";//设置header的class为classValue
{% endhighlight %}

方法三，先判断浏览器类型，再根据浏览器类型采用对应的写法。

###对象宽高赋值问题###

FireFox中类似 obj.style.height = imgObj.height 的语句无效。

#####【兼容处理】#####

统一使用 obj.style.height = imgObj.height + 'px';

###getElementById###

{% highlight lua %}
&lt;input id="id" type="button" value="click me" onclick="alert(id.value)"/&gt;
{% endhighlight %}

在Firefox中，按钮没反应，在IE中，就可以，因为对于IE来说，一个HTML 元素的 ID 可以直接在脚本中当作变量名来使用，而Firefox中不可以。

#####【兼容处理】#####

尽量采用W3C DOM 的写法，访问对象的时候，用document.getElementById("id") 以ID来访问对象，且一个ID在页面中必须是唯一的，同样在以标签名来访问对象的时候，用document.getElementsByTagName("div")[0] 。该方式得到较多浏览器的支持。

{% highlight lua %}
&lt;input id="id" type="button" value="click me" onclick="alert(document.getElementById('id').value)" /&gt;
{% endhighlight %}

###集合类对象访问###

IE下，可以使用()或[]获取集合类对象；Firefox下，只能使用[]获取集合类对象。如：

{% highlight lua linenos %}
document.write(document.forms("formName").src);
//该写法在IE下能访问到Form对象的scrc属性
{% endhighlight %}

#####【兼容处理】#####

统一使用[]获取集合类对象。

###frame的引用###

IE可以通过id或者name访问这个frame对应的window对象，而Firefox只可以通过name来访问这个frame对应的window对象。

#####【兼容处理】#####

使用frame的name来访问frame对象，另外，在IE和Firefox中都可以使用window.document.getElementById(”frameId”)来访问这个frame对象。

###parentElement###

IE中支持使用parentElement和parentNode获取父节点。而Firefox只可以使用parentNode。

#####【兼容处理】#####

因为firefox与IE都支持DOM，因此统一使用parentNode来访问父节点。

###table操作###

IE下table中无论是用innerHTML还是appendChild插入&lt;tr&gt;都没有效果，而其他浏览器却显示正常。

#####【兼容处理】#####

解决的方法是，将&lt;tr&gt;加到table的&lt;tbody&gt;元素中，如下面所示：

{% highlight lua linenos %}
var row = document.createElement("tr");
var cell = document.createElement("td");
var cell_text = document.createTextNode("插入的内容");
cell.appendChild(cell_text);
row.appendChild(cell);
document.getElementsByTagName("tbody")[0].appendChild(row);
{% endhighlight %}

###移除节点removeNode()和removeChild()###

appendNode在IE和Firefox下都能正常使用，但是removeNode只能在IE下用。
removeNode方法的功能是删除一个节点，语法为node.removeNode（false）或者node.removeNode（true），返回值是被删除的节点。
removeNode（false）表示仅仅删除指定节点，然后这个节点的原孩子节点提升为原双亲节点的孩子节点。
removeNode（true）表示删除指定节点及其所有下属节点。被删除的节点成为了孤立节点，不再具有有孩子节点和双亲节点。

#####【兼容处理】#####

Firefox中节点没有removeNode方法，只能用removeChild方法代替，先回到父节点，在从父节点上移除要移除的节点。

{% highlight lua %}
node.parentNode.removeChild(node); 
// 为了在ie和firefox下都能正常使用，取上一层的父结点，然后remove。
{% endhighlight %}

###childNodes获取的节点###

{% highlight lua linenos %}
&lt;ul id="main"&gt; 
  &lt;li&gt;1&lt;/li&gt;
  &lt;li&gt;2&lt;/li&gt;
  &lt;li&gt;3&lt;/li&gt;
&lt;/ul&gt; 
&lt;input type=button value="click me!" onclick="alert(document.getElementById('main').childNodes.length)"&gt;
{% endhighlight %}

分别用IE和Firefox运行，IE的结果是3，而Firefox则是7。Firefox使用DOM规范，"#text"表示文本（实际是无意义的空格和换行等）在Firefox里也会被解析成一个节点，在IE里只有有实际意义的文本才会解析成"#text"。

#####【兼容处理】#####

方法一，获取子节点时，可以通过node.getElementsByTagName()来回避这个问题。但是 getElementsByTagName对复杂的DOM结构遍历明显不如用childNodes，因为childNodes能更好的处理DOM的层次结构。

方法二，在实际运用中，Firefox在遍历子节点时，不妨在for循环里加上：

{% highlight lua %}
if(childNode.nodeName=="#text") continue;//或者使用nodeType == 1。
{% endhighlight %}

###Firefox不能对innerText支持###

Firefox不支持innerText，它支持textContent来实现innerText，不过textContent没有像innerText一样考虑元素的display方式，所以不完全与IE兼容。如果不用textContent，字符串里面不包含HTML代码也可以用innerHTML代替。

#####【兼容处理】#####

{% highlight lua linenos %}
if(document.all){
    document.getElementById('element').innerText = "my text";
} else{
    document.getElementById('element').textContent = "my text";
}
{% endhighlight %}


