<template>
    
<div id="box">

  <div calss="suspend">
    <div>A键：开始</div>
    <div>空格：暂停</div>
    <div>方向键：改变移动的方向</div>
    <div>速度：
      <input type="number" id="speed" v-model="speed">
    </div>
    <div>初始长度：
      <input type="number" v-model="length">
    </div>
    <div>每一行和每一列的个数<br>只能输入20到100的偶数
      <br>
      <input type="number" v-model="across">
    </div>
    <div>舞台的形状(只能输入4,5,6)
      <br>
      <input type="number" v-model="shape">
    </div>

    <!-- <button id="btn">
      开始
    </button> -->
  
  </div>


  <div id="hello">
  </div>

</div>
</template>

<script>
import * as PIXI from 'pixi.js'

export default {
  name: 'HelloWorld',
  data () {
    return {
      length: 3,
      snake: [],
      num:51,
      direction: 1,
      food: 53,
      n: 0,
      bool: false,
      speed: 100,
      list:{},
      coum: true,
      across:40,
      acr:0,
      data:[],
      a: 1,
      shape: 6

    }
  },
  watch: {
  },
  methods: {
    // change(e) {
    //   let num =  Number(this.across)
    //   console.log('333333333333333333333333',num)
    //   if (num > 100 || num < 20) {
    //     this.across = this.acr;
    //   } else {
    //     this.acr = num;
    //   }
      
    // }
  },
  mounted () {
    this.acr = this.across;
     for (var i = 1; i < this.length; i++) {
      this.snake.push(this.num--);
    }
    var This = this;
    function draw (graphics, seat, color) {
      graphics.lineStyle(1,16777215,1);
      graphics.beginFill(color, 1)
      graphics.drawRect(seat % This.across * (600/This.across), ~~(seat / This.across) * (600/This.across), (600/This.across) - 2, (600/This.across) - 2)
      // graphics.endFill();
    }
    function draw2 (graphics, seat, color) {
      graphics.lineStyle(1,16777215,0.3);
      graphics.beginFill(color, 1)
      graphics.drawRect(seat % This.across * (600/This.across) + 0.5, ~~(seat / This.across) * (600/This.across) + 0.5, (600/This.across) - 1, (600/This.across) - 1)
      // graphics.endFill();
    }
    this.list.Application = PIXI.Application
    this.list.Container = PIXI.Container
    this.list.app = new this.list.Application({
      width: 800,
      height: 600,
      antialias: true,
      transparent: false,
      forceCanvas: true,
      resolution: 1
    })
    this.list.app.renderer.backgroundColor = 16777215
    this.list.app.renderer.resize(600, 600)
    // this.list.app.view.style.border = '1px solid 16777215'
    this.list.app.renderer.view.style.position = 'absolute'
    this.list.app.renderer.view.style.display = 'block'
    this.list.app.renderer.view.style.left = 0
    this.list.app.renderer.view.style.top = 0
    this.list.app.renderer.view.style.bottom = 0
    this.list.app.renderer.view.style.right = 0
    this.list.app.renderer.view.style.margin = 'auto'
    this.list.app.renderer.autoResize = true
    document.getElementById('hello').appendChild(this.list.app.view)
    this.list.stage = new this.list.Container()
    this.list.app.stage.interactive = true
    this.list.graphics = new PIXI.Graphics()
    function table () {
      for(var i = 1; i < This.across; i++) {
        This.list.graphics.lineStyle(1,16777215,1);
        This.list.graphics.moveTo(600/This.across*i,0);
        This.list.graphics.lineTo(600/This.across*i,600);
        This.list.graphics.moveTo(0,600/This.across*i);
        This.list.graphics.lineTo(600,600/This.across*i);
      }
    }
    aaa()
    table();
    function aaa() {
      This.list.graphics.lineStyle(0,0x000000,1);
      let path = [];
      This.data = [];
      This.list.graphics.beginFill(0x99CCFF,1);
      This.shape = This.shape - 0;
      if (This.shape === 6) {
        var getDegree = function(vertices, i) {
          return 360 / vertices * (i + 0.5) + 90;
        }
      
        var getRadian = function(degree) {
            return degree * Math.PI / 180;
        };
          for (var i = 0; i < This.shape; i++) {
            // 计算偏转
            var degree = getDegree(This.shape, i),
              radian = getRadian(degree);
      
            // 增加1/3的canvas大小位移量以免被边缘挡住
              var x = Math.cos(radian) * 300 + 600 / 2;
              var y = Math.sin(radian) * 300 + 600 / 2 ;
              This.data.push([x,y])
          }
         
          path = [
          This.data[0][0],This.data[0][1],
          This.data[1][0],This.data[1][1],
          This.data[2][0],This.data[2][1],
          This.data[3][0],This.data[3][1],
          This.data[4][0],This.data[4][1],
          This.data[5][0],This.data[5][1]
        ]
        This.data.push([This.data[0][0],This.data[0][1]])
      } else if (This.shape === 5) {
        console.log(This.data,path)
        let n = 5;
        This.data = [[300,0]];
        for (var i = 0; i < n; i++) {
          let angle = (360 / n) * (i + 1) * Math.PI / 180;
          let actAngle = angle - Math.PI / 2;
          let x = Math.cos(actAngle) * 300 + 300;
          let y = Math.sin(actAngle) * 300 + 300;
          This.data.push([x,y])

        }
        path = [
          This.data[0][0],This.data[0][1],
          This.data[1][0],This.data[1][1],
          This.data[2][0],This.data[2][1],
          This.data[3][0],This.data[3][1],
          This.data[4][0],This.data[4][1],
          This.data[5][0],This.data[5][1]
        ]
      } else if (This.shape === 4) {
        path = [0,0,600,0,600,600,0,600,0,0]
      }
      This.list.graphics.drawPolygon(path);
      
    }
    this.list.app.stage.addChild(this.list.graphics)
    document.onkeydown = (event) => {
      this.direction = this.snake[1] - this.snake[0] == (this.n = [-1, -(This.across), 1, +(This.across)][(event).keyCode - 37] || this.direction) ? this.direction : this.n;
      if ((event).keyCode === 32) { 
        this.bool = !this.bool;
      } else if ((event).keyCode === 65) {
        if (this.coum) {
          if (this.shape>=4 && this.shape <= 6) {
            if (this.across <= 100 && this.across >= 20) {
              this.bool = true;
              if(this.across % 2 === 1) {
                this.across = parseInt(this.across) + 1;
              }
              begin();
            } else {
              alert('每一列和每一行的长度输入错误，不在规定范围以内');
            }
          } else {
            alert('舞台形状输入不正确！！！');
          }
        }
      }

      
    }
    function random () {
      This.food = ~~(Math.random() * This.across * This.across);
      if (This.shape !== 4) {
        let size = 600/This.across;
        let x = This.food%This.across * size + size/2;
        let y = ~~(This.food/This.across) * size + size/2;
         if ((x - 600)/(0 - 600) - (y - 300)/(300 -300) > 0 > 0) {
            y = y - size/2;
          } else {
            y = y + size/2;
            }
          if ((x - 300)/(300 - 300) - (y - 0)/(600 - 0) > 0) {
            x = x + size/2;
          } else {
            x = x - size/2;
          }
        for (var i = 0; i < This.data.length - 1; i++) {
          let x1 = This.data[i][0];
          let x2 = This.data[i+1][0];
          let y1 = This.data[i][1];
          let y2 = This.data[i+1][1];
          let n = (x-x1)/(x2-x1) - (y-y1)/(y2-y1);
          if (This.shape === 5) {
            if (
              (i === 0 && n > 0) ||
              (i === 1 && n < 0) ||
              (i === 2 && n < 0) ||
              (i === 3 && n > 0) ||
              (i === 4 && n < 0) 
            )  {
              This.a = 3;
            } 
          } else if (This.shape === 6) {
            if (
                  (i === 0 && n > 0) ||
                  (i === 1 && n < 0) ||
                  (i === 2 && n < 0) ||
                  (i === 3 && n > 0) ||
                  (i === 4 && n < 0)  
                  || (i === 5 && n < 0)
                  ){
                  This.a = 3;
                }
          }
        }
      }
          if (This.a === 3) {
            This.a = 1;
            random();
          } else {
            return This.a;
          }
    }
    function begin () {
      This.snake= []
      This.direction= 1
      This.food= This.across * This.across / 2 + This.across/2 + 2;
      This.n= 0
      This.list.graphics.beginFill(16777215, 1 )
      This.list.graphics.lineStyle(1,16777215,1);
      This.list.graphics.drawRect(0, 0, 600, 600)
      This.list.graphics.endFill()
      aaa()
      table();
      This.snake = [];
      This.num = This.across * This.across / 2 + This.across/2;
       for (var i = 1; i < This.length; i++) {
          This.snake.push(This.num--);
        }
      let tiemr = 0;
      let length = This.snake.length;
      This.coum = false;

      let temp = setInterval(() => {
        tiemr += This.speed;
        if (This.bool) {
          This.snake.unshift(This.n = This.snake[0] + This.direction);
          if (This.shape === 5 || This.shape === 6) {
            let size = 600/This.across;
            let x = This.n%This.across * size + size/2;
            let y = ~~(This.n/This.across) * size + size/2;
            if ((x - 600)/(0 - 600) - (y - 300)/(300 -300) > 0) {
                y = y - size/2;
              } else {
                y = y + size/2;
                }
              if ((x - 300)/(300 - 300) - (y - 0)/(600 - 0) > 0) {
                x = x + size/2;
              } else {
                x = x - size/2;
              }
            for (var i = 0; i < This.data.length - 1; i++) {
              let x1 = This.data[i][0];
              let x2 = This.data[i+1][0];
              let y1 = This.data[i][1];
              let y2 = This.data[i+1][1];
              let n = (x-x1)/(x2-x1) - (y-y1)/(y2-y1);
              if (This.shape === 5) {
                if (
                  (i === 0 && n > 0) ||
                  (i === 1 && n < 0) ||
                  (i === 2 && n < 0) ||
                  (i === 3 && n > 0) ||
                  (i === 4 && n < 0) ){
                  This.a = 2;
                }
              } else if (This.shape === 6) {
                 if (
                  (i === 0 && n > 0) ||
                  (i === 1 && n < 0) ||
                  (i === 2 && n < 0) ||
                  (i === 3 && n > 0) ||
                  (i === 4 && n < 0)  
                  || (i === 5 && n < 0)
                  ){
                  This.a = 2;
                }
              }
            }
          }
          if (This.snake.indexOf(This.n, 1) > 0 || This.n < 0 || This.n > This.across * This.across - 1|| This.direction == 1 && This.n % This.across == 0 || This.direction == -1 && This.n % This.across == This.across - 1 
        || This.a == 2
          ) {
            
            console.log('撞死啦')
            This.a = 1;
            This.coum = true;
            if (confirm(`游戏结束，用时：${tiemr/1000}秒，长度：${length}`)) {
              clearInterval(temp)
              return;
            } else {
              clearInterval(temp)
              return;
            }
          }
          draw(This.list.graphics, This.n, 0xFF700B);
          if (This.n == This.food) {//如果吃到食物
          length++;
            // while (This.snake.indexOf(This.food = ~~(Math.random() * This.across * This.across)) >= 0);//如果食物不在蛇的身体上
            
            while (This.snake.indexOf(random()) >= 0);//如果食物不在蛇的身体上
            draw(This.list.graphics, This.food, 0xFF700B)
          } else {
            draw2(This.list.graphics, This.snake.pop(), 0x99CCFF);
          }
        }
      },This.speed)
    }
    // app.render(stage)
  }
}
</script>
<style scoped>
* {
  padding: 0;
  margin: 0;
}
/* canvas {
  width: 800px;
  height: 50px;
} */
/* button {
  margin-top: 10px;
  background-color: #666;
} */
#div {
  /* margin: 0 auto; */
}
#canvas {
  /* position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
        z-index: 999999; */

}
/* #hello {
  height: 100%;
  width: 100%;
} */
#box > .suspend {
  /* position: absolute;
  width: 1000px;
  height: 800px;
  top: 0;
  left: 0;
  text-align: left; */
  background-color: red;
  /* z-index: 999; */
  /* float: left; */
}
</style>
