![轮播图的基本样式](http://upload-images.jianshu.io/upload_images/2768522-fe5b418e386a4e62.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# JavaScript写个轮播效果

这次也是一个适合**JavaScript初学者**的小练手，用JavaScript的基本知识去写一个轮播图，其实轮播图有很多方法去实现，像用一些框架，Bootstrap之类的，或者CSS3都可以轻松做出漂亮的轮播图，这次去用JavaScript实现，主要是为了锻炼自己使用Js的能力，代码非常简单，我会先放出HTML和CSS部分，最后详细讲解Js部分，还是那句话，重要的是思路，希望Js的初学者可以跟着我动手敲一敲，绝对对自己的能力有提升！

<!-- more -->

![效果演示：卡顿是因为GIF图片压缩了，损失了帧数](http://upload-images.jianshu.io/upload_images/2768522-176df9b3c1bef8f9.gif?imageMogr2/auto-orient/strip)


#### HTML代码部分
```JavaScript
 <div class="main_div">
   <div class="arrows">
         <span title="1" class="arrow"><</span>
         <span title="0" class="arrow" style="float: right">></span>
   </div>

   <ul class="ul_img">
         <li class="li_img"><img src="images/摄图网-水珠在竹叶上.jpg"/></li>
         <li class="li_img"><img src="images/摄图网-在海边的人.jpg"/></li>
         <li class="li_img"><img src="images/摄图网-清凉的荷叶.jpg"/></li>
         <li class="li_img"><img src="images/摄图网-绵延不绝的山岭.jpg"/></li>
   </ul>
</div>

<div style="margin-left: 600px">
         <div class="div_btn"></div>
         <div class="div_btn"></div>
         <div class="div_btn"></div>
         <div class="div_btn"></div>
</div>
```
整个HTML非常简单，分三部分，第一部分就是四张图片，第二部分就是左右方向键，第三部分就是底部的四个圆角矩形，没什么说的，为了简化代码，都是最简单的。

#### CSS代码部分

```JavaScript
<style>
        img {
            width: 100%;
        }

        .li_img {
            width: 800px;
            float: left;
            list-style: none;
        }

        .ul_img {
            width: 6000px;
            padding: 0px;
            margin: 0px;
            transition: all 2s;
        }

        .main_div {
            width: 800px;
            overflow: hidden;
            position: relative;
            top: 100px;
            left: 350px;
        }

        .arrows {
            z-index: 9999;
            position: absolute;
            padding-top: 230px;
            width: 800px;
        }

        .arrows span {
            font-size: 3em;
            color: seashell;
        }

        .arrows span:hover {
            /*变小手*/
            cursor: pointer;
            background-color: rgba(192, 192, 192, 0.29);
        }

        .div_btn {
            float: left;
            border-radius: 100px;
            background-color: aquamarine;
            width: 60px;
            height: 10px;
            margin-left: 10px;
            margin-top: 130px;
        }

        .div_btn:hover {
            background-color: aqua;

        }
    </style>
```
CSS部分也是尽量简洁，没有设置太花哨的样式，随意设置了一下，各位可以自己发挥，主要为了写JavaScript……唯一要注意的就是我为.li_img设置了一个transition。
由于是用固定像素设置的宽高，所以不同浏览器可能会显示样式有所区别，不影响功能就是了，这里我使用调试的浏览器是Chrome和Firefox
#### JavaScript部分
**请先不要直接看这部分代码**，先看我的思路讲解再看这部分，你绝对可以轻松理解
```JavaScript
<script>
    //跑动的次数
    var count = 0;
    //动画的执行方向
    var isgo = false;
    //定义计时器对象
    var timer;

    window.onload = function () {
        /*获取ul元素*/
        var ul_img = document.getElementsByClassName("ul_img")[0];
        //获取所有的li图片元素
        var li_img = document.getElementsByClassName("li_img");
        //获取控制方向的箭头元素
        var arrow = document.getElementsByClassName("arrow");
        //获取所有按钮元素
        var div_btn = document.getElementsByClassName("div_btn");
        div_btn[0].style.backgroundColor = "aqua";


        /*定义计时器，控制图片移动*/
        showtime();
        function showtime() {
            timer = setInterval(function () {
                if (isgo == false) {
                    count++;
                    ul_img.style.transform = "translate(" + -800 * count + "px)";
                    if (count >= li_img.length - 1) {
                        count = li_img.length - 1;
                        isgo = true;
                    }
                }
                else {
                    count--;
                    ul_img.style.transform = "translate(" + -800 * count + "px)";
                    if (count <= 0) {
                        count = 0;
                        isgo = false;
                    }
                }

                for (var i = 0; i < div_btn.length; i++) {
                    div_btn[i].style.backgroundColor = "aquamarine";
                }

                div_btn[count].style.backgroundColor = "aqua";

            }, 4000)
        }

        /*鼠标进入左右方向键操作*/
        for (var i = 0; i < arrow.length; i++) {
            //鼠标悬停时
            arrow[i].onmouseover = function () {
                //停止计时器
                clearInterval(timer);
            }
            //鼠标离开时
            arrow[i].onmouseout = function () {
                //添加计时器
                showtime();
            }
            arrow[i].onclick = function () {
                //区分左右
                if (this.title == 0) {
                    count++;
                    if (count > 3) {
                        count = 0;
                    }
                }
                else {
                    count--;
                    if (count < 0) {
                        count = 3;
                    }
                }

                ul_img.style.transform = "translate(" + -800 * count + "px)";

                for (var i = 0; i < div_btn.length; i++) {
                    div_btn[i].style.backgroundColor = "aquamarine";
                }
                div_btn[count].style.backgroundColor = "aqua";
            }
        }

        //鼠标悬停在底部按钮的操作
        for (var b = 0; b < div_btn.length; b++) {
            div_btn[b].index = b;
            div_btn[b].onmouseover = function () {

                clearInterval(timer);

                for (var a = 0; a < div_btn.length; a++) {
                    div_btn[a].style.backgroundColor = "aquamarine";
                }
                div_btn[this.index].style.backgroundColor = "aqua";
                //让count值对应
                //为了控制方向
                if (this.index == 3) {
                    isgo = true;
                }
                if (this.index == 0) {
                    isgo = false;
                }
                count = this.index;
                ul_img.style.transform = "translate(" + -800 * this.index + "px)";
            }
            div_btn[b].onmouseout = function () {
                //添加计时器
                showtime();
            }
        }
    }
</script>
```

### 思路详解
首先，在思考这个轮播图怎么去实现的时候，请先考虑要为这个轮播图设置什么样的功能，我设定的有三个功能：
- ##### 图片可以自动右向轮播，轮播至最后一张图片的时候，反向向左轮播，循环反复
- ##### 可以用左右方向键去控制图片轮播方向
- ##### 可以利用下方的圆角矩形来选择浏览某一张图片

在明确了功能之后，接下来依次解决不就行了，好，我们看第一个问题，怎么实现**图片可以自动右向轮播，轮播至最后一张图片的时候，反向向左轮播，循环反复**呢？没错，使用定时器setTimeout()或者setInterval()可以轻松解决，在这里我就使用setInterval()，如果有不太了解的，请点这里[*如何使用setInterval()*](http://www.w3cschool.cn/javascript/js-timing.html)。好了，接下是第一部分功能的代码思路：

- #### 第一个功能

要想让图片自动轮播，我们首先去设定一个函数showtime()，当然写完了这个函数，我们的第一个功能也就完成了，好的，那我就开始写了……
![](http://upload-images.jianshu.io/upload_images/2768522-da54f56c6270c100.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
好吧，当然不能直接写了，首先你得思考，既然图片一开始向右轮播，你总得先设定一个方向吧，然后再考虑，还得设定一个跑动的次数，比如初始位置为第一张图片，我要向右跑动3次，就可以到达第四张图片，然后还要设定一个定时器对象，用处以后会说到，这几个必须是全局变量，所以必须在一开始就声明：
```JavaScript
    //跑动的次数
    var count = 0;
    //动画的执行方向
    var isgo = false;
    //定义计时器对象
    var timer;
```
然后可以开写了，当然要先获取图片元素：
```JavaScript
    /*获取ul元素*/
    var ul_img = document.getElementsByClassName("ul_img")[0];
    //获取所有的li图片元素
    var li_img = document.getElementsByClassName("li_img");
```
好的，准备工作到此结束，图片轮播的原理就是图片排成一行，然后准备一个只有一张图片大小的容器，对这个容器设置超出部分隐藏，在控制定时器来让这些图片整体左移或右移，这样呈现出来的效果就是图片在轮播了，我们这里先function一个showtime()函数，并在里边添加一个定时器来为控制图片轮播做准备：
```JavaScript
  function showtime() {
         timer = setInterval(function (){ }, 4000);
      }
```
在上面，我定义了每次延迟4000毫秒（即4秒）来执行一次setInterval()里的匿名函数function(){ }，是为了尽可能的让轮播效果不至于太快。然后我在匿名函数function(){ }加入以下三行代码：
```JavaScript
  function showtime() {
         timer = setInterval(function (){
            if (isgo == false) {
               count++;
               ul_img.style.transform = "translate(" + -800 * count + "px)";
         }
     }, 4000);
 }
```
记得之前我们声明了isgo全局变量,并为它赋值为布尔值false吗？这里if判断语句会直接成立，if中的语句会让count加一，并为ul_img设置了style样式，让ul_img整体（即四张图片整体的ul）向左移动800px，（因为在CSS中为图片设置了width为800px），所以以上语句会控制图片集体向左推入800px的距离，让第二张图片进入到显示容器中，、（在此之前第二三四张图片都是隐藏的，因为我设置了超出部分隐藏），所以此时轮播图的状态是第二张图片显示，第一三四张图片隐藏，然后每隔4秒就会重复上述过程，然后这样就实现了轮播……吗？
![](http://upload-images.jianshu.io/upload_images/2768522-12bce848d17652a9.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
很快你就会发现问题，那就是当向左移动了三次（即count = 3时），显示的是第四张图片，这没问题对吧，但是再过4秒，可就不妙了，你会发现图片再往左移（即 count = 4时），妈蛋！没图了！显示的是空白，怎么解决呢？hin简单！加一个if判断语句不就行了！
```JavaScript
  function showtime() {
         timer = setInterval(function (){
            if (isgo == false) {
               count++;
               ul_img.style.transform = "translate(" + -800 * count + "px)";
                 if (count >= li_img.length - 1) {
                        count = li_img.length - 1;
                        isgo = true;
              }
         }
     }, 4000);
 }
```
以上代码，我加了一个判断语句，在count大于等于li.img.length-1（即count >= 3）时，禁止count再自增，同时改变isgo的值，让轮播图开始反向滚动，所以就会在增加一个else来描述isgo=true的情况：
```JavaScript
  function showtime() {
         timer = setInterval(function (){
        if (isgo == false) {
               count++;
               ul_img.style.transform = "translate(" + -800 * count + "px)";
                 if (count >= li_img.length - 1) {
                        count = li_img.length - 1;
                        isgo = true;
              }
         else {
                    count--;
                    ul_img.style.transform = "translate(" + -800 * count + "px)";
                    if (count <= 0) {
                        count = 0;
                        isgo = false;
                    }
              }
     }, 4000);
}
  showtime()；
```
else的情况就是控制图片反向轮播，所以以上代码很好理解，就是count--，并且在count <= 0时，为isgo重新设置false，让图片再正向轮播，循环往复，最后再调用showtime()函数，第一个功能到此为止就完全实现了！

- #### 第二个功能

第二个功能我们要添加鼠标进入左右两个方向键的操作，首先获取左右两个方向键。
```JavaScript
     //获取控制方向的箭头元素
     var arrow = document.getElementsByClassName("arrow");
```
好了，由于方向键有两个，所以我们要来用for循环来为它们绑定事件：
```JavaScript
 for (var i = 0; i < arrow.length; i++) {
      //鼠标悬停时
      arrow[i].onmouseover = function () {
      //停止计时器
      clearInterval(timer);
     }
      //鼠标离开时
      arrow[i].onmouseout = function () {
      //添加计时器
      showtime();
    }
}
```
以上四条语句为方向键绑定了两个事件，鼠标悬停时，我们利用clearInteral()来终止定时器，这就是前面我们把timer要声明为全局变量的原因，便于我们可以在想停止定时器的时候停止它。接下来我们为方向键添加onclick事件，以便我们可以通过其控制方向：
```JavaScript
 for (var i = 0; i < arrow.length; i++) {
      //鼠标悬停时
      arrow[i].onmouseover = function () {
      //停止计时器
      clearInterval(timer);
      }
      //鼠标离开时
      arrow[i].onmouseout = function () {
      //添加计时器
      showtime();
     }
     arrow[i].onclick = function () {
     //区分左右
     if (this.title == 0) {
       count++;
      if (count > 3) {
       count = 0;
     }
    }
      else {
       count--;
      if (count < 0) {
       count = 3;
     }
    }
   ul_img.style.transform = "translate(" + -800 * count + "px)";
 }
}
```
不知道大家注意到了没有，我在之前HTML中为左右方向键分别设置了title值：0和1；所以这里直接用title值来区分左右，为右方向键我们执行count++；为左方向键我们执行count--；同时也考虑到count>3和count<0的情况，这些之前提到过，这里不再赘述，到此为止，第二部分的功能也实现了，很简单吧

- #### 第三个功能

鼠标悬停在底部圆角矩形的操作，同样的道理，首先获取四个圆角矩形，然后用for循环为它们绑定事件：
```JavaScript
     var div_btn = document.getElementsByClassName("div_btn");
     div_btn[0].style.backgroundColor = "aqua";
     //鼠标悬停在底部按钮的操作
     for (var b = 0; b < div_btn.length; b++) {
            div_btn[b].index = b;
            div_btn[b].onmouseover = function () {}
            div_btn[b].onmouseout = function () {}
}
```
有的同学可能不太懂<code>div_btn[b].index = b;</code>这条语句干什么的用的，关于这个问题，涉及到循环绑定的一个坑，请大家看这篇文章[[关于在for循环中绑定事件打印变量i是最后一次](http://www.cnblogs.com/pssp/p/5215417.html)](http://www.cnblogs.com/pssp/p/5215417.html)

好，接下来我们先写鼠标悬停事件：
```JavaScript
   div_btn[b].onmouseover = function () {
      clearInterval(timer);

      for (var a = 0; a < div_btn.length; a++) {
          div_btn[a].style.backgroundColor = "aquamarine";
       }
          div_btn[this.index].style.backgroundColor = "aqua";
      //为了控制方向
      if (this.index == 3) {
         isgo = true;
       }
      if (this.index == 0) {
         isgo = false;
       }
      //让count值对应
      count = this.index;
      ul_img.style.transform = "translate(" + -800 * this.index + "px)";
    }
}
```
有了之前的基础，我在讲鼠标悬停事件的时候，各位应该就更容易理解了，我首先在鼠标悬停时用<code> clearInterval(timer);</code>来停止定时器，然后为每个圆角矩形都加上颜色，并且为悬停的圆角矩形变色，之后考虑到了大于三和小于零的情况，再之后，我把当前的index属性值赋给count，让用户可以通过悬停底部圆角矩形来选择看第几张图片。

最后再加上鼠标离开事件：
```JavaScript
 div_btn[b].onmouseout = function () {
      //添加计时器
      showtime();
    }
}
```

大功告成！！！至此所有功能写完。其实最后还有一步就是：
```JavaScript
 for (var a = 0; a < div_btn.length; a++) {
          div_btn[a].style.backgroundColor = "aquamarine";
       }
          div_btn[count].style.backgroundColor = "aqua";
}
```
将以上代码，添加到功能一和功能二的代码里，目的是，让图片自动轮播和控制左右方向键时，底部圆角矩形也能随之变色。

最后再放一遍完整的JS代码：
```JavaScript
<script>
    var count = 0;
    var isgo = false;
    var timer;

    window.onload = function () {
        var ul_img = document.getElementsByClassName("ul_img")[0];
        var li_img = document.getElementsByClassName("li_img");
        var arrow = document.getElementsByClassName("arrow");
        var div_btn = document.getElementsByClassName("div_btn");
        div_btn[0].style.backgroundColor = "aqua";


        showtime();
        function showtime() {
            timer = setInterval(function () {
                if (isgo == false) {
                    count++;
                    ul_img.style.transform = "translate(" + -800 * count + "px)";
                    if (count >= li_img.length - 1) {
                        count = li_img.length - 1;
                        isgo = true;
                    }
                }
                else {
                    count--;
                    ul_img.style.transform = "translate(" + -800 * count + "px)";
                    if (count <= 0) {
                        count = 0;
                        isgo = false;
                    }
                }
                for (var i = 0; i < div_btn.length; i++) {
                    div_btn[i].style.backgroundColor = "aquamarine";
                }
                div_btn[count].style.backgroundColor = "aqua";

            }, 4000)
        }


        for (var i = 0; i < arrow.length; i++) {
            arrow[i].onmouseover = function () {
                clearInterval(timer);
            }
            arrow[i].onmouseout = function () {
                showtime();
            }
            arrow[i].onclick = function () {
                if (this.title == 0) {
                    count++;
                    if (count > 3) {
                        count = 0;
                    }
                }
                else {
                    count--;
                    if (count < 0) {
                        count = 3;
                    }
                }

                ul_img.style.transform = "translate(" + -800 * count + "px)";

                for (var i = 0; i < div_btn.length; i++) {
                    div_btn[i].style.backgroundColor = "aquamarine";
                }
                div_btn[count].style.backgroundColor = "aqua";
            }
        }
        for (var b = 0; b < div_btn.length; b++) {
            div_btn[b].index = b;
            div_btn[b].onmouseover = function () {

                clearInterval(timer);

                for (var a = 0; a < div_btn.length; a++) {
                    div_btn[a].style.backgroundColor = "aquamarine";
                }
                div_btn[this.index].style.backgroundColor = "aqua";
                if (this.index == 3) {
                    isgo = true;
                }
                if (this.index == 0) {
                    isgo = false;
                }
                count = this.index;
                ul_img.style.transform = "translate(" + -800 * this.index + "px)";
            }
            div_btn[b].onmouseout = function () {
                showtime();
            }
        }
    }
</script>
```
