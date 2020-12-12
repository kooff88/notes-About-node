## 非阻塞I/O

```js
const glob = require('glob');


/**
// 阻塞I/O
*/
let result = null;
console.time('glob');
result = glob.sync(__dirname + '/**/*'); 
console.timeEnd('glob');

console.log('resule',result)



/**
// 非阻塞I/O
*/
var result = null;
console.time('glob');
glob(__dirname + '/**/*',function(err, res){
    result = res;
    console.log('resule', result);
});
console.timeEnd('glob');
console.log('1+1',1+1);

```