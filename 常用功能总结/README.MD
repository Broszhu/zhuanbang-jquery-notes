# 1、关于页面元素的引用

通过jQuery的$()引用元素包括通过id、class、元素名以及元素的层级关系及dom或者xpath条件等方法，且返回的对象为jquery对象（集合对象），不能直接调用dom定义的方法；
 
# 2、jQuery对象与dom对象的转换

只有jquery对象才能使用jquery定义的方法。注意dom对象和jquery对象是有区别的，调用方法时要注意操作的是dom对象还是 jquery对象。

普通的dom对象一般可以通过$()转换成jquery对象。
    
    var $msg=$(document.getElementById("msg"));

如上：则为jquery对象，可以使用jquery的方法；jQuery对象一般用$开头作为标示；

由于jquery对象本身是一个集合。是上一个数组；

所以如果jquery对象要转换为dom对象则必须取出其中的某一项，一般可通过索引取出。

如：$("#msg")[0]，$("div").eq(0)[0]，$("div").get(0)，$("td")[5]这些都是dom对象，可以使用dom中的方法，但不能再使用Jquery的方法。

以下几种写法都是正确的：
- $("#msg").html();
- $("#msg")[0].innerHTML;
- $("#msg").eq(0)[0].innerHTML;
- $("#msg").get(0).innerHTML;

# 3、如何获取jQuery集合的某一项

对于获取的元素集合，获取其中的某一项（通过索引指定）可以使用eq或get(n)方法或者索引号获取，要注意，eq返回的是jquery对象，而 get(n)和索引返回的是dom元素对象。

    $(this).get(0)与$(this)[0]等价

对于jquery对象只能使用jquery的方法，而dom对象只能使用dom的方法，如要获取第三个<div>元素的内容。有如下两种方法：

- $("div").eq(2).html(); //调用jquery对象的方法
- $("div").get(2).innerHTML; //调用dom的方法属性

# 4、同一函数实现set和get

Jquery中的很多方法都是如此，主要包括如下几个：

