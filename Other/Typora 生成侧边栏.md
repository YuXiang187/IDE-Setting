# Typora 生成侧边栏

## 准备工作：转HTML

打开typora文件 → 导出 → HTML

用编辑器(记事本,EditPlus,GVim,VsCode,Hbuilder),我用的Hbuilder,把如下代码粘贴到\</body>下方

```html
<body>
....
</body>
***粘贴到此处***
```

## 带折叠功能

```html
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>
<script>
    hljs.initHighlightingOnLoad();
</script>

<!--侧栏目录生成代码-->
<script>
    $(document).ready(function () {
        $("h1,h2,h3,h4,h5,h6").each(function (i, item) {
            //获取标签的名字,h1,还是h2
            var tag = $(item).get(0).localName;
            //为该标签设置id属性
            $(item).attr("id", "wow" + i);
            //添加一个页内超链接,并设置class选择器
            $("#category").append('<a class="new' + tag + '" href="#wow' + i + '">' + $(item).text() +
                '</a></br>');
            //为每一个标题超链接的class属性设置左边距
            $(".newh1").css("margin-left", 0);
            $(".newh2").css("margin-left", 20);
            $(".newh3").css("margin-left", 40);
            $(".newh4").css("margin-left", 60);
            $(".newh5").css("margin-left", 80);
            $(".newh6").css("margin-left", 100);
        });
        //设置class选择器为.book-body的html内容
        $(".book-body").html($(".book-body").nextAll())
    });
</script>
<style type="text/css">
    @media (max-width: 1600px) {
        .book-body {
            /* padding-left: 200px; */
            padding-right: 0px;
        }
    }

    @media (max-width: 1400px) {
        .book-body {
            /* padding-left: 200px; */
            padding-right: 0px;
        }
    }

    @media (max-width: 1200px) {
        .book-body {
            /* padding-left: 300px; */
            padding-left: 0px;
        }
    }

    @media (max-width: 700px) {
        .book-body {
            padding-left: 0px;
        }
    }

    @media (min-width: 600px) {
        #category {
            /* 绝对定位 */
            position: fixed;
            /* 目录显示的位置 */
            right: 0px;
            top: 0;
            /* 目录栏的高度,这里设置为60%主要是为了不挡住返回顶部和折叠按钮 */
            height: 93%;
            /* 设置边框这样和正文对比比较明显 */
            border: 2px solid #eaeaea;
            /* 开启垂直滚动条 */
            overflow-y: scroll;
        }

        /* 返回顶部按钮样式 */
        #topButton {
            position: fixed;
            float: right;
            right: 20px;
            top: 95%;
        }

        /* 展开或者折叠 */
        #foldOrUnfold {
            position: fixed;
            float: right;
            right: 100px;
            top: 95%
        }

        /* 正文的样式 */
        #book_body {
            width: 90%;
            display: block;
        }
        img{
        	display: block;
        }
        code,tt{
            color:#c7254e;
            background-color:#f9f2f4;
        }
    }

    @media (-webkit-max-device-pixel-ratio: 1) {
        ::-webkit-scrollbar-track-piece {
            background-color: #FFF
        }

        ::-webkit-scrollbar {
            width: 6px;
            height: 6px
        }

        ::-webkit-scrollbar-thumb {
            background-color: #c2c2c2;
            background-clip: padding-box;
            min-height: 28px
        }

        ::-webkit-scrollbar-thumb:hover {
            background-color: #A0A0A0
        }
    }
</style>
<script>
    // 展开或者折叠目录功能
    function showOrCloseCategory() {
        var id = document.getElementById("category");
        var book_body = document.getElementById("book_body");
        //如果展开了
        if (id.style.display == 'block') {
            //console.log("开始展开");
            id.style.display = 'none';
            id.style.width = "0%";
            book_body.style.width = "100%";
            book_body.style.paddingleft = 0;
        }
        //如果被折叠了
        else if (id.style.display == 'none') {
            //console.log("开始折叠");
            id.style.display = 'block';
            book_body.style.width = "90%";
            id.style.width = "20%"
        }
    }
    // 返回顶部功能
    function topFunction() {
        document.body.scrollTop = 0;
        document.documentElement.scrollTop = 0;
    }
</script>
<!--返回顶部-->
<button onclick="topFunction()" id="topButton">返回顶部</button>
<button onclick="showOrCloseCategory()" id="foldOrUnfold">折叠/展开</button>

<!--文章主体部分-->
<div class="book-body" id="book_body"> </div>
<!--目录栏，设置占用宽度为20%可以根据实际情况设置-->
<div class="book-summary" id="category" style="width:20%;display:block"></div>
```

