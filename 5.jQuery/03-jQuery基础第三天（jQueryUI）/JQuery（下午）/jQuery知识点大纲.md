# jQuery 第三天知识点大纲

## 一、复习
* 第一天
    * jQuery优点
    * jQuery入口函数
    * jQuery 选择器
* 第二天
    + jQuery CSS操作
    + jQuery 动画
    + jQuery Dom操作
    
--------------------------------------------------

## 二、jQuery的元素操作

### 2.1 jQuery获取元素的高和宽

- height ： $(selector).height(); //带参数设置，不带参数获取,参数是number类型
- width  ： $(selector).width(200);  //带参数设置，不带参数获取

- innerHeight  //只能获取：内边距+内容的高度
- innerWidth   //同上.........宽度

- outerHeight
- outerWidth    //获取：左右内边距+内容+**左右边框**    
      
```
    案例： 01 元素宽高.html
```


### 2.2 jQuery元素坐标操作

+ offset() 获取或设置元素相对于文档位置的方法
    + 返回一个object，包含left和top属性，值是相对于document的位置。
    + 如果传入一个参数，则是对元素重新设置相对于document的位置。  
        - **传入参数必须包括：left和top属性。比如： {left：100，top：150}**    

        例如：$(selector).offset({left:100, top: 150});    

+ position() 获取相对于其最近的定位的父元素的位置。
    * 只能获取，不能设置。
    * 相对与其最近的__定位__元素
    * 返回一个object，包含left和top属性    
    
    ```
    例如： $(selector).position();
    ```


+ 元素的滚动
    * scrollLeft() 获取或者设置元素水平方向滚动的位置
    * scrollTop() 获取或者设置窗口垂直方向的滚动的位置   
    * 例如：     
    ```
        $(selector).scrollLeft(100);
    ```


    * scroll() 事件触发或者绑定滚动事件
        - scroll(hander) 绑定滚动事件
        - scroll() 触发滚动事件
    ```
        $(selector).scroll(function(){
            //当选择的元素发生滚动的时候触发
        });
    ```

```
    案例：    
    02 元素滚动案例.html
    03图片滚动跟随案例.html
    04滚动固定定位案例.html
```

--------------------------------------------------------------------

## 三、jQuery事件绑定机制

### 3.1 简单事件绑定方法

+ .click(hander)  .click() //绑定事件 或者触发 click事件
+ .blur()  //失去焦点事件，同上
+ .hover(mousein, mouseleave) //鼠标移入，移出

+ mouseout： 当鼠标离开元素及它的子元素的时都会触发。
+ mouseleave: 当鼠标离开自己时才会触发，子元素不触发。

+ .dbclick() 双击
+ change 改变,比如：文本框发送改变，下来列表发生改变等...
+ focus 获得焦点
+ keyup, keydown, keypress :  键盘 键被按下。
+ mousedown, mouseover

+ 其他参考：http://www.w3school.com.cn/jquery/jquery_ref_events.asp

```
案例：
    05图片跟随鼠标.html
```


### 3.2 绑定事件的方式 bind方式（不推荐，1.7以后的jQuery版本被on取代）
+ 语法格式：.bind( eventType [, eventData ], handler )   
+ 参数说明
    - 第一个参数：事件类型
    - 第二个参数：传递给事件响应方法的数据对象，可以省略。
    - 事件响应方法中获取数据方式： e.data
    - 第三个参数：事件响应方法
+  **第二个参数可以省略。**

```
    例如：    
    $("p").bind("click", function(e){
        //事件响应方法
    });
```


### 3.3 delegate方式（推荐，性能高，支持动态创建的元素）
+ 语法格式：$(selector).delegate( selector, eventType, handler )
+ 语法说明：
    * 第一个参数:selector， 子选择器
    * 第二个参数：事件类型
    * 第三个参数：事件响应方法

```
    例如：   
    $(".parentBox").delegate("p", "click", function(){
        //为 .parentBox下面的所有的p标签绑定事件
    });
```
*优势：效率较高*

### 3.4 on方式（最现代的方式，兼容zepto，强烈建议使用的方式）
+ jQuery1.7版本后，jQuery用on统一了所有的事件处理的方法
+ 语法格式：$(selector).on( events [, selector ] [, data ], handler )
+ 参数介绍：
    * 第一个参数：events，事件名
    * 第二个参数：selector,类似delegate
    * 第三个参数: 传递给事件响应方法的参数
    * 第四个参数：handler，事件处理方法
```
    例如：
    //绑定一个方法
    $( "#dataTable tbody tr" ).on( "click", function() {
      console.log( $( this ).text() );
    });

    //给子元素绑定事件
    $( "#dataTable tbody" ).on( "click", "tr", function() {
      console.log( $( this ).text() );
    });

    //绑定多个事件的方式
    $( "div.test" ).on({
      click: function() {
        $( this ).toggleClass( "active" );
      }, mouseenter: function() {
        $( this ).addClass( "inside" );
      }, mouseleave: function() {
        $( this ).removeClass( "inside" );
      }
    });
```

### 3.5 one绑定一次事件的方式
+ .one( events [, data ], handler )
```
    例如：
    $( "p" ).one( "click", function() {
      alert( $( this ).text() );
    });
```

