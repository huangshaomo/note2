CDN网上链接地址：

```html
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js">
```

# Jquery

> jquery其实就是一堆的JS函数（JS库），也是普通的js而已，并不是全新的东西。
>
> JQuery对象就是通过jQuery包装DOM对象后产生的对象。JQuery对象是jQuery独有的，其可以使用jQuery里的方法，但是不能使用DOM的方法；
>
> 就是说JQuery对象包括DOM对象

jQuery对象

![chrome_jllmM7OE3W](D:/Typora/typora-user-images/Jquery/chrome_jllmM7OE3W.png)

DOM对象

![image-20200327172757871](D:/Typora/typora-user-images/Jquery/image-20200327172757871.png)

### 为什么要用Jquery？

- jquery面向用户良好的设计使得在使用过程中彻底解放了你记忆元素操作DOM的接口
- jquery中大量可复用的函数和发展过程中常年累计下来的插件库，可以极大的简化javascript的开发
- jquery在半数以上并没有复杂交互的网站中得以大面积使用，因为他们需要的仅仅是一项兼容低级浏览器而又呈现酷炫效果动画的页面（Jquery出到3，但大公司pc端依然用1.x版本，移动端2.x版本）
- jquery改变了数百万人编写javascript的方式，当然部分人已经觉得时过境迁，组件化，工程化，大行其道，但请不要忘记他的前端开发者的启蒙意义，且很多公司很多项目已经需要他，所有面试必会

### Jquery学习注意点

- Jquery只是辅助工具，不能完全替代js，二者并存的方式出现在项目中
- Jquery很庞杂，要会使用，但应重学思想
- Jquery方法很多，按需学习，把常用的有价值得学会
- Jquery api可以现查现用

### 引入Jquery工具库

CDN：`http://www.jq22.com/cdn/#a2`

中文文档：`www.jquery123.com`



### Jquery使用-起步

$(jquery)：$   $ === jQeury

$代表jQuery对象，同时也是一个函数对象

$():全局函数（选择器），接收原生DOM等各种参数规则，返回的是一个类数组（jquery对象）实例，选择器遵循CSS选择规则，jquery所有的方法都包装在实例的原型上

```html
<div></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log($('div'));
</script>
```

![image-20200324102850473](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200324102850473.png)

#### $可接受的参数规则

- **CSS 选择器**

- **jquery 特有规则**

  - :first	选择符合规则的第一个
  - :last     选择符合规则的最后一个
  - :odd     选择奇数位的所有元素（注意从0开始）
  - :even     选择偶数位的所有元素（注意从0开始）
  - :eq(数字)   以元素的下标为索引选择（类似nth-of-type()）

  ```css
   $('.wrapper ul li:eq(0)').css({width:100, height:100, backgroundColor:"red"})
  //选择wrapper元素下的ul下的第一个li 
  ```

- **Jquery属性选择器**

  - ’元素[属性="xxx"]‘

    - 属性$：表示选择属性值以xxx为结尾的元素

    - 属性^：表示选择属性值以xxx为开头的元素

    - 属性!：表示选择属性值不为xxx的元素

      

- **null     undefined** :不会报错，提供一定程度的容错机制，返回的是空的jquery实例对象

```html
<div class="wrapper">
    <div class="demo">
        <span>绿色</span>
    </div>
    <div class="demo">
        <p>红色</p>
    </div>
    <div class="demo">
        <span>橙色</span>
    </div>

</div>
<script>
    var colorsArr = ['green','red','orange']
    $('.wrapper .demo').each(function(index,ele){
        $(ele)
            .find('span')
            .css({color:colorsArr[index]})
    })
</script>
```

![image-20200325092253261](D:/Typora/typora-user-images/Jquery/image-20200325092253261.png)

找到所有div下面的span,我们知道第二个div下面没有span,为null对象，此时当前循环不会报错，即使CSS实例方法应用不上，提供了一定程度上的容错机制。**同时也暴露出了jquery是链式调用的，调用一个方法，返回一个对象，再基于该对象调用下一个方法，再返回一个方法...**



$()选择器有一个细节,就是经过该选择器的元素会被包装为一个jQuery实例对象返回

```html
<div>demo</div>
var oDiv = document.getElementsByTagName('div');
$(oDiv).css({color:red})
```

oDiv原生节点被$(oDiv)转换成对象了



- **$(function(){})**：自执行函数，类似于JS的DOMcontentLoaded事件，执行时机是在所有标签（DOM）加载完毕过后，该函数是在DOMcontentLoaded事件触发后触发。还有，该自执行函数和\$(document).ready(function(){})执行的时机是一样的，都是在标签（DOM)加载完后执行，哪个在前哪个在后跟书写的顺序有关

  ```javascript
  window.onlaod = function(){
      console.log('window.onload');
  }
  $(function(){
      console.log('DOMContentLoaded');
  })
  $(document).ready(function(){
      console.log('DOMContentLoaded');
  })
  
  //结果
  //DOMContentLoaded
  //DOMContentLoaded
  //window.onload
  ```

