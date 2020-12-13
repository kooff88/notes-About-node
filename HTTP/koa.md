## koa

### 比express 更极致的 request / response 简化
```js
ctx.status = 200
ctx.body = 'hello world'
```

### 使用async function 实现中间件

有“暂停执行” 功能， 在异步情况下符合洋葱模型



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id ="output" ></div>
    <br/>
    <button id ="rock">石头</button>
    <br/>
    <button id="scissor">剪刀</button>
    <br/>
    <button id="paper">布</button>
</body>
<script>
    const $button = {
        rock : document.getElementById('rock'),
        scissor : document.getElementById('scissor'),
        paper: document.getElementById('paper')
    }

    const $output = document.getElementById('output');

    Object.keys($button).forEach(key => {
        $button[key].addEventListener('click', function(){
            fetch(`http://${location.host}/game?action=${key}`)
            .then(res => {
                return res.text();
            })
            .then(text => {
                $output.innerHTML += text + '<br/>'
            })
        })
    })


</script>
</html>
```

index.js
```js
const fs = require('fs');
const game = require('./game');

const koa = require('koa');
const mount = require('koa-mount');

const app = new koa();

let playerWonCount = 0
let playLastAction = null;
let sameCount = 0;

app.use(
    mount('/favicon.ico',function(ctx){
        ctx.status = 200;
    })
)

const gameKoa = new koa();

app.use(
    mount(
        '/game',
        gameKoa
    )
)

gameKoa.use(
    async function(ctx,next){
    
        if (playerWonCount >= 3 ) {
            ctx.status= 500;
            ctx.body ='我再也不和你玩了';
            return;
        }
        await next();

        if (ctx.playerWon) {
            playerWonCount++
        }

    }
)  
 gameKoa.use(
     async function(ctx,next){
        const query = ctx.query;
        const playAction = query.action;
      
        if (!playAction) {
            ctx.status = 400;
        }

        if (sameCount == 9) {
            ctx.status = 500;
            ctx.body = '我再也不和你玩了'
        }
        if (playAction == playLastAction) {
            sameCount++;
            if (sameCount >= 3) {
                ctx.status = 400;
                ctx.body = '你作弊';
                sameCount = 9;
                return;
            }
        }

        if (playLastAction && playAction == playLastAction){
        }else {
            sameCount = 0;
        }
    
        playLastAction = playAction;
        ctx.playLastAction = playLastAction;
        await next();
    }

)

gameKoa.use(
    async function(ctx, next){
        const gameResult = game(ctx.playLastAction)
                // 洋葱模型 问题

        await new Promise(resolve => {
            ctx.status = 200;
            setTimeout(()=>{
         
                if (gameResult == 0) {
                    ctx.body = "平局"
                }else if (gameResult == 1){
                    ctx.body = "你赢了"
                    ctx.playerWon = true;
                }else {
                    ctx.body = "你输了"
                }
                resolve(ctx);
            },500)
        })
    }
)


app.use(
    mount('/', function(ctx) {
        ctx.body = fs.readFileSync(__dirname + '/index.html', 'utf-8')
    })
)

app.listen(3000);
```

game.js
```
module.exports = function (playerAction){
    var random = Math.random() * 3;
    
    if (random < 1) {
        var computerAction = 'rock';
    } else if (random > 2) {
        var computerAction = 'scissor';
    } else {
        var computerAction = 'paper';
    }
    
    if (computerAction === playerAction) {
        return 0;
    } else if (
        computerAction === 'rock' && playerAction === 'paper' ||
        computerAction === 'paper' && playerAction === 'scissor' ||
        computerAction === 'scissor' && playerAction === 'rock'
    ) {
        return -1;
    } else { 
        return 1;
    }
}
```