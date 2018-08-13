---
title: 猿圈团队面试题(一)
date: 2017-04-27 09:54
categories: '面试题'
type: ['面试题']
---
最近面试发现有个猿圈团队,出个测评,貌似需要写完面试题才能进入到下一个阶段,方便一下自己把自己做过的面试题整理一下.
## 一.js电子表
```css
        body {
            font-size: 100px;
            font-weight:bold;
            text-align:center;
        }
```
```html
<span id="hh"></span>:<span id="mm"></span>:<span id="ss"></span>
```
```javascript
        window.onload=function () {
            function $(selector) { return document.querySelector(selector) }

            var hh = hh || $('#hh'),
                mm = mm || $('#mm'),
                ss = ss || $('#ss');
//No.1
//开始写代码
            function update(){

                var time = new Date();
                var hour = time.getHours();
                var minute =time.getMinutes();
                var second = time.getSeconds();

                if(hour < 10){
                    hour = '0' + hour;
                }
                if (minute<10){
                    minute = '0'+minute;
                }
                if (second<10){
                    second = '0' + second;
                }
                hh.innerText = hour;
                mm.innerText = minute;
                ss.innerText = second;
                setTimeout(update,1000)
            }

            update();

//end_code
        }
```
## 二.响应式设计表单
将页面中的表单如果在移动设备上显示时，则将整个div的宽度设置为页面的100%；
 另当用户点击检查格式时，判断用户输入的内容格式是否正确以及是否为空，
 在出现错误时，将输入框颜色变红，并在输入框下方提示相关信息
