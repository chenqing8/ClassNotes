# 
#### 绘制步骤：
+ 准备画布
   + 画布是白色的，默认比例300*150
   ```css
   canvas{
    border:1px soild red;
   }
   ```
   + 如何设置画布大小（不建议在样式表中设置尺寸，可以在标签中设置）
   ```html
   <canvas width = "500px" height = "300px" id = "canvas"></canvas>
   ```
+ 准备绘制工具
   + 获取元素
   ```javascript
   var myCanvas = document.getElementById("canvas")
   ```
   + 获取上下文(获取工具箱)
   ```javascript
   var ctx = myCanvas.getContext('2d')
   ```
+ 开始绘图
   + 移动画笔到坐标为(100,100)的位置，起始坐标(0,0)在窗口的左上角,表示(x,y)
   ![](images/canvas/location.jpg)
   ```javascript
   ctx.moveTo(100,100)
   ```
   + 绘制直线(轨迹还不是真正的线)
   ```javascript
   ctx.lineTo(200,100)
   ```
   + 描边(把轨迹描绘出来)
   ```javascript
   ctx.stroke();
   ```
   >`问题一：画布的高宽为什么不能在样式表中设置？`
     因为在样式表中设置画布大小，会导致图形画布放大
     *所以在样式表中只能设置画布样式，不能设置大小
#### 图形绘制
  - 路径绘制(2选1)
    + 描边 `stroke()`
    中间没有颜色,当像素够大，起始点和lineTo无法完全闭合,原因就在于起始点是从中心开始的，可以使用canvas自动闭合，就是最后一条线不手动绘制`ctx.closePath()`
    + 填充 `fill()`中间带有颜色
    ![](images/canvas/path(1).jpg)
  - 图形绘制
    + 线 `lineTo()`
    + 线 `getLineDash()`获取虚线的不重复的排列方式
    + 虚线 `setLineDash([5,10])`第一个参数表示实线的宽度，第二个参数表示虚线的宽度，当两个相同时，可以合并[5],数组是用来表示排列方式，虚实交替。
    + 矩形 `rect(10,10,200,100)`在（10，10）的位置绘制一个宽度200，高度100的矩形，没有`beginPath`。
    + 描边矩形 `strokeRect(10,10,200,100)`在（10，10）的位置绘制一个宽度200，高度100的矩形，默认有`beginPath`,有描边效果，这不会覆盖别人的。
    + 填充矩形 `fillRect(10,10,200,100)`在（10，10）的位置绘制一个宽度200，高度100的矩形，默认有`beginPath`,有填充效果，这不会覆盖别人的。
    + 察除矩形区域 `clearRect(10,10,200,100)`在（10，10）的位置察除一个宽度200，高度100的矩形，默认有`beginPath`,有填充效果，这不会覆盖别人的。

#### 设置样式：
  作用域是全局的会造成样式覆盖，如何解决,`在每一个绘图开始加一个ctx.beginPath()开启新路径,对以上的代码不生效`
+ 描边颜色：`ctx.strokeStyle='blue'`
+ 描边宽度：`ctx.lineWidth=10`,默认单位是`px`
+ 线条两端样式线条越宽帽子越大：`ctx.lineCap='butt'`,默认是`butt`,还有`round`:两端同时延伸一块圆角的帽子;`square`:两端同时延伸一块直角的帽子
+ 相交线的拐点样式：`ctx.lineJoin='miter'`,默认是`miter`,还有`round`:圆角的拐点;`bevel`:直角的拐点
+ 虚线偏移 `ctx.lineDashoffset=20`如果是正的值，虚线就往后偏移，反之往前偏移
+ 虚线偏移 `ctx.fillStyle='#f00'`设置填充的颜色


#### 非0环绕规则
   >你想看一块区域是否会被填充颜色：
   1.从这个区域拉一条直线,从里到外，顺时针，以边最少的地方拉，相交越少越好，
   2.去看和这条直线相交的轨迹，
   3.如果顺时针的轨迹+1，逆时针轨迹-1，
   4.总和为0则不被填充，非0则被填充
   ![](images/canvas/zero(1).jpg)
