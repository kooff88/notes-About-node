## commonJS 模块规范

石头剪刀布 ， 赢三次 ，退出程序

index.js
```js
var playerAction = process.argv[process.argv.length - 1];

const game = require('./lib');

// const res = game(playerAction);
// console.log(res);

let count = 0
process.stdin.on('data', e => {
    const playerAction = e.toString().trim();

    // console.log(playerAction);
     const res = game(playerAction);
    console.log(res);
    if(res === -1){
        count++
    }

    if(count === 3) {
        console.log('你厉害 我不玩了');
        process.exit();
    }

})

```

./lib.js
```js
module.exports = function (playerAction){
    console.log(playerAction);
    
    var random = Math.random() * 3;
    
    if (random < 1) {
        var computerAction = 'rock';
    } else if (random > 2) {
        var computerAction = 'scissor';
    } else {
        var computerAction = 'paper';
    }
    
    console.log("电脑出了", computerAction)
    
    if (computerAction === playerAction) {
        console.log("平局")
        return 0;
    } else if (
        computerAction === 'rock' && playerAction === 'paper' ||
        computerAction === 'paper' && playerAction === 'scissor' ||
        computerAction === 'scissor' && playerAction === 'rock'
    ) {
        console.log('你赢了')
        return -1;
    } else { 
        console.log('你输了')
        return 1;
    }
}
```