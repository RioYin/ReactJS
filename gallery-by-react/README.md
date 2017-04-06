<h1>图片画廊</h1>

<h2使用YEOMAN生成项目文件</h2>

* 运行`npm install -g yo`安装YEOMAN,运行`yo --version`可以查看YEOMAN版本

<h2安装generator-react-webpack</h2>

* 运行`npm install -g generator-react-webpack`安装

<h2>利用react-webpack生成项目</h2>

* 运行`yo react-webpack gallery-by-react`生成项目

<h2>搭建本地服务器运行项目</h2>

* 运行`npm run serve`之后输入网址：`localhost:8000/webpack-dev-server/`查看运行结果

<h2>项目打包</h2>

* 运行`npm run dist`打包输出到dist文件夹

<h2>安装autoprefixer-loader</h2>

* 运行`npm install autoprefixer-loader --save-dev`安装

<h2>配置autoprefixer-loader</h2>

* 在cfg文件夹下的default.js文件中修改如下代码：

```
{
     test: /\.sass/,
     loader: 'style-loader!css-loader!sass-loader?outputStyle=expanded&indentedSyntax'
},
```
* 修改为：

```
{
      test: /\.scss/,
      loader: 'style-loader!css-loader!autoprefixer-loader?{browsers:["last 2 version", "firefox 15"]}!sass-loader?outputStyle=expanded'
},
```

* 同时将src文件夹下的styles文件下的App.css重命名为App.scss,并将components文件夹下的Main.js中的`require('styles/App.css');`修改为`require('styles/App.scss');`

* 由于scss文件需要安装sass-loader进行处理，而sass-loader依赖于node-sass，故对其进行安装

* 若运行`npm install sass-loader -save-dev`会发现无法进行安装，故先运行`set SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/`,再运行`npm install node-sass`即可

<h2>准备图片数据</h2>

* 在src文件夹下新建data文件夹，并在data文件夹下新建imageDatas.json文件如下:

```
[
	{
		"fileName":"1.jpg",
		"title":"No_1",
		"desc":"Number_1"
	},
	{
		"fileName":"2.jpg",
		"title":"No_2",
		"desc":"Number_2"
	},
	{
		"fileName":"3.jpg",
		"title":"No_3",
		"desc":"Number_3"
	},
	{
		"fileName":"4.jpg",
		"title":"No_4",
		"desc":"Number_4"
	},
	{
		"fileName":"5.jpg",
		"title":"No_5",
		"desc":"Number_5"
	},
	{
		"fileName":"6.jpg",
		"title":"No_6",
		"desc":"Number_6"
	},
	{
		"fileName":"7.jpg",
		"title":"No_7",
		"desc":"Number_7"
	},
	{
		"fileName":"8.jpg",
		"title":"No_8",
		"desc":"Number_8"
	},
	{
		"fileName":"9.jpg",
		"title":"No_9",
		"desc":"Number_9"
	},
	{
		"fileName":"10.jpg",
		"title":"No_10",
		"desc":"Number_10"
	},
	{
		"fileName":"11.jpg",
		"title":"No_11",
		"desc":"Number_11"
	},
	{
		"fileName":"12.jpg",
		"title":"No_12",
		"desc":"Number_12"
	},
	{
		"fileName":"13.jpg",
		"title":"No_13",
		"desc":"Number_13"
	},
	{
		"fileName":"14.jpg",
		"title":"No_14",
		"desc":"Number_14"
	},
	{
		"fileName":"15.jpg",
		"title":"No_15",
		"desc":"Number_15"
	},
	{
		"fileName":"16.jpg",
		"title":"No_16",
		"desc":"Number_16"
	}
]
```

* 将准备好的图片放入src文件夹下的images文件夹中

* 由于json文件需要安装json-loader进行处理，故运行`npm install json-loader -save-dev`进行安装

* 安装之后进行配置，在cfg文件夹下的default.js文件中添加如下代码：

```
{
    test: /\.json$/,
    loader: 'json-loader'
}
```

<h2>获取图片数据并将图片信息转换成图片URL路径信息</h2>

* 修改Main.js如下：

```
let imageDatas = require('../data/imageDatas.json');


//利用自执行函数将图片名信息转成图片URL路径信息
imageDatas = (function genImageURL(imageDatasArr) {
    for (let i = 0; i < imageDatasArr.length; i++) {
        let singleImageData = imageDatasArr[i];

        singleImageData.imageURL = require('../images/' + singleImageData.fileName);

        imageDatasArr[i] = singleImageData;
    }

    return imageDatasArr;
})(imageDatas);
```

<h2>搭建舞台</h2>

* 修改Main.js如下：

```
<section className="stage" ref="stage">
     <section className="img-sec">
          {imgFigures}
     </section>
     <nav className="controller-nav">
          {controllerUnits}
     </nav>
</section>
```

<h2>添加舞台样式</h2>

* 将App.scss清空，重新设置为：

```
html, body {
    width: 100%;
    height: 100%;

    background-color: #222;
}

.content {
    width: 100%;
    height: 100%;
}

/* stage -- start */
.stage {
    position: relative;

    width: 100%;
    height: 680px;
}
/* stage -- end */
/* image -- start */
.img-sec {
    position: relative;

    width: 100%;
    height: 100%;
    overflow: hidden;

    background-color: #ddd;


}
/* image -- end */

/* controller -- start */
.controller-nav {
    position: absolute;
    left: 0;
    bottom: 30px;
    z-index: 101;

    width: 100%;

    text-align: center;
}
/* controller -- end */
```