#### 实战
   1. 渐变线条：
   ```javascript
   /*基础代码;*/
   /*线是由点构成的;所以要一个一个的点进行绘制*/
   ctx.lineWidth=30;
   for(var i=0;i<255;i++){
      ctx.beginPath();
      ctx.moveTo(100+i-1,100);
      ctx.lineTo(100+i,100);
      ctx.strokeStyle=`rgb(${i},${i},${i})`
      ctx.stroke();
   }
   /*由此绘制了一个从黑到白的渐变*/
   ```
   2. 折线图绘制
   ```javascript
   /* 基础代码 */
   /* 1.绘制网格 */
   /* 网格大小 */
   var grid=10;

   /* 画多少条x轴的线 竖线*/
   /* var canvasW=mycanvas.width(); */
   var canvasW=ctx.canvas.width();
   var lineXTotal=Math.floor(canvasW/grid);
   for(var i=0;i<lineXTotal;i++){
      ctx.beginPath();
      ctx.moveTo(0-0.5,0);
      ctx.lineTo(i*grid-0.5,canvasH);
      ctx.strokeStyle=`#eee`
      ctx.stroke();
   }
   /* 画多少条y轴的线 横线*/
   /* var canvasH=mycanvas.heigth(); */
   var canvasH=ctx.canvas.heigth();
  var lineYTotal=Math.floor(canvasH/grid);
   for(var i=0;i<lineYTotal;i++){
      ctx.beginPath();
      ctx.moveTo(0,0-0.5);
      ctx.lineTo(canvasW,i*grid-0.5);
      ctx.strokeStyle=`#eee`
      ctx.stroke();
   }
   /* 2.绘制坐标系*/
   /* 确定原点 */
   /* 确定距离画布的距离 */
   /* 确定坐标系的长度 */
   /* 确定箭头，确定为一个等腰三角形 */
   /* 箭头填充 */

   <!-- 绘制原点 -->
   var space=20;/* 原点距离20 */
   var arrow=10;/* 箭头大小，垂直方向的线长度 */
   var canvasH=ctx.canvas.heigth();
   var canvasW=ctx.canvas.width();
   var x0=space;
   var y0=canvasH-space;/* canvas和我们想的坐标原点不一致 */

   <!-- 绘制x轴 -->
   ctx.moveTo(x0,y0);
   ctx.lineTo(canvasW-space,y0);
   ctx.lineTo(canvasW-space -arrow,y0+arrow/2);
   ctx.lineTo(canvasW-space -arrow,y0-arrow/2);
   ctx.lineTo(canvasW-space,y0);/* 必须手动闭合,因为他的起点在原点，闭合后就到原点去了 */
   ctx.fill();
   ctx.stroke();

   <!-- 绘制y轴 -->
   ctx.beginPath();
   ctx.moveTo(x0,y0);
   ctx.lineTo(space,space);
   ctx.lineTo(x0 + arrow/2,space+arrow);
   ctx.lineTo(x0 -arrow,space-arrow);
   ctx.lineTo(space,space);/* 必须手动闭合,因为他的起点在原点，闭合后就到原点去了 */
   ctx.fill();
   ctx.stroke();

   <!-- 绘制坐标点 -->
   var coord={
      x:200,
      y:100
   }
   var dotSize=6;
   ctx.moveTo(coord.x-dotSize/2,coord.y-dotSize/2);
   ctx.lineTo(coord.x+dotSize/2,coord.y-dotSize/2);
   ctx.lineTo(coord.x+dotSize/2,coord.y+dotSize/2);
   ctx.lineTo(coord.x-dotSize/2,coord.y+dotSize/2);
   ctx.closePath();/* 自动填充 */
   ctx.fill();

   <!-- 绘制折线图完整版（对象） -->
      /*1.构造函数*/
    var LineChart = function (ctx) {
        /*获取绘图工具*/
        this.ctx = ctx || document.querySelector('canvas').getContext('2d');
        /*画布的大小*/
        this.canvasWidth = this.ctx.canvas.width;
        this.canvasHeight = this.ctx.canvas.height;
        /*网格的大小*/
        this.gridSize = 10;
        /*坐标系的间距*/
        this.space = 20;
        /*坐标原点*/
        this.x0 = this.space;
        this.y0 = this.canvasHeight - this.space;
        /*箭头的大小*/
        this.arrowSize = 10;
        /*绘制点*/
        this.dottedSize = 6;
        /*点的坐标 和数据有关系  数据可视化*/
    }
    /*2.行为方法*/
    LineChart.prototype.init = function (data) {
        this.drawGrid();
        this.drawAxis();
        this.drawDotted(data);
    };
    /*绘制网格*/
    LineChart.prototype.drawGrid = function () {
        /*x方向的线*/
        var xLineTotal = Math.floor(this.canvasHeight / this.gridSize);
        this.ctx.strokeStyle = '#eee';
        for (var i = 0; i <= xLineTotal; i++) {
            this.ctx.beginPath();
            this.ctx.moveTo(0, i * this.gridSize - 0.5);
            this.ctx.lineTo(this.canvasWidth, i * this.gridSize - 0.5);
            this.ctx.stroke();
        }
        /*y方向的线*/
        var yLineTotal = Math.floor(this.canvasWidth / this.gridSize);
        for (var i = 0; i <= yLineTotal; i++) {
            this.ctx.beginPath();
            this.ctx.moveTo(i * this.gridSize - 0.5, 0);
            this.ctx.lineTo(i * this.gridSize - 0.5, this.canvasHeight);
            this.ctx.stroke();
        }
    };
    /*绘制坐标系*/
    LineChart.prototype.drawAxis = function () {
        /*X轴*/
        this.ctx.beginPath();
        this.ctx.strokeStyle = '#000';
        this.ctx.moveTo(this.x0, this.y0);
        this.ctx.lineTo(this.canvasWidth - this.space, this.y0);
        this.ctx.lineTo(this.canvasWidth - this.space - this.arrowSize, this.y0 + this.arrowSize / 2);
        this.ctx.lineTo(this.canvasWidth - this.space - this.arrowSize, this.y0 - this.arrowSize / 2);
        this.ctx.lineTo(this.canvasWidth - this.space, this.y0);
        this.ctx.stroke();
        this.ctx.fill();
        /*Y轴*/
        this.ctx.beginPath();
        this.ctx.strokeStyle = '#000';
        this.ctx.moveTo(this.x0, this.y0);
        this.ctx.lineTo(this.space, this.space);
        this.ctx.lineTo(this.space + this.arrowSize / 2, this.space + this.arrowSize);
        this.ctx.lineTo(this.space - this.arrowSize / 2, this.space + this.arrowSize);
        this.ctx.lineTo(this.space, this.space);
        this.ctx.stroke();
        this.ctx.fill();
    };
    /*绘制所有点*/
    LineChart.prototype.drawDotted = function (data) {
        /*1.数据的坐标 需要转换 canvas坐标*/
        /*2.再进行点的绘制*/
        /*3.把线连起来*/
        var that = this;
        /*记录当前坐标*/
        var prevCanvasX = 0;
        var prevCanvasY = 0;
        data.forEach(function (item, i) {
            /* x = 原点的坐标 + 数据的坐标 */
            /* y = 原点的坐标 - 数据的坐标 */
            var canvasX = that.x0 + item.x;
            var canvasY = that.y0 - item.y;
            /*绘制点*/
            that.ctx.beginPath();
            that.ctx.moveTo(canvasX - that.dottedSize / 2, canvasY - that.dottedSize / 2);
            that.ctx.lineTo(canvasX + that.dottedSize / 2, canvasY - that.dottedSize / 2);
            that.ctx.lineTo(canvasX + that.dottedSize / 2, canvasY + that.dottedSize / 2);
            that.ctx.lineTo(canvasX - that.dottedSize / 2, canvasY + that.dottedSize / 2);
            that.ctx.closePath();
            that.ctx.fill();
            /*点的连线*/
            /*当时第一个点的时候 起点是 x0 y0*/
            /*当时不是第一个点的时候 起点是 上一个点*/
            if(i == 0){
                that.ctx.beginPath();
                that.ctx.moveTo(that.x0,that.y0);
                that.ctx.lineTo(canvasX,canvasY);
                that.ctx.stroke();
            }else{
                /*上一个点*/
                that.ctx.beginPath();
                that.ctx.moveTo(prevCanvasX,prevCanvasY);
                that.ctx.lineTo(canvasX,canvasY);
                that.ctx.stroke();
            }
            /*记录当前的坐标，下一次要用*/
            prevCanvasX = canvasX;
            prevCanvasY = canvasY;
        });
    };
    /*3.初始化*/
    var data = [
        {
            x: 100,
            y: 120
        },
        {
            x: 200,
            y: 160
        },
        {
            x: 300,
            y: 240
        },
        {
            x: 400,
            y: 120
        },
        {
            x: 500,
            y: 80
        }
    ];
    var lineChart = new LineChart();
    lineChart.init(data);
   ```
   3. 渐变矩形
   ```javascript
   var a=ctx.createLinearGradient(10,10,50,10)/* 创建一个从（10，10）到（50，10）的方向的线性渐变 */
   a.addColorStop(0.5,'red')/* 在0.5的位置增加一个颜色断点 */
   a.addColorStop(1,'green')/* 在1的位置增加一个颜色断点 */
   ctx.fillStyle=a;
   ctx.fillRect(10,10,40,10)
   ```
















   