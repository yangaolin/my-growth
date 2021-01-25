#### 当内容超出容器时，容器会出现滚动条，其自带的滚动条有时无法满足我们审美要求，那么我们可以通过css伪类来实现对滚动条的自定义。
<br/>

#### 首先我们要了解滚动条。滚动条从外观来看是由两部分组成：1，可以滑动的部分，我们叫它滑块2，滚动条的轨道，即滑块的轨道，一般来说滑块的颜色比轨道的颜色深。
<br/>

滚动条的css样式主要有三部分组成：
```
    1、::-webkit-scrollbar   定义了滚动条整体的样式；

    2、::-webkit-scrollbar-thumb  滑块部分；

    3、::-webkit-scrollbar-thumb  轨道部分；
```
下面以overflow-y:auto;为例（overflow-x:auto同）

html代码：

```
<div class="test test-1">
  <div class="scrollbar"></div>
</div>
```

css代码

```
.test{
    width: 50px;
    height: 200px;
    overflow: auto;
    float: left;
    margin: 5px;
    border: none;
}
.scrollbar{
    width: 30px;
    height: 300px;
    margin: 0 auto;
 
}
.test-1::-webkit-scrollbar {/*滚动条整体样式*/
        width: 10px;     /*高宽分别对应横竖滚动条的尺寸*/
        height: 1px;
    }
.test-1::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
        border-radius: 10px;
         -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
        background: #535353;
    }
.test-1::-webkit-scrollbar-track {/*滚动条里面轨道*/
        -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
        border-radius: 10px;
        background: #EDEDED;
    }
```
　效果如下如：

![avatar](https://images2015.cnblogs.com/blog/897472/201705/897472-20170502160918836-545708149.png)

如果要改变滚动条的宽度：在::-webkit-scrollbar中改变即可；如果要改变滚动条滑块的圆角，在::-webkit-scrollbar-thumb 中改变；如果要改轨道的圆角在::-webkit-scrollbar-track中改变；

此外，滚动条的滑块不仅可以填充颜色还可以填充图片如下：

css代码：

```
.test-5::-webkit-scrollbar {/*滚动条整体样式*/
    width: 10px;     /*高宽分别对应横竖滚动条的尺寸*/
    height: 1px;
}
.test-5::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
    border-radius: 10px;
    background-color: #F90;
    background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, .2) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, .2) 50%, rgba(255, 255, 255, .2) 75%, transparent 75%, transparent);
}
.test-5::-webkit-scrollbar-track {/*滚动条里面轨道*/
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    /*border-radius: 10px;*/
    background: #EDEDED;
}
```
html代码：

```
<div class="test test-5">
  <div class="scrollbar"></div>
</div>
```
效果如下：
![avatar](https://images2015.cnblogs.com/blog/897472/201705/897472-20170502161504617-916133142.png)

以上就可以做出自己喜欢的滚动条了；

如果文档中会有多个滚动条出现，而且希望所有的滚动条样式是一样的，那么伪元素前面不需要加上class、id、标签名等之类的任何东西。