- 两个参数；css 选择器，context(上下文)

  - $(‘ul', 'wrapper')：选择ul，条件是在wrapper环境下的

  

### Jquery精髓—链式操作

```javascript
<div class="demo">1</div>
<div class="demo">2</div>
<div class="demo">3</div>
<script>
    (function () {
    	function jQuery(selector) {
        	return new jQuery.prototype.init(selector);
    	}
    //构造函数init，用于产生jQuery对象，首先根据选择规则找出对应的标签元素，然后把标		签元素加入到产生的jQuery对象中
    jQuery.prototype.init = function (selector) {
        //1、查看是什么选择器  class  id undefined null
        this.length = 0
        if (selector.indexOf('.') != -1) {
            var dom = document.getElementsByClassName(selector.slice(1))
            }else if(selector.indexOf('#') != -1){
                var dom = document.getElementById(selector.slice(1))
            }
        //2、把得到的元素放入到实例化的obj对象中
        if(dom.length == undefined){
            this[0] = dom;
            this.length ++;
        }else{
            for (let i = 0; i < dom.length; i++) {
                this[i] = dom[i];
                this.length ++;
            }
        }  
    }
    
    jQuery.prototype.css = function(config){
        //遍历所选全部元素
        for (let i = 0; i < this.length; i++) {
            //对每个元素添加设置的所有样式
            for(var attr in config){
                this[i].style[attr] = config[attr];
            }    
        }
        //链式操作,返回添加样式后的jQuery对象
        return this;
    }
    
    jQuery.prototype.init.prototype = jQuery.prototype;
    //改变init.prototype的指向，将实例化的init对象的__proto__指回jQeury原型上
    //意义是让每个被返回的对象都依旧带有jquery原型上的所有实例方法。
    
    window.$ = window.jQuery = jQuery;
})();

$('.demo')
    .css({width:'100px',height:'100px', backgroundColor:'red'})
    .css({color:'orange'})
</script>
```

调用jQuery函数，返回init实例对象，产生的对象的\_\_proto\_\_指向jQuery函数的prototype，对象的下标索引对应dom元素。



## 1. jQuery实例方法-DOM操作

 ### .get(index)

> 获得由选择器指定的原生 DOM 元素。也只有该方法是唯一返回DOM元素的，其他都返回jquery对象

模拟实现：

- 如果不填数字，默认取出所有选择器的所有节点并放入到一个数组中
- 如果填写了正数，取出正数对应的下标索引元素
- 如果填写了负数，取出（负数+长度）对应的下标索引元素

```javascript
jQuery.prototype.get =  function(num){
    if(num == null){
        return [].slice.call(this,0);
    }else{
        if(num >= 0){
            return this[num]
        }else{
            return this[num + this.length]
        }
    }
}
```



例子：

```html
<div class="demo">1</div>
<div class="demo">2</div>
<div class="demo">3</div>
<script>
	console.log($('.demo').get());	//(3) [div.demo, div.demo, div.demo]
    console.log($('.demo').get(1));	//<div class=demo>2</div>
    console.log($('.demo').get(-1));//<div class=demo>3</div>
</script>
```



### .eq(index)

> eq() 方法返回带有被选元素的指定索引号的jquery对象。索引号从 0 开头，所以第一个元素的索引号是 0（不是 1）,这个方法和选择器方法的eq非常类似，只不过这个使用起来更加灵活些

```html
$('.demo':eq(0))//只能操作单个

$('.demo').css().eq(0).css//操作多个的同时还可以对另一个进行单独操作
```

模拟实现：

- 如果不填参数(num)，返回一个空的jquery对象
- 如果参数为正数，返回正数对应的下标索引的jquery对象
- 如果参数为正数，返回负数对应的下标索引的jquery对象

```javascript
jQuery.prototype.eq = function(num){
    var dom = num != null ? (num >=0 ? this[num] : this[num + this.length]):null;
    return jQuery(dom)
}
```

jQuery(dom)等价于\$(null),但此时，我们之前的selector（$的参数规则）还未添加对null、undefined等的判断，现在需要完善下selector。

```javascript
if(selector == null){
	return this
}
//这里的this指的是返回构造函数init实例化的对象	jQuery.init {length: 0}

if (typeof selector == 'string' && selector.indexOf('.') != -1) {
	var dom = document.getElementsByClassName(selector.slice(1))
}else if(typeof selector == 'string' && selector.indexOf('#') != -1){
	var dom = document.getElementById(selector.slice(1))
}
if(selector instanceof Element){
	this[0] = selector;
	this.length++;
}
```

完善一：如果selector（$的参数规则）为null，或者undefined，我们返回一个实例化的\$构造函数对象

完善二：如果selector（$的参数规则）为dom对象（\$('div')），返回

jQuery源码的思想就是，能不写重复代码就不写，因此，最终完善后的selector（$的参数规则）如下

```javascript
(function () {
    function jQuery(selector) {
        return new jQuery.prototype.init(selector)
    }
    //创建jQeury对象，首先根据选择规则找出对应的标签元素，然后把标签元素加入到产生的jQuery对象中
    jQuery.prototype.init = function(selector){
        this.length = 0;
        // null  undefined
        if(selector == null){
            return this;
        }
        if (selector.indexOf('.') != -1) {  //class
            var dom = document.getElementsByClassName(selector.slice(1))
            }else if(selector.indexOf('#') != -1){//id
                var dom = document.getElementById(selector.slice(1));
            }
        //2、把得到的元素加入到实例化的obj对象中
        if (dom.length == undefined) {
            this[0] = dom;
            this.length++
        }else{
            for(var i = 0; i < dom.length; i++){
                this[i] = dom[i]
                this.length++;
            }
        }


    }
    jQuery.prototype.init.prototype = jQuery.prototype
    window.$ = window.jQuery = jQuery
})()
```



例子：

```html
<div class="demo">1</div>
<div class="demo">2</div>
<div class="demo">3</div>
<script>
	console.log($('.demo').eq()); //jQuery.init {length: 0}
    console.log($('.demo').eq(1));//jQuery.init {0: div.demo, length: 1}
   console.log($('.demo').eq(-1));//jQuery.init {0: div.demo, length: 1}
</script>
```



### .find(filter)

> 在原有元素的基础上，往下去查找后代元素，filter的查找规则跟$一样，都支持
>
> css selector 、jquery特有规则、dom等等，返回的是jquery对象

find形式与普通形式对比

```html
<div class="wrapper">
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
</div>
<script>
	$('.wrapper')
    	.css({position:'relative'})
    		find('ul')
    			.css({listStyle:'none'})
    				find('li')
    					.css({color:'red'})
</script>

<script>
	$('.wrapper')
    	.css({position:'relative'})
    $('.wrapper ul')
    	.css({listStyle:'none'})
    $('.wrapper ul li')
    	.css({color:'red'})
</script>
```

可以发现，普通形式每次都要从头开始写起，不方便



那find是如何知道它的父元素是谁呢？其实是通过实例上的prevObject属性得到的，prevObject存放的就是当前元素的父级元素

```javascript
console.log( $('.wrapper') )
//init [div.wrapper, prevObject: init(1), context: document, selector: ".wrapper"]
console.log( $('.wrapper').find('ul') )
//init [ul, prevObject: init(1), context: document, selector: ".wrapper ul"]
console.log( $('.wrapper').find('ul').find('li') )
//init(3) [li, li, li, prevObject: init(1), context: document, selector: ".wrapper ul li"]
console.log( $('.wrapper').find('ul').find('li').preObject == console.log( $('.wrapper').find('ul') ))
//false
```

例如wrapper的父元素就是document文档

ul的父元素就是wrapper

li的父元素就是ul

![image-20200324190233354](D:/Typora/typora-user-images/Jquery/image-20200324190233354.png)

上面的之所以为false跟对象的原理一样，

![image-20200324191433655](D:/Typora/typora-user-images/Jquery/image-20200324191433655.png)



### .filter(filter)

> filter() 方法返回符合一定条件的jquery对象。
>
> 针对原来的选中的元素进行限制（过滤），不符合条件的元素将从选择中移除，符合条件的元素将被返回。filter只针对的是当前选中的元素当前代进行过滤
>
> 可以添加跟$一样的查找规则或函数。

例一：添加查找规则

```html
<div class="wrapper">
    <ul>
        <li class="demo">1</li>
        <li>2</li>
        <li class="demo">3</li>
    </ul>
    <ul>
        <li class="demo">4</li>
        <li>5</li>
        <li class="demo">6</li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.wrapper ul')
        .find('li')
        .filter('.demo')
        .css({color:'red',listStyle:'none'})
</script>
```

![image-20200324211816012](D:/Typora/typora-user-images/Jquery/image-20200324211816012.png)

上面表示拿到wrapper ul下面class为demo的元素，然后添加样式



例二：添加函数，可以更灵活的进行过滤

```javascript
$('.wrapper ul li').filter(function(index,ele){
    //return index % 2  = 0
	console.log(index,ele,this)
})
```

![filter函数结果](D:/Typora/typora-user-images/Jquery/image-20200324193410597.png)



### .not(filter)

> not()方法与filter方法完全相对。
>
> not() 方法返回不符合一定条件的元素。
>
> 针对原来的选中的元素进行限制（过滤），符合条件的元素将从选择中移除，不符合条件的元素将被返回。

```html
<div class="wrapper">
    <ul>
        <li class="demo">1</li>
        <li>2</li>
        <li class="demo">3</li>
    </ul>
    <ul>
        <li class="demo">4</li>
        <li>5</li>
        <li class="demo">6</li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.wrapper ul')
        .find('li')
        .not('.demo')
        .css({color:'red',listStyle:'none'})
</script>
```

![image-20200324211859129](D:/Typora/typora-user-images/Jquery/image-20200324211859129.png)

### .has(filter)

> 返回has前面的选择器已选中的元素中后代必须拥有has条件中的元素。
>
> 就相当于满足了前面的规则的元素，其后代也必须满足has的条件
>
> 返回拥有
>
> 如需选取拥有多个元素在其内的元素，请使用逗号分隔

例一：

```html
<ul>
    <li>
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
        </ul>
    </li>
    <li>4</li>
    <li>5</li>
</ul>
<script s rc="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('li').has('ul').css({background:'red'})
</script>
```

上面的表示选择的li标签,，后代元素必须有ul,，因此，ul下面的第一个li会被选中

![chrome_30oX6S0wiQ](D:/Typora/typora-user-images/Jquery/chrome_30oX6S0wiQ.png)



例二：

```html
<h1>欢迎来到我的主页</h1>
<p>我的<span>名字</span>叫 Donald。</p>
<p>我住在<span>Duckburg</span>。</p>
<p>我最好的朋友是<b>Mickey</b>。</p>
<p>在这个例子中,我们选择内部有span或者b元素的所有p元素。</p>
<script>
	$("p").has("b,span").css("background-color","yellow");
</script>
```

![image-20200324213843994](D:/Typora/typora-user-images/Jquery/image-20200324213843994.png)

上面表示最终选择的标签li，其后代元素必须有b或者span。

在jQeury中，逗号（，）表示或者





### .is(filter)

> 前面选出的元素和后面选出的元素是否有交集，有的话返回true,没有返回false.
>
> 意思就是is选中的元素是否存在于前面选择器选中的元素中的其中一个，如果存在返回true,不存在返回false



例一：

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('ul').click(function(e){
        if($(e.target).is('li')){
            alert($(e.target).text());
        }else{
            alert($(this).text());
        }
   })
</script>
```



例二：

```html
<!DOCTYPE html>
<html>
<head>
  <style>li { cursor:pointer; }</style>
  <script src="https://code.jquery.com/jquery-latest.js"></script>
</head>
<body>
 
<ul id="browsers">
  <li>Chrome</li>
  <li>Safari</li>
  <li>Firefox</li>
  <li>Opera</li>
</ul>
<script>
  var $alt = $("#browsers li:nth-child(2n)").css("background", "#00FFFF");
  $('li').click(function() {
    if ( $alt.is( this ) ) {
      $(this).slideUp();
    } else {
      $(this).css("background", "red");
    }
  });
</script>
 
</body>
</html>
```

![Vv63moJRWM](D:/Typora/typora-user-images/Jquery/Vv63moJRWM.gif)

给索引为偶数的元素添加浅蓝色背景，并将选中的元素返回给alt，如果点击的元素是属于alt的，那么该元素隐藏，否则变红



### .add(filter)

> 集中元素进行统一操作

例一：

```html
<ul>
  <li>list item 1</li>
  <li>list item 2</li>
  <li>list item 3</li>
</ul>
<p>a paragraph</p>
<script>
$('li').add('p').css({backgroundColor:'red'})
</script>
```

首先选中所有的li元素，然后再把p元素合并到li里，一起集中进行统一操作,最后所有的li元素和p元素都会添加一个统一的样式。

![image-20200324225710507](D:/Typora/typora-user-images/Jquery/image-20200324225710507.png)



模拟实现

```javascript
//将所有元素统一到一起
jQuery.prototype.add = function(){
    var addObj = jQuery(selector);
    var baseObj = this;
    var newObj = jQuery(null);

    for(var i = 0 ; i < addObj.length; i++){
        newObj[newObj.length++] = addObj[i];
    }
    for(var i = 0; i < baseObj.length; i++){
        newObj[newObj.length++] = this[i];
    }
    //为该jQuery对象添加prevObject
    return this.pushStack(newObj);
}
```

表示将所有的元素统一放到一个新的jQuery对象中，然后给该对象添加一个回退属性，方便后面的end方法实现。end方法就是依靠该属性进行回退的。



### .end(filter)

> 回退操作，回退到当前元素的prevObject中的jQuery对象中

例二：

```html
<p>
    <span>Hello</span>, how are you?
</p>
<script>
    $("p").find("span").end().css("border", "2px red solid");
</script>
```

![image-20200324232934030](D:/Typora/typora-user-images/Jquery/image-20200324232934030.png)

给的是p标签添加border。



模拟实现：

```javascript
jQuery.prototype.end = function(){
    return this.prevObject;
}
```





add联合end例子

```html
<ul>
    <li class="demo">list item 1</li>
    <li class="demo">list item 2</li>
    <li class="demo">list item 3</li>
</ul>
<p class="p">a paragraph</p>

<script src="./myjQuery.js"></script>
<script>
 $('.demo').add('.p').css({backgroundColor:'red'}).end().css({color:'white'})
</script>
```

![image-20200324233200140](D:/Typora/typora-user-images/Jquery/image-20200324233200140.png)

给所有的demo类和p添加统一红色背景，然后从p回退到demo类，给所有的demo类单独添加白色字体颜色



## 2.jQuery实例方法-DOM操作(取赋一体相关方法)

### .CSS(css roles)

> CSS函数可读可写，如果取的值是颜色，会返回rgb

取值赋值

```html
<div class="demo"></div>
<div class="demo"></div>
<div class="demo"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    // css赋值
    $('.demo:first').css('width','100px')
        .css({width: '+=100',height: '100',backgroundColor:'red'})
    //css获取
    console.log( $('.demo:first').css('width') );
    console.log( $('.demo:first').css('backgroundColor') );
```

![image-20200325113612186](D:/Typora/typora-user-images/Jquery/image-20200325113612186.png)



例一：点击div，得到它的背影颜色

```html
<span id="result">&nbsp;</span>
<div style="background-color:blue;"></div>
<div style="background-color:rgb(15,99,30);"></div>
 
<div style="background-color:#123456;"></div>
<div style="background-color:#f11;"></div>
<script>
$("div").click(function () {
  var color = $(this).css("background-color");
  $("#result").html("That div is <span style='color:" +
                     color + ";'>" + color + "</span>.");
});
 
</script>
```

![wIAFNSTUiv](D:/Typora/typora-user-images/Jquery/wIAFNSTUiv.gif)



例二：突出段落中点击文本

```html
<p>
  Once upon a time there was a man
  who lived in a pizza parlor. This
  man just loved pizza and ate it all
  the time.  He went on to be the
  happiest man in the world.  The end.
</p>
<script>
  var words = $("p:first").text().split(" ");//将p文字内容以空间为条件放入数组
  var text = words.join("</span> <span>");
  $("p:first").html("<span>" + text + "</span>");
  $("span").click(function () {
    $(this).css("background-color","yellow");
  });
 
</script>
```

![dtWi2U7pfO](D:/Typora/typora-user-images/Jquery/dtWi2U7pfO.gif)



$不用需要注意的是，CSS添加的是内联样式，由于内联样式权重过高，会不利于响应式设计$



### .attr()

> 该属性是setAttribute 和 getAttribute的结合体，可赋值，可取值



**取值赋值**：

```html
<input class="demo" hsm="18" ></input>
<input class="input" type="checkbox" checked="">
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    //赋值
    $('.demo').attr('placeholder','你是夏日的徐凤')
    $('.input').attr('checked','biu')
	//取值
    console.log( $('.demo').attr('hsm') )
    console.log( $('.input').attr('checked') )
</script>
```

![image-20200325115303807](D:/Typora/typora-user-images/Jquery/image-20200325115303807.png)

像checked，selected，disabled等属性和值一致的属性，无论是否填了值，填了什么值，都不会改变他们原来的值，值都是等同于属性



### .prop()

> 该属性基于js的原生dom进行获取，特性映射，非特性不能映射，该属性能获取属性最真实的状态，返回的值是Boolean

特性映射：就是是属于标签本身就存在的属性，在赋值的时候，会映射到对应的标签中

非特性映射：就是不属于标签本身的属性，而是自定义的属性，在赋值的时候，不会映射到对应的标签中，也就是不会显示该属性在标签中



**取值赋值**

```html
<input class="demo" hsm="18" ></input>
<input class="input" type="checkbox" checked="">
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    //赋值
    $('.demo').prop('placeholder','你是夏日的徐凤')
    $('.input').prop('checked','biu')
	//取值
    console.log( $('.demo').prop('hsm') )
    console.log( $('.input').prop('checked') )
</script>
```

![image-20200325120312503](D:/Typora/typora-user-images/Jquery/image-20200325120312503.png)



#### attr与prop的区别

jQuery认为

attribute的checked，selected，disabled就是表示该属性初始状态的值，

而prop(property)的checked，selected， disabled表示该属性实时状态的值（true或false）

#### attr与prop的适用场景

- 对于HTML元素本身就带有的固有属性，在处理时，使用prop方法。
- 对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。



### .html(content)

> 类似原生的innerHTML，可取可赋，取值返回原生dom，需要注意的一点，取值的时候，如果有多个符合条件的元素，html只会取到第一个，比较特殊
>
> 参数表示内容，可以是字符串，可以是函数。



取值赋值

```html
//取值
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script>
//取值
console.log(  $('ul li').html() )	
//1

//赋值
var arrName = ['陈哥','成哥','邓哥','吴哥']
$('ul li').html(function(index,ele){
return '<span style="color:orange">'+arrName[index]+'</span>'
})
//陈哥  成哥  邓哥  吴哥
    
$('ul li').html('ok')    //ok  ok  ok   ok
</script>
```

![image-20200326154339068](D:/Typora/typora-user-images/Jquery/image-20200326154339068.png)

jQuery一般能把一堆东西集中进行操作，在JS中是很难办到的，但在这个html方法中 ，有些特殊，html只能单独取第一个进行操作。但赋值的时候依然可以多多个值操作



text()

> 类似原生的innnText，可取可赋，取值返回元素里边所有的文本节点，不同于html。
>
> text取值可以取到全部的文本节点
>
> 参数表示内容，可以是字符串、或者函数



取值赋值

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script>
//取值
console.log(  $('ul li').text() )	//1 2 3 4 5

//赋值
var arrName = ['陈哥','成哥','邓哥','吴哥']
$('ul li').text(function(index,ele){
	return	arrName[index];
})
//陈哥  成哥  邓哥  吴哥

$('ul li').text('ok')    //ok  ok  ok   ok

```

对于text方法，即使里面放入了标签元素，也不会当成标签元素去解析



### .size()

> 等价于length

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ul>
<script>
$('ul li').size()	//4
//等价于
$('ul li').length() //4
</script>
```



### .addClass()

> 给元素添加类名，可同时添加一个或多个类名，如果要添加多个，用空格隔开，
>
> 参数表示类名名称，可添加字符串、还有函数



赋值

```html
<div class="demo">1</div>
<div class="demo">2</div>
<div class="demo">3</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').eq(0).addClass('No1')
    $('.demo').eq(0).addClass('No2  No3')
    $('.demo').addClass(function(index,ele){
        return 'num'+index
    })
</script>
```

![image-20200325203651876](D:/Typora/typora-user-images/Jquery/image-20200325203651876.png)



### .removeClass()

> 溢出所选元素的类名，可同时移除一个或多个类名，同时移除多个，用空格隔开，如果移除的类名不存在，不会报错，跳过该元素，如果移除的是' ',则表示移除类名为undefined的
>
> 参数表示类名名称，可添加字符串，或者函数

```html
<div class="demo active No1 No2 ">1</div>
<div class="demo">2</div>
<div class="demo">3</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    $('.demo').eq(0).removeClass('active')
    $('.demo').eq(0).removeClass('No1  No2')
    $('.demo').removeClass(function(index,ele){
        if(index % 2 ==0){
            return 'demo'   
        }
        return '' //表示移除类名为undefined的
    })
</script>
```

![image-20200325205505934](D:/Typora/typora-user-images/Jquery/image-20200325205505934.png)

### .hasClass()

> 表示选中的元素中是否有某个类名，一次只能检测一个类名，参数没有函数形式
>
> 参数表示类名名称，只能填字符串

```html
<div class="demo active">1</div>
<div class="demo">2</div>
<div class="demo">3</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    console.log( $('.demo').eq(0).hasClass('active') )
</script>
</body>
```





综合案例-点击切换样式

```html
<style>
    .wrapper .style1{
        background-color: orange;
    }
    .wrapper .style1 li{
        color: purple;
    }
    .wrapper .style2{
        background-color: deeppink;
    }
    .wrapper .style2 li{
        color: yellow
    }

    .wrapper.active .style1{
        background-color: purple;
    }
    .wrapper.active .style1 li{
        color: orange;
    }
    .wrapper.active .style2{
        background-color: yellow;
    }
    .wrapper.active .style2 li{
        color: deeppink
    }
</style>
</head>
<body>
    <div class="wrapper">
        <ul class="style1">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
        <ul class="style2">
            <li>5</li>
            <li>6</li>
            <li>7</li>
            <li>8</li>
        </ul>
    </div>
    <script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

    <script>
        $('.wrapper').click(function(index,ele){
            if($('.wrapper').hasClass('active')){
                $('.wrapper').removeClass('active');
            }else{
                $('.wrapper').addClass('active');
            }
        })
    </script>
```

![sQL2kMxJn3](D:/Typora/typora-user-images/Jquery/sQL2kMxJn3.gif)

上面样式的切换也可以用CSS方法进行操作，但是没有用类名来的整洁，便于维护，复用性高，还节约代码请求带宽（css样式浏览器会缓存下来，而js不会浏览器缓存，这样如果会首先定义两套样式，到时候切换的只是css样式，没有向CSS方法那样用JS去动态修改样式消耗宽带）



### .val()

> 类似于value，获取**表单**相关元素的value值，因为表单才有value属性，对于获取，如果有多个元素，只能获取第一个，这个类似于前面的html。可以取值，可以赋值
>
> 参数可以是字符串，或者函数(index，oldvalue)，注意函数的第二个参数表示开始的值



取值赋值

```html
用户：<input type="text" value="huang"><br/>
密码：<input type="text" value="123456"><br/>
确认密码：<input type="text" value="123456"><br/>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    //取值
    console.log($('input').val())	//huang
    //等价于
    console.log($('input').eq(0).val())//huang

    // 赋值
    $('input').val(function(index,oldValue){
        return oldValue + index;
    }) //huang0  1234561	1234561
    
    $('input').eq(0).val('000')//000
</script>
```

![image-20200325215803125](D:/Typora/typora-user-images/Jquery/image-20200325215803125.png)

上面我们发现一个特点，即使我们修改了元素的value值，当在审查元素中的value并没有形成映关系，值还是最初始的值



### .serialize()

> 获取form表单的所有name,value属性值，形成一个默认用&将所有结果连在一起的字符串，如果想把获得的结果组成数组的形式，可以用serializeArray()方法

```html
用户：<input type="text" name="user" value="huang"><br/>
密码：<input type="text" name="password" value="123456"><br/>
确认密码：<input type="text" name="repassword" value="123456"><br/>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log( $('input').serialize() )
    console.log($('input').serializeArray());
</script>
```

![chrome_0bTYLZlVTh](D:/Typora/typora-user-images/Jquery/chrome_0bTYLZlVTh.png)



## JQuery实例方法-DOM操作(基于jQuery对象的增删改查)

$*********查询元素************$

### 获得兄弟方法：

#### .next()

> 获取当前元素的下一个兄弟元素，注意是**下一个**，而不是下面所有的兄弟元素，指的是一个。
>
> 参数表示限定条件，限定下一个元素还必须是指定的元素，否则无法选中

```html
<div class="wrapper">
    banana：<input type="checkbox" name="" id="">
    apple：<input type="checkbox" name="" id="">
    orange：<input type="checkbox" name="" id="">

    <input type="submit" value="login">
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log( $('input[type="checkbox"]').eq(0).next('input[type="checkbox"]') )
</script>
```

![chrome_M36Tbd8YZj](D:/Typora/typora-user-images/Jquery/chrome_M36Tbd8YZj.png)

表示选中input框属性为type="checkbox"的下一个兄弟节点，且兄弟节点也要满足属性有type="checkbox的元素。



#### .prev()

> 与next类似。
>
> 获取当前元素的上一个兄弟元素，注意是**上一个**，而不是上面所有的兄弟元素，指的是一个。
>
> 参数表示限定条件，限定上一个元素还必须是指定的元素，否则无法选中，参数填的是选择器规则



```html
<div class="wrapper">
    banana：<input type="checkbox" name="" id="">
    apple：<input type="checkbox" name="" id="">
    orange：<input type="checkbox" name="" id="">

    <input type="submit" value="login">
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log( $('input[type="checkbox"]').eq(1).prev('input[type="checkbox"]') )
</script>
```

![image-20200325223454663](D:/Typora/typora-user-images/Jquery/image-20200325223454663.png)

#### .nextAll()

> 获取当前元素下面所有的兄弟节点，不包括当前元素
>
> 参数表示限定条件，限定所有的兄弟元素还必须是指定的元素，否则无法选中，参数填的是选择器规则

```html
<div class="wrapper">
    全选：<input type="checkbox" name="" id="">
    banana：<input type="checkbox" name="" id="">
    apple：<input type="checkbox" name="" id="">
    orange：<input type="checkbox" name="" id="">
    <input type="submit" value="login">
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('input[type="checkbox"]').eq(0).click(function(index,ele){
        if ($(this).prop('checked')) {
            $(this).nextAll('input[type="checkbox"]').prop('checked',true)
        }else{
            $(this).nextAll('input[type="checkbox"]').prop('checked',false);
        }
    })
</script>
```

![mT5dS5RQ0E](D:/Typora/typora-user-images/Jquery/mT5dS5RQ0E-1585146432386.gif)



#### .prevAll() 

>  获取当前元素上面所有的兄弟节点，不包括当前元素
>
> 参数表示限定条件，限定所有的兄弟元素还必须是指定的元素，否则无法选中，参数填的是选择器规则

```html
<div class="wrapper">
    banana：<input type="checkbox" name="" id="">
    apple：<input type="checkbox" name="" id="">
    orange：<input type="checkbox" name="" id="">
    全选：<input type="checkbox" name="" id="">

    <input type="submit" value="login">
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('input[type="checkbox"]').eq(3).click(function(index,ele){
        if ($(this).prop('checked')) {
            $(this).prevAll('input[type="checkbox"]').prop('checked',true)
        }else{
            $(this).prevAll('input[type="checkbox"]').prop('checked',false);
        }
    })
</script>
```

![J78Otk23m8](D:/Typora/typora-user-images/Jquery/J78Otk23m8.gif)

prevAll和nextAll差不多



#### .nextUntil()   

>  获取当前元素下面所有的兄弟节点，直到遇到限定条件为止
>
> 参数表示限定条件，只能填选择器规则

```html
<div class="wrapper">
    <h1>水果</h1>
    全选：<input type="checkbox" >
    banana：<input type="checkbox" >
    apple：<input type="checkbox" >
    orange：<input type="checkbox" >
    <h1>nba</h1>
    全选：<input type="checkbox" >
    Rose<input type="checkbox" >
    Curry<input type="checkbox" >
    james<input type="checkbox" >
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('h1').next().click(function(){
        if( $(this).prop('checked') ){
            $(this).nextUntil('h1').prop('checked',true);
        }else{
            $(this).nextUntil('h1').prop('checked',false);
        }
    })
</script>
```

首先选中h1标签下的所有兄弟节点，点击某一个兄弟节点，如果点击的节点存在checked属性，则将该节点下直到下一个h1节点之间的所有节点添加一个checked属性，如果点击的节点不存在checked属性，则将该节点下直到下一个h1节点之间的所有节点添加一个checked属性，值设置为false(相当于移除checked属性)



案例二

```html
<div class="wrapper">
    all：<input type="checkbox">
    <h1>吃货清单</h1>
    all：<input type="checkbox">
    <h2>水果</h2>
    全选：<input type="checkbox">
    banana：<input type="checkbox" >
    apple：<input type="checkbox" >
    orange：<input type="checkbox" >

    <h2>蔬菜</h2>
    全选：<input type="checkbox">
    tomato：<input type="checkbox" >
    egg：<input type="checkbox" >
    potao：<input type="checkbox" >

    <h2></h2>
    <h1>明星清单</h1>
    all：<input type="checkbox">
    <h2>nba</h2>
    全选：<input type="checkbox">
    Rose：<input type="checkbox" >
    Curry：<input type="checkbox" >
    james：<input type="checkbox" >
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('input').eq(0).click(function(){
        if($(this).prop('checked')){
            $(this).nextAll('input[type="checkbox"]').prop('checked',true);
        }else{
            $(this).nextAll('input[type="checkbox"]').prop('checked',false)
        }
    })

    $('h1').next().click(function(){
        if($(this).prop('checked')){
            $(this).nextUntil('h1','input[type="checkbox"]').prop('checked',true);
        }else{
            $(this).nextUntil('h1','input[type="checkbox"]').prop('checked',false);
        }
    })

    $('h2').next().click(function(){
        if($(this).prop('checked')){
            $(this).nextUntil('h2','input[type="checkbox"]').prop('checked',true);
        }else{
            $(this).nextUntil('h2','input[type="checkbox"]').prop('checked',false);
        }
    })
</script>
```

![tjnspucSEF](D:/Typora/typora-user-images/Jquery/tjnspucSEF.gif)



#### .siblings()

>  获取当前元素的其他所有兄弟元素，不同于上面的有顺序（方向），这个只要是兄弟元素都能选中
>
> 参数表示限定条件，只能填选择器规则

```html
<ul>
   <li>list item 1</li>
   <li>list item 2</li>
   <li class="third-item">list item 3</li>
   <li>list item 4</li>
   <li>list item 5</li>
   <p>list item 6</p>
   <p>list item 7</p> 
</ul>
<script>
	$('li.third-item').siblings('li').css('background-color', 'red');
</script>
```

![image-20200326160143513](D:/Typora/typora-user-images/Jquery/image-20200326160143513.png)

上面表示选中类名为third-item的元素的兄弟元素，但限定只能是li标签的兄弟元素



#### .slice()

> 在一堆元素里截取某些元素进行集中操作，类似数组的方法，左闭右开区间[  )
>
> 参数表示索引下标，可填数字

```HTML
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
</ul>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('li').slice(1,6).css('backgroundColor','orange')
</script>
```

![image-20200326171634759](D:/Typora/typora-user-images/Jquery/image-20200326171634759.png)





### 获得父级方法：

#### .parent()

> 获取当前元素的上一级节点，注意只能是上一级，可取可赋，元素的子节点可以有多个，但父节点只能有一个，因为是树形结构
>
> 参数表示限定条件，填的是选择器规则



案例：添加购物车

```html
<div class="wrapper">

</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    var showArr = [{name:'nike',id:'101'},{name:'adidas',id:'102'}]
    var str='';
    showArr.forEach(function(ele,index){
        str +=`<div class="show" data-id="${ele.id}"><p>${ele.name}</p><button>add</button></div>`
    })
    $('.wrapper').html(str)

    var carArr = []
    $('button').click(function(){
        carArr.push( $(this).parent().attr('data-id') );
    })
</script>
```

动态创建标签，然后给button添加点击事件，把点击的物品放入购物车

![Cf3GoCwb6r](D:/Typora/typora-user-images/Jquery/Cf3GoCwb6r.gif)

#### .parents()

> 表示获取当前元素的所有父级元素，放到一个jQuery对象中，没有参数限定的话可以一直获取到顶层的html。
>
> 参数表示限定条件，限定只能是哪个父级，填的是选择器规则

```html
<ul>
    <li>
        <ul>
            <li class="demo"></li>
            <li></li>
            <li></li>
        </ul>
    </li>
</ul>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log( $('.demo').parents() )
</script>

```

![image-20200326163932978](D:/Typora/typora-user-images/Jquery/image-20200326163932978.png)

```javascript
 console.log( $('.demo').parents('li') )
```

![image-20200506102713686](D:/Typora/typora-user-images/Jquery/image-20200506102713686.png)

#### .closest()

>离当前元素最近的且满足条件的一个父级，与其他方法不同的是，查找范围是从自身开始，如果自身满足条件，也会被选中，该方法需要传参数，如果不传参数，一个都获取不到
>
>参数表示限定条件 填的是规则选择器

```html
<div class="wrapper">
    <ul>
        <li>
            <ul data-id="101">
                <li>nike</li>
                <li>200$</li>
                <li>
                    <button>add</button>
                </li>
            </ul>
        </li>
        <li>
            <ul data-id="102">
                <li>adiads</li>
                <li>200$</li>
                <li>
                    <button>add</button>
                </li>
            </ul>
        </li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    console.log( $('button').closest() )	//每写条件，得到空的jQuery对象
    console.log( $('button').eq(0).closest('button') ) //当前button被选中
    console.log( $('button').closest('button') )//当前的两个butoon都被选中
    
    console.log( $('button').eq(0).closest('ul') )//当前元素的上一个ul被选中
    console.log( $('button').closest('ul') )//当前的两个元素的两个最近ul都被选中
</script>
```

![mgwf7k17Fo](D:/Typora/typora-user-images/Jquery/mgwf7k17Fo.gif)

如果当前元素不止一个，会将条件选中的父级元素集中放到一个jQuery对象中

#### .offsetParent()

> 找到最近的且有定位 属性的父级元素,没有参数

```html
<style>
    .wrapper{
        position: relative;
    }
</style>    
<div class="wrapper">
    <div class="box">
        <span>123</span>
    </div>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log($('span').offsetParent());
</script>
```

选中拥有定位属性的wrapper

### 获得子级方法：

#### chirdren()

> 获取当前选中元素的所有子元素



### 移动元素位置:同级移动：

#### .insertBefore()

> js的原生方法，把前面选中元素插入到insertBefore选中元素的前面
>
> $(A).insertBefore(B)
>
> insert A  before B

```html
<style>
    .wrapper{
        width:200px;
        border: 1px solid #000;
        padding: 10px;
    }
    .wrapper div{
        width: 200px;
        height: 100px;
    }
    .wrapper .box1{
        background-color: purple;
    }
    .wrapper .box2{
        background-color: orange
    }
    .content{
        width: 200px;
        height: 50px;
        background-color: deeppink
    }
</style>
<div class="wrapper">
    <div class="box1">box1</div>
    <div class="box2">box2</div>
</div>
<div class="content">content</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.content').insertBefore('.box1')
</script>
```

![EM2jvojCY8](D:/Typora/typora-user-images/Jquery/EM2jvojCY8.gif)

上面表示把content插入到box1中去，



#### .before()

> before里边添谁，谁就在前头
>
> 参数只能填jQuery对象，不能填css规则，如果只填CSS规则，只会把规则当成字符串填进去。
>
> before方法是基于jQuery链式调用思想产生的，要操作谁，就把谁放到前头

```html
<script>
	$('.box1').before('.content')
</script>
```

![image-20200326183209796](D:/Typora/typora-user-images/Jquery/image-20200326183209796.png)

正确操作

```html
<script>
	$('.box1').before($('.content'))
</script>
```

![image-20200326183335962](D:/Typora/typora-user-images/Jquery/image-20200326183335962.png)

在insertBefore中，选择元素的时候(content)就把选中元素转换成jQuery对象了，然后再在insertBefore



#### .insertAfter()

> JS的原生方法，把前面选中元素插入到insertAfter选中元素的后面

```html
<script>
    $('.content').insertAfter($('.box1'))
    //一样的效果
    $('.content').insertAfter('.box1')
</script>
```

![image-20200326185247964](D:/Typora/typora-user-images/Jquery/image-20200326185247964.png)

其实在insertBefore，insertAfer里边也可以添jQuery对象，这样就不会记得那么混淆了



#### .after()

> after里边添谁，谁就在前头
>
> 参数只能填jQuery对象，不能填css规则，如果只填CSS规则，只会把规则当成字符串填进去。
>
> after方法是基于jQuery链式调用思想产生的，要操作谁，就把谁放到前头

```html
<script>
    $('.box1').after($('.content'));
</script>
```

![image-20200326185251865](D:/Typora/typora-user-images/Jquery/image-20200326185251865.png)



### 移动元素位置:移动到子级：

#### .append()

> 类似原生的appendChild
>
> 在每个匹配元素里面的子元素末尾处插入参数内容。
>
> $(匹配元素).appendTo(目标元素)
>
> 遵循链式调用思想

```html
<script>
    $('.wrapper').append($('.content'))
    //不一样的效果
    $('.wrapper').append('.content')	//把参数当字符串插入
</script>
```

![image-20200326190922192](D:/Typora/typora-user-images/Jquery/image-20200326190922192.png)

上面表示把append中的内容添加到wrapper元素中去



#### .appendTo()

> 类似于原生的appendChild方法
>
> 将匹配的元素插入到目标元素的子元素最后面
>
> appendTo方法是基于jQuery链式调用思想产生的，要操作谁，就把谁放到前头
>
> $(目标元素).appendTo(匹配元素)

```html
<script>
    $('.content').appendTo($('.wrapper'))
    //一样的效果
    $('.content').appendTo('.wrapper')
</script>
```

![image-20200326190922192](D:/Typora/typora-user-images/Jquery/image-20200326190922192.png)



#### .prepend()

> 将参数内容插入到每个匹配元素的第一个子元素前面
>
> 遵循链式调用思想

```html
<script>
    $('.wrapper').prepend($('.content'))
    //不一样的效果
    $('.wrapper').prepend('.content')
</script>
```

![image-20200326192408665](D:/Typora/typora-user-images/Jquery/image-20200326192408665.png)



#### .prependTo()

> 将匹配元素插入到参数内容元素的第一个子元素前面
>
> preappendTo方法是基于jQuery链式调用思想产生的，要操作谁，就把谁放到前头

```html
<script>
    $('.content').prependTo($('.wrapper'))
    //一样的效果
    $('.content').prependTo('.wrapper')
</script>
```

![image-20200326192223783](D:/Typora/typora-user-images/Jquery/image-20200326192223783.png)



### 删除元素方法：

#### .remove()

> 删除元素，同时删除该元素关联的事件，返回被删元素的jQuery对象
>
> （净身出户）

```html
<style>
    .wrapper{
        width:200px;
        height: 200px;
        background-color: deeppink;
    }
</style>
<div class="wrapper">
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.wrapper').click(function(){
        alert('被点击了');
    })
    $('.wrapper').remove().appendTo($('body'))
