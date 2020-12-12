## Promise

new Promise 对象, 异步操作非常好的 解决方案

```js
// // (function(){
// //     var promise =new Promise(function(resolve,reject){
// //         setTimeout(()=>{
// //             // resolve(3);
// //             reject(new Error('3'))
// //         },300)
// //         // setTimeout(()=>{
// //         //     // resolve();
// //         //     reject(new Error())
// //         // },500)
// //     }).then(res => {
// //         // console.log('res',res)
// //     }).catch(err => {
// //         console.log('err',err)
// //     })
    
// //     setTimeout(()=>{
// //         console.log(promise);
// //     },800)
// // })()


// (function(){
//     var promise = intervew();
//    var promise2  = promise
//         // .then(res => {
//         //     // console.log('smil')
//         //     // throw new Error('refuse');
//         //     return 'accept'
//         // })
//         .then(res => {
//             // return 'accept'
//             return new Promise(function(resolve,reject){
//                 setTimeout(()=>{
//                     resolve('accept');
//                 },400)
//             })

//         })

//     setTimeout(()=>{
//         console.log(promise);
//         console.log(promise2);
//     },800)

//     setTimeout(()=>{
//         console.log(promise);
//         console.log(promise2);
//     },1000)
       
// })()

// function intervew(){
//     return new Promise(function(resolve,reject){
//         setTimeout(function(){
//             if(Math.random() >0){
//                 resolve('success');
//             }else {
//                 reject(new Error('fail'));
//             }
//         },500)
//     })
// }


// (function(){
//     var promise =new Promise(function(resolve,reject){
//         setTimeout(()=>{
//             // resolve(3);
//             reject(new Error('3'))
//         },300)
//         // setTimeout(()=>{
//         //     // resolve();
//         //     reject(new Error())
//         // },500)
//     }).then(res => {
//         // console.log('res',res)
//     }).catch(err => {
//         console.log('err',err)
//     })
    
//     setTimeout(()=>{
//         console.log(promise);
//     },800)
// })()


(function(){
    // var promise = intervew(1)
    //     .then(res => {
    //        return intervew(2);
    //     })
    //     .then(res => {
    //         return intervew(3);
    //     })
    //     .then(res => {
    //         console.log('smile')
    //     })
    //     .catch(err => {
    //         console.log('cry at' + err.round + ' round');
    //     })
    // function intervew(round){
    //     return new Promise(function(resolve,reject){
    //         setTimeout(function(){
    //             if(Math.random() >0.4){
    //                 resolve('success');
    //             }else {
    //                 var error = new Error('fail');
    //                 error.round = round;
    //                 reject(error);
                    
    //             }
    //         },500)
    //     })
    // }
       
    Promise.all([
        intervew('aaa'),
        intervew('bbb')
    ]).then(()=>{
        console.log('smile')
    })
    .catch(err => {
        console.log('cry for'+ err.name)
    })

})()

function intervew(name){
    return new Promise(function(resolve,reject){
        setTimeout(function(){
            if(Math.random() >0.4){
                resolve(name + 'success');
            }else {
                var error = new Error('fail');
                error.name = name;
                reject(error);
            }
        },500)
    })
}


```