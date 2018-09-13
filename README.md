### 我的笔记
把我遇到的问题整理成笔记

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
> ##### 创建：2018/09/13 更新：2018/09/13
```html
可以使用指数衰减算法    y=-100(1-e^(-a*L))

e^: 指数函数 Math.exp
a:  衰减率，范围在0-1之间
L:  超出的距离/最大超出距离
```

* #### echarts Line图表获取鼠标移动的时候，当前鼠标所处位置的数据
> ##### 创建：2018/09/13 更新：2018/09/13
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

