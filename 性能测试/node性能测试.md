<!--
 * @Descripttion: 
 * @version: 
 * @Author: zhangpeng
 * @Date: 2020-12-19 20:59:48
 * @LastEditors: zhangpeng
 * @LastEditTime: 2020-12-19 21:14:28
-->
## node 性能测试

使用  node 自带 profile 工具

第一步： node --prof index.js

第二步： 另一终端跑 压力测试

第三步：  node --prof-process xxxxxxxxx.log  > profile.txt



### 工具

chrome 调试： node --inspect-brk index.js

clinic.js 