</script>
```

![6cxRVWUyYU](D:/Typora/typora-user-images/Jquery/6cxRVWUyYU.gif)

当标签元素被remove后又被添加回来，不会保留事件，说明在删除标签元素的时候，连同事件也删除了 

#### .detach ()

> 删除元素，但会保留该元素相关的事件，返回被删元素的jQuery对象
>
> （好聚好散）

```html
<style>
    .wrapper{
        width:200px;
        height: 200px;
        background-color: deeppink;
    }
</style>
<div class="wrapper">
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.wrapper').click(function(){
        alert('被点击了');
    })
    $('.wrapper').detach().appendTo($('body'))
</script>
```

![SEvgfrGiMi](D:/Typora/typora-user-images/Jquery/SEvgfrGiMi.gif)

#### empty()

> 清空当前选中元素的所有子元素



### 增加元素方法：

#### $()参数：标签字符串

> 该方法还有另一个功能，可以映射成documentCreate
>
> 可以用标签创建元素

```html
<style>
    div{
        width:100px;
        height: 100px;
        background-color: deeppink;
        margin-top: 5px;
    }
</style>   
<div>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('<div>nice</div>').appendTo('body')
    $('<div>very</div><div><span>good</span></div>').appendTo('body')
</script>
```

![image-20200326220543314](D:/Typora/typora-user-images/Jquery/image-20200326220543314.png)

#### .wrap()

> 给选择的元素添加一个直接包裹的父级。
>
> 参数可以是标签、jQuery对象，css规则（必须是存在于页面的元素，会以复制的形式），函数

```html
<style>
    div{
        width:100px;
        height: 100px;
        background-color: deeppink;
        margin-top: 5px;
    }
