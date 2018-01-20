## 搭建服务中心网站的demo

1.&nbsp;&nbsp;css-位置关系absolute和relative

2.&nbsp;&nbsp;css-loading动画

3.&nbsp;&nbsp;`<!DOCTYPE html>`必须添加,且在第一行

规范的html标准文档类型标识,解决浏览器之间一部分兼容问题,以及样式奇奇怪怪的bug,

比如bootstrap-table工具栏图标不一样高的问题

4.&nbsp;&nbsp;从整体上考虑界面设计,比如说从最开始就想到这是一个要支持手机端查看的web项目

	@media screen and (max-width: 980px){
		
	}

5.&nbsp;&nbsp;在只使用spring框架且没有特别设置初始欢迎页的情况下,

所有的非合法登录用户请求都会被发送到filter过滤,

然后非法请求都会回到登录页localhost:8080,

代码如下

	if (!"/".equals(requestURI) && !("/user/login").equals(requestURI) && !requestURI.endsWith("css")
	                && !requestURI.endsWith("js") && !requestURI.endsWith("svg")) {
	    //取得session. 如果没有session则自动会创建一个, 我们用false表示没有取得到session则设置为session为空.
		HttpSession session = req.getSession(false);
	    //如果session中没有任何东西.
		if (session == null || session.getAttribute("user") == null) {
	    	res.sendRedirect(req.getContextPath() + "/");
	        //返回
	        return;
	    }	            
	}

这就导致了,加载css,js,png,svg等的时候,如果使用url的方式加载,都将回到登录页.

最开始没有意识到这个问题完全是因为,以前一直使用Struts配合spring,

跳转的逻辑关系都在struts被代理,全然经验主义错误,     
   
于是最初以为是使用了缓存才出现这个问题,后来怀疑是css中`background-image: url(../img/xx.svg)`

的写法有问题,完全是弯路;最后在浏览器中对比初次登录和登录后再登录的背景图片路径,

发现初次加载的`../img/xx.svg`被重定向到`localhost:8080`;

……

总之非常坑了（挥手

6.&nbsp;&nbsp;在研究5的时候发现浏览器的缓存读取分为disk和memory,有待学习...

7.&nbsp;&nbsp;jsp中嵌入的java代码虽然不需要重启tomcat就可以在页面上进行刷新变更,但是该部分代码依然属于后端,

意思就是,不能像js一样,写函数方法,通过html元素去调用,函数的结果在html跑起来后已经变成死的,不会有反应的,

更具体深入的介绍还有待学习...

8.&nbsp;&nbsp;一些代码习惯并没有变成下意识,还需要有意识,甚至被提醒才去修改,

比如说抽取相同部分代码为一个函数,减少冗余;

9.&nbsp;&nbsp;hql、sql、socket、数据结构、各种算法烂得跟（……&*%&￥……#一样,那些年挂过的科,逃过的课,一点一点,都会在工作的坑里补回来的.

比如培训的时候讲的分页,讲的缓存,讲的聊天,怎么样,zz了吧,想不到吧.

10.&nbsp;&nbsp;公司产品的熟悉度远远不够...硬件知识基本归零