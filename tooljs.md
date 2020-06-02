### 1. 获取滚动条距离的封装方法

```javascript
function getScrollOffset(){
    if(window.pageXOffset){
        return {
            x: window.pageXOffset,
            y: window.pageYOffset
        } 
    }else{
        return {
            x:document.body.scrollLeft + document.documentElement.scrollLeft,
            y:document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

![image-20200411213257100](D:/Typora/typora-user-images/tooljs/image-20200411213257100.png)

}
        }

        return _getElementPosition(father)
    }
    return _getElementPosition(element)

}
4.获取样式
￼
function getStyle(obj,name){
    if (obj.currentStyle) {
        return obj.currentStyle[name];
    }else{
        return getComputedStyle(obj,false)[name];
    }
}
事件类
封装兼容性事件处理方法
￼
function addEvent(ele,type,handle){
    if (ele.addEventListener) {
        ele.addEventListener(type,handle,false)
    }else if(ele.attachEvent){
        ele.attachEvent('on'+type,function(){
            handle.call(ele)
        })
    }else{
        ele['on'+type] = handle;
    }
}
封装阻止事件冒泡函数
￼
function cancelBubble(event){
    if(event.stopPropagation){
        event.stopPropagation()
    }else{
        event.cancelBubble = true;
    }
}
封装阻止默认事件的函数
￼
function cancelHandler(event){
    if(event.preventDefault){
       event.preventDefault();
    }else{
        event.returnValue = false;
    }
    //return false封装不进去，因为对于函数来说这是结束函数返回false
}
xxx
￼

xxx

xxx

xxx

xxx

xxx

xxx

xxx

xxx

xxx

xxx

xxx

### 2. 封装可视区窗口方法

```java
function getViewportOffset(){
    if(window.innerWidth){
        return {
            x: window.innerWidth,
            y: window.innerHeight
        }
    }else if(document.compatMode === 'BackCompat'){
        return {
            x:document.body.clientWidth,
            y:document.body.clientHeight
        }
    }else{
        return {
            x: document.documentElement.clientWidth,
            y: document.documentElement.clientHeight
        }
    }
}
```



### 3. 封装元素相对于文档的坐标

```javascript
function getElementPosition(element){
    var target = element
    function _getElementPosition(element){
        if(element.parentNode.nodeName== 'HTML') return{
            left: target.offsetLeft,
            top:  target.offsetTop
        };
        var father = element.parentElement || element.parentNode;
        var fatherPos = window.getComputedStyle(father,null).getPropertyValue('position');
        if(father.nodeName == 'DIV'){
            if(fatherPos !== 'static'){
                return{
                    left: target.offsetLeft,
                    top:  target.offsetTop
                }
            }
        }

        return _getElementPosition(father)
    }
    return _getElementPosition(element)

}
```



### 4.获取样式

```javascript
function getStyle(obj,name){
    if (obj.currentStyle) {
        return obj.currentStyle[name];
    }else{
        return getComputedStyle(obj,false)[name];
    }
}
```



## 事件类

### 封装兼容性事件处理方法

```javascript
function addEvent(ele,type,handle){
    if (ele.addEventListener) {
        ele.addEventListener(type,handle,false)
    }else if(ele.attachEvent){
        ele.attachEvent('on'+type,function(){
            handle.call(ele)
        })
    }else{
        ele['on'+type] = handle;
    }
}
```

### 封装阻止事件冒泡函数

```javascript
function cancelBubble(event){
    if(event.stopPropagation){
        event.stopPropagation()
    }else{
        event.cancelBubble = true;
    }
}
```

### 封装阻止默认事件的函数

```javascript
function cancelHandler(event){
    if(event.preventDefault){
       event.preventDefault();
    }else{
        event.returnValue = false;
    }
    //return false封装不进去，因为对于函数来说这是结束函数返回false
}
```

### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

## Date类

### 格式化一个年月日

```javascript
//方法一：
function getDateString(data){
    var year = data.getFullYear().toString().padStart(4,"0")
    var month = data.getMonth().toString().padStart(2,"0")
    var day = data.getDate().toString().padStart(2,"0")
    var hour = data.getHours().toString().padStart(2,"0")
    var minutes = data.getMinutes().toString().padStart(2,"0")
    var second = data.getSeconds().toString().padStart(2,"0")
    return `${year}-${month}-${day} ${hour}:${minutes}:${second}`
}
console.log(getDateString(new Date()))

//方法二：
//例子：
//（new Date()）.format("yyyy-mm-dd hh:mm:ss") =>2006-07-02 08:02:08.423
//（new Date()）.format("yyyy-mm-dd h:m:s") =>2006-07-02 8:12:48.18
//substr(start,length)在字符串中抽取从 start 下标开始的指定数目的字符。
Date.prototype.format = function (format) {
    let o = {
        "M+": this.getMonth() + 1,   //月份
        "d+": this.getDate(),        //日
        "H+": this.getHours(),       //小时
        "m+": this.getMinutes(),     //分钟
        "s+": this.getSeconds(),     //秒
        "q+": Math.floor((this.getMonth) + 3 / 3),  //季度
        "f+": this.getMilliseconds() //毫秒

    }
    //对format进行一点点的替换
    if (/(y+)/.test(format)){
        format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
        console.dir(RegExp.$1);
    }
    for (let k in o){
        if (new RegExp("(" + k + ")").test(format)){
            format = format.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
        }
    }
    return format;
}


```





### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

### xxx

```javascript

```

## 原型

### 封装一个instanceof

```javascript
function_instanceof(A, B){
    var O = B.prototype;// 取B的显示原型
    A = A.__proto__;// 取A的隐式原型while (true) {
        //Object.prototype.__proto__ === nullif (A === null)
            returnfalse;
        if (O === A)// 这里重点：当 O 严格等于 A 时，返回 true;
        A = A.__proto__;
    }
}
```



## 通用插件类

### JS封装一个通用页码插件

```javascript

```

