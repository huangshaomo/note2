

# CSS3

## 1.介绍



|  prefix  |    browser     |
| :------: | :------------: |
| -webkit- | chrome，safari |
|  -moz-   |     mozila     |
|   -ms-   |       IE       |
|   -o-    |     opera      |

**预处理器**	pre-processor

less/sass	：往后的可维护性不高（因为已经经由sass编译后的.css代码无法消失）

cssNext插件：用来实现一些未来的标准的（未完全在各大浏览器实现的）



**后处理器**	post-processor

autoprefixer：补齐CSS兼容性代码，而且往后的可维护性高（因为撤掉该插件后补齐的代码也跟着自动消失）

post+插件（充分体现扩展性，200多个）



postCss：用JS实现的CSS的抽象的语法树（AST 	Abstract Syntax  Tree），上面的预处理器和后处理器都是居于该语法树进行实现的，所以postCss既不算是后处理器，也不算是预处理器，需要配合插件进行使用实现。



## 2.选择器类型（selector）	

### 2.1. 关系形选择器（Relationship Selectors）

#### 2.1.1 E + F(css2)

> 当前元素的下一个满足条件的兄弟元素节点

```html
<style>
    /* E + F :当前元素的下一个满足条件的兄弟元素节点*/
    div + p{
        background-color: red;
    }
</style>
<div>div</div>
<span class="demo">234</span>
<p class="demo">1</p>
<p>2</p>
<p>2</p>
<ul>
    <li>
        <p>4</p>
    </li>
</ul>
```

div + p的意思是：找到div元素下的下一个兄弟元素节点，看看该元素节点是不是p，很明显不是，div的下一个兄弟元素节点是span，因此，想要匹配第一个p，上面的要改为span + p

#### 2.1.2 E ~ F

> 当前元素的下所有满足条件的兄弟元素节点

```html
<style>
    /* E ~ F :当前元素的所有满足条件的兄弟元素节点*/
    div ~ p{
        background-color: red;
    }
</style>
<div>div</div>
<span class="demo">234</span>
<p class="demo">1</p>
<p>2</p>
<p>2</p>
<ul>
    <li>
        <p>4</p>
    </li>
</ul>
```

div ~ p的意思是：找到div元素下的所有的兄弟元素节点p，只要存在兄弟元素节点p，就会被选中



#### 2.1.3 E > F

> 只选择当前元素的直接后代（即子元素），而不选择其它后代的选择器。

```html
<style>
    /* E + F：当前元素下所有满足条件的子元素节点，不包括孙子节点*/
    div > p{
        background-color: red;
    }
</style>
<div>
    <p>222</p>
    <p>333<h6><p>hp2</p></h6></p>
    <h6><p>hp22</p></h6>
</div>
<span class="demo">234</span>
<p class="demo">1</p>
<p>2</p>
<p>3</p>
```

上面的只有div下面的两个p被选中，不包括p里面的后代



### 2.2. 属性选择器（Attribute Selectors）

#### 2.2.1 E[attr~="val"]（css2）

> 选择属性的值是用空格分隔的多个单词，其中一个单词的值等于 val的E元素

```html
<style>
    div[class~="a"]{
        background-color: green;
    }
</style>
<div class="a " >111</div>
<div class="a b">222</div>
<div class="aa">333</div>
```

上面的第一和第二个div被选中



#### 2.2.2 E[attr|="val"]

> 选择属性的值是用连字符"-"分隔的单词，并以 val 开头的E元素，主要用于 lang 属性，比如 "en"、"en-us"、"en-gb" 等

```html
<style>
    div[class~="a"]{
        background-color: green;
    }
</style>
<div class="a " >111</div>
<div class="a-testb">222</div>
<div class="b-test">333</div>
```

上面的第二个div被选中



#### 2.2.3 E[attr^="val"]

> 选择属性的值以 val 开头的E元素，val 为完整的单词或单词的一部分

```html
<style>
    div[class^="a"]{
        background-color: green;
    }
</style>
<div class="a " >111</div>
<div class="a-test">222</div>
<div class="b-test">333</div>
```

上面的第一和第二个div被选中



#### 2.2.4 E[attr$="val"]

> 选择属性的值以 val 结尾的E元素，val 为完整的单词或单词的一部分

```html
<style>
    div[class$="t"]{
        background-color: green;
    }
</style>
<div class="a " >111</div>
<div class="a-test">222</div>
<div class="b-test">333</div>
```

上面的第二和第三个div被选中



#### 2.2.5 E[attr*="val"]

> 选择属性的值包含 val 子字符串的E元素

```html
<style>
    div[class$="-"]{
        background-color: green;
    }
</style>
<div class="a " >111</div>
<div class="a-test">222</div>
<div class="b-test">333</div>
```

上面的第二和第三个div被选中



### 2.3. 伪元素选择器（Pseude-Element-Selectors）

#### 2.3.1 E::placeholder

> 只能改变input框placeholder提示文字的样式，兼容性极差

```html
<style>
    input::placeholder{
        color: green;
    }
</style>
<input type="text" placeholder="请输入用户名">
```

改变placeholder颜色为绿色



#### 2.3.2 E::selection

> 改变字体选中后的样式，只能设置三个样式：color、background、text-shadow，兼容性挺好

```html
<style>
    div:nth-of-type(1)::selection{
        color: #fff;
        background-color: yellow;
    }
    div:nth-of-type(2)::selection{
        color: #fff;
        background-color: red;
    }
</style>   
<div>小黄黄</div>
<div>小红红</div>
```



### 2.4 伪类选择器

> 被选择元素的一种状态

#### 2.4.1 E:not(s)、E:root、E:target

**E:not(s)**

> 选择将某个元素排除在外

```html
<style>
    div:not([class="demo"]){
        background-color: green;
    }
</style>
<div class="demo">demo1</div>
<div class="demo">demo2</div>
<div>demo3</div>
```

上面表示找出class不为demo的元素节点，第三个被选中



**E:root**

> 根选择器，在HTML中根是HTML，在XML中根是note

```html
:root{
	background-color: green;
}
//在HTML中等价于
html{
	background-color: green;
}
```



**E:target**

> 被标记成锚点之后（localtion.hash = xxx），xxx为id

```html
<style>
:target
{
border: 2px solid #D4D4D4;
background-color: #e5eecc;
}
</style>
<h1>这是标题</h1>

<p><a href="#news1">跳转至内容 1</a></p>
<p><a href="#news2">跳转至内容 2</a></p>

<p>请点击上面的链接，:target 选择器会突出显示当前活动的 HTML 锚。</p>

<p id="news1"><b>内容 1...</b></p>
<p id="news2"><b>内容 2...</b></p>

<p><b>注释：</b> Internet Explorer 8 以及更早的版本不支持 :target 选择器。</p>

```



案例：点击不同的按钮切换不同的背景色

```html
<style>
    :root,
    body{
        margin: 0;
        height: 100%;
    }
    #red,#green,#gray{
        height: 100%;
    }
    #red{
        background-color: red;
    }
    #green{
        background-color: green;
    }
    #gray{
        background-color: gray;
    }
    div.button-wrapper{
        position: absolute;
        width: 600px;
        height: 400px;
        top: 400px;
    }
    div.button-wrapper a{
        text-decoration: none;
        color: #fff;
        background-color: pink;
        font-size: 30px;
        border-radius: 3px;
        margin: 0 10px;
    }
    div[id]:not(:target){
        display: none;
    }
</style>    
<div class="button-wrapper">
    <a href="#red" class="bgred">red</a>
    <a href="#green" class="bggreen">green</a>
    <a href="#gray" class="bggray">gray</a>
</div>
<div id="red"></div>
<div id="green"></div>
<div id="gray"></div>
```

div[id]:not(:target)表示找到所有div有id属性的元素并且不等于target的

#### 2.4.2 first-child、last-child、only-child、nth-child、nth-last-child

> 个这几伪类选择器都会考虑其他元素对它们的影响

**first-child**

> 选择器匹配属于任意元素的第一个子元素的E元素，所谓影响，假设就是如果第一个位置上不是p元素，而是span元素，那么该p元素就不会被选中，因为第一个位置被span元素占了，所以这5个元素都不常用，

```html
<style>
    p:first-child{
        background-color: red;
    }
</style>
<body>
    <p>0</p>
    <div>
        <p>1</p>
        <p>2</p>
        <p>3</p>  
    </div>
    <p>4</p>
</body>
```

上面表示选择属于其父元素的首个子元素的每个 <p> 元素，并为其设置样式，上面的0和1都会被选中，0作为父元素body的第一个元素，1作为父元素div的第一个元素



**last-child**

> 选择器匹配属于任意元素的最后一个子元素的E元素

```html
<style>
    p:first-child{
        background-color: red;
    }
</style>
<body>
    <p>0</p>
    <div>
        <p>1</p>
        <p>2</p>
        <p>3</p>  
    </div>
    <p>4</p>
</body>
```

上面表示选择属于其父元素的最后个子元素的每个 <p> 元素，并为其设置样式，上面的3和4都会被选中，3作为父元素body的第一个元素，4作为父元素div的第一个元素



**only-child**

>选择器匹配属于任意元素只有一个子元素的E元素

```html
<style>
    span:first-child{
        background-color: red;
    }
    div:first-child{
        background-color: red;
    }
</style>
<body>
    <div>
        <span>1</span>
        <p>2</p>
    </div>
</body>
```

上面表示选择属于其父元素的仅有的一个子元素（div），并为其设置样式，其中body下面的div会被选中，因为只有body下仅有一个元素div，而div下有两个元素。



**nth-child(n)**

> 匹配父元素中的第 n 个子元素，元素类型没有限制。*n* 可以是一个数字，一个关键字，或者一个公式。

```html
<style>
    p:nth-child(2n){
        background-color:red
    }
</style>
<body>
    <div>
        <p>1</p>
        <p>2</p>
        <p>3</p>
        <p>4</p>
        <p>5</p>
    </div>
</body>
```

上面的第二个(2)和第四个(4)会被选中



**nth-last-child** 

> 逆匹配父元素中的第 n 个子元素，元素类型没有限制。*n* 可以是一个数字，一个关键字，或者一个公式。

```html
<style>
    p:nth-last-child(1){
        background-color: red;
    }
</style>

<body>
    <p>0</p>
    <div>
        <p>1</p>
        <p>2</p>
        <p>3</p>  
    </div>
    <p>4</p>
</body>
```

上面的几个伪类选择器都会考虑其他元素对它们的影响，所谓影响，假设就是如果第一个位置上不是p元素，而是span元素，那么该p元素就不会被选中，因为第一个位置被span元素占了，所以上面这些元素都不常用，



#### 2.4.3 first-of-type、last-of-type、:nth-of-type(n)、nth-last-of-type(n)

**first-of-type**

> 匹配元素其父级是特定类型的第一个子元素。

```html
<style>
    p:first-of-type{
        background-color: red;
    }
</style>
<body>
    <div>
        <span>0</span>
        <p>1</p>
        <p>2</p>
        <p>3</p>
        <p>4</p>
        <p>5</p>
        <p>6</p>
    </div>
    <p>7</p>
    <p>8</p>
</body>
```

上面的1和7会被选中，因为对于body来说，第一个p类型的就是7，对于div来说，头一个p类型是1



**last-of-type**

> 匹配元素其父级是特定类型的第一个子元素。

```html
<style>
    p:last-of-type{
        background-color: red;
    }
</style>
<body>
    <div>
        <span>0</span>
        <p>1</p>
        <p>2</p>
        <p>3</p>
        <p>4</p>
        <p>5</p>
        <p>6</p>
    </div>
    <p>7</p>
    <p>8</p>
</body>
```

上面的6和8会被选中，因为对于body来说，最后一个p类型的就是8，对于div来说，最后p类型是6



**nth-of-type(n)**

> 匹配同类型中的第n个同级兄弟元素。

```html
<style>
    p:nth-of-type(1){
        background-color: red;
    }
</style>
<body>
    <div>
        <span>0</span>
        <p>1</p>
        <p>2</p>
        <p>3</p>
        <p>4</p>
        <p>5</p>
        <p>6</p>
    </div>
    <p>7</p>
    <p>8</p>
</body>
```

上面的1和7会被选中，因为对于body来说，第一个p类型的就是7，对于div来说，头一个p类型是1



**nth-last-of-type(n)**

