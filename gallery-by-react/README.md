<h1>图片画廊</h1>

<h2>使用YEOMAN生成项目文件</h2>

* 运行`npm install -g yo`安装YEOMAN,运行`yo --version`可以查看YEOMAN版本

<h2>安装generator-react-webpack</h2>

* 运行`npm install -g generator-react-webpack`安装

<h2>利用react-webpack生成项目</h2>

* 运行`yo react-webpack gallery-by-react`生成项目

<h2>搭建本地服务器运行项目</h2>

* 运行`npm run serve`之后输入网址：`localhost:8000/webpack-dev-server/`查看运行结果

<h2>项目打包</h2>

* 运行`npm run dist`打包输出到dist文件夹

<h2>安装配置相关loader</h2>

* autoprefixer-loader
  * 先运行`npm install postcss-loader --save-dev`,再运行`npm install autoprefixer-loader --save-dev`安装
  * 在cfg文件夹下的default.js文件中修改如下代码：

```
{
      test: /\.scss/,
      loader: 'style-loader!css-loader!autoprefixer-loader?{browsers:["last 2 version", "firefox 15"]}!sass-loader?outputStyle=expanded'
},
```

* sass-loader
  * 先将src文件夹下的styles文件下的App.css重命名为App.scss,再将components文件夹下的Main.js中的`require('styles/App.css');`修改为`require('styles/App.scss');`
  * 依次运行`npm install node-sass`和`npm install sass-loader -save-dev`,若发现无法进行安装，则先运行`set SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/`,再运行`npm install node-sass`即可

* json-loader
  * json文件需要安装json-loader进行处理，故运行`npm install json-loader -save-dev`进行安装
  * 安装之后进行配置，在cfg文件夹下的default.js文件中添加如下代码：

```
{
    test: /\.json$/,
    loader: 'json-loader'
}
```

<h2>引入相关库，如：bootstrap和jQuery</h2>

* bootstrap
  * 运行`npm install --save react-bootstrap`安装
  * 先在头部引入相关模块，如引入按钮：`import { Button } from 'react-bootstrap';`
  * 然后在render中使用：`<Button >按钮</Button>`
  * 若想使用全部的CSS样式，则需在index.html文件中引入公共CDN：`<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/latest/css/bootstrap.min.css">`

* jQuery
  * 运行`npm install --save-dev jquery`安装
  * 在配置文件中添加下列代码：（如下）并在头部引入：`import $ from 'jquery';`
  
```
plugins:[

new webpack.ProvidePlugin({

  $:"jquery",

  jQuery:"jquery",

  "window.jQuery":"jquery"

})

]
```

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

<h2>设置单个图片组件</h2>

* 在Main.js中添加：

```
class ImgFigure extends React.Component{
  render(){
    return(
        <figure className="img-figure" >
          <img  src={this.props.data.imageURL} alt={this.props.data.title} />
          <figcaption>
            <h2 className="img-title">{this.props.data.title}</h2>
          </figcaption>
        </figure>

      );
  }
}
```

* 在App.scss中添加样式：

```
.img-sec {
    position: relative;

    width: 100%;
    height: 100%;
    overflow: hidden;

    background-color: #ddd;
    /*****添加代码*******/
    @at-root{
      .img-figure{
        position: absolute;

        width: 280px;
        height: 320px;
        margin: 0;
        padding: 40px;
        background-color: #fff;
        box-sizing: border-box;
      }
      figcaption{
        text-align: center;

        .img-title{
          margin:20px 0 0 0;
          color: #a7a0a2;
          font-size: 16px;
        }
      }
    }
}
```

<h2>给图片组件进行定位</h2>

* 在Main.js的AppComponent组件中添加：

