## cluster

配合 `apache` 压力测试工具 `ab`

app.js
```js
const fs = require('fs');
const http = require('http');

http.createServer(function (req, res) {
    res.writeHead(200, { 'content-type': 'text/html' });

    res.end(fs.readFileSync(__dirname + '/index.htm', 'utf-8'));

}).listen(3002, () => {
    console.log('listened 3002')
})

```

index.htm
```htm
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div>页面测试</div>
</body>

</html>
```



index.js
```js
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
    for (let i = 0; i < os.cpus().length / 2; i++) {
        cluster.fork();
    }

} else {
    require('./app');
}

```