## 主代码（仅普通功能）

```html
<!-- <script src="http://code.jquery.com/jquery-1.7.2.min.js"></script> -->
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
<!--侧栏目录生成代码-->
<script>
    //标题序号计数器
    var hCount = [0, 0, 0, 0, 0, 0];
    //设置计数器
    function setHCount(number) {
        //当前计数器加一
        hCount[number - 1]++;
        for (var i = number, length = hCount.length; i < length; i++) {
            //子目录计数器全部置零
            hCount[i] = 0;
        }
    }
    //重命名目录名称
    function setHTagValue(item, number) {
        //获取标题名
        var text = $(item).get(0).innerHTML;
        //初始化空字符串
        var before = "";
        //生成序号
        for (var i = 0, length = hCount.length; i < number; i++) {
            if (i < number - 1)
                before += hCount[i] + ".";
            else
                before += hCount[i] + " ";
        }
        //在标题前面加上序号
        $(item).get(0).innerHTML = before + text;
    }
    function renameHTag(item) {
        var tag = $(item).get(0).localName;
        if (tag === "h1") {
            setHCount(1);
           //console\.log("捕获到标签:%s", tag);
            setHTagValue(item, 1);
        }
        if (tag === "h2") {
            setHCount(2);
            //console.log("捕获到标签:%s", tag);
            setHTagValue(item, 2);
        }
        if (tag === "h3") {
            setHCount(3);
            //console.log("捕获到标签:%s", tag);
            setHTagValue(item, 3);
        }
        if (tag === "h4") {
            setHCount(4);
            //console.log("捕获到标签:%s", tag);
            setHTagValue(item, 4);
        }
        if (tag === "h5") {
            setHCount(5);
            //console.log("捕获到标签:%s", tag);
            setHTagValue(item, 5);
        }
        if (tag === "h6") {
            setHCount(6);
            //console.log("捕获到标签:%s", tag);
            setHTagValue(item, 6)
        }
    }
    $(document).ready(function () {
        $("h1,h2,h3,h4,h5,h6").each(function (i, item) {
            //给<H>类标签编号
            renameHTag(item);
            //获取标签的名字,h1,还是h2
            var tag = $(item).get(0).localName;
            //为该标签设置id属性
            $(item).attr("id", "wow" + i);
            //添加一个页内超链接,并设置class选择器
            //      $("#category").append('<a class="new'+tag+'" href="#wow'+i+'">'+$(this).text()+'</a></br>');
            $("#category").append('<a class="new' + tag + '" href="#wow' + i + '">' + $(item).text() + '</a></br>');
            //为每一个标题超链接的class属性设置左边距
            $(".newh1").css("margin-left", 0);
            $(".newh2").css("margin-left", 20);
            $(".newh3").css("margin-left", 40);
            $(".newh4").css("margin-left", 60);
            $(".newh5").css("margin-left", 80);
            $(".newh6").css("margin-left", 100);
        });
        //设置class选择器为.book-body的html内容
        $(".book-body").html($(".book-body").nextAll())
    });
</script>
<style type="text/css">
    @media (max-width: 1600px) {
        .book-body {
            /* padding-left: 200px; */
            padding-right: 0px;
        }
    }
    @media (max-width: 1400px) {
        .book-body {
            /* padding-left: 200px; */
            padding-right: 0px;
        }
    }
    @media (max-width: 1200px) {
        .book-body {
            /* padding-left: 300px; */
            padding-left: 0px;
        }
    }
    @media (max-width: 700px) {
        .book-body {
            padding-left: 0px;
        }
    }
    @media (min-width: 600px) {
        #category {
            /* 绝对定位 */
            position: fixed;
            /* left: 20px; */
            /* 目录显示的位置 */
            right: 0px;
            top: 0;
            /* 目录栏的高度,这里设置为60%主要是为了不挡住返回顶部和折叠按钮 */
            height: 79%;
            /* 开启垂直滚动条 */
            overflow-y: scroll;
            /* 开启水平滚动条 */
            overflow-x: scroll;
        }
    }
    @media (-webkit-max-device-pixel-ratio: 1) {
        ::-webkit-scrollbar-track-piece {
            background-color: #FFF
        }
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px
        }
        ::-webkit-scrollbar-thumb {
            background-color: #c2c2c2;
            background-clip: padding-box;
            min-height: 28px
        }
        ::-webkit-scrollbar-thumb:hover {
            background-color: #A0A0A0
        }
    }
</style>
<script>
    // id="category" οnclick="showOrCloseCategory()"
    function showOrCloseCategory() {
        var id = document.getElementById("category");
        var book_body=document.getElementById("book_body");
        //如果展开了
        if (id.style.display == 'block') {
            //console.log("开始展开");
            id.style.display='none';
            id.style.width="0%";
            book_body.style.width="100%";
            book_body.style.paddingleft=0;
        }
        //如果被折叠了
        else if (id.style.display =='none') {
            //console.log("开始折叠");
            id.style.display = 'block';
            book_body.style.width="90%";
            id.style.width="20%"
        }
    }
</script>
<!--返回顶部-->
<a href="javascript:scroll(0,0)" style="position:fixed;float:right;right:40px;top:80%">返回顶部</a>
<button onclick="showOrCloseCategory()" style="position:fixed;float:right;right:40px;top:85%">折叠/展开</button>
<!--文章主体部分-->
<div class="book-body" id="book_body" style="width:90%;display:block"> </div>
<!--目录栏，设置占用宽度为20%可以根据实际情况设置-->
<div  class="book-summary" id="category" style="width:20%;display:block" ></div>
```