### 3.6 解绑事件
+ unbind解绑 bind方式绑定的事件( 在jQuery1.7以上被 on和off代替)
    * $(selector).unbind();         //解绑所有的事件
    * $(selector).unbind("click");  //解绑指定的事件

+ undelegate解绑delegate事件
    * $( "p" ).undelegate();            //解绑所有的delegate事件
    * $( "p" ).undelegate( "click" );   //解绑所有的click事件
    
```
    例如：
    var foo = function () {
      // Code to handle some kind of event
    };
     
    // ... Now foo will be called when paragraphs are clicked ...
    $( "body" ).delegate( "p", "click", foo );
     
    // ... foo will no longer be called.
    $( "body" ).undelegate( "p", "click", foo );
```

+ off解绑on方式绑定的事件
    * $( "p" ).off();
    * $( "p" ).off( "click", "**" );    // 解绑所有的click事件，两个\*表示所有
    * $( "body" ).off( "click", "p", foo );
    
```
    案例1：
    var foo = function() {
      // Code to handle some kind of event
    };
     
    // ... Now foo will be called when paragraphs are clicked ...
    $( "body" ).on( "click", "p", foo );
     
    // ... Foo will no longer be called.
    $( "body" ).off( "click", "p", foo );

    案例2：（了解）解绑命名空间的方式：
    var validate = function() {
      // Code to validate form entries
    };
     
    // Delegate events under the ".validator" namespace
    $( "form" ).on( "click.validator", "button", validate );
     
    $( "form" ).on( "keypress.validator", "input[type='text']", validate );
     
    // Remove event handlers in the ".validator" namespace
    $( "form" ).off( ".validator" );
```

### 3.7 触发事件
+ 简单事件触发
    * $(selector).click(); //触发 click事件
+ trigger方法触发事件
    * $( "#foo" ).trigger( "click" );
+ triggerHandler触发 事件响应方法，不触发浏览器行为
    * $( "input" ).triggerHandler( "focus" );

### 3.8 event对象的简介
+ event.data            //传递的额外事件响应方法的额外参数

+ event.currentTarget === this   //在事件响应方法中等同于this，当前Dom对象

+ **event.target **         //事件触发源，不一定===this

+ **event.pageX**           //The mouse position relative to the left edge of the document
+ **event.pageY**

+ **event.stopPropagation()**//阻止事件冒泡
+ **e.preventDefault();**    //阻止默认行为

+ event.type                 //事件类型：click，dbclick...
+ event.which                //鼠标的按键类型：左1 中2 右3
+ keydown : a,b,c
+ event.keyCode// code的c是大写

```
    案例：07图片跟随案例.html
```

##四、 jQuery其他补充
+ 4.1 链式编程： end()补充
    * 补充五角星 评论案例
    * 第一步：鼠标移入，当前五角星和前面的五角星变实体。后面的变空心五角星
    * 第二步：鼠标点击的时候，为当前元素添加clicked类，其他的移除clicked类
    * 第三步：当鼠标移开整个评分控件的时候，把clicked的之前的五角星显示实心

+ 4.2 隐式迭代
    * 默认情况下，会自动迭代执行jQuery选择出来所有dom元素的操作。
    * 如果获取的是多元素的值，默认返回的是第一个元素的值。

+ 4.3 map函数
    * $.map(arry,function(object,index){})  *返回一个新的数组*
    * 
    * var arry = $("li").map(function(index, element){}) *注意参数的顺序是反的*
    
```
        var newArr = $.map($("li"), function(i, e) {
            return $(e).text() + i;//每一项返回的结果组成新数组
        });

        var newArr = $("li").map(function(elem, index){
            console.log("elem:" + elem);
            console.log("index:" + index);
            retrun index;
        });
```
+ 4.4 each函数
    * 全局的
    * $.each(array, function(index, object){})
    * 
    * $("li").each(function(index, element){} )
    * 参数的顺序是一致的。
```
    例如：
    $( "li" ).each(function() {
      $( this ).addClass( "foo" );
    });

    $( "li" ).each(function( index ) {
      console.log( index + ": " + $( this ).text() );
    });

    $( "div" ).each(function( index, element ) {});
```


+ 4.5 noConflict 全局对象污染冲突
    $  jQuery

    var $ = { name : "itecast" };

    <script src="jQueryDemos/jquery-1.11.3.min.js"></script>
    <!-- $ === jQuery -->

    var laoma_jQ = $.noConflict();//让jQuery释放 $, 让$ 回归到jQuery之前的对象定义上去。
    $.name

    jQuery

+ 4.6 jQuery.data()  jQuery对象的数据缓存。(了解)

    * $(".nav").data("name", 123);//设置值。
      var t  = $(".nav").data("name"); //获取值
      t.name = "18";//对象的更改，会直接同步到 元素的jQuery对象上去。

+ 4.7 会做jQuery插件

    * 全局jQuery函数扩展方法
```
     $.log = function() {
        
     }

     //$("li")
```

    * jQuery对象扩展方法
```
    $.fn.log = function() {
        
    }
```
    
+ 4.8 引入第三方插件
    * 背景色动画插件
    * lazyload 插件
    * jQuery UI 插件

+ 4.9 sublime 装插件
    * sublime 3
    *  第一步： 装上sublime的包管理器package control 
    *   ctrl+ ~  
    *   上网把 按照package control的那段代码，粘贴一下，然后回车。
    *   
    *  第二步：使用Ctrl + shift + p