```css
        body {
            padding: 0;
            margin: 0;
            background: #c0c0c0;
            text-align:center;
        }
        .formDiv {
            display: inline-block;
            border: 1px solid #ccc;
            background: #fff;
            border-radius: 10px;
            text-align: center;
            padding: 20px;
            width: 360px;
        }
        .user {
            display: inline-block;
            width: 100%;
            height: 26px;
            padding: 3px 0;
            font-size: 14px;
            color: #555;
            background-color: #fff;
            border: 1px solid #ccc;
            border-radius: 2px;
            -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075);
            box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075);
            -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
            transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
        }

        .checkBtn {
            background-color: #337ab7;
            padding: 5px 10px;
            border: 1px solid #337ab7;
            border-radius: 2px;
            margin-top: 10px;
            color: #fff;
        }
        .checkBtn:hover {
            background-color: #2a6496;
            border: 1px solid #2a6496;
        }
        .hint {
            display: none;
            color: red;
            font-size: 12px;
            padding: 6px;
        }
        /* No.1 */
        /* 开始写代码，请实现页面中的表单如果在移动设备上显示时，则将整个div的宽度设置为页面的100% */
        @media screen and (min-width: 400px) and (max-width: 700px) {
            .formDiv{
                width: 100%;
            }
        }
        .user-err{
            outline: red 1px solid;
        }
        .user-suc{
            outline: green 1px solid;
        }
        
        /*end_code*/
```
```html
<body>
<!--No.1-->
<!--开始写代码-->
<div class="formDiv">
    <input type="text" class="user" id="user" placeholder="请输入邮箱地址或手机号">
    <span class="hint"  > </span>
    <button class="checkBtn" id="checkBtn" >检查格式</button>
</div>

<!--end_code-->
</body>
```
```javascript
    window.onload=function () {
        function validate(str) {
            //No.1
            //开始写代码
            var PhoneReg = /^1\d{10}$/;
            var EmailReg = /^(\w-*\.*)+@(\w-?)+(\.\w{2,})+$/;
            if (PhoneReg.test(str) || EmailReg.test(str)) {
                console.log('通过')
                return true;
            }
            return false
            //end_code
        }

        var btn = document.getElementById('checkBtn');
        var userInput=document.getElementById('user');
        var hint = document.getElementsByClassName('hint')[0];
        btn.onclick = function check() {
            var user = document.getElementById("user").value.trim();
            //No.2
            //开始写代码
            console.log(user);
            if (user !== '') {
                if (validate(user)) {
                    userInput.className='user user-suc';
                    hint.style.display='none';
                    alert('通过')
                } else {
                    userInput.className='user user-err';
                    hint.style.display='block';
                    hint.innerText='用户名格式不正确'
                }
            } else {
                userInput.className='user user-err';
                hint.style.display='block';
                hint.innerText='用户名不能为空'
            }
            //end_code
        }
    }
```
## 三.变换背景动画
从不同方向使鼠标指针移过下面的内容，<br>
如从左侧移入时，由左侧匀速移入新的内容，新内容背景颜色变为黄色，<br>
如从右侧移入时，由右侧匀速移入新的内容，新内容背景颜色变为蓝色；<br>
上方移入和下方移入同理，上方颜色为红色，下方颜色为绿色<br>
```css
        .block {
            position: relative;

            display: inline-block;
            overflow: hidden;
            width: 10em;
            height: 10em;

            vertical-align: middle;

            -webkit-transform: translateZ(0);
        }


        .block_hoverer {
            position: absolute;
            z-index: 1;

            width: 71%;
            height: 71%;

            -webkit-transform: rotate(45deg);
            -moz-transform: rotate(45deg);
            -o-transform: rotate(45deg);
            transform: rotate(45deg);
        }

        /*No.1*/
        /*开始写代码，请定位hover后将显示的模块位置，请兼容Firefox、Safari、Chrome、Opera浏览器*/
        /*左*/
        .block_hoverer:nth-child(1) {
            /*border:1px solid red;*/
            top: 23px;
            left: -58px;
        }
        /*上*/
        .block_hoverer:nth-child(2) {
            /*border:1px solid green;*/
            top: -58px;
            left: 24px;
        }
        /*右*/
        .block_hoverer:nth-child(3) {
            /*border:1px solid blue;*/
            top: 23px;
            right: -58px;
        }
        /*下*/
        .block_hoverer:nth-child(4) {
            /*border:1px solid black;*/
            bottom: -62px;
            left: 21px;
        }
        .block_hoverer:hover {

        }
        /*end_code*/

        .block_content {
            position: absolute;
            top: 0;
            left: 0;

            width: 100%;
            height: 100%;

            text-align: center;
            line-height: 10em;

            background: #333;
            color: #FFF;

            -webkit-font-smoothing: subpixel-antialiased;

            -webkit-transition: all .3s ease;
            -moz-transition: all .3s ease;
            -o-transition: all .3s ease;
            transition: all .3s ease;

            -webkit-box-shadow: 0 -10em 0 0 red, 10em 0 0 0 blue, 0 10em 0 0 lime, -10em 0 0 0 orange;
            -moz-box-shadow: 0 -10em 0 0 red, 10em 0 0 0 blue, 0 10em 0 0 lime, -10em 0 0 0 orange;
            box-shadow: 0 -10em 0 0 red, 10em 0 0 0 blue, 0 10em 0 0 lime, -10em 0 0 0 orange;
        }


        /*从不同方向使鼠标指针移过下面的内容，
        如从左侧移入时，由左侧匀速移入新的内容，新内容背景颜色变为黄色，
        如从右侧移入时，由右侧匀速移入新的内容，新内容背景颜色变为蓝色；
        上方移入和下方移入同理，上方颜色为红色，下方颜色为绿色*/
        /*No.2*/
        /*开始写代码，请实现hover后的动画效果，请兼容Firefox、Safari、Chrome、Opera浏览器*/
        .block_hoverer:nth-child(1):hover ~ .block_content  {
            transform: translate(100%,0);
            -webkit-transform: translate(100%,0);
            -moz-transform: translate(100%,0) ;
            -o-transform:  translate(100%,0);
        }
        .block_hoverer:nth-child(2):hover ~ .block_content  {
            transform: translate(0,100%);
            -webkit-transform: translate(0,100%);
            -moz-transform: translate(0,100%);
            -o-transform:  translate(0,100%);
        }
        .block_hoverer:nth-child(3):hover ~ .block_content  {
            transform: translate(-100%,0);
            -webkit-transform: translate(-100%,0);
            -moz-transform: translate(-100%,0) ;
            -o-transform:  translate(-100%,0);
        }
        .block_hoverer:nth-child(4):hover ~ .block_content  {
            transform: translate(0,-100%);
            -webkit-transform: translate(0,-100%);
            -moz-transform: translate(0,-100%);
            -o-transform:  translate(0,-100%);
        }

        /*end_code*/

        body {
            padding: 2em;
            font: 16px/1.5 "Helvetica Neue", Arial, sans-serif;
            text-align: center;
        }
```
```html

<p>从不同方向使鼠标指针移过下面的内容</p>
<p>↓</p>
<span>→ </span>
<div class="block">
    <!--No.1-->
    <!--开始写代码-->
    <div class="block_hoverer"></div>
    <div class="block_hoverer"></div>
    <div class="block_hoverer"></div>
    <div class="block_hoverer"></div>
    
    <!--end_code-->
    <div class="block_content">
        Hover me!
    </div>
</div>
<span> ←</span>
<p>↑</p>
```
## 延伸,通过js判断鼠标进入的方向
```javascript
        var block=document.getElementsByClassName('block')[0];
        block.onmouseenter=function (e) {
            var w = this.offsetWidth;
            var h = this.offsetHeight;
            var x = e.pageX - this.getBoundingClientRect().left - w/2;
            var y = e.pageY - this.getBoundingClientRect().top - h/2;
            var direction = Math.round((((Math.atan2(y, x) * 180 / Math.PI) + 180) / 90) + 3) % 4; //direction的值为“0,1,2,3”分别对应着“上，右，下，左”
            var eventType = e.type;
            // 0 上 1 右 2 下 3左边
            console.log(direction,eventType);
            if(direction === 0){
                //上
            }else if (direction === 1){
                //右
            } else if (direction === 2){
                //下
            }else if (direction === 3){
                //左
            }
        }
```
<p align=right>---by  lori.Wang</p>