```
class AppComponent extends React.Component {

    constructor() {
        super();
        this.Constant = {
            centerPos: {
                left: 0,
                right: 0
            },
            hPosRange: { //水平方向的取值范围
                leftSecx: [0, 0],
                rightSecx: [0, 0],
                y: [0, 0]
            },
            vPosRange: { //垂直方向的取值范围
                x: [0, 0],
                topY: [0, 0]
            }
        };
        this.state = {
            imgsArrangeArr: []
        };
    }

    //重新布局所有图片，其中centerIndex指定居中布局图片的索引
    rearrange(centerIndex) {
        let imgsArrangeArr = this.state.imgsArrangeArr,
            Constant = this.Constant,
            centerPos = Constant.centerPos,
            hPosRange = Constant.hPosRange,
            vPosRange = Constant.vPosRange,
            hPosRangeLeftSecX = hPosRange.leftSecx,
            hPosRangeRightSecX = hPosRange.rightSecx,
            hPosRangeY = hPosRange.y,
            vPosRangeTopY = vPosRange.topY,
            vPosRangeX = vPosRange.x,

            imgsArrangeTopArr = [],
            topImgNum = Math.floor(Math.random() * 2), //取一个或者不取
            topImgSpliceIndex = 0,
            imgsArrangeCenterArr = imgsArrangeArr.splice(centerIndex, 1);

        //首先居中centerIndex的图片
        imgsArrangeCenterArr[0] = {
            pos: centerPos
        };


        //取出要布局到上侧的图片的状态信息
        topImgSpliceIndex = Math.ceil(Math.random(imgsArrangeArr.length - topImgNum));
        while (topImgSpliceIndex === centerIndex) {
            topImgSpliceIndex = Math.ceil(Math.random() * (imgsArrangeArr.length - topImgNum));
        }
        imgsArrangeTopArr = imgsArrangeArr.splice(topImgSpliceIndex, topImgNum);

        //布局位于上侧的图片
        imgsArrangeTopArr.forEach((value, index) => {
            imgsArrangeTopArr[index] = {
                pos: {
                    top: getRangeRandom(vPosRangeTopY[0], vPosRangeTopY[1]),
                    left: getRangeRandom(vPosRangeX[0], vPosRangeX[1])
                }
            };
        });

        //布局位于左右两侧的图片
        for (let i = 0, j = imgsArrangeArr.length, k = j / 2; i < j; i++) {
            let hPosRangeLORX = null;

            //前半部分布局左边，后半部分布局右边
            if (i < k) {
                hPosRangeLORX = hPosRangeLeftSecX;
            } else {
                hPosRangeLORX = hPosRangeRightSecX;
            }

            imgsArrangeArr[i] = {
                pos: {
                    top: getRangeRandom(hPosRangeY[0], hPosRangeY[1]),
                    left: getRangeRandom(hPosRangeLORX[0], hPosRangeLORX[1])
                }
            };
        }

        if (imgsArrangeTopArr && imgsArrangeTopArr[0]) {
            imgsArrangeArr.splice(topImgSpliceIndex, 0, imgsArrangeTopArr[0]);
        }

        imgsArrangeArr.splice(centerIndex, 0, imgsArrangeCenterArr[0]);

        this.setState({
            imgsArrangeArr: imgsArrangeArr
        });
    }

    //组件加载之后，为每张图片计算其位置的范围
    componentDidMount() {

        //获取舞台大小
        let stageDOM = ReactDOM.findDOMNode(this.refs.stage),
            stageW = stageDOM.scrollWidth,
            stageH = stageDOM.scrollHeight,
            halfStageW = Math.ceil(stageW / 2),
            halfStageH = Math.ceil(stageH / 2);

        //获取imgFigure大小
        let imgFigureDOM = ReactDOM.findDOMNode(this.refs.imgFigure0),
            imgW = imgFigureDOM.scrollWidth,
            imgH = imgFigureDOM.scrollHeight,
            halfImgW = Math.ceil(imgW / 2),
            halfImgH = Math.ceil(imgH / 2);

        //计算中心图片的位置
        this.Constant.centerPos = {
            left: halfStageW - halfImgW,
            top: halfStageH - halfImgH
        };

        //计算左侧和右侧区域图片排列位置的取值范围
        this.Constant.hPosRange.leftSecx[0] = -halfImgW;
        this.Constant.hPosRange.leftSecx[1] = halfStageW - halfImgW * 3;
        this.Constant.hPosRange.rightSecx[0] = halfStageW + halfImgW;
        this.Constant.hPosRange.rightSecx[1] = stageW - halfImgW;
        this.Constant.hPosRange.y[0] = -halfImgH;
        this.Constant.hPosRange.y[1] = stageH - halfImgH;

        //计算上侧区域图片排列位置的取值范围
        this.Constant.vPosRange.topY[0] = -halfImgH;
        this.Constant.vPosRange.topY[1] = halfStageH - halfImgH * 3;
        this.Constant.vPosRange.x[0] = halfStageW - imgW;
        this.Constant.vPosRange.x[1] = halfStageW;
        this.rearrange(0);
    }

    render() {

        let controllerUnits = [];
        let imgFigures = [];

        imageDatas.forEach((value, index) => {

            if (!this.state.imgsArrangeArr[index]) {
                this.state.imgsArrangeArr[index] = {
                    pos: {
                        left: 0,
                        top: 0
                    }
                };
            }

            imgFigures.push(<ImgFigure key={index} data={value} ref={'imgFigure' + index}
            arrange={this.state.imgsArrangeArr[index]}/>);

            controllerUnits.push(<ControllerUnit/>);
        });

        return (
            <section className="stage" ref="stage">
                <section className="img-sec">
                    {imgFigures}
                </section>
                <nav className="controller-nav">
                    {controllerUnits}
                </nav>
            </section>
        );
    }
}
```