- $("#msg").html(); //返回id为msg的元素节点的html内容。
- $("#msg").html("<b>new content</b>");//将“<b>new content</b>” 作为html串写入id为msg的元素节点内容中,页面显示粗体的new content
- $("#msg").text(); //返回id为msg的元素节点的文本内容。
- $("#msg").text("<b>new content</b>");//将“<b>new content</b>” 作为普通文本串写入id为msg的元素节点内容中,页面显示<b>new content</b>
- $("#msg").height(); //返回id为msg的元素的高度
- $("#msg").height("300"); //将id为msg的元素的高度设为300
- $("#msg").width(); //返回id为msg的元素的宽度
- $("#msg").width("300"); //将id为msg的元素的宽度设为300
- $("input").val("); //返回表单输入框的value值
- $("input").val("test"); //将表单输入框的value值设为test
- $("#msg").click(); //触发id为msg的元素的单击事件
- $("#msg").click(fn); //为id为msg的元素单击事件添加函数

同样blur,focus,select,submit事件都可以有着两种调用方法
 
# 5、集合处理功能

对于jquery返回的集合内容无需我们自己循环遍历并对每个对象分别做处理，jquery已经为我们提供的很方便的方法进行集合的处理。
包括两种形式：
    
    $("p").each(function(i){
        this.style.color=['#f00','#0f0','#00f'][i];
    });//为索引分别为0，1，2的p元素分别设定不同的字体颜色。
 
    $("tr").each(function(i){
        this.style.backgroundColor=['#ccc','#fff'][i%2];
    })//实现表格的隔行换色效果
    
    $("p").click(function(){
        console.log($(this).html())
    }) //为每个p元素增加了click事件，单击某个p元素则输出其内容

 
# 6、扩展我们需要的功能

    $.extend({
        min: function(a, b){return a < b?a:b; },
        max: function(a, b){return a > b?a:b; }
    }); //为jquery扩展了min,max两个方法
    
    //使用扩展的方法（通过“$.方法名”调用）：
    console.log("a=10,b=20,max="+$.max(10,20)+",min="+$.min(10,20));


# 7、支持方法的连写

所谓连写，即可以对一个jquery对象连续调用各种不同的方法。例如：

    $("p").each(function(i){
            this.style.color=['#f00','#0f0','#00f'][ i ]
        })
        .click(function(){
            console.log($(this).html())
        });

# 8、操作元素的样式

主要包括以下几种方式：

- $("#msg").css("background"); //返回元素的背景颜色
- $("#msg").css("background","#ccc") //设定元素背景为灰色
- $("#msg").height(300); $("#msg").width("200"); //设定宽高
- $("#msg").css({ color: "red", background: "blue" });//以名值对的形式设定样式
- $("#msg").addClass("select"); //为元素增加名称为select的class
- $("#msg").removeClass("select"); //删除元素名称为select的class
- $("#msg").toggleClass("select"); //如果存在（不存在）就删除（添加）名称为select的class

# 9、完善的事件处理功能

jQuery已经为我们提供了各种事件处理方法，我们无需在html元素上直接写事件，而可以直接为通过jquery获取的对象添加事件。

如：

    $("#msg").click(function(){
        alert("good");
    }) //为元素添加了单击事件
    
    $("p").click(function(i){
        this.style.color=['#f00','#0f0','#00f'][i]};
    )//为三个不同的p元素单击事件分别设定不同的处理

jQuery中几个自定义的事件：

-（1）hover(fn1,fn2)：

一个模仿悬停事件（鼠标移动到一个对象上面及移出这个对象）的方法。当鼠标移动到一个匹配的元素上面时，会触发指定的第一个函数。当鼠标移出这个元素时，会触发指定的第二个函数。

    //当鼠标放在表格的某行上时将class置为over，离开时置为out。
    $("tr").hover(function(){
       $(this).addClass("over");
    },
    function(){
       $(this).addClass("out");
    });
    
-（2）ready(fn)

当DOM载入就绪可以查询及操纵时绑定一个要执行的函数。

    $(document).ready(function(){
        console.log("Load Success");
    })//页面加载完毕提示“Load Success”,相当于onload事件。与$(fn)等价;

- 3）toggle(evenFn,oddFn): 

每次点击时切换要调用的函数。如果点击了一个匹配的元素，则触发指定的第一个函数，当再次点击同一元素时，则触发指定的第二个函数。随后的每次点击都重复对这两个函数的轮番调用。

    //每次点击时轮换添加和删除名为selected的class。
    $("p").toggle(function(){
       $(this).addClass("selected");
    },function(){
       $(this).removeClass("selected");
    });

- （4）trigger(eventtype):
 
在每一个匹配的元素上触发某类事件。

例如：
    
    $("p").trigger("click"); //触发所有p元素的click事件
    
- （5）bind(eventtype,fn)，unbind(eventtype): 

事件的绑定与反绑定,从每一个匹配的元素中（添加）删除绑定的事件。例如：

    $("p").bind("click", function(){
        console.log($(this).text());
    }); //为每个p元素添加单击事件
    $("p").unbind(); //删除所有p元素上的所有事件
    $("p").unbind("click") //删除所有p元素上的单击事件


# 10、几个实用特效功能

其中toggle()和slidetoggle()方法提供了状态切换功能。如toggle()方法包括了hide()和show()方法。

slideToggle()方法包括了slideDown()和slideUp方法。

# 11、几个有用的jQuery方法

##### $.each
 
$.each(obj, fn)：通用的迭代函数。可用于近似地迭代对象和数组（代替循环）。如
    
    $.each([0,1,2],function(i, n){
        console.log( "Item #" + i + ": " + n ); 
    });
    
等价于：

    var tempArr=[0,1,2];
    for(var i=0;i<tempArr.length;i++){
       console.log("Item #"+i+": "+tempArr[ i ]);
    }
    
也可以处理json数据，如

    $.each( { name: "John", lang: "JS" }, function(i, n){ 
        console.log( "Name: " + i + ", Value: " + n ); 
    });
    
结果为：

    Name:name, Value:John
    Name:lang, Value:JS

##### $.extend(target,prop1,propN):

用一个或多个其他对象来扩展一个对象，返回这个被扩展的对象。

这是jquery实现的继承方式如：

    $.extend(settings, options);//合并settings和options，并将合并结果返回settings中，相当于options继承setting并将继承结果保存在 setting中。
    var settings = $.extend({}, defaults, options);//合并defaults和options，并将合并结果返回到setting中而不覆盖default内容。

可以有多个参数（合并多项并返回）

##### $.map(array, fn)：

数组映射。把一个数组中的项目(处理转换后)保存到到另一个新数组中，并返回生成的新数组。如：

    var tempArr=$.map( [0,1,2], function(i){ return i + 4; });
    //tempArr内容为：[4,5,6]
    
    var tempArr=$.map( [0,1,2], function(i){ return i > 0 ? i + 1 : null; });
    //tempArr内容为：[2,3]

##### $.merge(arr1,arr2):合并两个数组并删除其中重复的项目。

    $.merge( [0,1,2], [2,3,4] ) //返回[0,1,2,3,4]

##### $.trim(str)：删除字符串两端的空白字符。

    $.trim(" hello, how are you? "); //返回"hello,how are you? "

# 12、解决自定义方法或其他类库与jQuery的冲突

很多时候我们自己定义了$(id)方法来获取一个元素，或者其他的一些js类库如prototype也都定义了$方法，如果同时把这些内容放在一起就会引起变量方法定义冲突，Jquery对此专门提供了方法用于解决此问题；

使用jquery中的jQuery.noConflict();方法即可把变量$的控制权让渡给第一个实现它的那个库或之前自定义的$方法。之后应用Jquery的时候只要将所有的$换成jQuery即可，如原来引用对象方法$("#msg")改为jQuery("#msg")。如：

    jQuery.noConflict();// 开始使用jQuery
    jQuery("div p").hide();// 使用其他库的 $()
    $("content").style.display = 'none';

### 如何禁用右键单击上下文菜单
    $(document).bind('contextmenu', function (e) {
        return false;
    });

### 把某个元素放在屏幕的最中间
    jQuery.fn.center = function () {
         this.css('position', 'absolute');
         this.css('top', ($(window).height() - this.height())/ +$(window).scrollTop() + 'px');
         this.css('left', ($(window).width() - this.width())/ 2 + $(window).scrollLeft() + 'px');
         return this;
    }
    //这样来使用上面的函数：
    $(element).center();

### 如何切换页面上的所有复选框
    var tog = false;
    // 或者为true，如果它们在加载时为被选中状态的话
    $('a').click(function () {
        $("input[type=checkbox]").attr("checked", !tog);
        tog = !tog;
    });

### 如何基于一些输入文本来过滤一个元素列表

    //如果元素的值和输入的文本相匹配的话
    //该元素将被返回
    $('.someClass').filter(function () {
        return $(this).attr('value') == $('input#someId').val();
        // 如果包含输入内容就显示的，可以用contaions这个方法来判断的
    })

### 如何把整个的列表元素（LI）变成可点击的


    <ul>
    <li><a href="#">Link 1</a></li>
    <li><a href="#">Link 2</a></li>
    <li><a href="#">Link 3</a></li>
    <li><a href="#">Link 4</a></li>
    </ul>

        $page.on('click','ul li',function (e) {
            e.preventDefault();
            window.location.href = $(this).find("a").attr("href");
        });

### 如何检查图像是否已经被完全加载进来
    $('#theImage').attr('src', 'image.jpg').load(function () {
        alert('This Image Has Been Loaded');
    });

### 检查cookie是否启用
    var dt = new Date();
    dt.setSeconds(dt.getSeconds() + 60);
    document.cookie = "cookietest=1; expires=" + dt.toGMTString();
    var cookiesEnabled = document.cookie.indexOf("cookietest=") != -1;
    if (!cookiesEnabled) {
        //没有启用cookie
    }
    //原理是，用过设置一个临时的cookie，然后判断这个cookie是否存在，如果不存在则说明，浏览器没有开启cookie