</style>
<div class="demo">
    <span></span>
    <span></span>
</div>

<div class="demo">
    <p></p>
    <p></p>
</div>
<div class="css_rules"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    //标签
    $('.demo').wrapr('<div class="tag"></div>')
	//jQuery对象
    $('.demo').wrap($('<div class="jQ_object"></div>'));
    //CSS规则
    $('.demo').wrap('.css_rules');
     //函数
    $('.demo').wrap(function(index){
        return `<div class="func${index}"></div>`
    })
</script>
```

![chrome_uKi40su9EH](D:/Typora/typora-user-images/Jquery/chrome_uKi40su9EH.png)

上面表示给选择的元素添加一个直接包裹的父级。

#### .wrapinner()

> 给当前选择的元素的内部子元素添加一个子级
>
> 参数可以是标签、jQuery对象，css规则（必须是存在于页面的元素，会以复制的形式），函数



```html
<style>
    div{
        width:100px;
        height: 100px;
        background-color: deeppink;
        margin-top: 5px;
    }
</style>
<div class="demo">
    <span></span>
    <span></span>
</div>

<div class="demo">
    <p></p>
    <p></p>
</div>
<div class="css_rules"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    //标签
    $('.demo').wrapInner('<div class="tag"></div>')
	//jQuery对象
    $('.demo').wrapInner($('<div class="jQ_object"></div>'));
    //CSS规则
    $('.demo').wrapInner('.css_rules');
     //函数
    $('.demo').wrapInner(function(index){
        return `<div class="func${index}"></div>`
    })
</script>
```

​    ![chrome_AIR77B10nd](D:/Typora/typora-user-images/Jquery/chrome_AIR77B10nd.png)

上面给demo类元素的内部子元素添加一个直接包裹的div元素

#### .wrapAll()

> 给多个选中的元素添加一个统一的爹，原理以第一个选中的元素位置为基准，把其他元素放到基准元素的后边，再用一个元素包裹起来

```html
<div class="wrapper">
    <div class="demo"></div>
    <div class="demo"></div>
</div>
<div class="demo"></div>

<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').wrapAll('<div class="box"></div>')
</script>
```

![image-20200326225121206](D:/Typora/typora-user-images/Jquery/image-20200326225121206.png)







```html
<div class="wrapper1">
    <div class="demo"></div>
    <div class="demo"></div>
</div>
<div class="wrapper2">
    <div class="demo"></div>
    <div class="demo"></div>
</div>

<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').wrapAll('<div class="box"></div>')
</script>
```

![image-20200326225226828](D:/Typora/typora-user-images/Jquery/image-20200326225226828.png)

#### .unwrap()

> 移除当前元素的上一直接父级，直到遇到结构标签位置（body,html），结构标签肯定不能删除，否则的话就乱套了

```html
<div class="wrapper1">
    <div class="contain">
        <div class="box">
            <div class="demo"></div>
        </div>
    </div>
</div>

<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').unwrap() //删除box
    $('.demo').unwrap() //删除contain
    $('.demo').unwrap() //删除wrapper

    $('.demo').unwrap() //为结构标签body,无法再删除
</script>
```

#### .clone

> 深度克隆dom对象，返回jQuery对象，默认情况下会克隆样式 ，但不会克隆事件
>
> 参数表示是否克隆事件，有true和false两个参数
>
> 所有元素数据（ `.data()`方法返回的内容）同样被复制到新的克隆元素。



```html
<style>
    .tp1{
        display:none
    }
</style>
<table class="stb">
    <tr>
        <th>名字</th>
        <th>年龄</th>
        <th>班级</th>
    </tr>
    <tr class="tp1">
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>

<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    var studentArr = [
        {
            name:'小明',
            age:18,
            class:3

        },
        {
            name:'小张',
            age:10,
            class:2
        },
        {
            name:'小李飞刀',
            age:8,
            class:1
        }
    ]
    var oWrapper = $('.stb');
    studentArr.forEach(function(ele,index){
        var oCloneDom = $('.tp1').clone().removeClass('tp1')
        oCloneDom
            .find('td')
            .eq(0)
            .text(ele.name)
            .next()
            .text(ele.age)
            .next()
            .text(ele.class)

        oCloneDom.appendTo(oWrapper)
    })
</script>
```

首先对数据进行遍历操作，然后先深度克隆一个类名为.tp1的标签，其所有子节点也都会被克隆，然后移除克隆的标签附带的.tp1类名。最后将得到的数据分别放入到该标签结构中



#### .data

> 允许在jQuery中的DOM对象上存储一些信息（数据），但在该方法存储的数据不会反应到标签行间之中，但如果行间有属性的话，也可以通过.data(属性名)的方式取得存储的数据
>
> 参数表示需要存储的数据，可以是字符串形式，也可以是对象形式,对象的获取形式直接填对象里边的属性名即可
>
> $('.demo').data('data-name','hsm')
>
> data有个特点，就是存储进去的是什么类型的值，取出来也是什么类型的值，不会像attr或prop方法取出来时都会变成字符串
>
> 数据并非真正存在dom对象中，而是存在dom对象的jQuery对象中，只是两者存在映射关系而已



```html
<style>
    .tp1{
        display: none;
    }
</style>
<div class="wrapper">
    <div class="tp1">
        <p></p>
        <span></span>
        <button>add</button>
    </div>

    <p class="show">
        <span>sum</span>
        <span class="sum">0</span>
    </p>
</div>

<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    var shopArr = [
        {
            name:'james solider',
            shopName:'nike',
            price:110,
            id:'1001'

        },{
            name:'Rose crazyLight',
            shopName:'adidas',
            price:90,
            id:'2002'

        },{
            name:'funny mud go pee ',
            shopName:'361',
            price:180,
            id:'3003'

        },
    ]
    var show = $('.show');
    shopArr.forEach(function(ele,index){
        var oCloneDom = $('.tp1').clone().removeClass('tp1')
        oCloneDom.data({
            id:ele.id,
            shopName:ele.shopName,
            price:ele.price
        }).find('p')
            .text(ele.name)
            .next()
            .text(ele.price)

        oCloneDom.insertBefore(show)
    })

    $('.wrapper button').click(function(){

        $('.sum').text( +$('.sum').text() +  $(this).parent().data('price') );
        console.log($(this).parent().data('id') )
    })
</script>
```

![dtrttjqjZM](D:/Typora/typora-user-images/Jquery/dtrttjqjZM.gif)

将数据存入到.data中，方便到时候拿取。



**不用attr或prop存属性的原因**

- 会显示在标签的行间，过多的属性的话会导致标签过于臃肿
- attr方法和prop方法都是对DOM的操作，耗费性能，而.data不涉及对DOM的操作，所以在性能上比attr和prop好
- jQuery把data数据存在一个映射池(cache)里边，映射池操作的是JS，虽然跟DOM有映射，但没有直接操作DOM
- 由于数据都存在一个映射池里边，因此更加规范





## 4.jQuery实例方法—事件

### 事件绑定

#### .on()

> 指定你需要绑定何种类型的事件，类似js的addEventListener事件，on方法能绑定任何事件类型，除了能绑定系统事件，还有自定义事件。
>
> 参数有四个，分别表示填写的事件类型(type)、选择器(selector)、数据(data)、事件处理函数(handler)。
>
> data可以存任何类型的值，且值存在事件处理函数中的事件对象（e.data）中，需要注意的一点是，如果数据是字符串，且不填selector的话，按照参数的顺序如果可能会被认为是selector
>
> selector是一个选择器字符串，用于过滤出被选中的元素中能触发事件的后代元素。如果选择器是 `null` 或者忽略了该选择器，那么被选中的元素总是能触发事件。
>
> e（event）表示事件对象，e.target表示事件源，就得点击的元素

```html
<style>
    .demo{
        height: 100px;
        width: 100px;
        background-color: deeppink;
    }
</style>    
<div class="demo"></div>


<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').on('mouseenter',function(){
        alert('ok')
    })

    //也可以给一个元素同时绑定多个事件
    $('.demo').on({
        click:function(){
            console.log('click');
        },
        mouseenter:function(){
            console.log('mouseenter');
        },
        mouseleave:function(){
            console.log('mouseleave');
        }
    })
</script>
```

鼠标移入方块触发mouseenter事件





#### .one()

> 一次性事件，只触发一次
>
> 参数有四个，分别表示填写的事件类型(type)、选择器(selector)、数据(data)、事件处理函数(handler)。

```html
<a href="https://www.baidu.com" target="_blank">jump</a>


<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    $('a').one('click',function(){
        window.open('https://www.taobao.com')
        return false    //阻止默认事件触发
    })
