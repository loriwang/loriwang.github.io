---
title: less&&的问题
date: 2020-12-14 11:48
categories: 'css less'
type: 'css'
---
最近在开发项目的时候碰到一个一个less编译的问题, 可能是对less语法不太熟悉, 也可能是less语法在设定的时候就存在的问题, 我我们先看一段代码

``` less
@prefix: ~'box';
.@{prefix}{
    border:1px solid red;
    &&-wrap{
        border: 1px solid blue
    }
}
```

那么跟我们想的那样, 这段代码编译之后应该是这样的

``` css
.box {
    border: 1px solid red;
}

.box.box-wrap {
    border: 1px solid blue;
}
```

但是因为我的项目有个主题的问题, 目前实现的方案是在 body上增加一个 `data-theme="white"` 这样的方式来确定主题及主题的作用域, 所以上边的代码在我的项目中less应该是这样的

``` less
@prefix: ~'box';
[data-theme="white"] {
    .@{prefix}{
        border:1px solid red;
        &&-wrap{
            border: 1px solid blue
        }
    }
}
```

不过如果是这样, 编译之后就出现了**亿**点点小差错

``` css
[data-theme="white"] .box {
    border: 1px solid red;
}

[data-theme="white"] .box[data-theme="white"] .box-wrap {
    border: 1px solid blue;
}
```

这样就跟我们预想的出现了很大的偏差, 原来 `.box-wrap` 就根本不会生效. 那么会不是是属性选择器的问题导致的呢, 我们把上面的属性选择器更改为类选择器作为尝试

``` less
@prefix: ~'box';
.container{
    .@{prefix}{
        border:1px solid red;
        &&-wrap{
            border: 1px solid blue
        }
    }
}
```

编译之后结果是这样的

``` css
.container .box {
    border: 1px solid red;
}

.container .box.container .box-wrap {
    border: 1px solid blue;
}
```

那么这部分就不是选择器的问题, 如果换成标签选择器还是会出现同样的结果

``` less
body .box {
  border: 1px solid red;
}
body .boxbody .box-wrap {
  border: 1px solid blue;
}

```

emmm 这就不是**亿**点点差错了, 如果这样一旦你的文件存在嵌套的问题就会引申出大麻烦, 我现在吧这部分进行多重的嵌套看看

``` less
@prefix: ~'box';
body{
    .container{
        div{
            .@{prefix}{
                border:1px solid red;
                &&-wrap{
                    border: 1px solid blue
                }
            }
        }

    }
}

```
我们期待的结果是 
```css
body .container div .box{
    border:1px solid red;
}
body .container div .box.box-wrap{
    border: 1px solid blue
}
```
实际的输出结果是

```css
body .container div .box {
  border: 1px solid red;
}
body .container div .boxbody .container div .box-wrap {
  border: 1px solid blue;
}

```

所以第一个`&` 匹配的是 `.@{prefix}body .container div`,就是其上的所有选择器,第二个`&`匹配的是 `.@{prefix}`

所以解决方案 就是 不去使用`&&`改成 即可,如下
```less
@prefix: ~'box';
body{
    .container{
        div{
            .@{prefix}{
                border:1px solid red;
                &.@{prefix}-wrap{
                    border: 1px solid blue
                }
            }
        }

    }
}
```