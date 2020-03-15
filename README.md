# vue-tic-tac-toe Vue井字棋
Use Vue to rewrite React's Official [Tutorial][react-tutorial]  
使用 Vue 改写 React 的官方[教程][react-tutorial-zh]


## Build local environment 搭建本地环境 

### Install Software 安装软件 

> [Node][node]  
[yarn][yarn]  
[vue-cli][vue-cli]

### Make a new project 创建一个新的项目

>vue create vue-tic-tac-toe  
cd vue-tic-tac-toe

Code for now 目前的代码
>git clone https://github.com/vinzid/vue-tic-tac-toe.git  
git checkout 1-static

### Delete sample files 删除示例文件

Delete all files in the src folder except main.js  
删除 src 文件夹中除了 main.js 以外的其它文件
>cd src  
shopt -s extglob  
rm -rf !(main.js)  

Code for now 目前的代码
>git checkout 2-empty

### Add Static Files 增加静态文件

Add new file to src folder as following  
在 src 文件夹中增加文件如下
>https://github.com/vinzid/vue-tic-tac-toe/blob/3-static/src/App.vue

Create new folder in src  
在 src 增加文件夹
> mkdir components  
cd components

Add files to components folder as following  
在 components 文件夹中增加文件如下
>https://github.com/vinzid/vue-tic-tac-toe/blob/3-static/src/components/Board.vue  
https://github.com/vinzid/vue-tic-tac-toe/blob/3-static/src/components/Square.vue

Start project to see the empty tic-tac-toe field
启动项目可以看到空白的井字格
>cd vue-tic-tac-toe  
yarn serve

Code for now 目前的代码
>git checkout 3-static


## Add Data Handling 增加数据处理

### Add props 增加 props

Pass a prop call value to Square in Board.vue  
在 Board.vue 中传递一个名为 value 的 prop 到 Square
```
<div class="board-row">
  <Square value="1" />
  <Square value="2" />
  <Square value="3" />
</div>
<div class="board-row">
  <Square value="4" />
  <Square value="5" />
  <Square value="6" />
</div>
<div class="board-row">
  <Square value="7" />
  <Square value="8" />
  <Square value="9" />
</div>
```

Add value prop to componnect definition and template in Square.vue  
在 Square.vue 的组建定义和模版中增加 value prop
```
<button class="square">
  {{ value }}
</button>
```
```
export default {
  name: 'Square',
  props: ['value']
}
```

Code for now 目前的代码
>git checkout 4-props

### Add Reactivity 增加交互

Add click event to the botton element to change value  
增加点击事件至按钮元素以更新值
```
<button class="square" @click="setValue()">
```
```
export default {
  name: 'Square',
  //props: ['value'],
  data() {
    return {
      value: null
    }
  },
  methods: {
    setValue() {
      this.value = 'X';
    }
  }
}
```

Code for now  目前的代码
>git checkout 5-click


## Completing the Game 完善游戏

### Lifting Value Up 数值提升

Add click event to Square in Board.vue, So that it can emit  
在 Board.vue 增加点击事件至 Square，以便它能触发
```
<div class="board-row" v-for="row in board">
  <Square v-for="square in row" :square="square" :value="squares[square]" @click="handleClick" />
</div>
```
```
data() {
  return {
    status: 'Next player: X',
    board: [
      [1, 2, 3],
      [4, 5, 6],
      [7, 8, 9]
    ],
    squares: Array(9).fill(null),
  }
},
methods: {
  handleClick(i) {
    const squares = this.squares.slice();
    squares[i] = 'X';
    this.squares = squares;
  }
},
```

Emit click event of Board in click event handler in Square.vue  
在 Square.vue 的点击事件处理器中触发 Board 的点击事件
```
<button class="square" @click="setValue()">
```
```
setValue() {
  this.$emit('click', this.square);
}
```

Code for now  目前的代码
>git checkout 6-emit

### Taking Turns 轮流落子

