```html
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>
<script>
    hljs.initHighlightingOnLoad();
</script>
<style type="text/css">
		#category a{
			text-decoration: none; 
			color: black;
		}
		/* #category {line-height: 2em;} */
		.newh{
			padding-top: 0.5em;
			padding-bottom: 0.5em;
		}
		.newh:hover{
			background: #E6F0FE;
		}
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
						float: left;
						margin-right: 2em;
            /* right: 0px; */
            top: 0;
            /* 目录栏的高度,这里设置为60%主要是为了不挡住返回顶部和折叠按钮 */
            height: 93%;
            /* 设置边框这样和正文对比比较明显 */
            border: 0 solid #eaeaea;
            /* 开启垂直滚动条 */
            overflow-y: scroll;
						width:25%;
						display:block;
						/* z-index: 99;; */
						background: white;
						
        }

        /* 正文的样式 */
        #write {
            width:100%;
            display: block;
						/* float:right; */
        }
        img{
        	display: block;
        }
				
        code,tt{
            color:#c7254e;
            background-color:#f9f2f4;
        }
				
        /* 返回顶部按钮样式 */
        #topButton {
            position: fixed;
            float: right;
            right: 20px;
            top: 95%;
						z-index: 99;
        }

        /* 展开或者折叠 */
        #foldOrUnfold {
            position: fixed;
            float: right;
            right: 20px;
            top: 89%;
						z-index: 99;
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
		@media (max-width: 599px) {
			#category {
					/* 绝对定位 */
					position: fixed;
					/* 目录显示的位置 */
					float: left;
					/* right: 0px; */
					top: 0;
					width: 85%;
					/* 目录栏的高度,这里设置为60%主要是为了不挡住返回顶部和折叠按钮 */
					height: 90%;
					/* 设置边框这样和正文对比比较明显 */
					border: 2px solid #eaeaea;
					/* 开启垂直滚动条 */
					overflow-y: scroll;
					z-index: 99;
					display: none;
					background: white;
					font-size: 20px;
					/* line-height: 2em; */
			}
			
			/* 正文的样式 */
			#book_body {
					width: 100%;
					display: block;
			}
			img{
				display: block;
			}
			code,tt{
					color:#c7254e;
					background-color:#f9f2f4;
			}

			/* 返回顶部按钮样式 */
			#topButton {
					position: fixed;
					float: right;
					right: 2%;
					top: 95%;
					z-index:99;
			}

			/* 目录展开或者折叠 */
			#foldOrUnfold {
					position: fixed;
					float: left;
					right: 4%;
					top: 90%;
					z-index:99;
			}
	}
</style>
<!--侧栏目录生成代码-->
<script>
    $(document).ready(function () {
        $("h1,h2,h3,h4,h5,h6").each(function (i, item) {
            //获取标签的名字,h1,还是h2
            var tag = $(item).get(0).localName;
            //为该标签设置id属性
            $(item).attr("id", "wow" + i);
            //添加一个页内超链接,并设置class选择器
            $("#category").append('<div class="newh"><a class="new' + tag + '" href="#wow' + i + '">' + $(item).text() +
                '</a></div>');
            //为每一个标题超链接的class属性设置左边距
            $(".newh1").css("margin-left", "0em");
            $(".newh2").css("margin-left", "1em");
            $(".newh3").css("margin-left", "2em");
            $(".newh4").css("margin-left", "3em");
            $(".newh5").css("margin-left", "4em");
            $(".newh6").css("margin-left", "5em");
						$(".newh1").css("font-size", "1.5em");
						$(".newh2").css("font-size", "1.2em");
						$(".newh3").css("font-size", "1em");
						$(".newh4").css("font-size", "0.9em");
						$(".newh5").css("font-size", "0.8em");
						$(".newh1").css("font-weight", "bold");
						$(".newh2").css("font-weight", "bold");
        });
        //设置class选择器为.book-body的html内容
        $(".book-body").html($(".book-body").nextAll())
				$(".newh").on("click",function(){
					console.log($(this).children("a"));
					$(this).children("a")[0].click();
				});
    });
</script>
<script>
    // 展开或者折叠目录功能
    function showOrCloseCategory() {
        var id = document.getElementById("category");
        var write = document.getElementById("write");
				//console.log(id.style.display);
        //如果展开了
        if (id.style.display == 'block') {
            //console.log("开始展开");
            id.style.display = 'none';
						write.style.float = "";
						write.style.left = "0em";
        }
        //如果被折叠了
        else if (id.style.display == 'none') {
            //console.log("开始折叠");
            id.style.display = 'block';
						write.style.float = "right";
						write.style.left = "1em";
        }
    }
    // 返回顶部功能
    function topFunction() {
        document.body.scrollTop = 0;
        document.documentElement.scrollTop = 0;
    }
</script>
<!--返回顶部-->
<button onclick="topFunction()" id="topButton">TOP↑</button>
<button onclick="showOrCloseCategory()" id="foldOrUnfold">目录</button>

<!--文章主体部分-->
<div class="book-body" id="book_body"> </div>
<!--目录栏-->
<div class="book-summary" id="category" style="display:none"></div>
```

引入到`<head>`标签中