* 在Main.js中添加：

```
import ReactDOM from 'react-dom';

function getRangeRandom(low, high) {
    return Math.ceil(Math.random() * (high - low) + low);
}
```

* 在Main.js的ImgFigure组件中添加：

```
class ImgFigure extends React.Component {

    render() {

        let styleObj = {};

        //如果props属性中指定了这张图片的位置，则使用
        if (this.props.arrange.pos) {
            styleObj = this.props.arrange.pos;
        }

        return (
            <figure className={imgFigureClassName} style={styleObj}>
                <img src={this.props.data.imageURL}
            alt={this.props.data.title}
            />
                <figcaption>
                    <h2 className="img-title">{this.props.data.title}</h2>
                    <div className="img-back">
                        <p>
                            {this.props.data.desc}
                        </p>
                    </div>
                </figcaption>
            </figure>
        );
    }
}
```

<h2>给图片组件添加旋转,翻转等</h2>

* 在Main.js的AppComponent组件中添加：

```
class AppComponent extends React.Component {

    constructor() {
        super();
        this.Constant = {
            centerPos: {
                left: 0,
                right: 0
            },
            hPosRange: { //水平方向的取值范围
                leftSecx: [0, 0],
                rightSecx: [0, 0],
                y: [0, 0]
            },
            vPosRange: { //垂直方向的取值范围
                x: [0, 0],
                topY: [0, 0]
            }
        };
        this.state = {
            imgsArrangeArr: []
        };
    }

    /*
    * 翻转图片
    * @param index 输入当前被执行inverse操作的图片索引值
    * @return {Function} 这是一个闭包函数，返回一个真正待被执行的函数
    */
    inverse(index) {
        return () => {
            let imgsArrangeArr = this.state.imgsArrangeArr;

            imgsArrangeArr[index].isInverse = !imgsArrangeArr[index].isInverse;

            this.setState({
                imgsArrangeArr: imgsArrangeArr
            });
        };
    }

    //利用rearrange函数，居中对应图片
    center(index) {
        return () => {
            this.rearrange(index);
        };
    }

    //重新布局所有图片，其中centerIndex指定居中布局图片的索引
    rearrange(centerIndex) {
        let imgsArrangeArr = this.state.imgsArrangeArr,
            Constant = this.Constant,
            centerPos = Constant.centerPos,
            hPosRange = Constant.hPosRange,
            vPosRange = Constant.vPosRange,
            hPosRangeLeftSecX = hPosRange.leftSecx,
            hPosRangeRightSecX = hPosRange.rightSecx,
            hPosRangeY = hPosRange.y,
            vPosRangeTopY = vPosRange.topY,
            vPosRangeX = vPosRange.x,

            imgsArrangeTopArr = [],
            topImgNum = Math.floor(Math.random() * 2), //取一个或者不取
            topImgSpliceIndex = 0,
            imgsArrangeCenterArr = imgsArrangeArr.splice(centerIndex, 1);

        //首先居中centerIndex的图片
        imgsArrangeCenterArr[0] = {
            pos: centerPos,
            rotate: 0,
            isCenter: true
        };


        //取出要布局到上侧的图片的状态信息
        topImgSpliceIndex = Math.ceil(Math.random(imgsArrangeArr.length - topImgNum));
        while (topImgSpliceIndex === centerIndex) {
            topImgSpliceIndex = Math.ceil(Math.random() * (imgsArrangeArr.length - topImgNum));
        }
        imgsArrangeTopArr = imgsArrangeArr.splice(topImgSpliceIndex, topImgNum);

        //布局位于上侧的图片
        imgsArrangeTopArr.forEach((value, index) => {
            imgsArrangeTopArr[index] = {
                pos: {
                    top: getRangeRandom(vPosRangeTopY[0], vPosRangeTopY[1]),
                    left: getRangeRandom(vPosRangeX[0], vPosRangeX[1])
                },
                rotate: get30DegRandom(),
                isCenter: false
            };
        });

        //布局位于左右两侧的图片
        for (let i = 0, j = imgsArrangeArr.length, k = j / 2; i < j; i++) {
            let hPosRangeLORX = null;

            //前半部分布局左边，后半部分布局右边
            if (i < k) {
                hPosRangeLORX = hPosRangeLeftSecX;
            } else {
                hPosRangeLORX = hPosRangeRightSecX;
            }

            imgsArrangeArr[i] = {
                pos: {
                    top: getRangeRandom(hPosRangeY[0], hPosRangeY[1]),
                    left: getRangeRandom(hPosRangeLORX[0], hPosRangeLORX[1])
                },
                rotate: get30DegRandom(),
                isCenter: false
            };
        }

        if (imgsArrangeTopArr && imgsArrangeTopArr[0]) {
            imgsArrangeArr.splice(topImgSpliceIndex, 0, imgsArrangeTopArr[0]);
        }

        imgsArrangeArr.splice(centerIndex, 0, imgsArrangeCenterArr[0]);

        this.setState({
            imgsArrangeArr: imgsArrangeArr
        });
    }

    //组件加载之后，为每张图片计算其位置的范围
    componentDidMount() {

        //获取舞台大小
        let stageDOM = ReactDOM.findDOMNode(this.refs.stage),
            stageW = stageDOM.scrollWidth,
            stageH = stageDOM.scrollHeight,
            halfStageW = Math.ceil(stageW / 2),
            halfStageH = Math.ceil(stageH / 2);

        //获取imgFigure大小
        let imgFigureDOM = ReactDOM.findDOMNode(this.refs.imgFigure0),
            imgW = imgFigureDOM.scrollWidth,
            imgH = imgFigureDOM.scrollHeight,
            halfImgW = Math.ceil(imgW / 2),
            halfImgH = Math.ceil(imgH / 2);

        //计算中心图片的位置
        this.Constant.centerPos = {
            left: halfStageW - halfImgW,
            top: halfStageH - halfImgH
        };

        //计算左侧和右侧区域图片排列位置的取值范围
        this.Constant.hPosRange.leftSecx[0] = -halfImgW;
        this.Constant.hPosRange.leftSecx[1] = halfStageW - halfImgW * 3;
        this.Constant.hPosRange.rightSecx[0] = halfStageW + halfImgW;
        this.Constant.hPosRange.rightSecx[1] = stageW - halfImgW;
        this.Constant.hPosRange.y[0] = -halfImgH;
        this.Constant.hPosRange.y[1] = stageH - halfImgH;

        //计算上侧区域图片排列位置的取值范围
        this.Constant.vPosRange.topY[0] = -halfImgH;
        this.Constant.vPosRange.topY[1] = halfStageH - halfImgH * 3;
        this.Constant.vPosRange.x[0] = halfStageW - imgW;
        this.Constant.vPosRange.x[1] = halfStageW;
        this.rearrange(0);
    }

    render() {

        let controllerUnits = [];
        let imgFigures = [];

        imageDatas.forEach((value, index) => {

            if (!this.state.imgsArrangeArr[index]) {
                this.state.imgsArrangeArr[index] = {
                    pos: {
                        left: 0,
                        top: 0
                    },
                    rotate: 0,
                    isInverse: false,
                    inCenter: false
                }
            }

            imgFigures.push(<ImgFigure key={index} data={value} ref={'imgFigure' + index}
            arrange={this.state.imgsArrangeArr[index]} inverse={this.inverse(index)}
            center={this.center(index)}/>);

            controllerUnits.push(<ControllerUnit/>);
        });

        return (
            <section className="stage" ref="stage">
                <section className="img-sec">
                    {imgFigures}
                </section>
                <nav className="controller-nav">
                    {controllerUnits}
                </nav>
            </section>
        );
    }
}
```