> 匹配同类型中的倒数第n个同级兄弟元素。

```html
<style>
    p:nth-last-of-type(1){
        background-color: red;
    }
</style>
<body>
    <div>
        <span>0</span>
        <p>1</p>
        <p>2</p>
        <p>3</p>
        <p>4</p>
        <p>5</p>
        <p>6</p>
    </div>
    <p>7</p>
    <p>8</p>
</body>
```

上面的6和8会被选中，因为对于body来说，最后一个p类型的就是8，对于div来说，最后p类型是6



#### 2.4.4 E:empty、checked、disabled、read-only、read-write

**E:empty**

> 看元素节点内部是否为空(没有其他节点)，在这里注释节点被认为不算节点,也是空

```html
<style>
    div:empty{
        border: 1px solid #000;
        height: 100px;
    }
</style>    
<div><span>123</span></div>
<div></div>
<div>234</div>  
<div><!--  --></div>
```



**E:checked**

> 匹配每个选中的输入元素（仅适用于单选按钮或复选框）。

```html
<style>
    input:checked + span{
        background-color: green;
    }
    input:checked + span::after{
        content: "隔壁老王 电话xxx,请务必小心行事";
        color: #fff;
    }
</style>
<label for="">
    一个小惊喜
    <input type="checkbox">
    <span></span>
</label>
```

上面表示点击复选框时会弹出一段话



**E:disabled**

> 匹配每个禁用的的元素（主要用于表单元素）。

```html
<style>
    input{
        border:1px solid #999
    }
    input:enabled{
        backgroud-color:green
    }
    input:disabled{
        border:1px solid #f20;
        backhround-color:#fcc;
        box-shadow: 1px 2px 3px #f20
    }
</style>
<input type="text">
<input type="text" disable>
```

**E:read-only**

> 用于选取设置了 "readonly" 属性的元素。

```html
<style>
    input:read-only{
        color:red
    }
</style>
<input type="text">
<input type="text" readonly value="逆只能瞅着，真干不了别的">
```



**E:read-write**

> 用于匹配可读及可写的元素。

```html
<style>
    input:read-write{
        color:red
    }
</style>
<input type="text" value="你别瞅着，有种干点什么">
<input type="text" readonly value="逆只能瞅着，真干不了别的">
```

实现手风琴和选项卡





## 3. Border&background

### 3.1. Border-radius

<img src="D:/Typora/typora-user-images/CSS3/css3 (20).png" alt="css3 (20)" style="zoom:50%;" />



<img src="D:/Typora/typora-user-images/CSS3/css3 (22).png" alt="css3 (22)" style="zoom:50%;" />

<img src="D:/Typora/typora-user-images/CSS3/css3 (19).png" alt="css3 (19)" style="zoom:50%;" />

通过上面的图片我们可以看出，随着圆的水平半径和垂直半径的增大，越来越接近我们定义的盒子的宽高度，当圆的水平半径和垂直半径等于盒子宽高度的一半时，此时该盒子正好容纳一个完整的圆，当圆的水平半径和垂直半径等于盒子宽高度时，此时盒子只能容纳1/4圆的圆

圆半径：盒子长度 = 1:4

```html
<style>
    div{
        width: 100px;
        height: 100px;
        background-color: pink;
        border-top-left-radius: 25px 25px;
        border: 3px solid #000;
    }  
</style>      

<div></div>

```

![css3 (14)](D:/Typora/typora-user-images/CSS3/css3 (14).png)



圆半径：盒子长度 = 1:2

```html
<style>
    div{
        width: 100px;
        height: 100px;
        background-color: pink;
        border-top-left-radius: 50px 50px;
        border: 3px solid #000;
    }
</style>
<div></div>
```

![css3 (15)](D:/Typora/typora-user-images/CSS3/css3 (15).png)



圆半径：盒子长度 = 1:1

```html
<style>
    div{
        width: 100px;
        height: 100px;
        background-color: pink;
        border-top-left-radius: 100px 100px;
        border: 3px solid #000;
    }
</style>
<div></div>
```

<img src="D:/Typora/typora-user-images/CSS3/css3 (16).png" alt="css3 (16)" style="zoom:50%;" />

实现一个半圆

```html
<style>
    div{
        width: 200px;
        height: 100px;
        background-color: pink;
        border-top-left-radius: 100px 100px;
        border-top-right-radius: 100px 100px;
        border: 3px solid #000;
    }
</style>
<div> </div>
```

<img src="D:/Typora/typora-user-images/CSS3/css3 (17).png" alt="css3 (17)" style="zoom:50%;" />

由于盒子宽度不能装下一个完整的半圆，因此需要扩大盒子的宽度。



**总结**

所谓圆角，就是切割盒子的四个角，每一个角对应圆的水平半径和垂直半径，4个角对应圆的4个水平半径和垂直半径，顺序是**左上、右上、右下、左下** 	

表示方法有：

​	1. border-radius: 10px 20px 30px 40px / 10px 20px 30px 40px 

 2. border-top-left-radius:10px 10px

    border-top-right-radius:20px 20px

    border-bottom-right-radius:30px 30px

    border-bottom-left-radius:40px 40px

![css3 (13)](D:/Typora/typora-user-images/CSS3/css3 (13).png)



border-radius:10px 20px **顺序表示是左上、右下 / 右上、左下**

border-radius:10px  20px 30px	**顺序表示是 左上 / 右上、左下/ 右下**

### 3.2. Border-shadow 

>  box-shadow：[阴影模式] 	水平偏移量	垂直偏移量	模糊范围	传播距离 	颜色

阴影模式：盒子阴影分为内阴影（inset）和 外阴影(outset)，默认情况下是外阴影

水平偏移量：阴影往左或右偏移，正数往左，负数往右

垂直偏移量：阴影往上或下偏移，正数往下，负数往上

模糊范围（blur）：高斯模糊，阴影的大小是基于原来边框的的位置向边框的两边同时去模糊

传播距离：表示在四个方向扩展阴影的距离



阴影可以同时设置多个，可以用（，）隔开

效果一：发光多彩盒子

![css (4)](D:/Typora/typora-user-images/CSS3/css (4).png)

```html
<style>
    body{
        background-color: #000;
    }
    div{
        position: absolute;
        width: 200px;
        height: 200px;
        border: 1px solid #fff;
        top: calc(50% - 50px);
        left: calc(50% - 50px);
        box-shadow: inset 0px 0px 10px #fff,
            0px -3px 10px #f0f,
            3px 0px 10px #ff0, 
            0px 3px 10px #0ff,
            -3px 0px 10px #00f;
    }
</style>
<div></div>
```



效果二：阴影重叠之发光彩球

![css (3)](D:/Typora/typora-user-images/CSS3/css (3).png)

```html
<style>
    body{
        background-color: #000;
    }
    div{
        position: absolute;
        width: 300px;
        height: 300px;
        top: calc(50% - 150px);
        left: calc(50% - 150px);
        border-radius: 50%;
        box-shadow: inset 0px 0px 50px #fff,
            inset 10px 0px 80px #f0f,
            inset -10px 0px 80px #0ff,
            inset 10px 0px 150px #f0f,
            inset -10px 0px 150px #0ff,
            0px 0px 50px #fff,
            -10px 0px 80px #f0f,
            10px 0px 80px #0ff;

    }
</style>
<div></div>
```



效果三：spread应用之小太阳

![css (2)](D:/Typora/typora-user-images/CSS3/css (2).png)



```html
<style>
    body{
        background-color: #000;
    }
    div{
        position: absolute;
        width: 50px;
        height: 50px;
        left: calc(50% - 25px);
        top: calc(50% - 25px);
        border-radius: 50%;
        border: none;
        background-color: #fff;
        box-shadow: 0px 0px 100px 50px #fff,
            0px 0px 250px 125px #ff0;
    }
</style>
<div></div>
```



效果四：鼠标hover盒子出现阴影和放大

```html
<style>
    div{
        position: absolute;
        width: 100px;
        height: 50px;
        top: calc(50% - 25px);
        left: calc(50% - 50px);
        background-color: #fff;
        box-shadow: 0px 1px 2px rgba(0,0,0,.1);
        transition: all .6s;

    }
    div::after{
        content: "";
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        border-radius: 5px;
        box-shadow: 0px 5px 15px rgba(0,0,0,.3);
        opacity: 0;
        transition: all .6s;
    }
    div:hover{
        transform: scale(1.25,1.25);
    }
    div:hover::after{
        opacity: 1;
    }
</style>
<div></div>
```



总结：背影在文字的下面，在阴影的上面

​			文字在一切的上面



### 3.3. Border-image

> 给border加上背景颜色

- border-image-source：给背景添加资源，支持URL引入图片跟渐变色（linear-Gradient(颜色1，颜色2)）默认由上往下填充

- border-image-slice：有1到4值，分别是top，right，bottom，left，只能填数字或百分比，顺序是上右下左，不添值的话默认100%。最后还有一个fill值，填了fill最中间的就不会被忽略

  情况一：图片为300 X 300; top、right、bottom、left 等于100

  ![css3 (2)](D:/Typora/typora-user-images/CSS3/css3 (2).png)

  

  上面的最中间的9会被忽略掉，每个方向对应每个方块进行填充

  1：top-left

  2：top-right

  3：bottom-right

  4：bottom-left

  5：top-left-right

  6：top--right-bottom

  7：right-bottom-left

  8：top-bottom-left
  
  9：top-right-bottom-left(该区域会被忽略)如果添加了fill该区域就不会被忽略

图片为300 X 300，默认情况：100% 100% 100% 100%

![css3 (3)](D:/Typora/typora-user-images/CSS3/css3 (3).png)



top-left：整个方块

top-right：整个方块

bottom-right：整个方块

bottom-left：整个方块

5：top-left-right：

6：top--right-bottom

7：right-bottom-left

8：top-bottom-left



- border-image-outset：可以让背景图片往外衍生，超出定义的border值，不能为负值，没有效果
- border-image-width：设置border里面的图片的宽度占整个border宽度的多少，默认为1表示border里面图片的宽度与border的宽度比例为1:1，也可设置px值，如果填auto的话，默认会与slice看齐，如果slice为100，width就为100px，如果不加px
- border-image-repeat：解决5678区域宽度增大的时候这部分的图像如何填充的问题，默认值为stretch，可选值有【stretch|repeat|round|space】，可以同时填两个参数，表示水平的和垂直的
  - stretch：将被分割的图像使用拉伸的方式来填充满边框图像区域
  - repeat：将图片砍半在图像的左右填充
  - round：先拉伸，等到宽度满足达到图像的1.5倍宽度时会复制一个平均放到右边，此时图像会有点压缩感，等到宽度达到图像的2倍时会是两个完整的图像
  - space：不拉伸，拿空白补，等到宽度满足达到图像的2倍宽度时会复制一个平均放到右边

联合写法：border-image:source slice width outset  repeat;



总结：slice不能写px的原因是在联合写法中易与width混淆，不易区分



### 3.4. background

background用于设置背景，有多个属性：

#### background-image

可以添加图片或渐变色作为背景颜色，该属性在CSS2中就存在，在CSS3中新增了可以同时添加多占图片(URL)作为背景图片，用逗号（，）隔开默认下前一张图片的Z-index值大于后面的，会覆盖后面的

应用场景一·：在网速不佳的时候展示占位图片，占位图片一般是像素很低的，等到高清图片加载完后再展示高清图片，前面一张的z-index值大于后面的图片，因此占位图片一般在后面



一个盒子展示两张图片

```html
<style>
    div{
        position: absolute;
        width: 300px;
        height: 300px;
        border: 1px solid #000;
        left: calc(50% - 150px);
        top: calc(50% - 150px);
        background-image: url(./images/pic2.jpeg),url(./images/pic7.jpeg);
        background-size: 150px 300px,150px 300px;
        background-repeat: no-repeat;
        background-position: 0 0 ,150px 0;
    }
</style>
<div></div>
```

![css3 (4)](D:/Typora/typora-user-images/CSS3/css3 (4).png)

#### background-repeat

设置图片无法填充盒子宽度时是否重复图片展示，有个可选值：repeat，no-repeat，repeat-x ,repeat-y，round，space默认repeat。round，space是CSS3新增的值，该属性可以同时添加两个CSS3值，一个代表水平方向的重复处理，一个代表垂直方向的重复处理，如果是CSS2值只能添加一个