</script>
```

先跳淘宝，再跳百度

#### .off()

> 解绑事件
>
> 有3个参数，参数1表示解绑的事件类型，参数2表示selector，参数3表示解绑的事件函数名称， 不填参数默认解绑元素的所有事件
>
> 如果要取消事件委托类事件，比如给ul绑定事件，事件委托到li，如果想取消li的事件，可以用on一样怎么设置就怎么取消

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<div class="demo" style="height: 100px; width: 100px; background-color: deeppink;"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>

<script>
    function clickOne(){
        console.log('clickOne');
    }
    function clickTwo(){
        console.log('clickTwo');
    }
    function mouseenter(){
        console.log('mouseenter');
    }

    $('.demo').on('click',clickOne)
    $('.demo').on('click',clickTwo)
    $('.demo').on('moouseenter',mouseenter)

    $('.demo').off()    //解绑.demo元素所有事件
    $('.demo').off('click')    //解绑.demo元素所有click事件,保留mouseenter事件
    $('.demo').off('click',clickTwo)    //解绑.demo元素click类型事件clickTwo事件处理函数，不再执行该函数


    $('ul').on('click','li',clickOne)    //给ul添加事件，会委托到所有的li
    $('ul').off('click','li',clickOne)  //取消ul的事件

</script>
```



#### .trigger()

> 帮你主动触发事件，无论是系统事件还是自定义事件，准确来是，是主动调用该类型事件的事件处理函数
>
> 两个参数：参数一表示事件类型，参数二表示传递给handler的参数

```html
<div class="demo" style="width: 100px; height: 100px; background-color: orange;"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').on('pageLoad',function(e,a,b){
        console.log(e,a,b)
    })
    $('.demo').trigger('pageLoad',[1,2])
</script>
```

<img src="D:/Typora/typora-user-images/Jquery/image-20200328170207487.png" alt="image-20200328170207487" style="zoom: 200%;" />

上面声明了一个自定义事件pageLoad，像这种自定义事件没有触发条件，我们无法通过某种操作去触发它，而trigger表示自动去触发事件，无论是系统事件还是自定义事件



针对自定义事件模拟实现

```javascript
jQuery.prototype.myOn = function(type,handler){
    //cacheEvent建立一个缓存对象，用来缓存(存放)自定义事件
    for(var i = 0; i < this.length; i++){
        if(!this[i].cacheEvent){
            this[i].cacheEvent = {}
        }
        if(!this[i].cacheEvent[type]){
            this[i].cacheEvent[type] = [ handler ]
        }else{
            this[i].cacheEvent[type].push(handler);
        }
    }
}

jQuery.prototype.myTrigger = function(type){
    var paramsArr = arguments.length > 1 ? [].slice.call(arguments,1) : [];
    //myTrigger的第一个参数是事件类型名称，因此需要切割掉
    //Arguments(5) ["pageLoad", 1, 2, 3, 4]

    var self = this
    for(var i = 0; i < this.length; i++){
        if(this[i].cacheEvent[type]){
            this[i].cacheEvent[type].forEach(function(ele,index){
                ele.apply(self,paramsArr)
            })
        }
    }
}
```

myOn事件用于用处创建各种系统事件和自定义事件，这里实现的只是自定义事件

myTrigger事件用于执行在myOn创建事件对应的的执行函数。

> 对于myOn，首先给jQuery对象里边所有的DOM创建一个缓存对象，用来缓存(存放)不同类型的自定义事件，然后判断缓存对象中是否存在某种事件，如果该事件不存在，在缓存对象以该类型事件作为属性名，属性值为一个数组，存放该事件类型对应的事件处理函数，放到数组里是为了预防该事件类型中可能有多个事件处理函数，到时候可以按顺序处理，如果该事件类型存在，说明里面有事件处理函数了，再把新的事件处理函数push进该数组中即可

> 对于myTrigger，首先判断是否传入数据参数，如果有，将数据获取到并放入一个新数组中返回给paramsArr，如果没有，返回一个空数组给paramsArr，然后，既然该事件是主动触发函数，就需要知道到哪些类型事件需要执行他的事件处理函数，第一个参数type就是告诉我们需要执行该类型事件，既然要执行该类型事件，就需要判断该元素的缓存对象中是否存在该类型事件，如果存在该类型事件，就遍历该类型事件存放事件处理函数的数组，把事件处理函数拿出来一个一个的执行，myTrigger的第二个数据参数，作为事件处理函数的参数



#### .hover() 

> 有两个函数，第一个代表mouseenter事件的处理函数，第二个代表mouseleave事件的处理函数

```html
<div class="demo" style="width: 100px;height: 100px;background-color: #f20;"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').on('mouseenter',function(){
        console.log('mouseenter');
    }).on('mouseleave',function(){
        console.log('mouseleave');
    })

    //等价于
    $('.demo').hover(function(){
        console.log('mouseenter');
    },function(){
        console.log('mouseleave');
    })
</script>
```

![iC5pobXkOM](D:/Typora/typora-user-images/Jquery/iC5pobXkOM.gif)



## 5.jQuery实例方法-动画

### .show()

> 元素显示，用的是display属性，会还原成第一次设置的dispaly值，就是如果默认设置为display:inline-block.等隐藏再show的时候，dispaly还是inline-block，而不是block，只有没有设置的display的时候，显示的时候才会默认为display
>
> 有三个参数，
>
> 参数一表示duration，填毫秒数，表示从隐藏到过渡的时长，该方法默认参与变化的属性有width,height,opacity,padding margin
>
> 参数二表示贝塞尔曲线运行状态，linear，swing(慢快慢)
>
> 参数三表示状态变换结束后触发的回调



### .hide()

> 元素隐藏，用的是display属性
>
> 有三个参数，
>
> 参数一表示duration，填毫秒数，表示从隐藏到过渡的时长，该方法默认参与变化的属性有width,height,opacity,padding margin
>
> 参数二表示运行状态(贝塞尔曲线)，linear，swing(慢快慢)
>
> 参数三表示状态变换结束后触发的回调



```html
<style>
    .demo{
        wi
    }
    ul{
        display: none;
    }
</style>
</head>
<body>
    <div class="demo">
        <p>书籍：</p>
        <ul>
            <li>挪威的森林</li>
            <li>平凡世界</li>
            <li>悲伤世界</li>
            <li>假如给我三天光明</li>
        </ul>
    </div>

    <script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
    <script>
        $('p').on('mouseenter',function(){
            $(this).next().show(2000)
        })
        $('.demo').on('mouseleave',function(){
            $('p').next().hide(2000)
        })
    </script>
```

![Qx0jkSomrg](D:/Typora/typora-user-images/Jquery/Qx0jkSomrg.gif)



### .Toggle

> 根据元素的当前状态，如果显示状态就隐藏，如果隐藏状态就显示，用的是display属性。
>
> 有三个参数，
>
> 参数一表示duration，填毫秒数，表示从隐藏到过渡的时长，该方法默认参与变化的属性有width,height,opacity,padding margin
>
> 参数二表示运行状态(贝塞尔曲线)，linear，swing(慢快慢)
>
> 参数三表示状态变换结束后触发的回调

```javascript
$('p').on('click',function(){
	$(this).next().toggle()
})
```



### .fadeIn()

> 淡入，用的是display属性，并在这个基础上加了opacity属性
>
> 有三个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示状态变换结束后触发的回调

### .fadeOut

> 淡出，用的是display属性，并在这个基础上加了opacity属性
>
> 有三个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示状态变换结束后触发的回调

### .fadeTo（）

> 渐进到哪，针对opacity属性，
>
> 有四个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示渐进到哪（opacity）
>
> 参数三表示状态变换结束后触发的回调

```html
<div class="demo">
    <p>书籍：</p>
    <ul>
        <li>挪威的森林</li>
        <li>平凡世界</li>
        <li>悲伤世界</li>
        <li>假如给我三天光明</li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('p').on('click',function(){
        if($(this).next().css('display') == 'none'){
            $(this).next().fadeTo(1500,0.6,'swing',function(){
                console.log('显示');
            })
        }else{
            $(this).next().fadeTo(1500,0.1,'swing',function(){
                console.log('隐藏');
            })
        }
    })

</script>
```

![01aeQuItLI](D:/Typora/typora-user-images/Jquery/01aeQuItLI.gif)

### .fageToggle

> 淡入淡出，用的是display属性，并在这个基础上加了opacity属性
>
> 有三个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示状态变换结束后触发的回调

```html
<div class="demo">
    <p>书籍：</p>
    <ul>
        <li>挪威的森林</li>
        <li>平凡世界</li>
        <li>悲伤世界</li>
        <li>假如给我三天光明</li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('p').on('click',function(){
        $(this).next().fadeToggle(1500,'swing',function(){
            console.log('点击了');
        })
    })

</script>
```

![aa7UZMgUp3](D:/Typora/typora-user-images/Jquery/aa7UZMgUp3.gif)



### .slideDown

> 向下滑动，用的是display属性，并在这个基础上侧重height高度的变化
>
> 有三个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示状态变换结束后触发的回调



### .slideUp

> 向上滑动，用的是display属性变换，并在这个基础上侧重height高度的变化
>
> 有三个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示状态变换结束后触发的回调

```html
<style>
    *{
        margin:0px
        padding:0px
    }
    .demo{
        width: 200px;
        border: 1px solid #000;
    }
    ul{
        display: none;
    }
</style>
<div class="demo">
    <p>书籍：</p>
    <ul>
        <li>挪威的森林</li>
        <li>平凡世界</li>
        <li>悲伤世界</li>
        <li>假如给我三天光明</li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('p').on('click',function(){
        if($(this).next().css('display') == 'none'){
            $(this).next().slideDown(1500,'swing',function(){
                console.log('显示');
            })
        }else{
            $(this).next().slideUp(1500,'swing',function(){
                console.log('隐藏');
            })
        }
    })

</script>
```

![JxRrbS5V2m](D:/Typora/typora-user-images/Jquery/JxRrbS5V2m-1585411406942.gif)

### .slideToggle

> 上下滑动，用的是display属性变换，并在这个基础上侧重height高度的变化
>
> 有四个参数
>
> 参数一表示过渡时长
>
> 参数二表示运动状态
>
> 参数三表示状态变换结束后触发的回调

```html
    <style>
        *{
            margin:0px;
            padding:0px;
        }
        .demo{
            width: 200px;
            border: 1px solid #000;
        }
        ul{
            display: none;
        }
    </style>>
<div class="demo">
    <p>书籍：</p>
    <ul>
        <li>挪威的森林</li>
        <li>平凡世界</li>
        <li>悲伤世界</li>
        <li>假如给我三天光明</li>
    </ul>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('p').on('click',function(){
        $(this).next().slideToggle(1500,'swing',function(){
                console.log('slide');
            })
    })

</script>
```

![JxRrbS5V2m](D:/Typora/typora-user-images/Jquery/JxRrbS5V2m-1585411406942.gif)

### animate()

> 给元素添加动画
>
> 有4个参数（target，duration(400ms)，easing，callback）





#### start()

> 启动动画

```html
<style>
    .demo{
        position: absolute; 
        width: 100px;
        height: 100px; 
        background-color: orange;
    }
</style>
<button id="startBtn">start</button>
<button id="stopBtn">stop</button>
<button id="finishBtn">finish</button>
<div class="demo"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/jquery-easing/1.4.1/jquery.easing.min.js"></script>
<script>
    $('#startBtn').on('click',function(){
        $('.demo')
        .animate({width:'+=50',height:'+=50',left:'+=100',top:'+=100'},1000,'easeInBounce')
            .delay(1000)
            .animate({left:'+=200'},1000,'easeInOutBounce')
    })
    $('#stopBtn').on('click',function(){
        $('.demo').stop(true,true)
    })
    $('#finishBtn').on('click',function(){
        $('.demo').finish()
    })
</script>
```

![hu7P28et2I](E:/software/apps data/shareX-info/screen-shot/2020-03/hu7P28et2I.gif)

#### stop ()

> 中断当前运动，默认进行下一段运动（如果存在多段animate运动）
>
> 有2个参数，都为Boolean
>
> 参数1表示是否终止运动，true表示结束运动，不会再进行下一段移动
>
> 参数2表示是否停止当前运动，并直接运动当前运动段的到 目标点

stop(true)效果

![tGU83qDtDQ](D:/Typora/typora-user-images/Jquery/tGU83qDtDQ.gif)

stop(true,true)效果

![9FNCKvJR5I](E:/software/apps data/shareX-info/screen-shot/2020-03/9FNCKvJR5I.gif)

#### finish()

> 直接运动到最后一段运动的目标点

![1JdXrqserP](D:/Typora/typora-user-images/Jquery/1JdXrqserP.gif)



#### delay()

> 延迟动画执行，
>
> 参数表示延迟的毫秒数

 



#### jQuery.fx.off

> 是否关闭过渡动画，
>
> jQuery.fx.off = true表示关闭动画过渡

![jK6FKTgcjt](D:/Typora/typora-user-images/Jquery/jK6FKTgcjt.gif)



动画效果扩展插件jQuery.easing

<img src="D:/Typora/typora-user-images/Jquery/image-20200329170911844.png" alt="image-20200329170911844" style="zoom:150%;" />

![image-20200329171038358](D:/Typora/typora-user-images/Jquery/image-20200329171038358.png)



animate可以通过回调的方式进行一段一段动画的操作，不用回调的话，也可以不断的调用aimate方法进行一段一段动画的操作



#### animate内置原理—队列

##### .queue(type,function)

> 该方法接受两个参数，一个是队列的名称，一个是该队列的执行函数，会放到一个对象中，属性名是队列的名称（type）,属性值是存放在数组里的执行函数。
>
>  {type:[function]}
>
> 执行函数接受一个参数Next，该参数表示队列的下一个执行函数，通过调用该参数可以达到执行当当前队列函数后继续执行下一个队列执行函数，而不用通过手动调用

