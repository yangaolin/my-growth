### 钉钉微服务webpack配置调整方案
<br/>

#### 1: Vue CLI配置修改方法

* a 修改build下webpack.prod.config.js。去掉图中三处hash（.[chunkhash]）；
![avatar](https://images2018.cnblogs.com/blog/854495/201803/854495-20180301201208362-1300891677.png)

* b  修改build下webpack.base.config.js。去掉图中hash；
![avatar](https://images2018.cnblogs.com/blog/854495/201803/854495-20180301201230515-422927283.png)

#### 2:  非Vue CLI配置修改方法：

&emsp;&emsp;类似CLI，通常去除config文件中的output的filename和chunkFilename的hash值以及rules中对应的hash值。