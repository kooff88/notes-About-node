<!--
 * @Descripttion: 
 * @version: 
 * @Author: zhangpeng
 * @Date: 2020-12-19 08:41:10
 * @LastEditors: zhangpeng
 * @LastEditTime: 2020-12-19 10:08:47
-->
## API服务

### RESTful

1. 简单易懂

2. 可快速搭建

3. 在数据聚合方面有很大劣势: 不同数据在不同表，需要查多接口，接口返回数据很多无用。



### GraphQL

Facebook开发实现的API服务的库，

专注数据聚合，前端需要什么就返回什么数据， 不冗余。

让前端有 “自定义查询” 数据的能力。



官方示例
index.js
```js
var { graphql, buildSchema } = require('graphql');
 
// Construct a schema, using GraphQL schema language
var schema = buildSchema(`
  type Query {
    hello: String
  }
`);
 
// The root provides a resolver function for each API endpoint
var root = {
  hello: () => {
    return 'Hello world!';
  },
};
 

// 做一层封装
module.exports = function(query){
    // Run the GraphQL query '{ hello }' and print out the response
    return graphql(schema, query, root).then((response) => {
    // console.log(response);
        return response;
    });

}
```

request.js
```js
const query = require('./index.js');

app.use(async (ctx) => {
    const res = await query(ctx.query);
    ctx.body = res;
})

```


### 示例

`koa-graphql` 与 `grapgql` 


index.js
```js
const app = new (require('koa'));

const mount = require('koa-mount');
const static = require('koa-static');
const graphqlHttp = require('koa-graphql')


app.use(
    graphqlHttp({
        schema: require('./schema')
    })
)
app.listen(3000);
```

schema.js
```js
const { graphql, buildSchema } = require('graphql')

const schema = buildSchema(`
    type Comment {
        id: Int,
        name:String
    }

    type Query {
        comment:[Comment]
    }
`)

schema.getQueryType().getFields().comment.resolve = () => {
    return [
        {
            id:1,
            name:'大棚'
        }
    ]
}

module.exports = schema;

```