```html
<div class="demo"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').queue('chain',function(next){
        console.log('over1');
        next()
    }).queue('chain',function(next){
        console.log('over2');
        next()
    }).queue('chain',function(next){
        console.log('over3');
        next()
    }) 
</script>

```



##### .dequeue(type)

> 把在某个队列（type）中存放在数组中的对应执行函数按顺序拿出来执行。
>
> 参数表示队列的名称，只有知道名称才能找到该队列的存放执行函数的数组

```javascript
console.log($('.demo').queue('chain'))//查看该队列的数组
console.log( $('.demo').dequeue('chain') )//执行队列的执行函数
console.log($('.demo').queue('chain'))//查看该队列的数组
```

![image-20200329223352021](D:/Typora/typora-user-images/Jquery/image-20200329223352021.png)

##### .clearQueue(type)

> 清空队列，简而言之就是把队列的数组里的执行函数清空

```javascript
console.log($('.demo').queue('chain'))//查看该队列的数组
console.log( $('.demo').clearQueue('chain') )//清空队列数组里的回调函数
console.log($('.demo').queue('chain'))//查看该队列的数组
```

![image-20200329223544770](D:/Typora/typora-user-images/Jquery/image-20200329223544770.png)



在animate内部，animate的这种链式调用方式也是基于这种queue方法去实现的，就类比下面

```html
<div class="demo"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo')
        .css({position:'absolute',left:0,top:0,width:100,height:100,background:'orange'})
        .on('click',function(){
        $(this).dequeue('chain')
    }).queue('chain',function(next){
        $(this).animate({width:110,height:110,left:100,top:100})
        next()
    }).queue('chain',function(next){
        $(this).animate({width:120,height:120,left:200,top:200})
        next()
    }).queue('chain',function(next){
        $(this).animate({width:130,height:130,left:300,top:300})
        next()
    })
</script>
```

![Jwd1t9J5FL](D:/Typora/typora-user-images/Jquery/Jwd1t9J5FL.gif)

上面animate的链式调用是借助了queue队列去实现的。在点击的时候，把之前queue方法存入数组中的所有执行函数一个个取出来，按顺序执行，每执行一个执行函数，就在内部调用animate方法，这样就达到按顺序animate调用的形式，而在真正animate内部的实现方式，也是基于这种方式去实现的



#### animate的queue的原理实现

（将数据存储在dom对象中）

```javascript
jQuery.prototype.myQueue = function(){
    var queueObj = this;//获得操作的jquery对象
    var queueName = arguments[0] || 'fx'：//获得第一个参数
    var addFunc = arguments[1] || null;//获得第二个参数
    var len = arguments.length;	//获得参数的总个数
    //如果长度为1,表明只是获取该存放队列的数组
    if(len == 1){
        return queueObj[0][queueName] ;
    }
    //如果队列对应的数组不存在，创建数组并放入执行函数，否则，往数组中push执行函数
    queueObj[0][queueName] == undefined? queueObj[0][queueName] = [addFunc] : queueObj[0][queueName].push(addFunc)

    //链式调用，要返回修改后的JQuery对象
    return this;

}

jQuery.prototype.myDequeue = function(){
    var self = this;
    var queueName = arguments[0] || 'fx';
    var queueArr = this.myQueue(queueName);//查询该队列名称对应存放执行函数的数组;
    var currFunc = queueArr.shift();//把数组的第一位拿出来执行
    if(currFunc == undefined){//递归的出口，如果数组中取出的值为空
        return;

    }
    //执行该队列的执行函数
    var next = function(){
        self.myDequeue(queueName)
    }
    currFunc(next)
    return this;
}
```

> 1、myQueue方法模拟queue方法；该方法接受两个参数，一个是队列的名称，一个是该队列的执行函数，队列的名称作为jquery对象里的Dom对象的属性名，值为数组，存放该队列的所有执行函数。（在源码中，所有队列都存放在一个全局的data中）。当给该方法传递一个参数时，说明是查询该队列的数组，当给该方法传递两个参数时，表示往该队列中的数组中存放执行函数，如果该队列名不存在，创建一个空数组并加入执行函数，如果该队列名已存在，把执行函数push进去数组中。由于是链式调用，最后要返回修改过后的jQuery对象

> 2、myDequeue方法模拟deQueue方法()，该方法用来逐个取出队列中存在于数组的执行函数，挨个执行。首先先获取到需要执行的队列名称，然后取出该队列名称对应的数组，再用数组shift()方法把首位的执行函数取出执行，把下一个执行函数（next）当成参数传递给该执行函数，如果执行函数内部调用next，就会继续执行下一个执行函数

![image-20200331223514309](D:/Typora/typora-user-images/Jquery/image-20200331223514309.png)



#### 模拟自己的animate

```javascript
jQuery.prototype.myDelay = function(duration){
    var queueArr = this[0]['fx'];
    queueArr.push(function(next){
        setTimeout(function(){
            next()
        },duration)
    })
}


jQuery.prototype.myAnimate = function(){
    var len = this.length;//获得当前jquery对象dom对象个数
    var self = this;

    //最后添加到队列里的内容函数
    var baseFunc = function(next){
        var times = 0;
        for(var i = 0; i < len; i++){
            //json就是一堆CSS代码
            startMove(self[i],json,function(){
                times++;
                if(times == len){
                    callback && callback()
                    next();
                }
            })
        }
    }



    function getStyle (obj, attr) {
        if (obj.currentStyle) {
            return obj.currentStyle[attr];
        }else {
            return window.getComputedStyle(obj,false)[attr];
        }
    }

    function startMove (obj, json, callback) {
        clearInterval(obj.timer);
        var iSpeed;
        var iCur;
        var name;
        obj.timer = setInterval(function () {
            var bStop = true;
            for (var attr in json) {                            
                if (attr === 'opacity') {                                
                    name = attr;
                    iCur = parseFloat(getStyle(obj, attr)) * 100;
                }else {
                    iCur = parseInt(getStyle(obj, attr));
                }                            
                iSpeed = (json[attr] - iCur) / 7;
                if (iSpeed > 0) {
                    iSpeed = Math.ceil(iSpeed);
                }else {
                    iSpeed = Math.floor(iSpeed);
                }
                if (attr === 'opacity') {
                    obj.style.opacity = (iCur + iSpeed) / 100;
                }else {
                    obj.style[attr] = iCur + iSpeed + 'px';
                }
                if (json[attr] - iCur !== 0) {
                    bStop = false;
                }
            }
            if (bStop) {
                clearInterval(obj.timer);
                callback();
            }
        }, 30);
    }  
} 
```

第一个animate是立即执行的，后面的animate需要等待前一个animate执行完成后才开始执行

> Delay解析：

> animate解析：遍历jquery所有的dom对象，给每个对象添加动画，设置times为集数器，等待所有的dom对象完成动画后，执行回调



## jQuery实例方法-位置图形

### .offset()

> 可取可赋，表示获取当前DOM对象距离文档(document)的距离，得到的数值是一个对象{left，top}，不考虑父级是否存在定位
>
> 原生的获取的是有定位元素的父级别 

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .wrapper{
        position: relative;
        width: 200px;
        height: 200px;
        background-color: #f10;
        top: 200px;
        left: 200px;
    }
    .wrapper .demo{
        position: absolute;
        width: 50px;
        height: 50px;
        background-color: #0f0;
        left: 50px;
        top: 50px;

    }
</style>
<div class="wrapper">
    <div class="demo"></div>
    <button>取消wrapper的定位并重新设置demo偏移量</button>


</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>      
    console.log('demo',$('.demo').offset());
    console.log('wrapper',$('.wrapper').offset());
    $('button').on('click',function(){
        $('.wrapper').css('position','static');//取消wrapper定位
        $('.demo').offset({left:100,top:100});//重新给demo设定偏移量
        console.log('demo',$('.demo').offset());
        console.log('wrapper',$('.wrapper').offset());
    })
</script>
```

![QOLMU9Fwme](D:/Typora/typora-user-images/Jquery/QOLMU9Fwme.gif)



### .position()

> 可取不可赋，表示获取当前元素相对于最近的有定位元素的父级的距离，得到的结果是一个对象{left,top}
>
> 类似原生的offsetLeft，offsetTop

```javascript
<script>      
    console.log('wrapper',$('.wrapper').position());
	console.log('demo',$('.demo').position());

$('button').on('click',function(){
    $('.wrapper').css('position','static');//取消wrapper定位
    $('.demo').position({left:100,top:100});//重新给demo设定偏移量
    console.log('wrapper',$('.wrapper').position());
    console.log('demo',$('.demo').position());

})
</script>
```

![8YFPq6hW79](D:/Typora/typora-user-images/Jquery/8YFPq6hW79-1586076110656.gif)

可以发现，赋值对于position方法来说无效。



### .scrollTop()、scrollLeft()

> 可取可赋值，表示获取滚动条滚动的距离。主体是外部容器
>
> 例如$('window').scrollTop()获取的就是超出当前页面最上端的高度，scrollLeft与此同理，也可以理解为获取当前元素滚动条位置
>
> 赋值只能填数字，不能填字符串
>
> 类似原生的window.pageXOffset，windo.pageYOffset

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }

    body {
        width: 2000px;
        height: 2000px;
        background-image: radial-gradient(#00f,#f0f) ;
    }
    button{
        position: fixed;

    }
</style>
<button>获取当前元素滚动条位置</button>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('button').on('click', function () {
        console.log('scrollTop', $(window).scrollTop());
        console.log('scrollLeft', $(window).scrollLeft());
    })

</script>
```

![GmaHgwrJZv](D:/Typora/typora-user-images/Jquery/GmaHgwrJZv.gif)

例二：

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }

    .demo{
        position: relative;
        width: 200px;
        height: 300px;
        border: 1px solid #000;
        overflow: scroll;
        top: 50px;
        left: 50px;
    }
    .demo p{
        width: 2000px;
        height: 2000px;
        background-image: radial-gradient(#00f,#f0f) ;
    }
    button{
        position: fixed;
        left: 0;
        top: 0;
    }
</style>
<div class="demo">
    <p></p>
</div>
<button>获取当前元素滚动条位置</button>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('button').on('click', function () {
        console.log('scrollTop', $('.demo').scrollTop());
        console.log('scrollLeft', $('.demo').scrollLeft());
    })
</script>
```

![hIxcS37ia7](D:/Typora/typora-user-images/Jquery/hIxcS37ia7.gif)

### .width()、.height()

> 获取当前元素的宽高度，只针对内容区(content)的大小，不包括padding,border,返回的结果是数字



### .innerWidth()、.innerHeight()

> 获取当前元素的宽高度，针对内容区（content）+内边距（padding）的大小，不包括border，返回的结果是数字



### .outerWidth()、.outerheight()

> 获取当前元素的宽高度，针对内容区（content）+内边距（padding）+边界（border）的大小，返回的结果是数字
>
> 该方法有一个参数，
>
> 参数1表示是否把margin的距离也算上，默认为false，为true表示
>
> content+padding+border+margin



```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .demo{
        width: 100px;
        height: 100px;
        background-color: orange;
        padding: 15px;
        border: 5px solid #000;
        margin: 20px;

    }
</style>
<div class="demo"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    //content
    console.log('width',$('.demo').width())
    console.log('height',$('.demo').height())
    //content + padding
    console.log('innerWidth',$('.demo').innerWidth())
    console.log('innerHeight',$('.demo').innerHeight())
    //content + padding+border
    console.log('outerWidth',$('.demo').outerWidth())
    console.log('outerHeight',$('.demo').outerHeight())
    //content + padding + border + margin
    console.log('outerWidth+margin',$('.demo').outerWidth(true))
    console.log('outerHeight+mragin',$('.demo').outerHeight(true))
</script>
```

![image-20200405180352463](D:/Typora/typora-user-images/Jquery/image-20200405180352463.png)

![image-20200405180326951](D:/Typora/typora-user-images/Jquery/image-20200405180326951.png)





## jQuery实例方法-遍历索引

### .each()

> 对选中的多个dom元素进行循环，参数为函数，函数的两个参数分别为当前dom元素的索引和当前原生dom
>
> 该方法类似数组的forEach方法
>
> 该方法接受两个参数，第一个是数据，第二个是函数（index,ele）



给元素添加内容和属性

```html
<ul class="demo">
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo li').each(function(index,ele){
        $(ele)
            .text(index)
            .addClass('demo'+index)
    })
</script>
```

![image-20200405185710175](D:/Typora/typora-user-images/Jquery/image-20200405185710175.png)





### index

> 获得当前元素在兄弟中的索引
>
> 有一个参数，填选择器，表示在同类元素中的索引如果点击的不是同类元素，返回-1

```html
<div class="demo">
    <span>span-1</span>
    <i>i-1</i>
    <span>span-2</span>
    <i>i-2</i>
    <span>span-3</span>
    <i>i-3</i>
</div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $('.demo').children().on('click',function(e){
        console.log($('.demo span').index($(e.target)))
    })
</script>
```

![2SPYdeYLuB](D:/Typora/typora-user-images/Jquery/2SPYdeYLuB.gif)



## jQuery工具方法

### $.type

> 判断数据类型，$.isArray(), \$.isFunction() ,\$.isWindow()...，
>
> 是typeof的升级版
>
> typeof（）里面的数据是什么类型
>
> $.type（）里面的数据到底是什么

```html
<script>
    console.log('---------javaScript-----------')
    console.log('未定义:',typeof undefined)             //undefined
    console.log('空:',typeof null)                      //object
    console.log('字符串:',typeof '123')                 //string
    console.log('数字:',typeof 123)                     //number
    console.log('布尔:',typeof true)                    //boolean
    console.log('对象:',typeof {})                      //object
    console.log('数组:',typeof [])                      //object
    console.log('日期构造函数',typeof new Date());       //object
    console.log('自定义构造函数:',typeof new Person())   //object
    console.log('包装类',typeof new Number())           //object
    console.log('函数:',typeof function(){})            //function


    function Person(){}

