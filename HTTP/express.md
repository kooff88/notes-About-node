## express 框架

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
const http = require('http');
const fs = require('fs');
const url = require('url')
const querystring = require('querystring');
const express =require('express');

const app = express();
const game = require('./game');


app.get('/favicon.ico', function(req,res){
    res.status(200)
    return;
})

let playerWonCount = 0
let playLastAction = null;
let sameCount = 0;

app.get('/game', function(req,res,next){
    
    if (playerWonCount >= 3 || sameCount == 9) {
        res.status(500);
        res.send('我再也不和你玩了');
        return;
    }
    next();

    if (res.playerWon) {
        playerWonCount++
    }

},function(req,res,next){
    
    const query = req.query;
    const playAction = query.action;
  
    if (playLastAction && playAction == playLastAction){
        sameCount++;
    }else {
        sameCount = 0;
    }
    playLastAction = playAction;
    res.playLastAction = playLastAction;
    next();
    },
    function(req,res){
        const gameResult = game(res.playLastAction)
        console.log(gameResult)
        // res.status(200);
        if (sameCount >= 3) {
            res.status(400);
            res.send('你作弊')
            sameCount = 9;
            return;
        }
       
    
        if (gameResult == 0) {
            res.send('平局')
        }else if (gameResult == 1){

            // 洋葱模型 问题
            setTimeout(()=>{
                res.send('你赢了')
                res.playerWon = true;
            },500)
           
        }else {
            res.send('你输了')
        }
    
    }

)

app.get('/', function(req,res){
    res.send(fs.readFileSync(__dirname + '/index.html', 'utf-8'))
})


app.listen(3000);
```


game.js
```js
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