* 在Main.js的ImgFigure组件中添加：

```
class ImgFigure extends React.Component {

    constructor(props) {
        super(props);
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick(e) {

        if (this.props.arrange.isCenter) {
            this.props.inverse();
        } else {
            this.props.center();
        }


        e.stopPropagation();
        e.preventDefault();
    }

    render() {

        let styleObj = {};

        //如果props属性中指定了这张图片的位置，则使用
        if (this.props.arrange.pos) {
            styleObj = this.props.arrange.pos;
        }

        if (this.props.arrange.rotate) {
            (['Moz', 'ms', 'Webkit', '']).forEach((value) => {
                styleObj[value + 'Transform'] = 'rotate(' + this.props.arrange.rotate + 'deg)';
            });
        }

        if (this.props.arrange.isCenter) {
            styleObj.zIndex = 11;
        }

        let imgFigureClassName = 'img-figure';

        imgFigureClassName += this.props.arrange.isInverse ? ' is-inverse' : '';

        return (
            <figure className={imgFigureClassName} style={styleObj} onClick={this.handleClick}>
                <img src={this.props.data.imageURL}
            alt={this.props.data.title}
            />
                <figcaption>
                    <h2 className="img-title">{this.props.data.title}</h2>
                    <div className="img-back" onClick={this.handleClick}>
                        <p>
                            {this.props.data.desc}
                        </p>
                    </div>
                </figcaption>
            </figure>
        );
    }
}
```