</script>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    console.log('---------jQuery-----------')
    console.log('未定义',$.type(undefined)); //undefined
    console.log('空',$.type(null));      //null   typeof是 object
    console.log('字符串',$.type('123'));     //string
    console.log('数字',$.type(123));       //number
    console.log('布尔',$.type(true));      //boolean
    console.log('对象',$.type({}));        //object
    console.log('数组',$.type([1,2,3]));   //array
    console.log('日期构造函数',$.type(new Date));  //date
    console.log('自定义构造函数',$.type(new Person()));//object
    console.log('包装类',$.type(new Number())+ typeof new Number())//手动加了个object，便于区分，jQuery在这点做的不够好
    console.log('函数',$.type(function(){}));//function

    function Person(){}
</script>
```

![image-20200405205415402](D:/Typora/typora-user-images/Jquery/image-20200405205415402.png)



#### $.isArray()

> 判断所填参数是否是数组，基本雷同于JS的isArray方法

```javascript
var arr = [1,2,3]
$.isArray(arr)	//true
```



#### $.isFunction()

> 判断所填参数是否是函数，如果填入的是立即执行函数，会等到函数执行完毕后在判断，符合常理

```javascript
console.log($.isFunction(function(){}()))	//false,因为拿的是立即执行函数的结果进行判断
console.log($.isFunction(function(){}));	//true
```



#### $.isWindow()

```javascript
console.log($.isWindow(window)) //true
```





### $.trim()

> 消除空格，文字与文字之间的无法消除，等同于js字符串的trim方法，返回的结果会变成字符串类型



```javascript
$.trim('    11213 32    ')	//"11213 32"
var str = '   11213 32   '
str.trim()					//"11213 32"
```



### $.proxy()

> 改变this指向，改变的方法类似于JS的bind。与bind不同的是第一个参数是方法，而不是指向
>
> 表示的是把一个目标方法里面的this的指向改变一下，形成一个新的方法返回，
>
> 有两个参数，
>
> 第一个参数表示函数(方法)，
>
> 第二个参数表示指向目标
>
> 表示将第一个参数方法里边的this指向第二个参数

```javascript
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    function Person(){
    console.log(this.name);
    console.log(this.age);
}

var obj = {
    name:'HSM',
    age:'18'
}

var person = $.proxy(Person,obj)
person() // HSM  18
</script>
```

单对象编程例子

```html
<div id="demo" style="width: 100px; height: 100px; background-color: #f10;"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    var list = {
        init:function(){
            this.msg = 123;
            this.dom = document.getElementById('demo')
            this.bindEvent();   //this指向list
        },
        bindEvent:function(){   
            //谁点击，this就指向谁，这里的（this.show）中的this指向了dom ,因此需要改变this指向
            this.dom.onclick = $.proxy(this.show,this)
            //等同于如下bind
            // this.dom.click = this.show.bind(this)
        },
        show:function(){
            console.log(this.produseMs(this.msg))
        },
        produseMs:function(ms){
            return ms + 234;
        }
    }
    list.init();
</script>
```

![R1tbPVGI3G](D:/Typora/typora-user-images/Jquery/R1tbPVGI3G.gif)



找this的时候如果发现问题，应该往上面去找，而不是在当前区域找，因为问题发生在被调用的时候，而不是发生在执行的时候



### $.noConflict()

> 防止$变量命名冲突

```javascript
var $c = $.noConflict()//将$全局变量转接到$c身上
```



### $.each()

> 针对数组、类数组、对象的遍历循环，目前该方法已经不常用了，被forEach替代了

```javascript
var arr = [1,2,3,4]
$.each(arr,function(index,ele){
    console.log(index,'----',ele)
})
```

#### $.map()

> 针对数组的每一位进行操作，不改变原数组，返回新的数组

```javascript
var arr = [1,2,3,4]
var result = $.map(arr,function(index,ele){
   return ele*index
})
console.log(result); //[0,2,6,12]
```



### $.parseJSON()

> 把严格符合json形式的字符串转换成对象，与原生JSON.parse()一模一样，抄的JSON.parse()
>
> JSON字符串形式：最外边一定是单引号，属性名一定要双引号扩起来

```javascript
var json = '{"a":123,"b":"str","c":true}';
var a = $.parseJSON(json)
console.log(a);
//{a: 123, b: "str", c: true}
```

stringify方法jQuery没有实现，因为jQuery认为这方法不常用，没必要实现，如果有需要，直接使用原生的SON.stringify())方法

```javascript
JSON.stringify(a)
//"{"a":123,"b":"str","c":true}"
```





### $.makeArray()

> 类数组转换成数组，不改变原来的类数组，返回变化后的新数组
>
> 至多添加两个参数
>
> 如果填一个参数，表示把该参数转换成数组
>
> 如果填两个参数，会把第一个参数的内容添加到第二个参数中

一个参数

```javascript
var obj = {
	0:'a',
	1:'b',
	2:'c',
	length:3
}
var arr = $.makeArray(obj)
console.log(arr); //["a","b","c"]
```

两个参数

```javascript
var obj = {
	0:'a',
	1:'b',
	2:'c',
	length:3
}
var arr = [1,2,3,4]
console.log($.makeArray(obj,arr))//(7) [1, 2, 3, 4, "a", "b", "c"]

console.log($.makeArray(arr,obj))
//{0: "a", 1: "b", 2: "c", 3: 1, 4: 2, 5: 3, 6: 4, length: 7}
```



### \$.extend()、\$.fn.extend()

> 两个方法本质上差不多。只有一些小小的区别，都是插件扩展（工具方法），
>
> $.extend()用处就是可以把自己自定义的函数和方法加入到jquery里的**工具方法**里面\$.fn.extend()是加到**实例方法**里面。
>
> 参数是json形式



产生输入的两个数直接的随机数，左闭右开区间（用$.extend()）

```html
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $.extend({
        difinedMandom:function(start,final){//产生随机数
            var len = final - start;
            return Math.random()*len + start
        }
    })

    console.log($.difinedMandom(1,5))	//产生1-5之间的随机数
</script>
```

![Bofbv2w6KG](D:/Typora/typora-user-images/Jquery/Bofbv2w6KG.gif)



元素的拖拽（用$.fn.extend()）

```html
<div id="demo" style="width: 100px; height: 100px; background-color: deeppink; position: relative;"></div>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    $.fn.extend({
        drag : function(){
            var disX,  
                disY,  
                self = this;
            $(this).on('mousedown',function(e){
                disX =  e.pageX - $(this).offset().left;//距离元素最左边的水平距离
                disY =  e.pageY - $(this).offset().top;//距离元素最上边的垂直距离

                $(document).on('mousemove',function(e){
                    $(self).css({left:e.pageX - disX, top:e.pageY - disY})
                })
                $(document).on('mouseup',function(e){
                    $(document).off('mousemove').off('mouseup')
                })
                console.log(disX,disY);

            })
            return this
        }
    })

    $('#demo').drag()
</script>
```

![awCZ1ohVMt](D:/Typora/typora-user-images/Jquery/awCZ1ohVMt.gif)

dis保存鼠标在点击demo元素的时候的位置，当拖动元素时（move）,以元素的最左边为参考单位，移动的距离（left）等于当前鼠标悬停时距离文档的位置，（以X轴来说，y轴同理）减去鼠标点击的点距离方块最最边的距离。

![image-20200406165841098](D:/Typora/typora-user-images/Jquery/image-20200406165841098.png)





在克隆问题上，这两个方法基本是一模一样的

```javascr

$.extend = $.fn.extend = funtion(){

​		this --->  $;

​		this ---> this.fn

}	
```



浅层克隆

>  $.extend（参数1，参数2，参数3....），
>
>  \$.fn.extend（参数1，参数2，参数3....）
>
>  把后边参数的数据克隆到参数1中去，相同的覆盖，不同的加入obj1，对于引用值，直接复制其引用





深层克隆

> $.extend（true，参数1，参数2，参数3....） 
>
> \$.fn.extend（参数1，参数2，参数3....）
>
> 深层克隆对于对象克隆的不是引用值，而是新创建的一个对象



### $ajax()

> 发送网络请求
>
> BaseUrl: [https://developer.duyiedu.com/jq/movie
>
> $.ajax是有返回值的，而返回值就是promise的\$.deferred()
>
> 参数为对象，接受的属性有

- url：请求地址
- type：请求类型
- data：需要发送给服务器的数据
- success：请求成功后的处理函数
- error：请求失败后的处理函数
- complete：完整的请求发送后，不管成功或失败都会触发处理函数，最后执行
- context：改变函数上下文，可以修改this的指向，例如$.context(\$('.wrapper'))把处理函数中的this指向.wapper元素
- timeout：请求超时时间，
- async：是否异步，默认异步（true），异步就是非阻塞，非异步就是阻塞，异步任务会将ajax()放到事件队列中去，等待同步任务执行完成后再执行，而不会阻塞同步任务的执行
- dataType：’jsonp‘，期待服务器返回的数据类型，期待为jsonp类型，如果服务器返回的不是jsonp类型，会触发error，如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断。





### $.Callbacks(’once‘|'unique'|'memory')回调

> 当异步任务完成后处理的函数就叫回调函数。异步就是JS线程不监听，而是交给其他线程去监听（比如网络线程），回调就是事件被触发后的函数，从事件队列 中被放到执行栈中去执行的函数，这个函数就是回调函数
>
> **由于JS是单线程，因 此异步编程必然逃脱不了回调，但回调并不一定都要在异步的情况下，满足某种条件就执行也叫回调，回调是个广泛的概念**



> 调用该回调方法会返回一个回调对象，这个回调对象就相当于一个大框，它里面可以放一些你想要回调的处理函数。
>
> 参数有4个：
>
> - 'once'：fire()方法只执行一次，即使多次调用这个方法，内部原理是在fire过一次后，判断是否存在Once参数，如果存在，把存放方法的数组清空，达到只能fire一次的目的
> - 'memory'：表示只要fire过，后面即使加入新的处理函数，也会立即执行。
> - ‘stopOnFalse’：fire方法执行时，遇到return false的处理函数，终止执行，队列后续的处理函数不再执行
> - ’unique’：即使用add方法加入多次同个处理函数，也只会执行一次，默认是可以执行多次的
>
> 
>
> .add( )方法往该框里加处理函数)，等到需要使用的使用，用.fire()方法，
>
> .fire()方法可以接受多个参数，会当成回调的处理函数的参数



**once方法例子—多次调用fire方法：**

```javascript
function a(x,y){
    console.log('a',x,y);
}
function b(x,y){
    console.log('b',x,y);
}
var callback = $.Callbacks('once');
callback.add(a,b)
```

![image-20200408152724538](D:/Typora/typora-user-images/Jquery/image-20200408152724538.png)

如果不加once关键字时：

![image-20200408152844744](D:/Typora/typora-user-images/Jquery/image-20200408152844744.png)

可以看到fire方法会执行两次。



**memory方法例子**：

> 只要fire一次过，后加的处理函数也能立即执行

不加memory时

```javascript
function a(x,y){
    console.log('a',x,y);
}
function b(x,y){
    console.log('b',x,y);
}
function c(x,y){
    console.log('c',x,y)
}

var callback = $.Callbacks();
callback.add(a,b)
callback.fire(10,20)
callback.add(c)
```

![image-20200408153331476](D:/Typora/typora-user-images/Jquery/image-20200408153331476.png)

加了memory后

```javascript
var callback = $.Callbacks('memory');
```

![image-20200408153509479](D:/Typora/typora-user-images/Jquery/image-20200408153509479.png)



**stopOnFalse：**

> fire方法执行时，遇到return false的处理函数，终止执行，队列后续的处理函数不再执行

默认情况下

```javascript
function a(x,y){
    console.log('a',x,y);
}
function b(x,y){
    console.log('b',x,y);
    return false
}
function c(x,y){
    console.log('c',x,y)
}

var callback = $.Callbacks();
callback.add(a,b,c)
callback.fire(10,20)
```

![image-20200408154124140](D:/Typora/typora-user-images/Jquery/image-20200408154124140.png)



加了stopOnFalse后

```javascript
var callback = $.Callbacks('stopOnFalse');
```

![image-20200408154024564](D:/Typora/typora-user-images/Jquery/image-20200408154024564.png)



**unique**

> 相同处理函数，只执行一次，即使回调对象中存在多个相同处理函数

默认情况下

```javascript
function a(x,y){
    console.log('a',x,y);
}
function b(x,y){
    console.log('b',x,y);
    return false
}


var callback = $.Callbacks();
callback.add(a,b)
callback.fire(10,20)

