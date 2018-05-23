# 8.2 location对象

location提供与当前窗口加载的文档有关的信息&一些导航功能；

window.location和document.location引用的是用一个对象；

location将url解析为独立的片段:

<table>
    <tr>
        <th>属性名</th>
        <th>例子</th>
        <th>说明</th>
    </tr>
    <tr>
        <td>hash</td>
        <td>"#contents"</td>
        <td>返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串</td>
    </tr>
    <tr>
        <td>host</td>
        <td>"www.wrox.com:80"</td>
        <td>返回服务器名称和端口号（如果有）</td>
    </tr>
    <tr>
        <td>hostname</td>
        <td>"www.wrox.com"</td>
        <td>返回不带端口号的服务器名称</td>
    </tr>
    <tr>
        <td>href</td>
        <td>"http:/www.wrox.com"</td>
        <td>返回当前加载页面的完整URL,而location对象的toString()方法也返回这个值</td>
    </tr>
    <tr>
        <td>pathname</td>
        <td>"/WileyCDA/"</td>
        <td>返回URL中的目录和（或）文件名</td>
    </tr>
    <tr>
        <td>port</td>
        <td>"8080"</td>
        <td>返回URL中指定的端口号,如果URL中不包含端口号，则这个属性返回空字符串</td>
    </tr>
    <tr>
        <td>protocol</td>
        <td>"http:"</td>
        <td>返回页面使用的协议。通常是http:或https:</td>
    </tr>
    <tr>
        <td>search</td>
        <td>"?q=javascript"</td>
        <td>返回URL的查询字符串。这个字符串以问号开头</td>
    </tr>
</table>




  
  
  
  
  
  
  
  
  