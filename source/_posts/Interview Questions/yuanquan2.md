---
title: 猿圈团队面试题(二)
date: 2017-05-02 16:49
categories: ['面试题']
type: ['面试题']
---
## 等边三角形绕正方形圆周运动
使用div元素创立一个边长为30px的等边三角形（尖朝上）<br>
，让该等边三角形以5秒为周期重复的沿着边长为200px的正方形做顺时针运动，<br>
起始颜色为红色，到右上角时平滑的渐变为黄色，到右下角时渐变为蓝色，<br>
到左下角时渐变为绿色；请兼容Firefox、Safari、Chrome、Opera浏览器 <br>
```css
        body{
            position: relative;
            width: 200px;
            height: 200px;
            border: 1px green solid;
            margin: 100px;
        }
        div{
            width: 0;
            height: 0;
            position: absolute;
            top:-30px;
            left:-30px;
            /*No.1*/
            /*开始写代码，请实现等边三角形，以及制定动画效果，请兼容Firefox、Safari、Chrome、Opera浏览器*/
            -webkit-animation:roll 5s linear infinite;
            -o-animation:roll 5s linear infinite;
            animation: roll 5s linear infinite;
            -webkit-transform-origin:131px 131px;
            -moz-transform-origin:131px 131px;
            -ms-transform-origin:131px 131px;
            -o-transform-origin:131px 131px;
            transform-origin:131px 131px;

            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 30px solid red;
            /*end_code*/
            display: inline-block;
        }
        /*No.2*/
        /*开始写代码，请实现详细的动画效果，请兼容Firefox、Safari、Chrome、Opera浏览器*/

        @keyframes roll{
            0%{
                -webkit-transform: rotate(0deg);
                -moz-transform: rotate(0deg);
                -ms-transform: rotate(0deg);
                -o-transform: rotate(0deg);
                transform: rotate(0deg);
            }
            25%{
                border-bottom: 30px solid yellow;
            }
            50%{
                border-bottom: 30px solid blue;

            }
            75%{
                border-bottom: 30px solid green;

            }
            100%{
                -webkit-transform: rotate(360deg);
                -moz-transform: rotate(360deg);
                -ms-transform: rotate(360deg);
                -o-transform: rotate(360deg);
                transform: rotate(360deg);
            }
        }
        @-webkit-keyframes roll {
            0%{
                -webkit-transform: rotate(0deg);
                -moz-transform: rotate(0deg);
                -ms-transform: rotate(0deg);
                -o-transform: rotate(0deg);
                transform: rotate(0deg);
            }
            25%{
                border-bottom: 30px solid yellow;
            }
            50%{
                border-bottom: 30px solid blue;

            }
            75%{
                border-bottom: 30px solid green;

            }
            100%{
                -webkit-transform: rotate(360deg);
                -moz-transform: rotate(360deg);
                -ms-transform: rotate(360deg);
                -o-transform: rotate(360deg);
                transform: rotate(360deg);
            }
        }
        @-moz-keyframes roll {
            0%{
                -webkit-transform: rotate(0deg);
                -moz-transform: rotate(0deg);
                -ms-transform: rotate(0deg);
                -o-transform: rotate(0deg);
                transform: rotate(0deg);
            }
            25%{
                border-bottom: 30px solid yellow;
            }
            50%{
                border-bottom: 30px solid blue;

            }
            75%{
                border-bottom: 30px solid green;

            }
            100%{
                -webkit-transform: rotate(360deg);
                -moz-transform: rotate(360deg);
                -ms-transform: rotate(360deg);
                -o-transform: rotate(360deg);
                transform: rotate(360deg);
            }
        }
        @-o-keyframes roll {
            0%{
                -webkit-transform: rotate(0deg);
                -moz-transform: rotate(0deg);
                -ms-transform: rotate(0deg);
                -o-transform: rotate(0deg);
                transform: rotate(0deg);
            }
            25%{
                border-bottom: 30px solid yellow;
            }
            50%{
                border-bottom: 30px solid blue;

            }
            75%{
                border-bottom: 30px solid green;

            }
            100%{
                -webkit-transform: rotate(360deg);
                -moz-transform: rotate(360deg);
                -ms-transform: rotate(360deg);
                -o-transform: rotate(360deg);
                transform: rotate(360deg);
            }
        }
        @-ms-keyframes  roll{
            0%{
                -webkit-transform: rotate(0deg);
                -moz-transform: rotate(0deg);
                -ms-transform: rotate(0deg);
                -o-transform: rotate(0deg);
                transform: rotate(0deg);
            }
            25%{
                border-bottom: 30px solid yellow;
            }
            50%{
                border-bottom: 30px solid blue;

            }
            75%{
                border-bottom: 30px solid green;

            }
            100%{
                -webkit-transform: rotate(360deg);
                -moz-transform: rotate(360deg);
                -ms-transform: rotate(360deg);
                -o-transform: rotate(360deg);
                transform: rotate(360deg);
            }
        }
        @-khtml-keyframes roll {
            0%{
                -webkit-transform: rotate(0deg);
                -moz-transform: rotate(0deg);
                -ms-transform: rotate(0deg);
                -o-transform: rotate(0deg);
                transform: rotate(0deg);
            }
            25%{
                border-bottom: 30px solid yellow;
            }
            50%{
                border-bottom: 30px solid blue;

            }
            75%{
                border-bottom: 30px solid green;

            }
            100%{
                -webkit-transform: rotate(360deg);
                -moz-transform: rotate(360deg);
                -ms-transform: rotate(360deg);
                -o-transform: rotate(360deg);
                transform: rotate(360deg);
            }
        }
```
```html
<body>
<div></div>
</body>
```

<p align=right>---by  lori.Wang</p>