Add data xIsNext, and switch when click  
增加数据 xIsNext，并在点击时切换
```
data() {
  return {
    next: 'X',
    ...
    xIsNext: true
  }
},
methods: {
  handleClick(i) {
    const squares = this.squares.slice();
    squares[i] = this.xIsNext ? 'X' : 'O';
    this.squares = squares;
    this.xIsNext = !this.xIsNext;
    this.next = this.xIsNext ? 'X' : 'O';
  }
},
```

Code for now 目前的代码
>git checkout 7-alter


### Declaring a Winner 判断胜者

Add function to calculate winner  
增加计算胜者的函数
```
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

Add winner logic  to click handler  
增加点击处理函数的胜者逻辑
```
handleClick(i) {
  const squares = this.squares.slice();
  if (calculateWinner(squares)) {
    alert('Winner was determined!');
    return;
  }
  if (squares[i]){
    alert('Place was taken!');
    return
  }
  squares[i] = this.xIsNext ? 'X' : 'O';
  this.squares = squares;
  const winner = calculateWinner(squares);
  if (winner) {
    this.status = 'Winner: ' + winner;
    return;
  }
  this.xIsNext = !this.xIsNext;
  this.status = `Next player: ${this.xIsNext ? 'X' : 'O'}`;
}
```

Code for now 目前的代码
>git checkout  8-winner


## Adding Time Travel 增加事件旅行

### Storing a History of Moves 保存历史记录

Add history data to App.vue, and add squares prop to Board  
在 App.vue 增加数据 history，并为 Board 增加 prop squares
```
data() {
  return {
    history: [{
      squares: Array(9).fill(null),
    }],
    ...
  }
},
```
```
<Board :squares="history[history.length - 1].squares" @click="handleClick" />
```

Relocate calculateWinner and handleClick from Board.vue to App.vue, and update the latter  
将 calculateWinner 和 handleClick 从 Board.vue 转移到 App.vue，并更新后者
```
handleClick(i) {
  const history = this.history;
  const current = history[history.length - 1]
  const squares = current.squares.slice();
  ...
  squares[i] = this.xIsNext ? 'X' : 'O';
  history.push({
    squares: squares
  });
  ...
}
```

Code for now 目前的代码
>git checkout 9-history

### Showing the Past Moves 展示历史步骤记录

Add stepNumber to App.vue, change squares prop's value of Board, and update stepNumber in click handler of Board
在 App.vue 增加 stepNumber，更新 Board 的 prop squares 的取值，并在 Board 的点击处理函数中更新 stepNumber
```
data() {
    return {
      ...
      stepNumber: 0,
      ...
    }
  },
```
```
<Board :squares="history[this.stepNumber].squares" @click="handleClick" />
```
```
handleClick(i) {
  const history = this.history.slice(0, this.stepNumber + 1);
  ...
  this.history = history.concat([{
    squares: squares
  }]);
  this.stepNumber = history.length;
},
```

Add jumpTo method, and add move list  
增加方法 jumpTo，并增加步骤列表
```
jumpTo(step) {
  if(step === this.stepNumber){
    alert(`On move #${step} already!`);
    return;
  }
  this.stepNumber = step;
  this.xIsNext = (step % 2) === 0;
  this.status = `Next player: ${this.xIsNext ? 'X' : 'O'}`;
}
```
```
<ol>
  <li v-for="(squares, index) in history" :key="index" :class="index === stepNumber ? 'move-on' : ''">
    <button @click="jumpTo(index)">{{ 0 === index ? 'Go to game start' : `Go to move #${index}` }}</button>
  </li>
</ol>
```

Code for now 目前的代码
>git checkout master



[react-tutorial]: https://reactjs.org/tutorial/tutorial.html
[react-tutorial-zh]: https://zh-hans.reactjs.org/tutorial/tutorial.html
[node]: https://nodejs.org
[yarn]: https://yarnpkg.com
[vue-cli]: https://cli.vuejs.org