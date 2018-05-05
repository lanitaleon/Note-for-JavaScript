# BOM

BOM的核心对象是window，表示浏览器的一个实例；

是ECMAScript规定的Global对象，是js访问浏览器窗口的接口；

## 全局作用域

定义全局变量与在window对象上直接定义属性还是有一点差别：

全局变量不能通过delete操作符删除，而直接在window对象上的定义的属性可以；

<pre>
<code>
var age = 29;
window.color = "red";
//在IE < 9 时抛出错误，在其他所有浏览器中都返回false
delete window.age;
//在IE < 9 时抛出错误，在其他所有浏览器中都返回true
delete window.color; //returns true
alert(window.age); //29
alert(window.color); //undefined
</code>
</pre>

使用var语句添加的window属性有一个名为[[Configurable]]的特性;

这个特性的值被设置为false,因此这样定义的属性不可以通过delete操作符删除

尝试访问未声明的变量会抛出错误，但是通过查询window对象;

可以知道某个可能未声明的变量是否存在;

<pre>
<code>
//这里会抛出错误，因为oldValue 未定义
var newValue = oldValue;
//这里不会抛出错误，因为这是一次属性查询
//newValue 的值是undefined
var newValue = window.oldValue;
</code>
</pre>

## 窗口关系及框架

每个框架都有自己的window对象，window对象的name属性是框架的名称；

也可以通过数值索引（从0开始，从左到右从上到下）在frames集合中访问相应的window对象；

<pre>
<code>
<html>
<head>
    <title>Frameset Example</title>
</head>
<frameset rows="160,*">
    <frame src="frame.htm" name="topFrame">
    <frameset cols="50%,50%">
        <frame src="anotherframe.htm" name="leftFrame">
        <frame src="yetanotherframe.htm" name="rightFrame">
    </frameset>
</frameset>
</html>
</code>
</pre>

可以通过window.frames[0]或者window.frames["topFrame"]来引用上方的框架;

最好使用top来引用这些框架top.frames[0];

然而frameset好像是deprecated html tag……emmm

与top相对的另一个window对象是parent;

parent（父）对象始终指向当前框架的直接上层框架;

在某些情况下，parent有可能等于top;

但在没有框架的情况下，parent一定等于top（此时它们都等于window;

<pre>
<code>
<html>
<head>
    <title>Frameset Example</title>
</head>
<frameset rows="100,*">
    <frame src="frame.htm" name="topFrame">
    <frameset cols="50%,50%">
        <frame src="anotherframe.htm" name="leftFrame">
        <frame src="anotherframeset.htm" name="rightFrame">
    </frameset>
</frameset>
</html>


<html>
<head>
    <title>Frameset Example</title>
</head>
<frameset cols="50%,50%">
    <frame src="red.htm" name="redFrame">
    <frame src="blue.htm" name="blueFrame">
</frameset>
</html>
</code>
</pre>


除非最高层窗口是通过window.open()打开的，否则其window对象的name属性不会包含任何值;

self对象始终指向window；实际上，self和window对象可以互换使用;

引入self对象的目的只是为了与top和parent对象对应起来，因此它不格外包含其他值;

可以window.parent,window.top，也可以window.parent.parent.frames[0]；

在使用框架的情况下，浏览器中会存在多个Global对象；

在每个框架中定义的全局变量会自动成为框架中window对象的属性；

由于每个window对象都包含原生类型的构造函数，因此每个框架都有一套自己的构造函数；

这些构造函数一一对应，但并不相等；

例如，top.Object并不等于top.frames[0].Object；

这个问题会影响到对跨框架传递的对象使用instanceof操作符

## 窗口位置

跨浏览器获取窗口左边和上边的位置；

<pre>
<code>
var leftPos = (typeof window.screenLeft == "number") ?
    window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ?
    window.screenTop : window.screenY;
</code>
</pre>

如果是双显示器,可能会显示成负数，比如我的left:-1920；

因为各个浏览器对此的实现不统一，跨浏览器的条件下无法取得窗口左边和上班的精确坐标值；

但是，使用moveTo()和moveBy()方法有可能将窗口精确移动到一个新位置；

<pre>
<code>
//将窗口移动到屏幕左上角
window.moveTo(0,0);
//将窗向下移动100 像素
window.moveBy(0,100);
//将窗口移动到(200,300)
window.moveTo(200,300);
//将窗口向左移动50 像素
window.moveBy(-50,0);
</code>
</pre>

但是这两个方法可能会被浏览器禁用；

在Opera 和IE 7（及更高版本）中默认就是禁用的；

这两个方法都不适用于框架，只能对最外层的window 对象使用；


## 窗口大小

因为各个浏览器的实现不统一，无法确定浏览器窗口本身的大小，但是可以获得页面视口的大小；

<pre>
<code>
var pageWidth = window.innerWidth,
    pageHeight = window.innerHeight;
if (typeof pageWidth != "number"){
    if (document.compatMode == "CSS1Compat"){
        pageWidth = document.documentElement.clientWidth;
        pageHeight = document.documentElement.clientHeight;
    } else {
        pageWidth = document.body.clientWidth;
        pageHeight = document.body.clientHeight;
    }
}
</code>
</pre>

document.compatMode可以确定页面是否处于标准模式；

移动设备中，window.innerWidth 和window.innerHeight保存着可见视口，也就是屏幕上可
见页面区域的大小;

移动IE通过document.documentElement.client-Width 和document.documentElement.clientHeihgt 提供了相同的信息；

在其他移动浏览器中，document.documentElement度量的是布局视口；

移动IE浏览器通过document.body.clientWidth 和document.body.clientHeight提供了相同的信息；

IE就是ze样不一样的烟火了；

resizeTo()和resizeBy()方法可以调整浏览器窗口的大小；

<pre>
<code>
//调整到100×100
window.resizeTo(100, 100);
//调整到200×150
window.resizeBy(100, 50);
//调整到 300×300
window.resizeTo(300, 300);
</code>
</pre>

resizeTo()接收浏览器窗口的新宽度和新高度;

resizeBy()接收新窗口与原窗口的宽度和高度之差;

但是这两个方法可能会被浏览器禁用；

在Opera 和IE 7（及更高版本）中默认就是禁用的；

这两个方法都不适用于框架，只能对最外层的window 对象使用；

是不是突然眼熟(

## 导航和打开窗口

window.open()接收4个参数:

* 要加载的URL,
* 窗口目标,
* 一个特性字符串,
* 一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值;

通常只须传递第一个参数，最后一个参数只在不打开新窗口的情况下使用;

<pre>
<code>
//等同于<a href="http://www.wrox.com" target="topFrame"></a>
window.open("http://www.wrox.com/", "topFrame");
</code>
</pre>

如果给window.open()传递的第二个参数并不是一个已经存在的窗口或框架;

那么该方法就会根据在第三个参数位置上传入的字符串创建一个新窗口或新标签页;

如果没有传入第三个参数，那么就会打开一个带有全部默认设置（工具栏、地址栏和状态栏等）的新浏览器窗口或者打开一个新标签页——根据浏览器设置;

在不打开新窗口的情况下，会忽略第三个参数;

第三个参数是一个逗号分隔的设置字符串，表示在新窗口中都显示哪些特性;

> 整个特性字符串中不允许出现空格

<pre>
<code>
window.open("http://www.wrox.com/", "wroxWindow",
    "height=400,width=400,top=10,left=10,resizable=yes");
</code>
</pre>