* 在App.scss中添加：

```
.img-sec{
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #ddd;
  perspective: 1800px;
  overflow: hidden;

  @at-root{
    .img-figure{
      position: absolute;
      width: 280px;
      height: 320px;
      margin: 0;
      padding:40px;
      background-color: #fff;
      box-sizing:border-box;
      cursor: pointer;
      transform-origin:0 50% 0;
      transform-style:preserve-3d;
      transition:transform .6s ease-in-out, left .6s ease-in-out,top .6s ease-in-out;

      &.is-inverse{
        transform:translate(280px) rotateY(180deg);
      }
    }

    figcaption{
      text-align: center;

      .img-title{
        margin:20px 0 0 0;
        color:#a7a0a2;
        font-size: 16px;
      }

      .img-back{
        position: absolute;
        left:0;
        top:0;
        width: 100%;
        height: 100%;
        padding:50px 40px;
        overflow: auto;

        color:#a7a0a2;
        font-size: 22px;
        line-height: 1.25;
        text-align: left;
        background-color: #fff;
        box-sizing: border-box;
        transform: rotateY(180deg) translateZ(1px);
        backface-visibility:hidden;

        p{
          margin:0;
        }
      }
    }
  }
}
```

<h2>添加控制组件</h2>

* 在Main.js的ControllerUnit组件中添加：