## 侧边栏带序号

> 仅侧边栏带序号

```html
/* 正文标题区: #write */
/* [TOC]目录树区: .md-toc-content */
/* 侧边栏的目录大纲区: .sidebar-content */
/*仅侧边栏有序号*/

/** 
 * 说明：
 *     Typora的标题共有6级，从h1到h6。
 *     我个人觉得h1级的标题太大，所以我的标题都是从h2级开始。
 *     个人习惯每篇文章都有一个总标题，有一个目录，所以h2级的标题前两个都不会计数。
 *     一般情况下，我虽然不使用h1级的标题，但是为了以防万一，h1级的标题前两个也都不会计数。
 *     若想启用h1级标题，就取消包含“content: counter(h1) "."”项的注释，然后将包含“content: counter(h2) "."”的项注释掉即可。
 */ 
/** initialize css counter */
/*   .sidebar-content,*/
.sidebar-content{
	/* 设置全局计数器的基准 */
	/* 因为我喜欢从h2级标题用起，所以这里设置为h2,已修改为h1 */
    counter-reset: h1
}

 .outline-h1 {
    counter-reset: h2
}

 .outline-h2 {
    counter-reset: h3
}

 .outline-h3 {
    counter-reset: h4
}

 .outline-h4 {
    counter-reset: h5
}

 .outline-h5 {
    counter-reset: h6
}

/** put counter result into headings */

.outline-h1>.outline-item>.outline-label:before {
    counter-increment: h1;
    content: counter(h1) " "
}

/* 使用h1标题时，去掉前两个h1标题的序号，包括正文标题、目录树和大纲 */
/* nth-of-type中的数字表示获取第几个h1元素，请根据情况自行修改。 */


.outline-h1:nth-of-type(0)>.outline-item>.outline-label:before{
	counter-reset: h1;
	content: ""
}


h2.md-focus.md-heading:before, /** override the default style for focused headings */
.outline-h2>.outline-item>.outline-label:before {
        text-decoration: none;
    counter-increment: h2;
    content: counter(h1) "." counter(h2) " "
}

/* 使用h2标题时，去掉前两个h2标题的序号，包括正文标题、目录树和大纲 */
/* nth-of-type中的数字表示获取第几个h2元素，请根据情况自行修改。 */




h3.md-focus.md-heading:before, /** override the default style for focused headings */
.outline-h3>.outline-item>.outline-label:before {
	text-decoration: none;
    counter-increment: h3;
     content: counter(h1) "." counter(h2) "." counter(h3) " " 
    /*content: counter(h2) "." counter(h3) ". " */
}


h4.md-focus.md-heading:before,
.outline-h4>.outline-item>.outline-label:before {
	text-decoration: none;
    counter-increment: h4;
     content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) " " 
    /*content: counter(h2) "." counter(h3) "." counter(h4) ". " */
}


h5.md-focus.md-heading:before,
.outline-h5>.outline-item>.outline-label:before {
	text-decoration: none;
    counter-increment: h5;
     content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) " " 
    /*content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) ". " */
}


h6.md-focus.md-heading:before,
.outline-h6>.outline-item>.outline-label:before {
	text-decoration: none;
    counter-increment: h6;
     content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) " " 
    /*content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) ". " */
}
/*侧边栏文字颜色*/

.outline-h1,
.outline-h2,
.outline-h3,
.outline-h4
.outline-h5
{
  /*color: rgba(0, 160, 233, 0.5);*/
  color: rgba(0, 128, 0, 1);
/** override the default style for focused headings */

h3.md-focus:before,
h4.md-focus:before,
h5.md-focus:before,
h6.md-focus:before {
    color: inherit;
    border: inherit;
    border-radius: inherit;
    position: inherit;
    left:initial;
    float: none;
    top:initial;
    font-size: inherit;
    padding-left: inherit;
    padding-right: inherit;
    vertical-align: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```

