# PHP轮询（Event Loop）

PHP代码是由上往下执行, 很多时候php都是在等获取完数据。比如 执行过程中我们可能要等获取完远程的数据，又或者执行完一个复杂的sql查询，反正都是在等。**难道就不能在程序等待的时候干点别的事情吗？**

如果你有写过JS，你可能会想到回调和DOM事件。另外还可能想到php中也有回调，但处理回调的方式可能不大一样。下面，我们将来讨论下event loop如何工作的，还有怎么在PHP中使用event loop。


一、什么是轮询（Event Loop）
----------------------------
首先，扫下盲，轮询也称为事件循环，具体解释可以猛击[这里](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)


为了更好去理解轮询，我们来看下在浏览器中怎么使用js代码实现的：

```javascript
<script type="text/javascript">
    setTimeout(function() {
        console.log("inside the timeout");
    }, 1);

    console.log("outside the timeout");
</script>
```

chrome浏览器控制器中，我们可以看到先打印`outside the timeout`，然后打印`inside the timeout`。

然后, 我们勉强使用php来模拟js中的setTimeout，其代码如下：

```php
<?php
function setTimeout(callable $callback, $delay) {
    $now = microtime(true);

    while (true) {
        if (microtime(true) - $now > $delay) {
            $callback();
            return;
        }
    }
}

setTimeout(function() {
    print "inside the timeout";
}, 1);

print "outside the timeout";
?>
```

显然，php中的输出顺序是：
- `inside the timeout`
- `outside the timeout`


二、PHP中的轮询
---------------

在PHP中，我们可以使用类库Icicle来实现，代码如：

```php
use Icicle\Loop;

Loop\timer(0.1, function() {
    print "inside timer";
});

print "outside timer";

Loop\run();
```

或者使用类库React来实现：

```php
$loop = React\EventLoop\Factory::create();

$loop->addTimer(0.1, function () {
    print "inside timer";
});

print "outside timer";

$loop->run();
```

> 友情提醒：以上需要php版本v5.5及以上，还要安装composer



参考资料
---------

- [javascript事件轮询(event loop)详解](http://chen498402552-163-com.iteye.com/blog/1997633)
- [理解Node.js的事件循环（Event Loop）和线程池](http://ourjs.com/detail/54c480c4232227083e000016)
- [PHP轮询类库：reactphp](https://github.com/reactphp/event-loop)
- [PHP轮询类库：icicle.io](https://github.com/icicleio)
