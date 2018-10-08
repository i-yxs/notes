* #### Date对象在Safari浏览器不支持 `new Date('1970/08/06')` 方式创建Date对象
> ##### 创建：2018/09/13 更新：2018/09/13
```javascript
function getDate() {
    var list;
    if (arguments.length > 1) {
        list = Array.prototype.slice.call(arguments);
    } else {
        list = arguments[0].split(/[- : \/]/);
        //因为使用单个参数创建Date对象时，月份不会自动减一
        if (list.length >= 2) {
            list[1] -= 1;
        }
    }
    return eval('new Date(' + list.join(',') + ')');
}

getDate('1970/08/06')
getDate(1970,8,6)
```

* #### 模拟ios滚动条滑到顶部或底部时，继续滑动会出现类似阻力的效果，越是继续滑动阻力越大
> ##### 创建：2018/09/13
```html
可以使用指数衰减算法    y=-100(1-e^(-a*L))

e^: 指数函数 Math.exp
a:  衰减率，范围在0-1之间
L:  超出的距离/最大超出距离
```

* #### echarts Line图表获取鼠标移动的时候，当前鼠标所处位置的数据
> ##### 创建：2018/09/13
```javascript
var chart = echarts.init('');
chart.getZr().on('mousemove', function (params) {
    params.event.preventDefault();
    var pointInPixel = [params.offsetX, params.offsetY];
    if (chart.containPixel('grid', pointInPixel)) {
        var index = chart.convertFromPixel({ seriesIndex: 0 }, [params.offsetX, params.offsetY])[0];
        //index为鼠标所处位置的数据下标
        if (index >= 0 && index < data.length - 1) {
            //当移动到边缘时，index可能会超出范围，这里判断一下，data为图表数据
        }
    }
});
```

* #### 让IE浏览器支持document.currentScript.src
> ##### 创建：2018/09/14
```javascript
if (document.currentScript === undefined) {
    document.currentScript = (function () {
        var src = '';
        var stack;
        try {
            //强制报错,以便捕获e.stack
            a.b.c();
        } catch (e) {
            //safari的错误对象只有line,sourceId,sourceURL
            stack = e.stack;
            if (!stack && window.opera) {
                //opera 9没有e.stack,但有e.Backtrace,但不能直接取得,需要对e对象转字符串进行抽取
                stack = (String(e).match(/of linked script \S+/g) || []).join(' ');
            }
        }
        if (stack) {
            //取得最后一行,最后一个空格或@之后的部分
            stack = stack.split(/[@ ]/g).pop();
            stack = stack[0] == '(' ? stack.slice(1, -1) : stack;
            //去掉行号与或许存在的出错字符起始位置
            src = stack.replace(/(:\d+)?:\d+$/i, '');
        } else {
            var nodes = document.getElementsByTagName('script');
            for (var i = 0, node; node = nodes[i++];) {
                if (node.readyState === 'interactive') {
                    src = node.src;
                    break;
                }
            }
        }
        return { src: src };
    })();
}
```

* #### axios发送FormData数据不成功，打开Network发现发送的参数为一个对象字符串，检查main.js是否配置了transformRequest
> ##### 创建：2018/10/08
```javascript
$axios.defaults.transformRequest = [function (data) {
  var ret = ''
  for (var it in data) {
    ret += ret ? '&' : '';
    ret += it + '=' + data[it];
  }
  return ret
}];

//将上面的代码替换为

$axios.defaults.transformRequest = [function (data) {
  if (Object.prototype.toString.call(data) === '[object Object]') {
    var ret = ''
    for (var it in data) {
      ret += ret ? '&' : '';
      ret += it + '=' + data[it];
    }
    return ret
  }
  return data;
}];
```