## 侧边栏与正文、TOC都带序号

```html
/* 正文标题区: #write */
/* [TOC]目录树区: .md-toc-content */
/* 侧边栏的目录大纲区: .sidebar-content */

/** 
 * 说明：
 *     Typora的标题共有6级，从h1到h6。
 *     我个人觉得h1级的标题太大，所以我的标题都是从h2级开始。
 *     个人习惯每篇文章都有一个总标题，有一个目录，所以h2级的标题前两个都不会计数。
 *     一般情况下，我虽然不使用h1级的标题，但是为了以防万一，h1级的标题前两个也都不会计数。
 *     若想启用h1级标题，就取消包含“content: counter(h1) "."”项的注释，然后将包含“content: counter(h2) "."”的项注释掉即可。
 */ 
/** initialize css counter */
#write, .sidebar-content,.md-toc-content{
	/* 设置全局计数器的基准 */
	/* 因为我喜欢从h2级标题用起，所以这里设置为h2,已修改为h1 */
    counter-reset: h1
}

#write h1, .outline-h1, .md-toc-item.md-toc-h1 {
    counter-reset: h2
}

#write h2, .outline-h2, .md-toc-item.md-toc-h2 {
    counter-reset: h3
}

#write h3, .outline-h3, .md-toc-item.md-toc-h3 {
    counter-reset: h4
}

#write h4, .outline-h4, .md-toc-item.md-toc-h4 {
    counter-reset: h5
}

#write h5, .outline-h5, .md-toc-item.md-toc-h5 {
    counter-reset: h6
}

/** put counter result into headings */
#write h1:before,
.outline-h1>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h1>.md-toc-inner:before {
    counter-increment: h1;
    content: counter(h1) " "
}

/* 使用h1标题时，去掉前两个h1标题的序号，包括正文标题、目录树和大纲 */
/* nth-of-type中的数字表示获取第几个h1元素，请根据情况自行修改。 */

#write h1:nth-of-type(0):before,
.outline-h1:nth-of-type(0)>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h1:nth-of-type(0)>.md-toc-inner:before{
	counter-reset: h1;
	content: ""
}

#write h2:before,
h2.md-focus.md-heading:before, /** override the default style for focused headings */
.outline-h2>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h2>.md-toc-inner:before {
        text-decoration: none;
    counter-increment: h2;
    content: counter(h1) "." counter(h2) " "
}

/* 使用h2标题时，去掉前两个h2标题的序号，包括正文标题、目录树和大纲 */
/* nth-of-type中的数字表示获取第几个h2元素，请根据情况自行修改。 */



#write h3:before,
h3.md-focus.md-heading:before, /** override the default style for focused headings */
.outline-h3>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h3>.md-toc-inner:before {
	text-decoration: none;
    counter-increment: h3;
     content: counter(h1) "." counter(h2) "." counter(h3) " " 
    /*content: counter(h2) "." counter(h3) ". " */
}

#write h4:before,
h4.md-focus.md-heading:before,
.outline-h4>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h4>.md-toc-inner:before {
	text-decoration: none;
    counter-increment: h4;
     content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) " " 
    /*content: counter(h2) "." counter(h3) "." counter(h4) ". " */
}

#write h5:before,
h5.md-focus.md-heading:before,
.outline-h5>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h5>.md-toc-inner:before {
	text-decoration: none;
    counter-increment: h5;
     content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) " " 
    /*content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) ". " */
}

#write h6:before,
h6.md-focus.md-heading:before,
.outline-h6>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h6>.md-toc-inner:before {
	text-decoration: none;
    counter-increment: h6;
     content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) " " 
    /*content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) ". " */
}

/** override the default style for focused headings */
#write>h3.md-focus:before,
#write>h4.md-focus:before,
#write>h5.md-focus:before,
#write>h6.md-focus:before,
h3.md-focus:before,
h4.md-focus:before,
h5.md-focus:before,
h6.md-focus:before {
    color: inherit;
    border: inherit;
    border-radius: inherit;
    position: inherit;
    left:initial;
    float: none;
    top:initial;
    font-size: inherit;
    padding-left: inherit;
    padding-right: inherit;
    vertical-align: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```

## 终极vlook插件

[Github链接](https://github.com/MadMaxChow/VLOOK/releases)

[Gitee链接](https://gitee.com/maxchow/VLOOK)
