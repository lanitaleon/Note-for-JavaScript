<blockquote>JS开头的md文件是阅读JavaScript高级程序设计(第三版)的手记,因为看前面内容的时候忘记了这个手记,所以从6.2开始写,期间会夹杂日常所得,在 **其他** 的标题下</blockquote>

## 其他

#### 查 [filter](https://juejin.im/post/5a5f3eaf518825733201a6a7) 的时候看到关于去重的三种方式

    var arrS = [1, 2, 3, 4, 5, 5, 5, 8, 8, 2, 3, 9, 4, 1, 2, 'a', 'a', 'c', 'd'];
    var resF = arrS.filter(function (item, idx, arr) {
        return arr.indexOf(item) === idx;
    });
    //这个去重明显是依赖indexOf,跟filter关系不大吧
    console.log(resF);
    //数组[]数字还是数字,字符串还是字符串
    var arrSet = new Set(arrS);
    console.log(arrSet);
    //set{}数字还是数字,字符串还是字符串
    var obj = {};
    arrS.forEach(function (item) { obj[item] = item });
    // keys 返回所有可枚举属性的字符串数组
    console.log(Object.keys(obj));
    //数组[]全部是字符串

filter结合map做数据筛选的方式,评论提到性能问题,有些认同,但没有测试

#### 一个很*&&……%￥的面试题

[can (a==1 && a==2 && a==3) ever evaluate to true](https://stackoverflow.com/questions/48270127/can-a-1-a-2-a-3-ever-evaluate-to-true)

有意思的是

    a = [1,2,3];
    a.join = a.shift;
    console.log(a == 1 && a == 2 && a == 3);

最高赞同解

    const a = {
      i: 1,
      toString: function () {
        return a.i++;
      }
    }

    if(a == 1 && a == 2 && a == 3) {
      console.log('Hello World!');
    }

这里`toString`等同于`valueOf`

由此看到[宽松相等](https://www.zhihu.com/question/46943112/answer/122096589)

    [] == ![]

解释如下:
1. 等号右边有!,优先级比==高,先计算右边结果;
2. []为非假值,false;
3. ==任意一边有boolean,,将其转为number,右边变成0;
4. ==两边为object和boolean,将object转number

    Number([].valueOf()) ==> 0

至此,等式成立.


# 第六章 面向对象的程序设计

## 工厂模式