```

![image-20200408154345719](D:/Typora/typora-user-images/Jquery/image-20200408154345719.png)

加了unique参数后

```javascript
var callback = $.Callbacks('unique');
```

![image-20200408154444728](D:/Typora/typora-user-images/Jquery/image-20200408154444728.png)



为什么需要回调？

内置函数：因为JS是单线程，异步编程能优化体验，防止阻塞页面，回调函数最终会回到JS线程中执行

运动函数：animate当你满足某个状态，接下来要去做另外一件事情



#### $.Callbacks原理实现

> 只实现once和memory和once memory三种情况

```javascript
jQuery.myCallbacks = function(){
    //接受参数：'once' 'memory'  'once memory' null
    //1.存储参数
    var options = arguments[0] || '';//'once memory'算一个参数，因为都在一个字符串中
    //2.存放通过add来加入的方法
    var list = []       
    //3.记录当前要执行的函数的索引
    var fireIndex = 0;
    //4. myCallbacks使用memory参数时需要用到的变量，用于记录哪个方法（函数）已经被fire执行过
    var fired = false
    //5. 存放通过fire方法传入的参数
    var args = [];

    var fire = function(){
        //把所有的方法拿出来执行
        for(; fireIndex < list.length; fireIndex++){
            list[fireIndex].apply(window,args);//用apply是因为args是数组
        }
        //once参数的判断：该参数让fire()方法只能执行一次,内部实现是把存放方法的数组清空，达到只能执行一次的目的
        if(options.indexOf('once')!= -1){
            list = [];
            fireIndex = 0;//保险，强制归零
        }
    }

    //调用该工具类方法，最终会返回一个对象，该对象里边有add，fire等各种方法
    return {
        //把方法加入到list中
        add:function(){
            for(var i = 0; i < arguments.length; i++){
                if(arguments[i] instanceof Array){
                    list = list.concat(arguments[i])
                }else if(typeof arguments[i] == 'function'){
                    list.push(arguments[i])
                }else{
                    console.log( new TypeError('类型错误，只能传变量或数组'));
                }
            }

            //memory参数的判断：在第一次fire过后的add中判断，如果memory参数存在并且被fire过了的话,调用add方法时就直接调用fire方法
            if(options.indexOf('memory') != -1 && fired){
                fire() //调用add方法后，list中的方法+1，继续调用fire,循环又能继续了
            }

            return this
        },
        //把list中的方法拿出来挨个执行，fire方法可以接收参数给list中的方法当成参数执行
        fire:function(){
            fireIndex = 0 //归零，因为fireIndex在上面被当成索引来使用，不归零无法重新开始
            args = arguments;
            fired = true;//memory的变量：
            fire()
        }
    }
}
```

1.测试**不传参数情况**

```javascript
var cb = $.myCallbacks();   //不传任何参数的情况
//验证$.myCallbacks不传参数的情况
cb.add([a,b]);
cb.add(b);
cb.fire('$.myCallbacks()不传参数');
cb.fire('$.myCallbacks()不传参数');
```

![image-20200409213920893](D:/Typora/typora-user-images/Jquery/image-20200409213920893.png)

**正确**



**2. 测试once情况**：——理想结果，fire只执行一次

```javascript
var cbOnce = $.myCallbacks('once'); //传once的情况   
//验证$.myCallbacks传once参数的情况
cbOnce.add([a,b]);
cbOnce.add(b);
cbOnce.fire('$.myCallbacks()传once参数');
cbOnce.fire('$.myCallbacks()传once参数');
```

![image-20200409214020093](D:/Typora/typora-user-images/Jquery/image-20200409214020093.png)

**正确**，fire只执行一次，第二次无法再执行，因为存放方法的数组已被清空



**3. 测试传memory情况**——理想结果，存放方法的数组不会重新执行，因为索引没有归零，而是延长了数组长度后再次调用fire方法

```javascript
var cbMemory = $.myCallbacks('memory') //传memory的情况

//验证$.myCallbacks传memory参数的情况
cbMemory.add(a);
cbMemory.add(b);
cbMemory.fire('$.myCallbacks()传memory参数');
cbMemory.add(c);
cbMemory.fire('$.myCallbacks()传memory参数');
```

![image-20200409214540809](D:/Typora/typora-user-images/Jquery/image-20200409214540809.png)

**正确**

**4. 测试once + memory的情况**——理想结果，在第一次fire过后的add方法，会根据memory方法，去延长fire，而第二次的fire，会根据once方法，把存放方法的数组清空，达到无法第二次fire

```javascript
var cbOnceMemory = $.myCallbacks('once memory') //传once和memory的情况,这个也是一个参数，因为都在一个字符串中
// //验证$.myCallbacks传once 和memory参数的情况
cbOnceMemory.add(a);
cbOnceMemory.add(b);
cbOnceMemory.fire('$.myCallbacks()传once和memory参数');
cbOnceMemory.fire('$.myCallbacks()传once和memory参数');
cbOnceMemory.add(c);

```

![image-20200409214939545](D:/Typora/typora-user-images/Jquery/image-20200409214939545.png)

**正确**





### $.deferred()异步

> $.ajax是有返回值的，而返回值就是promise的\$.deferred()，所谓\$.deferred()，就是有状态的回调。
>
> 调用该方法会返回一个延迟对象，该延迟对象包括三个推向状态的方法和三个状态的执行方法
>
> 三个推向状态的方法：resolve【成功】，reject【失败】，notify【继续】。可以传递参数，会被执行方法的回调函数接收。推向resoltve或reject状态后便无法更改，类似promise
>
> 三个状态的执行方法：done【成功】，fail【失败】，progress【继续】

![image-20200409141412092](D:/Typora/typora-user-images/Jquery/image-20200409141412092.png)



例一：

```javascript
df = $.Deferred();

df.done(function(){
    console.log('运行成功');
})

df.fail(function(){
    console.log('运行失败');
})

df.progress(function(){
    console.log('正在启动中...');
})


setInterval(function(){
    var score = Math.random() * 100;
    if(score > 60){
        df.resolve()
    }else if(score < 50){
        df.reject()
    }else{
        df.notify()
    }
},500)

```

![AseMgX2six](D:/Typora/typora-user-images/Jquery/AseMgX2six.gif)

结论：

- 当notify方法被调用时，说明是个继续状态，还未结束，需要继续，直到变成resolve状态或reject状态才会停止

- 推向状态的三个方法是可以在外部被干预（调用）的，因为这几个方法就存在延迟对象中

![image-20200409141412092](D:/Typora/typora-user-images/Jquery/image-20200409141412092.png)

```javascript
df.resolve()//手动调用该方法
df.reject()//手动调用该方法
```

![image-20200409145450985](D:/Typora/typora-user-images/Jquery/image-20200409145450985.png)

如果我们希望这几个改变状态方法在外部被使用的话，可以使用该延迟对象的.promise方法。该方法会导致延迟对象只返回三个状态的执行方法，而不返回三个推向状态的方法，因此，这样外部就无法手动改变这个状态。



例2：用promise方法

```html
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    function creatScore() {
        const df = $.Deferred();
        setInterval(function () {
            var score = Math.random() * 100;
            if (score > 60) {
                df.resolve('nice')
            } else if (score < 40) {
                df.reject('oh my God')
            } else {
                df.notify('go on')
            }
        }, 500)
        return df.promise()
    }

    var df = creatScore()
    df.done(function (res) {
        console.log(`运行成功${res}`);
    })

    df.fail(function (res) {
        console.log(`运行失败${res}`);
    })

    df.progress(function (res) {
        console.log(`正在启动中...${res}`);
    })
</script>
```

![image-20200409151454865](D:/Typora/typora-user-images/Jquery/image-20200409151454865.png)

可以发现，该延迟对象中已经没有那三个推向状态的方法了，而且调用该方法的时候会报错。



#### then方法

> 简化注册的写法。
>
> 跟promise的then类似，第一个是成功的回调，第二个是失败的回调，第三个是继续的回调。
>
> then方法默认返回一个promise方法的延迟对象，return 表示传递参数给下一个then的后续处理函数，如果return 返回的是延迟对象，后面的then就变成了该延迟对象的then了。



**用处一：该方法也跟promise的then一样支持串联**

```html
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    function creatScore() {
        const df = $.Deferred();
        setInterval(function () {
            var score = Math.random() * 100;
            if (score > 60) {
                df.resolve('nice')
            } else if (score < 40) {
                df.reject('bad')
            } else {
                df.notify('go on')
            }
        }, 500)
        return df.promise()
    }

    var df = creatScore()

    df.then(function(res){
        console.log(`运行成功${res}`);
        return 'very nice'
    },function(res){
        console.log(`运行失败${res}`);
        return 'very bad'
    },function(res){
        console.log(`正在启动中...${res}`);
        return 'keep go on'
    }).then(function(msg){
        console.log(`再次运行成功${msg}`);
    },function(msg){
        console.log(`再次运行失败${msg}`);
    },function(msg){
        console.log(`还在启动中${msg}`);
    })
</script>
```

![dbbL1HEZLh](D:/Typora/typora-user-images/Jquery/dbbL1HEZLh.gif)



deferred还是于promise有效不同的，promise下一个then的状态，取决于resolve的then的后续处理函数的运行情况，如果该后续处理函数运行成功，then返回的promise状态就是resolve，如果运行失败，then返回的promise状态就是reject





**用处二：如果前一个then处理完之后return返回的是一个全新的deferred延迟对象，而不是字符串，之后的then会依据该新的promise进行处理，也是有点类似promise，但比promise容易理解些**

```html
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
    function creatScore() {
        const df = $.Deferred();
        setInterval(function () {
            var score = Math.random() * 100;
            if (score > 60) {
                df.resolve('nice')
            } else if (score < 40) {
                df.reject('bad')
            } else {
                df.notify('go on')
            }
        }, 500)
        return df.promise()
    }

    var df = creatScore()

    df.then(function(res){
        console.log(`运行成功${res}`);
        var innerDf  = $.Deferred();
        setInterval(function(){
            innerDf.resolve('very good');
            return innerDf.promise();
        },1000)
        return innerDf
    },function(res){
        console.log(`运行失败${res}`);
        var innerDf  = $.Deferred();
        setInterval(function(){
            innerDf.reject('very bad');
            return innerDf.promise();
        },1000)
        return innerDf
    },function(res){
        console.log(`正在启动中...${res}`);
        var innerDf  = $.Deferred();
        setInterval(function(){
            innerDf.notify('keep go on');
            return innerDf.promise();
        },1000)
        return innerDf
    }).then(function(msg){
        console.log(`再次运行成功${msg}`);
    },function(msg){
        console.log(`再次运行失败${msg}`);
    },function(msg){
        console.log(`还在启动中${msg}`);
    })
</script>
```

返回的参数会落到下一个then中相应的执行方法的参数中。

![fNRFF0Yk60](D:/Typora/typora-user-images/Jquery/fNRFF0Yk60.gif)







### $.Deferred原理实现

> 仅实现推向状态的三个方法和三个状态的执行方法

```javascript
//工具方法：仅实现推向状态的三个方法和三个状态的执行方法
jQuery.myDeferred = function(){
    //本质上是三个callbacks，三个callbacks对应有三个不同的存放方法的数组（list）
    // done resolve   fail rejcet   progress notify
    var arrCallbacks = [
        [   
            jQuery.myCallbacks('once memory'),'done','resolve'
        ],[
            jQuery.myCallbacks('once memory'),'fail','reject'
        ],[
            jQuery.myCallbacks('memory'),'progress','notify' 
        ]
    ]
    //该变量用于后面多次调用推向状态方法时，阻止再次执行，因为推向状态后无法再改变，为true表示还是挂起状态，状态还未更改
    var pendding = true;
    //定义一个对象，该对象是调用myDeferred工具方法返回的对象， done,resolve,fail,rejcet,progress,notify
    //方法都定义在其中，接下来，就是如何往deferred中加方法。
    var deferred = {};
    for(var i = 0; i < arrCallbacks.length; i++){
        //注册
        //deferred['done'] = function(){}
        //deferred['fail'] = function(){}
        //deferred['progress'] = function(){}
        deferred[arrCallbacks[i][1]] = (function(index){//往deferred添加各种状态的执行方法，done,fail,progress
            return function(func){  //func函数是调用done/fail/progress传入的后续处理函数
                arrCallbacks[index][0].add(func)  //往调用jQuery.myCallbacks()返回的延迟对象中的add方法中添加方法（函数）
            }
        })(i)//这里必须写立即执行函数，因为要保存每次循环的i,否则由于函数不会立即执行，而循环已经完毕，导致i==length

        //触发
        //deferred['resolve'] = function(){}
        //deferred['reject'] = function(){}
        //deferred['notify'] = function(){}
        deferred[arrCallbacks[i][2]] = (function(index){
            return function(){
                var args = arguments;
                if(pendding){
                    arrCallbacks[index][0].fire.apply(window,args)//调用不同的callbacks，执行不同list中存放的方法,用apply只是方便把数组当做参数传入而已，this指向在这里没有任何意义
                    arrCallbacks[index][2] == 'resolve' ||arrCallbacks[index][2] == 'reject' ? pendding = false : pendding = true
                }
            }
        })(i)


    }
    console.log(deferred);
    return deferred
}
```

![image-20200409233235489](D:/Typora/typora-user-images/Jquery/image-20200409233235489.png)

![sEhJQUnArD](D:/Typora/typora-user-images/Jquery/sEhJQUnArD.gif)

### $.when

> 该工具方法类似Promise的工具方法all，一个失败全失败，全部成功才成功。
>
> 参数可以是一个或多个延迟对象，
>
> 返回值是一个Promise对象
>
> 该when方法也有一个then方法，该方法的成功就是全部延迟对象返回resolve才执行成功的后续处理函数，只要有一个延迟对象返回一个reject就执行失败的后续处理函数。
>
> 该方法的应用场景在于同时处理多个延迟对象，汇总结果进行操作

```javascript
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>

    $.when(creatScore1(),creatScore2(),creatScore3()).then(function(){
    console.log('全部resolve咯');
},function(){
    console.log('可恶，有reject的存在');
})

function creatScore1() {
    const df = $.Deferred();
    setInterval(function () {
        var score = Math.random() * 100;
        if (score > 50) {
            df.resolve('nice')
        } else if (score < 40) {
            df.reject('oh my God')
        } else {
            df.notify('go on')
        }
    }, 500)
    return df.promise()
}

function creatScore2() {
    const df = $.Deferred();
    setInterval(function () {
        var score = Math.random() * 100;
        if (score > 50) {
            df.resolve('nice')
        } else if (score < 40) {
            df.reject('oh my God')
        } else {
            df.notify('go on')
        }
    }, 500)
    return df.promise()
}

function creatScore3() {
    const df = $.Deferred();
    setInterval(function () {
        var score = Math.random() * 100;
        if (score > 50) {
            df.resolve('nice')
        } else if (score < 40) {
            df.reject('oh my God')
        } else {
            df.notify('go on')
        }
    }, 500)
    return df.promise()
}
</script>
```

![t4FxZUHkpL](D:/Typora/typora-user-images/Jquery/t4FxZUHkpL.gif)