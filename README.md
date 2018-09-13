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