- repeat-x：仅在X轴方向重复平铺，Y轴留白
- repeat-y：仅在Y轴方向重复平铺，X轴留白
- repeat：repeat-x与repeat-y的结合，XY轴都重复平铺
- no-repeat:
- round：首先重复平铺图片，如果盒子宽度无法容纳一张新图片时，会拉图片，等到盒子宽度达到可以再容纳一张新图片时再把新图片补进来
- space：重新重复平铺图片，而且图片与图片之间有空隙，不会拉伸图片，等到各个图片间的水平（或垂直）空隙达到一张图片的宽度时再把新图片补进来



#### background-origin

设置图片展示的起始位置（左上），有3个可选值，content-box，padding-box，border-box，**默认padding-box**，但没有设置图片展示的结束位置（右下）

```html
<style>        
    div{
        position: absolute;
        width: 200px;
        height: 200px;
        left: calc(50% - 100px);
        top: calc(50% - 100px);
        background-image: url(./images/pic4.jpeg);
        padding: 20px;
        border: 20px solid transparent;
        background-repeat: no-repeat;
        background-origin: padding-box;	<!-- 默认值 -->
        background-clip: border-box;	<!-- 默认值 -->
    }
</style>
<div></div>
```

![css3 (6)](D:/Typora/typora-user-images/CSS3/css3 (6).png)

![css3 (7)](D:/Typora/typora-user-images/CSS3/css3 (7).png)

#### background-position

设置图片的绝对位置，该属性相对于background-origin的值作为原点进行定位,换句话说origin直接决定了position从哪个地方开始定位，可选值有三种类型：百分比，px，还有center、left、right、top、bottom ，默认值0% 0% ,效果等同于left top



#### background-clip

设置图片展示的结束位置（右下），有4个可选值，content-box，padding-box，border-box，text，**默认border-box**，text 表示用文字内容区域  反切背景图片，就是文字颜色用背景填充，该值仅在-webkit-中好使，而且需要配合一个属性使用，该属性也是webkit的私有属性

![css3 (7)](D:/Typora/typora-user-images/CSS3/css3 (7).png)



```css
-webkit-background-clip:text;
background-clip:text;
-webkit-text-fill-color:transparent;
text-fill-color:transparent;
```



文字遮罩效果

```html
<style>
    .fontBg{
        position: absolute;
        width: 400px;
        height: 200px;
        border: 1px solid #000;
        left: calc(50% - 200px);
        top: calc(50% - 100px);
        font-size: 80px;
        font-weight: 1000;
        text-align: center;
        line-height: 200px;
        -webkit-background-clip:text;
        background-clip:text;
        -webkit-text-fill-color:transparent;
        background-image: url(./images/pic2.jpeg);
        background-position: 10px 100px;
        transition: all .6s;
    }
    .fontBg:hover{
        background-position: center center;
    }
</style>
<div class="fontBg">成哥很帅</div>

```

![css3 (5)](D:/Typora/typora-user-images/CSS3/css3 (5).png)



#### background-attachment

- 设置背景图像相对于容器（盒子）进行定位，不跟容器里的内容文字进行定位，有点类似于相对于视口进行fixed定位，默认值是：scroll 

  - local：背景图片相对于内容区里的文字进行定位
  - fixed：背景图片相对于视口进行定位，但超出容器后部分依旧会消失

  ```html
      <style>
          div {
              width: 500px;
              height: 700px;
              border: 1px solid red;
              background-image: url(./images/pic4.jpeg);
              background-size: 300px 400px;
              background-repeat: no-repeat;
              background-position: 100px 100px;
              overflow: scroll;
              background-attachment: local;
  
          }
      </style>
  <div>
      xxx*10000
  </div>
  ```



#### background-size

用于设置背景图像的尺寸：可选值有3种，有px，cover，contain

- cover：在不更改图片比例而更改图片大小的情况下，用一张图片进行缩放直到填充满整个容器，图片可能存在超出容器的风险。cover的情况是:一条边跟容器对齐，另一条边要大于等于容器，否则不满足cover的效果，是contain的效果

- contain：在不更改图片比例而更改图片大小的情况下，让容器包含一张完整图片，首先是让图片先充满一个方向的（水平方向），容器可能存在留白的风险

例子：

```html
<style>
    div {
        width: 500px;
        height: 700px;
        border: 1px solid red;
        background-image: url(./images/pic4.jpeg);
        background-repeat: no-repeat;
        background-attachment: local;
        background-size: cover;
    }
</style>
<div>

</div>
```



cover展示：

<img src="D:/Typora/typora-user-images/CSS3/css3 (9).png" alt="css3 (9)" style="zoom:50%;" />





conntain展示：



<img src="D:/Typora/typora-user-images/CSS3/css3 (8).png" alt="css3 (8)" style="zoom: 50%;" />



#### 线性渐变和辐射渐变

- linear-gradient(渐变方向，起始颜色（起止点），结束颜色（起止点）)
  - 渐变方向：默认to bottom ,可以设置角度和方向值（to-left,to-left-top...）
  - 起止点：如果左边存在起止点，两点范围内发生渐变过渡，如果左边不存在起止点，从左边到起止点不发生渐变过渡

```html
<style>
    div{
        width: 300px;
        height: 100px;
        background-image: linear-gradient(90deg,#00f 30px,#f0f 270px);
    }
</style>
<div> </div>
```

![css3 (11)](D:/Typora/typora-user-images/CSS3/css3 (11).png)



- radial-gradient(圆类型 圆半径 at 圆心位置，起始颜色，（起止点），结束颜色，（起止点）)

  - 圆类型：circle、ellipse

  - 圆半径：

    - losest-corner——距离最近父容器的角
    - losest-side——距离最近父容器的边
    - farthest-corner——距离最远父容器的角
    - farthest-side——距离最远父容器的边

  - 圆心位置：填像素值，默认在容器中心

    

## 4、text

### 4.1. text-shadow

> 有4个值，分别为水平偏移量，垂直偏移量，模糊范围、传播距离

效果一：浮雕

```html
<style>
    body{
        background-color: #ddd;
    }
    div{
        position: absolute;
        width: 700px;
        height: 100px;
        font-size: 80px;
        left: calc(50% - 350px);
        top: calc(50% - 50px);
        text-shadow: 1px 1px  #000,-1px -1px #fff;
    }
</style>
<div></div>
```

![css3 (12)](D:/Typora/typora-user-images/CSS3/css3 (12).png)



所谓浮雕效果：就是太阳从左上方照射的时候，由于文字是凸起，左上方被阳光照射到，偏白，而右下标被文字遮挡了会造成黑色的阴影

效果2：镂刻

```html
<style>
    body{
        background-color: #ddd;
    }
    div{
        position: absolute;
        width: 700px;
        height: 100px;
        font-size: 80px;
        left: calc(50% - 350px);
        top: calc(50% - 50px);
        text-shadow: -1px -1px  #000,-1px -1px #fff;
    }
</style>
<div></div>
```

![css3 (1)](D:/Typora/typora-user-images/CSS3/css3 (1).png)



所谓镂刻效果：就是太阳从左上方照射的时候，由于文字是凹下去的，左上方被没有被阳光照射到，偏黑，而右下方被文字直接照射到所以会偏白

效果三：发光火焰文字效果

```html
<style>
    body{
        background-color:#000;
    }
    div{
        position: absolute;
        width: 700px;
        height: 100px;
        font-size: 80px;
        left: calc(50% - 350px);
        top: calc(50px);
        text-shadow: 0px 0px 10px #0f0,
            0px 0px 20px #0f0,
            0px 0px 30px #0f0;
        color:#ddd;
        transition: text-shadow 1s;

    }
    div:hover{
        text-shadow: 0px 0px 10px #f00,
            0px 0px 20px #f00,
            0px 0px 30px #f00;
    }
    div::before{
        content: "NO ";
        text-shadow: 0px 0px 10px #f00,
            0px 0px 20px #f10,
            0px 0px 30px #f20,
            0px -5px 20px #f10,
            0px -10px 20px #f20,
            0px -15px 20px #f10,
            0px -20px 40px #f20;
        opacity: 0;
        transition: opacity 1s;
    }
    div:hover::before{
        opacity: 1;

    }
</style>
<div></div>
```

![noAccess](D:/Typora/typora-user-images/CSS3/noAccess.png)



效果4：死神来了

```html
<style>      
    div{
        position: absolute;
        width: 400px;
        height: 150px;
        font-size: 100px;
        left: calc(50% - 350px);
        font-weight: bolder;
        color: #fff;
        background-image: url(./images/eye.jpeg);
        background-repeat: no-repeat;
        background-size: 400px 300px;
        background-position: -400px -50px;
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color:transparent;
        transition: all .6s;
        text-shadow: -5px -5px 3px rgba(255, 0, 255, 0.1),
            5px 5px 3px rgba(0, 255, 255, .1)
    }
    div:hover{
        background-position: 0 -50px;
    }
</style>
<div>  </div>
```

![column5 (2)](D:/Typora/typora-user-images/CSS3/column5 (2).png)



这里添加了text-shadow后：阴影跑到了文字的最上方，正常是阴影的文字的下方，出现该问题的原因是background-clip属性，一旦用文字剪切背景的话，文字就成了背景的一部分，而背景肯定是在内容的最下边的，阴影是在文字的下方和背景上方，此时文字也变成背景的一部分了话，阴影自然就在背景的上方

层级：文字 > 阴影 > 背景

解决方法是降低阴影的透明度，让它看起来像在下面



**效果5：影分身之术**

```html
<style>
    div{
        position: absolute;
        width: 600px;
        height: 150px;
        font-size: 60px;
        left: calc(50% - 350px);
        top: 200px;
        font-weight: bolder;
        color: #f10;
        text-shadow: 0px 0px 5px #f10,
            0px 0px 10px #f20,
            0px 0px 15px #f30;
        transition: all 1.3s;
    }
    div:hover{
        text-shadow: 10px -10px 10px #f00,
            50px 50px 10px #f0f,
            -50px 50px 10px #0f0,
            -10px 10px 10px #ff0,
            50px -50px 10px #00f,
            10px 10px 10px #0ff,
            -10px -10px 10px #dd0,
            -50px -50px 10px #c99;


    }
</style>
<div>  </div>
```

![column5 (3)](D:/Typora/typora-user-images/CSS3/column5 (3).png)



### 4.2. text-stroke

> 文本描边：有两个属性值，第一个表示描边粗细，第二个表示描边颜色

```html
    <style>
         div{
             position: absolute;
             width: 500px;
             height: 150px;
             left: calc(50% - 250px);
             top: calc(50% - 75px);
             font-size: 100px;
             font-weight: bolder;
             font-family: simsun;
             -webkit-text-stroke:1px red;
         }
    </style>
<div>
    成哥很帅
</div>
```

不存在的字体我们需要用@font-face功能引入，该功能有个需要注意的格式，不能放到然后盒子里边，要放到最外边

```html
@font-face{
	font-family:"自己取的字体的名字"
	src:url() fotmat()；
}
```



字体格式：

- truetype(.ttf)：由微软和苹果共同研发的一种字体格式，兼容性较好
- opentype(opt)：由微软和adobe共同研发的一种字体格式，比truetype强大，可以认为是truetye的超集
- woff(.woff)：是truetype和opentype的合集压缩版本，是一种未来的格式，但目前兼容性不是很高
- .eot：IE研发的
- .svg

format：字体格式提示器，告诉操作系统这是什么类型的文件，

浏览器之所以能打开各种格式的文件，都是因为浏览器通过MIME协议调用了操作系统去请求能打开相应格式的程序

MIME(.ttf .txt .pdf...)：实际上是定义了一些映射，把对应的格式映射到不同的程序

