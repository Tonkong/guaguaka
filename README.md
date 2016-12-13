guaguaka插件
========

微信刮刮卡,依赖`jQuery`或`Zepot`。

*注：zepto需要引入data模块，本例中的zepto已内置*

# 一、用法实例
```html
<!-- html -->
<div id="scratchpad"></div>
```

```javascript
// js
var sp = $("#scratchpad").wScratchPad({ //执行刮奖区域方法
    width: 150, //刮奖区宽度
    height: 40,
    image2:'./images/activity-scratch-card-bg.jpg',
    color: "#a9a9a7", //刮奖区域覆盖背景颜色
    scratchDown: function() {
        //鼠标按下去时
    },
    scratchUp: function() {
        //鼠标按下松开时
    },
    scratchMove: function(e, percent) { //鼠标点击开刮
        //参数:percent 刮开的百分比,e:当前事件对象
        if (percent >= 20) {
            if (num != 1) return false; //多次刮卡提交一次请求
            if (total > 5) alert("本次活动总共可以刮5次,你已经刮了5次!");
            $.get("prize.json", {}, function(data) { //请求后台返回奖项名称
                document.getElementById("prize").innerHTML = data.obj.prize; //
                /*A 谢谢参与*/
                if (data.err != 1) {
                    return false;
                }
                /*B 中奖*/
                document.getElementById("prizename").innerHTML = data.obj.prize; //
                document.getElementById("sncode").innerHTML = data.obj.sncode;
                $("#zjl").show("fast"); //显示中奖信息 你中了XXX
                num++; //记录刮了几次,防止打开页面多次刮多次请求
                total++ //记录参与活动几次用,一共可以5次
            }, "json");
        }
    }
});
```
# 二、$('selector').wScratchPad(options)
## options
- 参数类型: [object || string [, string]]

### 参数说明--object：
```javascript
width: 210, // set width - best to match image width
height: 100, // set height - best to match image height
image: null, // set image path
image2: null, // set overlay image path - if set color is not used
color: '#336699', // set scratch color - if image2 is not set uses color
overlay: 'none', // set the type of overlay effect 'none', 'lighter' - only used with color
size: 10, // set size of scratcher
scratchDown: null, // scratchDown callback
scratchUp: null, // scratchUp callback
scratchMove: null, // scratcMove callback
cursor: null, // Set path to custom cursor
delay: 0 //重置后延迟渲染时间，可以560可以解决低端手机webview不强制渲染的bug
```

### 参数说明--string [, string]]
wScratchPad支持传入字符串参数，用来调用实例内置方法，分别是`reset`和`clear`
```javascript
sp.wScratchPad('reset'); //用来重置画布
sp.wScratchPad('clear'); //用来清除画布
```

当传入第二参数的时，则两个参数表示键值对，用来修改实例内部配置`settings`，如：
```javascript
sp.wScratchPad('enabled',false); //表示将画布enabled设置成false
```
*注：object中的参数都可以通过这种方法修改*

# 三、data(一般人我不告诉它)
实例化后，实例对象的data上存储了所用的事例方法和属性。可以通过下面方法获取这些方法和属性
```javascript
var data = $('selector').data('_wScratchPad');
```