```
class ControllerUnit extends React.Component {

    constructor(props) {
        super(props);
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick(e) {


        //如果点击的是当前正在选中态的按钮，则翻转图片，否则将对应的图片居中
        if (this.props.arrange.isCenter) {
            this.props.inverse();
        } else {
            this.props.center();
        }

        e.stopPropagation();
        e.preventDefault();
    }

    render() {

        let controllerUnitClassName = "controller-unit";

        //如果对应的是居中的图片，显示按钮的居中态
        if (this.props.arrange.isCenter) {
            controllerUnitClassName += " is-center";

            //如果同时对应的时候翻转图片，显示按钮的翻转态
            if (this.props.arrange.isInverse) {
                controllerUnitClassName += " is-inverse";
            }
        }

        return (
            <span className={controllerUnitClassName} onClick={this.handleClick}></span>
        );
    }
}
```

* 在Main.js的AppComponent组件中添加：

```
render() {

        let controllerUnits = [];
        let imgFigures = [];

        imageDatas.forEach((value, index) => {

            if (!this.state.imgsArrangeArr[index]) {
                this.state.imgsArrangeArr[index] = {
                    pos: {
                        left: 0,
                        top: 0
                    },
                    rotate: 0,
                    isInverse: false,
                    inCenter: false
                }
            }

            imgFigures.push(<ImgFigure key={index} data={value} ref={'imgFigure' + index}
            arrange={this.state.imgsArrangeArr[index]} inverse={this.inverse(index)}
            center={this.center(index)}/>);

            controllerUnits.push(<ControllerUnit key={index} arrange={this.state.imgsArrangeArr[index]}
            inverse={this.inverse(index)} center={this.center(index)}/>);
        });

        return (
            <section className="stage" ref="stage">
                <section className="img-sec">
                    {imgFigures}
                </section>
                <nav className="controller-nav">
                    {controllerUnits}
                </nav>
            </section>
        );
    }
```

* 在src文件夹下新建fonts文件夹，并在fonts文件夹中新建icons文件夹，将字体文件放入iconsz中,其中eot适配IE;woff适配chrome及Firefox;ttf适配chrome、Firefox、opera、Safari、Android、iOS4.2+;svg适配iOS4.1

* 在App.scss中添加：

```
@font-face{
  font-family: "icons-turn-arrow";
  src:url("../fonts/icons/turn-arrow.eot") format("embedded-opentype"),
      url("../fonts/icons/turn-arrow.woff") format("woff"),
      url("../fonts/icons/turn-arrow.ttf") format("truetype"),
      url("../fonts/icons/turn-arrow.svg") format("svg");
}

.controller-nav{
  position: absolute;
  left: 0;
  bottom: 30px;
  z-index: 101;
  width: 100%;
  text-align: center;

  @at-root {
    .controller-unit{
      display: inline-block;
      width: 30px;
      height: 30px;
      margin:0 5px;
      text-align: center;
      vertical-align: middle;
      cursor: pointer;
      background-color: #aaa;
      border-radius: 50%;
      transform:scale(.5);
      transition:transform .6s ease-in-out, background-color .3s;

      &.is-center{
        background-color: #888;
        transform:scale(1);

        &::after{
          color:#fff;
          line-height: 30px;
          font-family: "icons-turn-arrow";
          font-size: 80%;
          content:"\e600";

          -webkit-font-smoothing:antialiased;
          -moz-osx-font-smoothing:grayscale;
        }

        &.is-inverse{
          background-color: #555;
          transform:rotateY(180deg);
        }
      }

    }
  }
}
```

<h2>打包发布</h2>

* 将绝对路径改为相对路径：
  * 修改src文件夹下的index.html中的`<script type="text/javascript" src="/assets/app.js"></script>`为`<script type="text/javascript" src="assets/app.js"></script>`
  * 修改cfg文件夹下的default.js中的`publicPath: '/assets/',`为`publicPath: 'assets/',`
  
* 运行`npm run dist`进行打包
* 通过`../dist`进行访问