> MIME(Multipurpose Internet Mail Extensions)多用途互联网邮件扩展类型。是设定某种[扩展名](https://baike.baidu.com/item/扩展名/103577)的[文件](https://baike.baidu.com/item/文件/6270998)用一种[应用程序](https://baike.baidu.com/item/应用程序/5985445)来打开的方式类型，当该扩展名文件被访问的时候，[浏览器](https://baike.baidu.com/item/浏览器/213911)会自动使用指定应用程序来打开。多用于指定一些[客户端](https://baike.baidu.com/item/客户端/101081)[自定义](https://baike.baidu.com/item/自定义/6666)的[文件名](https://baike.baidu.com/item/文件名/608450)，以及一些媒体文件打开方式。



### 4.3. white-space

> 设置如何处理元素内的空白。

- normal：默认情况，空白会被浏览器忽略
- pre：原封不动的保留你输入时的状态，空格，换行都会保留，并且当文字超出边界时不换行，等同于pre标签
- nowrap：与normal一致，文本不会换行,文本会在同一行上继续，直到遇到br标签为止
- pre-wrap：与pre一致，不同的是文字超出边界时会自动换行
- pre-line：与normal值一致，但是会保留文本输入时的换行

```html
<style>
    *{
        margin: 0px;
        padding: 0px;
    }
    div{
        white-space: pre;
    }
</style>
<div>       
    人生哲理！成哥很帅</div>
```

![image-20200309172120729](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200309172120729.png)

### 4.4. word-break

> 规定自动换行的处理方法。

可选值

- normal：默认规则，依据各自语言的规则，允许在字间发生换行
- kepp-all：对于CJK(中文，韩文，日文)文本不允许在字符内发生换行，Non-CJK文字表现同`normal`
- break-all：对于Non-CJK文字允许在任意字符内发生换行

- break-word：会换行展示一个完整的英文单词  



### 4.5. columns

> 该属性有两个属性值，分别为：column-width  column-count。
>
> 该属性兼容性不好
>

子属性

- **column-width**：给列定义个最小的宽度，默认值为`auto`，表示将根据column-count列的数量自动调整列宽

- **column-count**：设置列数

- **column-gap**：列间距，通常默认是16px

- **column-rule**：用于设置列的边框，类似于border，区别是不占用任何空间，因此设了column-rule不会导致列宽的变化。另外如果边框宽度大于column-gap列间距，将不会显示边框。

- **column-span**：用于设置夸列。默认none表示不跨列，all表示跨域所有列

- **column-fill**：用于统一列高，默认值auto各列的高度随着内容自动调整，balance所有列高都设为最高的列的高

  

**该属性一般不适用于我们的瀑布流布局**

因为该属性的算法是必须最少有一个最高柱高才能放下一张图片，如果所有列的柱高都相同，会减少一列达到某一列是最高柱高后才能继续放下一张

```html
<style>
    div{
        column-count: 4;
        column-rule-style: solid;

    }
    img{
        width: 200px;
        height: 250px;
    }
</style>    
<div>
    <img src="./images/pic4.jpeg" alt="">
    <img src="./images/pic1.jpeg" alt="">
    <img src="./images/pic2.jpeg" alt="">
    <img src="./images/pic3.jpeg" alt="">
    <img src="./images/pic4.jpeg" alt="">
    <img src="./images/pic5.jpeg" alt="">
    <img src="./images/pic6.jpeg" alt="">
    <img src="./images/pic6.jpeg" alt="">
    <img src="./images/pic6.jpeg" alt="">
    <!-- <img src="./images/pic6.jpeg" alt=""> -->
</div>
```

假设我们设置了4列，一共有9张图片，如果按照我们的思路，应该是排成4列，第1列3张，剩余的3列都是2张，但实际的效果却不是如此

<img src="D:/Typora/typora-user-images/CSS3/column5 (5).png" alt="column5 (5)" style="zoom:50%;" />



实际的效果变成了3行3列，出现这种情况的原因是我们在加入第9张图片之前的时候，此时的页面布局如下，是正常的

<img src="D:/Typora/typora-user-images/CSS3/column5 (6).png" alt="column5 (6)" style="zoom:50%;" />

但当我们插入第9张图片时，发现所有柱高都一致，这时会减到3列，变成第一列3行，第二列3行，第三列2行，这时就存在两个最高柱高了，然后才插入图片。

如果继续插入第10张就会回到我们预想中4列的样子了，因为存在一个最高柱高

<img src="D:/Typora/typora-user-images/CSS3/column5 (1).png" alt="column5 (1)" style="zoom:50%;" />



该属性很适用于小说的文字翻页

因为该属性能灵活的让浏览器来决定在文字该在哪里换列。我们所做的只需指定每列的宽度和列数即可。

效果一：实现小说文字划动切换效果

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>小说文字滑动切换效果</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        div {
            width: 300px;
            height: 500px;
            border: 1px solid red;
            cursor: pointer;
        }

        .content {
            column-width: 300px;
            column-gap: 20px;
            border: none;
            transition: all .5s;

        }

    </style>
</head>

<body>
    <div class="wrapper">
        <div class="content">
            来源：新浪科技

            新浪科技讯 3月9日上午消息，携程CEO孙洁今日发布内部信称，从本月开始，自己和董事局主席梁建章将0薪。公司高管层也提出自愿降薪，最低半薪，直至行业恢复。

            孙洁表示，经历了保障消费者权益、帮扶产业链供应商为主的“战疫”第一阶段，上周开始，携程已经进入“战疫”的第二阶段，联动数百个目的地、全产业链资源方共同启动了“旅游复兴V计划”。

            为做好长期“战疫”的准备，孙洁宣布，从本月开始自己和梁建章开始0薪；公司高管层也提出自愿降薪，最低半薪，直至行业恢复；其他员工暂缓涨薪；服务部一线员工可正常调涨薪资。

            “相比整个行业来说，携程拥有最强的资金储备，同时我们也应该拥有最强的应变和判断力，来迎接长期的挑战和机遇。短暂的韬光养晦是为了长远的厚积薄发。相信我们大家同心协力，很快会迎来旅游行业的再度蓬勃增长。”孙洁说。(张俊)

            以下为孙洁内部信全文：

            和衷共济，戮力谋远

            亲爱的伙伴：

            经历了保障消费者权益、帮扶产业链供应商为主的“战疫”第一阶段，上周开始，携程已经进入“战疫”的第二阶段，我们联动数百个目的地、全产业链资源方共同启动了“旅游复兴V计划”。

            V代表触底反弹的势能和决心。在全力防控疫情的背景下，我们发起了“旅游复兴目的地十项倡议”、建立了“旅游复苏数据平台”。当行业遭遇低谷期，携程责无旁贷，这既是带领行业自救的责任，也是携程自身提前布局、创新思路、化危为机，释放动能的切实努力。

            当下，国内疫情进入较为稳定的阶段，全球疫情仍前景未明，我们在对未来保有信心的同时，也要理性看待，做好长期“战疫”的准备。

            在这方面，我和James将带领集团高管做出表率，并诚挚地希望每一个携程人与我们一起和衷共济，戮力谋远。

            从本月开始，James和我开始0薪；公司高管层也提出自愿降薪，最低半薪，直至行业恢复；其他员工暂缓涨薪；服务部一线员工可正常调涨薪资。

            疫情带给我们挑战，也带给我们契机。比如，我们再次刷新了消费者服务标准；比如我们重新搭建规则，建立起行业生态共同体；比如，我们获得了难得的检视自我的机会，我们是否把宝贵的资源用在了新形势下最需要的地方？希望各团队根据实际业务情势，积极制定针对性的应对之策。

            相比整个行业来说，携程拥有最强的资金储备，同时我们也应该拥有最强的应变和判断力，来迎接长期的挑战和机遇。短暂的韬光养晦是为了长远的厚积薄发。相信我们大家同心协力，很快会迎来旅游行业的再度蓬勃增长。
        </div>
    </div>
    <script>
        var wrapperBox = document.getElementsByClassName('wrapper')[0];
        var contentBox = document.getElementsByClassName('content')[0];
        var lock = false;
        wrapperBox.onmousedown = function (e) {
            lock = true;
            var x1 = e.clientX;
            var y1 = e.clientY;
            wrapperBox.onmousemove = function (e) {
                if (!lock) return;
                var x2 = e.clientX;
                var y2 = e.clientY;
                var left = contentBox.style.transform.match(/-?\d+/g) ? contentBox.style.transform.match(/-?\d+/g)[0] : 0;
                var left_pull = Number(left) - 320;
                var right_pull = Number(left) + 320;
                if (x1 - x2 > 0) {
                    contentBox.style.transform = 'translateX(' + left_pull + 'px)';
                } else {
                    contentBox.style.transform = 'translateX(' + right_pull + 'px)';
                }
                lock = false;
            }
        }

    </script>
</body>

</html>
```



## 5. Box

### IE6混杂模式盒子

**属性**：box-sizing

可选值

- content-box	默认标准盒模型
- border-box     IE6混杂盒模型

应用场景：

1. 宽度不固定，但是内边距（padding）固定

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .wrapper{
        width: 100%;
        height: 300px;
        border: 2px solid #000;
    }

    .content:first-of-type{
        float: left;
        width: 50%;
        height: 100px;
        background-color: orange;
        padding: 0 10px;
        box-sizing: border-box;

    }
    .content:last-of-type{
        float: left;
        width: 50%;
        height: 100px;
        background-color: #0ff;
    }
</style>
<div class="wrapper">
    <div class="content"></div>
    <div class="content"></div>
</div>
```

![IE6混杂模式](D:/Typora/typora-user-images/CSS3/IE6混杂模式.png)

2. input框为100%时，天生带border,会导致长度溢出

#### IE6混杂模式盒子与标准盒子的区别

> 计算方式不一样

标准盒模型：boxWidth = width + border\*2 + padding\*2

IE6混杂盒模型：BoxWidth = width

​							contentWidth = width - border \* 2 + padding \* 2



#### overflow

> 溢出容器内容如何处理

可选值

- visible：默认值，对溢出内容不做处理，内容可能超出容器

- hidden：隐藏溢出容器内容，而且不出现滚动条

- scroll：隐藏溢出容器内容，溢出的内容可以通过滚动呈现

  auto：内容不溢出容器。不出现滚动条，溢出容器后就出现滚动条 ，textarea的overflow默认就是auto

- clip：溢出

注意点：如果overflow-x或overflow-y属性其中一个设置了其他值而不是默认值（visible）,另一个属性将会自动将默认值更改成auto

```css
overflow-x: 就会
//下面的overflow会变成
overflow-y: auto
```





**resize**

> 可以让用户自定义调整元素大小

可选值

- none：默认值，不允许用户调整元素大小
- both：用户可以调节元素的宽度和高度

- horizontal：用户可以调整元素的宽度
- vertical：用户可以调整元素的高度

注意点：该属性需要配合overflow一起使用，该属性类似textarea一样可以改变元素大小



### Flex盒模型



#### 弹性盒子属性

##### 1. flex-direction

>  定义主轴的方向，默认为row

可选值：

- row
- row-reverse
- column
- column-reverse

##### 2. flex-wrap

> 定义项目长度超过flex容器时项目是否换行，默认nowrap

可选值：

- nowrap(默认值)：不换行
- wrap：换行
- wrap-reverse：倒过来换行

##### 3. justify-content

> 基于主轴的水平对齐方式、默认flex-start

- flex-start(默认值)：弹性项目将基于主轴的起始位置对齐
- flex-end：弹性项目将基于主轴的结束位置对齐
- center：弹性项目将具有主轴的中间位置对齐
- space-between：两端对齐，弹性项目之间的间隔都相等
- space-around：每个弹性项目两侧的间隔相等，所以，弹性项目之间的间隔比弹性项目与边框的间隔大一倍

##### 4. align-items

>  基于交叉轴的水平对齐方式，默认stretch，以一个弹性项目为单位

- flex-start：弹性项目将基于交叉轴的起始位置对齐
- flex-end：弹性项目将基于交叉轴的结束位置对齐
- center：弹性项目将基于交叉轴的中间位置对齐
- baseline：弹性项目将基于第一个弹性项目的第一行文字的基线对齐
- stretch(默认值)：如果弹性项目没有设置高度为为auto，弹性项目的高度将沾满整个容器的高度

##### 5. align-content

> 基于弹性项目所在行在交叉轴方向上的对齐方式，默认stretch 。以一行为单位

- flex-start：弹性项目行将基于交叉轴的起始位置对齐
- flex-end：弹性项目行将基于交叉轴的结束位置对齐
- center：弹性项目行将基于交叉轴的中间位置对齐
- space-between：弹性项目行基于两端对齐，弹性项目行与行之间的间隔都相等
- space-around：每个弹性项目行两侧的间隔相等，所以，弹性项目行之间的间隔比弹性项目行与边框的间隔大一倍
- stretch（默认值）：轴线占满整个交叉轴，弹性项目无高度时才生效

##### 6. flex-flow

> 是flex-direction 和 flex-wrap的结合体：默认值  row   nowrap 



#### 弹性项目属性

##### 1. order

> 决定弹性项目的排列顺序，值越小，位置越靠前，默认为0



##### 2. align-self

> 弹性项目作为一个个体，基于交叉轴的水平排列方式，优先级比父级的align-items大，但比align-content小，默认值为auto

- auto(默认值)：如果该的值为默认值auto，则其计算值为父元素的"align-items"取的值，如果父元素没有设置该属性，则属性值为stretch
- flex-start：弹性个体项目将基于交叉轴的起始位置对齐
- flex-end：弹性个体项目将基于交叉轴的结束位置对齐
- center：弹性个体项目将基于交叉轴的中间位置对齐
- baseline：弹性个体项目将基于第一个弹性项目的第一行文字的基线对齐
- stretch：如果弹性个体项目没有设置高度为为auto，弹性项目的高度将沾满整个容器的高度

##### 3. flex

> 有三个属性，分别为flex-grow，flex-shrink，flex-basis，默认0  1 auto  

- 如果缩写 flex:1	,则其计算值为 1  1  0%
- 如果缩写flex:auto,  则其计算值为  1   1  auto
- 如果flex:none,    则其计算值为  0   0   auto
- 如果flex: 0  auto  或flex:inital,  则其计算值为 0 1 auto，即flex初始值



##### 4. flex-grow

> 设置该属性的弹性项目按照设置的比例自动分配剩余宽度/高度，默认为0表示参与不瓜分剩余空间

```html
<style>
    .wrapper{
        width: 600px;
        height: 600px;
        border: 1px solid #000;
        display: flex;
    }
    .content{
        width: 100px;
        height: 100px;
        border: 1px solid green;
        box-sizing: border-box;
    }
    .content:first-of-type{
        flex-grow: 1;
    }
    .content:nth-of-type(2){
        flex-grow: 2;
    }
    .content:nth-of-type(3){
        flex-grow: 3;
    }
</style>    
<div class="wrapper">
    <div class="content">1</div>
    <div class="content">2</div>
    <div class="content">3</div>
</div>
```

![flex-grow](D:/Typora/typora-user-images/CSS3/flex-grow.png)

可以看出，计算规则为**当前份数/总份数 * 剩余空间 = 当前份数所占空间**

```html
总份数 = 1+2+3=6
第一个份数所占剩余空间:1/6 * 300 = 50px
第二个份数所占剩余空间:2/6 * 300 = 100px
第三个份数所在剩余空间:3/6 * 300 = 150px
```



##### 5. flex-basis

> 设置弹性项目的基础宽度，权重比width高，**默认值auto**，取width的宽度



##### 6. flex-shrink

> 当弹性项目总宽度大于弹性盒子的宽度时，是否收缩弹性项目宽度以和弹性盒子宽度保持一致

```html
<style>
    .wrapper{
        width: 600px;
        height: 600px;
        border: 1px solid #000;
        display: flex;
    }
    .content{
        width: 200px;
        height: 100px;
        /* border: 1px solid green; */
        box-sizing: border-box;
    }
    .content:first-of-type{
        flex-shrink: 1;
    }
    .content:nth-of-type(2){
        flex-shrink: 1;
    }
    .content:nth-of-type(3){
        flex-shrink: 3;
        width: 400px;
    }
</style>
<div class="wrapper">
    <div class="content">1</div>
    <div class="content">2</div>
    <div class="content">3</div>
</div>

```



#### flex-shrink属性的计算规则


  ![shrink属性的计算规则](D:/Typora/typora-user-images/CSS3/shrink属性的计算规则.png)

  我们知道，弹性盒子总宽度为600px，弹性总项目长度为800px，超出了盒子200px，需要给设置flex-shrink的弹性项目进行压缩

  如果我们根据flex-grow的计算规则，计算出来的值如下  

```html
  总份数 = 1+1+3=5
  第一个份数所需让出空间:1/5 * 200 = 40px
  第二个份数所需让出空间:1/5 * 200 = 40px
  第三个份数所需让出空间:3/5 * 200 = 120px
```

再加上各个弹性项目本身宽度（200），相减得出三个弹性项目的宽度分别是160 	160	280，然而得出的结果与我们预期的并不相符，结果是175	175	250，说明计算规则并不是和flex-grow的一致，正确的



  计算规则为：首先计算加权值，把加权值当做总宽度，用当前压缩比例乘以弹性项目内容区宽度再除以加权值，最后再乘以需要压缩的宽度得出每份压缩份数。

  ```html
  加权值（总宽度）：1*200 + 1*200 + 3*400 = 1600
  第一个份数所需让出空间:1*200/1600 *200  = 25px
  第二个份数所需让出空间:1*200/1600 *200  = 25px
  第三个份数所需让出空间:3*400/1600 *200 = 150px
  ```

  最终得出的宽度分别是：

  1项目:   200 - 25 = 175px

  2项目：200 - 25 = 175px

  3项目:   400 - 150 = 250px; 



注意点：在这里还有一个点需要注意的是，加权值是比例乘以的是内容区（content）的宽度，而不是整个盒子包括padding+border的宽度。为了证明，我们看下面一个例子

```html
<style>
    .wrapper{
        width: 600px;
        height: 600px;
        border: 1px solid #000;
        display: flex;
    }
    .content{
        width: 200px;
        height: 100px;
        box-sizing: border-box;
        /* border: 1px solid green; */
        padding: 0 80px;
    }
    .content:first-of-type{
        background-color: #0f0;
        flex-shrink: 1;
    }
    .content:nth-of-type(2){
        background-color: #ff0;
        flex-shrink: 1;
    }
    .content:last-of-type{
        width: 400px;
        background-color: #00f;
        flex-shrink: 3;

    }
</style>
<div class="wrapper">
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
</div>
```

![shrink](D:/Typora/typora-user-images/CSS3/shrink.png)

![绿色盒子区](D:/Typora/typora-user-images/CSS3/绿色盒子区.png)

![蓝色盒子区](D:/Typora/typora-user-images/CSS3/蓝色盒子区.png)



这个例子与上面的差不多，唯一的区别是给每个弹性项目左右各加了80px的padding，此时内容区剩余的宽度为 40   40  240。总的弹性项目宽度分别为200  200 400，再经过压缩后，内容区的宽度变为30  30  60，而每个项目的padding值并没有减少，压缩后总的弹性项目宽度分别为190  190  220。从这里就可以看出，加权值计算的是项目内容区的宽度，而不是整个项目盒子的宽度。

```html
加权值：40 * 1 + 40 * 1 +　240 * 3 = 800;

第一个弹性项目需要压缩的宽度：40 * 1/800 * 200 = 10px
第二个弹性项目需要压缩的宽度：40 * 1/800 * 200 = 10px
第三个弹性项目需要压缩的宽度：240 * 3/800 * 200 = 180px

压缩后总的弹性项目宽度分别为
200-10 = 190  
200-10 = 190  
400-180 = 220
```



**总结：加权值计算的是真实内容区（content）的大小* flex-shrink值，无论是在标准盒模型中，还是在IE6混杂盒模型中都是一样，计算的都是内容区的宽度。**



如果不信，我们手动的把宽度设为标准盒模型下的宽度

```html
<style>
    .wrapper{
        width: 600px;
        height: 600px;
        border: 1px solid #000;
        display: flex;
    }
    .content{
        width: 40px;
        height: 100px;
        padding: 0 80px;
    }
    .content:first-of-type{
        background-color: #0f0;
        flex-shrink: 1;
    }
    .content:nth-of-type(2){
        background-color: #ff0;
        flex-shrink: 1;
    }
    .content:last-of-type{
        width: 240px;
        background-color: #00f;
        flex-shrink: 3;

    }
</style>
<div class="wrapper">
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
</div>
```

在上面，我们去掉了。怪异盒子，换上了标准盒子，并手动的把宽度值分别设置为 40  40  240

![shrink1](D:/Typora/typora-user-images/CSS3/shrink1.png)

得到的结果依旧是一样的。



#### flex-basis与width的区别

1. flex-basis本质上设置的是min-width的宽度，提供的是宽度的下限，如果同时设置了basis和width，width本质上设置的是max-width的宽度，提供的是宽度的上限，如果basis大于width，也只能取width,如果width小于basis，也只能去basis

2. 设置了flex-basis容器的宽度会被不换行的内容撑开，width不会



#### 当项目被内容撑开的时候，不再参与压缩

无论什么情况下，被不换行内容撑开的容器，该容器不会被压缩计算

```html
<style>
    .wrapper{
        width: 600px;
        height: 600px;
        border: 1px solid #000;
        display: flex;
    }
    .content{
        height: 100px;
    }
    .content:first-of-type{
        background-color: #0f0;
        flex-shrink: 5;
        flex-basis: 200px;
    }
    .content:nth-of-type(2){
        background-color: #ff0;
        flex-shrink: 1;
        flex-basis: 200px;

    }
    .content:last-of-type{
        background-color: #00f;
        flex-shrink: 1;
        flex-basis: 400px;

    }
</style>    
<div class="wrapper">
    <div class="content">zmnkjashdahsdoiaabaks</div>
    <div class="content"></div>
    <div class="content"></div>
</div>
```

如果想要该容器能够被压缩计算，需要用break-word属性给文字换行。



#### flex案例

##### 1. 居中

```html
<style>
    .wrapper{
        resize: both;
        overflow: hidden;
        width: 300px;
        height: 300px;
        border: 1px solid #f0f;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .content{
        width: 100px;
        height: 100px;
        border: 1px solid #00f;
    }
</style>
<div class="wrapper">
    <div class="content"></div>
</div>
```

![居中](D:/Typora/typora-user-images/CSS3/居中.png)



##### 2. 可动态添加的导航栏

```html
<style>
    .wrapper{
        width: 300px;
        height: 300px;
        border: 1px solid #000;
        display: flex;
    }
    .item{
        flex: auto;
        height: 30px;
        color: #f20;
        border-radius: 5px;
        text-align: center;
        line-height: 30px;

    }
    .item:hover{
        background-color: #f20;
        color: #fff;
    }
</style>
<div class="wrapper">
    <div class="item">天猫</div>
    <div class="item">淘宝</div>
    <div class="item">京东</div>
    <div class="item">聚划算</div>
</div>
```

![动态导航栏](D:/Typora/typora-user-images/CSS3/动态导航栏.png)

##### 3. 等分布局

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .wrapper{
        resize: both;
        overflow: hidden;
        width: 400px;
        height: 200px;
        border: 1px solid black;
        display: flex;
    }
    .content{
        flex: 1 1 auto;
        border: 1px solid green;
        height: 100px;
        box-sizing: border-box;
    }
    .content:nth-of-type(2){
        flex: 0 0 50%;
    }
    .content:nth-of-type(3){
        /* 伸的多，压的也多 */
        flex: 2 2 auto;
    }
</style>    
<div class="wrapper">
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
</div>
```

![等分布局](D:/Typora/typora-user-images/CSS3/流式布局.png)

##### 4. 流式布局

```html
<style>
    .wrapper{
        resize: both;
        overflow: hidden;
        width: 400px;
        height: 600px;
        border: 1px solid black;
        display: flex;
        flex-wrap: wrap;
        align-content: flex-start;
    }
    .content{
        width: 100px;
        border: 1px solid green;
        height: 100px;
        box-sizing: border-box;
    }
</style>
<div class="wrapper">
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
    <div class="content"></div>
</div>
```



##### 5. 圣杯布局

> 所谓圣杯布局，就是header占高度的20%，footer占高度的20%，left占宽度的20%，right占宽度的20%，中间自适应

```html
<style>
    .wrapper{
        resize: both;
        overflow: hidden;
        width: 400px;
        height: 400px;
        border: 1px solid #000;
        display: flex;
        flex-direction: column;
    }
    .header, .footer, .left, .right{
        flex: 0 0 20%;
        border: 1px solid green;
    }
    .contain{
        flex: 1 1 auto;
        border: 1px solid green;
        display: flex;
    }
    .center{
        flex: 1 1 auto;
        border: 1px solid green;
    }
</style>
<div class="wrapper">
    <div class="header"></div>
    <div class="contain">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
    </div>
    <div class="footer"></div>
</div>
```



## 6. transition(过渡动画)

> 过渡动画，将一个状态过渡到另一个状态，使得状态转换显的不那么突兀

- transition-property：监听具有动画过渡的属性，默认值为all
  - all:监听所有具有动画过渡的属性
  - IDENT：指定要进行过渡的CSS属性
  - none：不指定过渡的CSS属性
- transition-duration：动画过渡的时间
- transition-timing-function：运作状态函数
  - linear：线性过渡，等同于贝塞尔曲线（0.0, 0.0, 1.0, 1.0 ）
  - ease：平滑过渡，等同于贝塞尔曲线（0.25, 0.1, 0,.25 1.0 ）
  - ease-in：由慢到快，等同于贝塞尔曲线（0.42, 0.0, 1.0, 1.0 ）
  - ease-out：由快到慢，等同于贝塞尔曲线（0.0, 0.0, 0.58, 1.0 ）
  - ease-in-out：由慢到快再到慢，等同于贝塞尔曲线（0.42, 0.0, 0.58, 1.0 ）
  - step-start：等同于steps(1,start)
  - step-end：等同于steps(1,end)
  - steps
  - cubic-bezier(number,number,number,number)：特点的贝塞尔曲线类型，4个数值需在【0，1】之间
- transition-delay：延迟多少秒再进行动画



### 6.1. 三次贝塞尔曲线

> 所谓贝塞尔曲线，就是用一段曲线去描述一段运动状态。
>
> 三次贝塞尔曲线在表示运动状态的时候基本上就是个st曲线，四个参数分别表示两个坐标点（控制点）。斜率K为正就表示往正方向移动，斜率越大，速度越快，同理，斜率K为负就表示往反向移动，斜率越大，速度越快，斜率不变，运动速度也就不变，为匀速运动

斜率k = (y1 - y2)/(x1 - x2)



![st坐标轴](D:/Typora/typora-user-images/CSS3/st坐标轴-1584115390886.png)

因此，对于贝塞尔曲线的坐标点，我们可以看作（时间1，路程1，时间2，路程2）v=s/t

三次贝塞尔曲线方程（曲线）：

**B(t) = P₀(1 - t)³ + 3P₁t(1 - t)² + 3P₂t²(1 - t) + P₃t³ ,t ∈ [0,1]**



B(t)为t时间下点的坐标

P₀代表起始点、P₃代表结束点

中间的P₁ - P₂代表控制点，控制点越多，贝塞尔曲线的阶数越高，可绘制的图像也越复杂



![三次贝塞尔曲线](D:/Typora/typora-user-images/CSS3/三次贝塞尔曲线.gif)



二次贝塞尔曲线方程（抛物线）：

**B(t) = (1 - t)²P₀ + 2t(1 - t)P₁ + t²P₂ ,t ∈ [0,1]**

![二次贝塞尔曲线](D:/Typora/typora-user-images/CSS3/二次贝塞尔曲线.gif)





一次贝塞尔曲线（线段）：

**B(t) = (1 - t)P₀ + tP₁ ,t ∈ [0,1]**

![一次贝塞尔曲线](D:/Typora/typora-user-images/CSS3/一次贝塞尔曲线-1584114684525.gif)



## 7. animation

> 可以实现多状态改变的连续动画



- animation-name：动画名称

- animation-duration：动画的持续时间

- animation-timing-function：动画的过渡类型，针对的是每一段关键帧的运动状态，而不是整个的运动状态

- animation-delay：动画延迟的时间

- animation-iteration-count：动画循环的次数，默认执行1次，无论执行几次，都算一个完整的动画

  - number：指定对象动画的具体循环次数
  - infinite：无限循环

- animation-direction：动画在循环中是否反向运动，默认normal表示正常方向

  - normal(默认值)：正常方向
  - reverse：反方向运动
  - alternate：动画先正常运行再反反向运行，并持续交替进行，前提的循环次数要 >=2
  - alternate-reverse：动画先反方向运行再正方向运行，并持续交替运行，前提的循环次数要 >=2

- animation-play-state：动画的状态，默认为running，表示运动，兼容性不算好，在一些场景中可以会出现暂停后会重新开始的现象的错误行为

  - running：运动
  - paused：暂停

- animation-fill-mode：动画时间之外的状态

  - forward：在动画结束之后，设置动画为最后一帧的状态
  - backward：在动画开始之前，设置动画为开始帧的状态
  - both：在动画开始之前，设置动画为第一帧的状态，在动画结束之后，设置动画为 最后一帧的状态

  



```html
关键帧容器 自定义容器名称
@keyframes run{
	里面用关键帧记录的运动状态改变次数,关键帧只能用百分数记录,起始关键帧和结束关键帧可以换成from和to表示
}

用animation读取关键帧来执行
animation：自定义容器名称，持续时间，过渡类型，延迟时间，循环次数，是否反向运动，状态，时间之外状态
```

一个元素可以同时并行执行多个关键帧。

关键帧容器兼容写法

- @keyframes

- -webkit-@keyframes

- -moz-@keyframes



案例一：正方形盒子运动

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    div{
        position: absolute;
        width: 100px;
        height: 100px;
        background-color: orange;
        animation: run 4s cubic-bezier(0,0,1,1),color-change 4s  cubic-bezier(0,0,1,1);
    }
    @keyframes run {
        0%{
            top: 0px;
            left: 0px;
        }
        25%{
            top: 0px;
            left: 100px;
        }
        50%{
            top: 100px;
            left: 100px;
        }
        75%{
            top: 100px;
            left: 0px;
        }
        100%{
            top: 0px;
            left: 0px;
        }

    }
    @keyframes color-change {
        0%{
            background-color:#0f0;
        }
        6%{
            background-color:#ff0;
        }
        100%{
            background-color:#00f;
        }

    }
</style>
<div></div>
```



效果一：日落月升

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    body{
        background: #000;
    }
    .space{
        width: 100%;
        height: 500px;
        background-image: linear-gradient(to bottom, rgba(0,130,255,1),rgba(255,255,255,1));
        animation: space-change 15s infinite;
    }
    @keyframes space-change{
        0%{
            opacity: 0.3;
        }
        25%{
            opacity: 1;
        }
        /* 上面是太阳升起部分 */
        50%{
            opacity: 0.3;
        }
        75%{
            opacity: 0.1;
        }
        /* 上面是太阳落下起部分，月亮升起部分 */
        100%{
            opacity: 0.3;
        }
    }
    .sun{
        position: absolute;
        width: 50px;
        height: 50px;
        left: calc(50% - 25px);
        top: calc(50% - 25px);
        border-radius: 50%;
        transform: scale(.5,.5);
        background-color: #fff;
        box-shadow: 0px 0px 80px 60px #fff,
            0px 0px 250px 125px #ff0,
            inset 0px 0px 10px #fff;
        animation: sunrise 15s ease-in-out infinite;
    }
    @keyframes sunrise{
        0%{
            opacity: 0;
        }
        10%{
            opacity: 1;
            transform: scale(0.7,0.7) translateY(0px);
        }
        25%{
            opacity: 1;
            transform: scale(0.5,0.5) translateY(-400px);
        }
        50%{
            opacity: 0;
            transform: scale(0.7,0.7) translateX(400px) translateY(0px);

        }
        100%{
            opacity: 0;
        }
    }
    .moon{
        position: absolute;
        left: calc(50% + 400px);
        top: calc(50% - 50px);
        width: 100px;
        height: 100px;
        background-color: #fff;
        border-radius: 50%;
        box-shadow: 0px 0px 8px #fff,
            inset 0px 0px 8px #fff;
        animation: moonrise 15s cubic-bezier(0,0,0.5,0.5) infinite;

    }
    .moon::after{
        position: absolute;
        content: "";
        width: 90px;
        height: 90px;
        background-color: #000;
        border-radius: 50%;
        left: -10px;
        top: -10px;
    }
    @keyframes moonrise{
        0%{
            opacity: 0;
        }
        50%{
            opacity: 0;
            transform: translateY(0);
        }
        70%{
            opacity: 1;
            transform: translateY(-300px);
        }
        80%{
            opacity: 1;
            transform: translateY(-300px);
        }
        100%{
            opacity: 0;
            transform: translateY(0);
        }
    }

</style>
<div class="space"></div>
<div class="sun"></div>
<div class="moon"></div>
```

![image-20200312170454950](D:/Typora/typora-user-images/CSS3/image-20200312170454950-1584115269868.png)



#### 7.1. timing-function的steps

> 阶跃函数,是transition-timing-function和animation-timing-function两个属性的属性值，他是表示两个关键帧之间的动画效果，默认是ease。

steps（）有两个参数：

- 参数1：是把这次过渡分成几段，这几段其实是在时间上分为几段去显示执行，可以理解为经过几帧			  跳到下一个状态，为1表示

- 参数2：有两个可选值，分别为start和end，
  - start表示在时间开始后的第一帧跳掉下一个状态，然后等待执行该状态的总时间结束，再立即跳到下一个状态的第一帧数
  - end：表示在当前时间结束前的最后一帧跳到另一个状态。这个特点是在执行到最后一个时，动画时间结束，最后一个状态只会出现一瞬间
  
  ```html
  @keyframes xxx{
  	0%{}
  	25%{}
  	50%{}
  	75%{}
  	100%{}
  }
  animation:all 1s steps(1,end)
  
  //表示动画执行一秒，然后1步过渡到下一个状态，关键帧一共有4个状态，则每个状态分得的时间是0.25秒，这里我们要思考一个问题，应该在什么时间跳转到下一个状态呢？是等2.5s后跳到下一个状态，还是先跳到下一个状态，然后等2.5s结束后再继续跳下一个状态。如果是第一种状态（end），当我们执行到最（75%）状态的时候，继续等待2.5s后再进行到最后一个状态，当刚好跳到最后一个状态的时候，此时动画时间刚好到一秒，动画结束，动画复位，此刻最后一个状态只是出现了一瞬间就恢复到原位了
  ```
  
  

#### 7.2. steps案例

##### 7.21. 打字效果

> 实现效果是用了遮罩的样式，上面盒子先覆盖所有文字，然后盒子逐渐右移，每次移动一个文字的距离。总宽度=字体的个数 X 字体的大小

```html
<style>
    *{
        margin: 0px;
        padding: 0px;
    }
    @keyframes cursor{
        0%{
            border-left-color: rgba(0, 0, 0, 0);
        }
        50%{
            border-left-color: rgba(0, 0, 0, 1);

        }
        100%{
            border-left-color: rgba(0, 0, 0, 0);

        }
    }

    @keyframes move{
        0%{
            left: 0;
        }
        100%{
            left: 100%;
        }
    }
    div{
        position: relative;
        display: inline-block;
        font-size: 80px;
        background-color: red;
        height: 100px;
        line-height: 100px;
        font-family: monospace;
    }
    div::after{
        content: "";
        position: absolute;
        left: 0;
        top: 10px;
        height: 90px;
        width: 100%;
        background-color: orange;
        box-sizing: border-box;
        border-left: 2px solid #000;
        animation: cursor 1s steps(1,end) infinite, move 30s steps(30,end);
    }
</style>
<div>AB CDEF GHIJKLM NOPKR SQUVWSYZ</div>
```

![image-20200313121930918](D:/Typora/typora-user-images/CSS3/image-20200313121930918-1584114624875.png)





##### 7.22. 表盘效果

> 

```html
<style>
    div.clock{
        position: relative;
        background-image: url("./images/clock.png");
        width: 512px;
        height: 512px;
        background-size: cover;
        background-repeat: no-repeat;
    }
    @keyframes secondrun{
        0%{
            transform: rotate(180deg);
        }
        100%{
            transform: rotate(540deg);
        }
    }
    div.second{
        position: absolute;
        left: 246.5px;
        top: 180px;
        background-image: url("./images/second.png");
        background-size: cover;
        background-repeat: no-repeat;
        width: 16px;
        height: 278px;
        z-index: 3;
        /* 设置旋转中心 */
        transform-origin: center  76px;
        transform:rotate(180deg);
        /* 60秒，分60步完成，相当于走一步用1s,每步旋转6度 */
        animation: secondrun 60s steps(60,end) infinite;
    }
    @keyframes minuteRun{
        0%{
            transform: rotate(180deg);
        }
        100%{
            transform: rotate(540deg);
        }
    }
    div.minute{
        position: absolute;
        left: 238px;
        top: 240px;
        width: 32px;
        height: 218px;
        background-image: url("./images/minute.png");
        background-repeat: no-repeat;
        background-size: cover;
        transform-origin: center  16px;
        transform:rotate(180deg);
        z-index: 2;
       	/* 3600秒，分60步完成，相当于走一步用60s,每步旋转6度 */
        animation: minuteRun 3600s steps(5,end);
    }
    div.hour{
        position: absolute;
        left: 238px;
        top: 240px;
        width: 32px;
        height: 148px;
        background-image: url("./images/hour.png");
        background-repeat: no-repeat;
        background-size: cover;
        transform-origin: center  16px;
        z-index:1;
    }

</style>
<div class="clock">
    <div class="second"></div>
    <div class="minute"></div>
    <div class="hour"></div>
</div>
```

![image-20200313121839483](D:/Typora/typora-user-images/CSS3/image-20200313121839483-1584114604419.png)





##### 7.23. 跑马效果

```html
<style>
    div{
        width: 200px;
        height: 100px;
        background-image: url("./images/horse.png");
        background-size: cover;
        background-repeat: no-repeat;
        background-position: 0 0;
        /* 1秒，分12步进行，每一步移动2400/12=200px */
        animation: run 1s steps(12,end) infinite;
    }
    @keyframes run {
        0%{
            background-position: 0 0;
        }
        100%{
            background-position: -2400px 0;
        }
    }
</style>
<div class="horse"></div>
```

![Rec 0019](D:/Typora/typora-user-images/CSS3/Rec 0019-1584114587855.gif)



## 8.transform

transform-origin：用于设置所有transform属性的旋转轴。默认值center center

transform-style：perspective-3d

perspective：

perspective-origin：

### 1.  rotate(deg)

> 将元素进行旋转，定义了一种将元素围绕一个定点旋转而不变形的转换。指定的角度定义了旋转的量度。若角度为正，则顺时针方向旋转，否则逆时针方向旋转。旋转180°也被称为点反射，转顺序的问题、

![image-20200313173033398](D:/Typora/typora-user-images/CSS3/image-20200313173033398-1584114568074.png)

- rotateX：沿着X轴旋转，从右往左看，顺时针旋转为正，逆时针旋转为父

- rotateY：沿着Y轴旋转，从上往下看，顺时针旋转为父，逆时针旋转为正
- rotateZ：沿着Z轴旋转，从正对电脑屏幕来看，顺时针为正，逆时针为父
- rotate3d(x,y,z,angle)： 是一个矢量，有方向，将该矢量作为一个空间轴，xyz三个值的比例将会成为矢量的方向，用来设定轴的方向,angle用来设定旋转角度

2D旋转

```html
<style>
    body{
        perspective: 800px;
        transform-style: preserve-3d;
        perspective-origin: 0px 0px;
    }
    div{
        width: 200px;
        height: 200px;
        background-image: url("../images/clock.png");
        background-size: 200px 200px;
        transform: rotateX(45deg) rotateY(20deg) rotateZ(110deg);
    }
</style>
<div></div>
```

![image-20200313164948151](D:/Typora/typora-user-images/CSS3/image-20200313164948151-1584114550973.png)



3D旋转

```html

    <style>
        body{
            perspective: 800px;
            transform-style: preserve-3d;
            perspective-origin: 0px 0px;
        }
        div{
            width: 200px;
            height: 100px;
            background-image: url("../images/pic4.jpeg");
            background-size: 200px 200px;
            transform: rotate3d(1,1,1,0deg);
        }
    </style>
<div>  </div>
```

![image-20200313170127420](D:/Typora/typora-user-images/CSS3/image-20200313170127420-1584114520856.png)



3D旋转

```html
<style>
    body{
        perspective: 800px;
        transform-style: preserve-3d;
        perspective-origin: 0px 0px;
    }
    div{
        width: 200px;
        height: 200px;
        background-image: url("../images/pic4.jpeg");
        background-size: 200px 200px;
        transform: rotate3d(1,1,0,0deg);
    }
</style>
<div></div>
```



![image-20200313172904686](D:/Typora/typora-user-images/CSS3/image-20200313172904686-1584114495173.png)



### 2. scale(x, y) 

> 伸缩变换，有两个参数，分别为X，Y
>
> 不过有几个注意点：
>
> 1、伸缩的不是元素大小，伸缩的是此元素的变化坐标轴的刻度
>
> ![坐标轴刻度](D:/Typora/typora-user-images/CSS3/image-20200313195721507-1584114418411.png)
>
> 
>
> 2、scale可以进行叠加操作，后一个scale会依就前一个scale的比例进行伸缩变换
>
> ```css
> transform: scale(.5,.5) scale(3,3);
> 等价于
> transform: scale(1.5,1.5);
> ```
>
> 
>
> 3、伸缩的影响会被一直保留下去，而且轴会随着旋转而旋转
>
> ```css
> //如何理解伸缩的影响会被一直保留下去？看一个例子
> transform: scale(2,1) rotate(-90deg)scale(2,1);
> 上面表示，首先水平坐标轴是平行于地面的，首先伸缩scale(2,1)表示元素水平坐标轴的刻度增大一倍数，然后开始旋转totate(-90deg),此时伸缩轴也随着旋转而旋转，等到旋转到-75deg时，此刻之前的Y轴跑到了之前X轴的位置，我们发现，Y轴的宽度明显也变大了，而之前在X轴方向的图片明显缩短了,假设缩到-90deg时，前Y轴会彻底跑到前X轴的方向，此时前Y轴的坐标轴就是前X轴的坐标轴，刻度自然也会增大一杯，而前X轴跑到了前Y轴的位置，刻度自然缩写一倍,而最后再伸缩scale(2，1)的时候，会在新的X轴上进行伸缩
> ```
>
> 
>
> 原图![scale原图](D:/Typora/typora-user-images/CSS3/scale原图-1584114994036.png)
>
> scale(2,1)![scale(D:/Typora/typora-user-images/CSS3/scale(2,1)-1584114956682.png)](D:/Typora/typora-user-images/CSS3/scale(2,1).png)
>
> rotate(-75deg)![image-20200313210723526](D:/Typora/typora-user-images/CSS3/image-20200313210723526-1584114859902.png)
>
> 
>
> scale(2,1)
>
> ![scale(2,1)](D:/Typora/typora-user-images/CSS3/image-20200313211231802-1584114808056.png)

```html
<style>
    body{
        perspective: 800px;
        transform-style: preserve-3d;
        perspective-origin: 300px 300px;
    }
    div{
        margin-left: 200px;
        margin-top: 600px;
        width: 200px;
        height: 200px;
        background-image: url("../images/pic4.jpeg");
        background-size: 200px 200px;
        transform-origin: 0px 0px;
        transform: scale(2,1) rotate(-90deg) scale(2,1);
    }
</style>
<div>  </div>
```



- scaleX(x)：伸缩元素的X变化坐标轴的刻度

- scaleY(y)：伸缩元素的Y变化坐标轴的刻度

- scaleZ(z)：伸缩元素的Z变化坐标轴的刻度

- scale3d(x,y,z)：伸缩元素的XYZ变化坐标轴的刻度

  z轴的拉伸

  ```html
     <style>
          body{
              perspective: 800px;
              transform-style: preserve-3d;
              perspective-origin: 300px 300px;
          }
          div{
              position: absolute;
              left: 200px;
              top: 200px;
              width: 200px;
              height: 200px;
              background-image: url("../images/pic4.jpeg");
              background-size: cover;
              transform-origin: 0px 0px;
              transform: rotateY(0deg) scaleZ(1) rotateX(0deg);
          }
      </style>
  ```

  拉伸是受到第二个坐标轴的影响，该坐标轴的Z方向是从左到右

  ![scale拉伸](D:/Typora/typora-user-images/CSS3/image-20200314133250486.png)

### 3. skew(ax,ay)

> 该函数定义了一个元素在二维平面上的倾斜转换，默认倾斜为0，0，倾斜的是元素的变化坐标轴，而不是元素本身。

参数值：

- `ax` 是一个`angle`表示用于沿横坐标扭曲元素的角度。

- `ay` 是一个 `angle`表示用于沿纵坐标扭曲元素的角度。如果未定义，则其默认值为0，导致纯水平倾斜。

```html
   <style>
        *{
            margin: 0;
            padding: 0;
        }
        body{
            perspective: 800px;
            transform-style: preserve-3d;
            perspective-origin: 300px 300px;
        }
        div{
            position: absolute;
            left: 200px;
            top: 200px;
            width: 200px;
            height: 200px;
            background-image: url("../images/pic4.jpeg");
            background-size: cover;
            transform-origin: center center;
            transform: skew(45deg,0deg);

        }
    </style> 
	<div></div>
    <hr style="position: absolute; top: 200px; width: 100%;">
    <hr style="position: absolute; top: 400px; width: 100%;">
```

skew前

![skew前](D:/Typora/typora-user-images/CSS3/image-20200314135540462.png)

skew后

![skew后](D:/Typora/typora-user-images/CSS3/image-20200314135905080.png)

我们发现，尽管元素发生了倾斜，但是在Y轴的高度依旧没有发生变化，依然和未拉伸的图形高度保持一致，根据数学常识，元素倾斜后，在原Y轴上的高度一定会减少，而不是没有变化，这只能说明一个问题，就是不仅坐标轴倾斜了，连坐标轴的刻度都被拉伸了了

![skew](D:/Typora/typora-user-images/CSS3/image-20200314135119272.png)



skewX(xdeg)：

skewY(ydeg)：

![image-20200314142914422](D:/Typora/typora-user-images/CSS3/image-20200314142914422.png)

### 4. translate(x,y )

> 移动，会受到变化坐标轴伸缩变换的影响，定点位置参照transform-origin。

translate居中

```
left:50%
transform:translatex(-50%);
//
top:50%
transform:translatey(-50%)
```

这个居中不需要知道子级元素的宽高也能实现居中定位。



### 5. perspective

> 景深，人物离屏幕的距离，我们看到的所有结果，都是在元素在屏幕上的投影

![景深](D:/Typora/typora-user-images/CSS3/image-20200314152653194.png)



perspective需配合translateZ的使用才能发挥出其真正的作用，如果没有配合translateZ使用，perspective设置多少都没有意义。

![image-20200314160902328](D:/Typora/typora-user-images/CSS3/image-20200314160902328.png)

情况一：perspective: 800px ，translateZ：100px

perspective: 800px表示距离屏幕800px的距离，当设置transletez：100px时，表示圈圈向屏幕前（左）移动100px，此时我们发现该元素的投影投射在屏幕上的时候会变大

![translate100px](D:/Typora/typora-user-images/CSS3/image-20200314154014838.png)

如果我们再将眼睛更加靠近屏幕（perspective减少），此刻元素在屏幕上的投影会越来越大

![image-20200314154308370](D:/Typora/typora-user-images/CSS3/image-20200314154308370.png)

当眼睛越来越靠近屏幕的时候，（perspective接近于100的时候），此时看到的圈圈将无限大。



情况二：perspective: 800px ，translateZ：-100px

perspective: 800px表示距离屏幕800px的距离，当设置transletez：-100px时，表示圈圈向屏幕后（右）移动100px，此时我们发现该元素的投影投射在屏幕上的时候会变小

![translate-100](D:/Typora/typora-user-images/CSS3/image-20200314155316381.png)

如果我们再将眼睛更加靠近屏幕（perspective减少），此刻元素在屏幕上的投影会越来越小

![image-20200314155402212](D:/Typora/typora-user-images/CSS3/image-20200314155402212.png)

当眼睛越来越靠近屏幕的时候，（perspective接近于0的时候），此时看到的圈圈将无限小。

注意：perspective需要设置在元素的父级上，这个才有元素展现的空间。



**总结：**

1. translateZ为0的时候，元素本身就在屏幕上，没有投影，无论perspective如何改变，元素的大小都不会变化
2. translateZ为正的时候，perspective越大（眼睛离屏幕越远），看到的元素在屏幕上的投影越小，反之perspective越小（眼睛离屏幕越近），看到的元素在屏幕上的投影越大
3. translateZ为负的时候，perspective越大(眼睛离屏幕越远)，看到的元素在屏幕上的投影越大，反之perspective越小（眼睛离屏幕越近），看到的元素在屏幕上的投影越小



#### 5.1. perspective-origin：

> 视角，就是眼睛看的屏幕的位置

```html
<style>
    * {
        margin: 0px;
        padding: 0px;
    }
    :root{
        height: 100%;
    }
    body{
        perspective: 800px;
        height: 100%;
    }
    .content1,
    .content2,
    .content3,
    .content4,
    .content5{
        width: 200px;
        height: 200px;
        background-image: url("../images/pic4.jpeg");
        background-size: cover;
        position: absolute;
        top: 200px;
        transform: rotateY(45deg);
    }
    .content1{
        left: 200px;
    }
    .content2{
        left: 400px;
    }
    .content3{
        left: 600px;
    }
    .content4{
        left: 800px;
    }
    .content5{
        left: 1000px;
    }
</style>    
<div class="content1"></div>
<div class="content2"></div>
<div class="content3"></div>
<div class="content4"></div>
<div class="content5"></div>
<script>
    document.body.onmousemove = function(e){
        this.style.perspectiveOrigin =""+ e.pageX + "px " + e.pageY  + "px";
    }
</script>
```

![image-20200314163652660](D:/Typora/typora-user-images/CSS3/image-20200314163652660.png)

当我们移动鼠标的时候，鼠标就是我们的视角，眼睛视角的不同，看到的几张图片拉伸也不同，在鼠标所放位置的图片是最标准的。其余的会有不同程度的拉伸



如果我们在子元素上设置视角的话，相当于给每个元素自身上都加上了一只眼睛，每只眼睛都对着图片的正中间，所有展示出的效果都是一致的，

在子元素上设置：`transform: perspective(800px)`,需要先干掉父级的景深

```css
    .content1,
    .content2,
    .content3,
    .content4,
    .content5{
        width: 200px;
        height: 200px;
        background-image: url("../images/pic4.jpeg");
        background-size: cover;
        position: absolute;
        top: 200px;
        transform: transform: perspective(800px) rotateY(45deg);
    }
```

![image-20200314164557388](D:/Typora/typora-user-images/CSS3/image-20200314164557388.png)

不过该属性有点缺陷：

- 不能放在transform的最后面，由于不同浏览器的问题，可能无法解析该属性
- 无法为在子元素的设置了该属性的元素设置perspective-origin属性，默认只是center center，因为并没有为我们开发这个功能接口



**景深也是可以叠加的**



#### 5.2. transform-style

>  指定某元素是（看起来）位于三维空间内，还是在该元素所在的平面内被扁平化

可选值：

- flet(默认值)：指定子元素位于此元素所在平面内
- perspective-3d：指定子元素定位在三位空间内

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    body{
        perspective: 800px;
        perspective-origin: 300px 100px;
    }
    .wrapper{
        position: absolute;
        left: 200px;
        top: 200px;
        width: 200px;
        height: 200px;
        background-color: #f10;
        transform: rotateY(45deg);
        transform-style: preserve-3d;

    }
    .demo{
        width: 200px;
        height: 200px;
        background-image: url("../images/pic4.jpeg");
        background-size: cover;
        transform: translateZ(100px);
    }
</style>
<div class="wrapper">
    <div class="demo"></div>
</div>
```

![image-20200314170737189](D:/Typora/typora-user-images/CSS3/image-20200314170737189.png)

#### 5.3. transform-origin

> 设置旋转轴，也可以让旋转轴变成空间形式



### 6、矩阵（matrix）

> 所谓矩阵，就是transform给我们选中的计算规则

平面变换的矩阵基础

![image-20200315170510726](D:/Typora/typora-user-images/CSS3/image-20200315170510726.png)

**translate的反推导**

|1 0 e |			|x|			 |x + e|

|0 1 f  |	*	  |y|      =     |y + f|

|0 0 1 |            |1|              |1|



上面的0 0 1 是为了方便计算添加的值，没有什么用处，

上面的x y是要变换图形的原始的坐标

上面的e,f是我们选添的值



得出的结论x+e，y+f是不是像我们的平移，基于xy原坐标向右移动e，向下移动f个单位

matrix(1,0,0,1,e,f) === translate(x,y)



**scale的反推导**

要想推导出scale，我们就要明白scale是如何变化的，scale的变化就相当于，x轴上原始的大小* 变化的倍数 = ax，y轴上原始的大小*变化的倍数 = yx，基于这两个数，我能就能反推变化的i帽和j帽

|a 0 0 |			|x|			 |ax|

|0 d 0 |	*	  |y|      =     |dy|

|0 0 1 |            |1|              |1|



matrix(a,0,0,d,0,0) === scale(x ,y )



**rotate的反推导**

![image-20200315175057126](D:/Typora/typora-user-images/CSS3/image-20200315175057126.png)

|cos() -sin() 0 |			|x|			 |cons()x + -sin()y |

|sin()  cos() 0 |	*	  |y|      =     |sin()x + cos()y|

|0 0 1 |                         |1|              |1|



matrix(cos(),sin(),-sin(),cos(),0,0) = rotate



**skew的反推导**

matrix(1,tan(y),tan(x),1,0,0) ===skew()



**3D的缩放平移**

matrix(1,,0,0,0,0,1,0,0,0,0,1,0,x,y,z,1)缩放

matrix(x,0,0,0,0,y,0,0,0,0,z,0,0,0,0,1)平移





## 线性代数

### 1、向量究竟是什么？

在计算机的表示中，向量 === 数字列表，用[x,y]表示。

向量加：[x1,y1] + [x2,y2] = [x1+x2，y1+y2]

向量数乘：倍数*[x,y] =[倍数 \* x , 倍数 \* y] 

### 2. 向量的线性组合，张成的空间和基

基：

所谓基，就是i帽和j帽在xy坐标轴的基向量

i帽：一个指向正右方，长度为1的单位向量

j帽：一个指向正上方，长度为1的单位向量

基向量，也叫单位向量，是单位长度为1的向量，如下图中：`i帽` 和 `j帽` 就是这个二维坐标系的基向量：



张成的空间：

向量v与向量w全部线性组合构成的向量集合成为“张成的空间”

![image-20200315155158798](D:/Typora/typora-user-images/CSS3/image-20200315155158798.png)



![image-20200315160602181](D:/Typora/typora-user-images/CSS3/image-20200315160602181.png)

可以看成行和列相乘



### 9、性能优化

#### CPU和GPU

cpu擅长逻辑运算

GPU擅长浮点运算

### 浏览器渲染流程

1. 首先按顺序加载html、css、js
2. css构建一个rules tree（规则树，里面只有css的一些规则什么的）
3. js调用domAPI形成一个dom tree（节点树）
4. js调用cssomAPI，该接口会结合css rules tree  和 dom tree形成cssom Tree（和dom tree差不多，只是多加了css规则，并且display属性也依旧在树上）
5. dom tree 和cssom Tree 组合形成renderTree(依旧和cssom tree差不多，只不过display属性的元素不会挂在该树上，也就不会渲染，只留下真正需要渲染的)
6. 先基于render tree 进行layout（布局）重排（reflow）
7. 最后再基于render tree 进行paint(上色) 重绘（repaint）
8. 逻辑图（多层矢量图）----> 实际绘制（栅格化），绘制的时候尽量调用GPU进行绘制，谷歌浏览器天然默认用GPU进行绘制。



触发reflow（重排）的情况：

- 改变窗口大小
- 改变文字大小
- 内容的改变，输入框输入文字
- 激活伪类：如：hover
- 操作class属性
- 脚本操作DOM
- 计算offsetwidth 和offsetHeight。调用这两个属性时系统会再一次进行reflow得到当前最新的值
- 设置style属性



触发repaint（重绘）的情况：

- 如果只是改变某个元素的背景色，文字颜色，边框颜色，不影响它周围或内部布局的属性

repaint 速度快于reflow





触发GPU到新的层进行操作的属性

opacity:

transform:translatez(0)



GPU加速

will-change：监听需要变化的属性，提醒GPU新起一个层。在新一个层面单独操作该属性

will-change:transform

```html
div:hover{
	will-change:transform //hover的时候提醒GPU单独对tranform属性新开一层进行操作
}
div:active{
	transfrom:scale(1,2) //点击过后关闭will-change
}
```



浏览器刷新页面的屏幕是1s 60次

平均16.7mm

GPU可以在一帧里渲染好页面，那么当你改动页面的元素或实现动画的时候，将会非常流畅





## 10. 硬货！显示器的成像原理以及PX的实际意义

**空间混色法：**

一个像素由三个像点组成（红绿蓝），三个像点的排列方式在空间物理的设备上是并排排列的，而不是叠加到一起的，但是颜色需要叠加才能产生其他颜色，那这是如果办到的？实际上就是利用入眼对于太小的不可视的原因，将某一个颜色放小，这样人眼看到的就是两种颜色的混色

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .wrapper{
        display: flex;
    }
    .demo{
        width: 1px;
        height: 10px;
    }
    .demo:nth-of-type(2n){
        background-color: #f10;/* 红色 */
    }
    .demo:nth-of-type(2n+1){
        background-color: #00f;/* 蓝色 */
    }
</style>
<div class="wrapper">
    div.demo*1000
</div>
```

![image-20200316135556447](D:/Typora/typora-user-images/CSS3/image-20200316135556447.png)

可以发现颜色变成了紫色



**像素**：

像素由三个像点组成，像点才是最小单位



**crt显示屏 和 lcd液晶屏成像原理：**



CRT显示屏就是我们以前的大头电视机

成像原理：CRT显示器的工作原理是用电子束激发屏幕内表面的荧光粉来显示图像。由灯丝加热阴极，阴极发射电子，然后在加速极电场的作用下，经聚焦极聚成很细的电子束，在阳极高压作用下，获得巨大的能量，以极高的速度去轰击荧光粉层。荧光粉单元受到激发产生丰富的色彩。

![image-20200316144436056](D:/Typora/typora-user-images/CSS3/image-20200316144436056.png)

<img src="D:/Typora/typora-user-images/CSS3/image-20200316143505806.png" alt="image-20200316143505806" style="zoom:150%;" />

荧光粉的排列方式就是像点的排列方式



点距：同色点之间的距离就叫点距，是描述成像细腻以否的重要指标，点距越小，像素点就越小，成像越细腻。点距可以用来表示像素大小，但不一定等同于像素，约等于像素。

![image-20200316144507423](D:/Typora/typora-user-images/CSS3/image-20200316144507423.png)

**物理像素**

**dpi**

> 一英寸(2.54cm)所能容纳的像素点数 (i in = 2.54cm)，dpi越高，表示一英寸下可容纳的像素点越多（点距数）。

96dpi ≈ 2.54 /100 = 0.25mm（一个像素为0.25mm，则一个像点≈0.07mm）,表示一英寸里有96个像素点

dpi = ppi

dpi 打印机在一英寸的屏幕里面可以打印多少个墨点

ppi 一英寸所能容纳的像素点数（点距数）



**参照像素**（标杆）——CSS像素——逻辑像素

> 为了让不同机器上的显示起能有一样的体验效果，就是以这个像素呈现时的大小去转变，一般是以96dpi屏幕以一臂之遥的距离看到的大小为标准进行转换，其他dpi屏幕会依照96dpi像素相转换，最终达到在不同dpi屏幕上显示同样的大小。

​								200dpi / 96dpi =  2

设备像素比 dpr = 物理像素/css像素  

在不同设备上的实际物理像素 = css像素 X dpr

 **需要明白的一点是，设备像素比只能是整数，因为不可能把像素劈半个。**

ipone6的dpr = 2，说明物理像素是我们css像素的两倍，只要我们设置50px，在iphone6上显示的实际大小物理像素就是：css像素 x dpr



我们也管css编程的逻辑像素方式，叫做逻辑屏幕

![逻辑屏幕](D:/Typora/typora-user-images/CSS3/image-20200316154847109.png)

不看分辨率

1920 x1080 固定宽高下，展示的像素点数



### 视口

width = device-width：移动端设备的宽度就等价于实际页面的宽度，就是如果页面宽度为980，手机宽度为375，如果设置width=980的时候，相当于把页面的980缩到375宽度内，会导致所个元素变小，元素变小说明css像素，width设置的是layout视口，layout就是整个页面的pc大小. width：是布局视口的width
device-width：是理想视口的宽度

initial-scale:1.0：实际宽度和移动端设备的宽度是1:1的比例



psd原稿的尺寸是按照设备像素设计，以iphone6为例，就是750px