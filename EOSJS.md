## 1. 背景
前面的章节中，我们一直都是使用命令行的方式去调用智能合约，在实际的使用中，还是需要提供界面给用户才能够完整的实现软件的功能。我们一般的软件架构都会有前端和后端，在区块链项目上，区块链本身就可以作为数据载体，这次我们就来实现一个简单的后端访问链上数据的功能。

## 2. EOSJS简介
EOSJS是EOSIO官方提供的通过NodeJS访问链上合约的库，不同版本的EOSIO区块链需要使用不同版本的EOSJS来访问，以下是官方提供的列表（通过NPM列中的命令就能安装对应的版本）：
|eosjs|NPM|EOSIO|Docker Hub|
| :--: | :--: | :--: | :--: |
|tag: 14.x.x|npm install eosjs (version 14)|tag: v1.0.3|eosio/eos:v1.0.3|
|tag: 13.x.x|npm install eosjs (version 13)|tag: dawn-v4.2.0|eosio/eos:20180526|
|tag: 12.x.x|npm install eosjs (version 12)|tag: dawn-v4.1.0|eosio/eos:20180519|
|tag: 11.x.x|npm install eosjs@dawn4 (version 11)|tag: dawn-v4.0.0|eosio/eos:dawn-v4.0.0|
|tag: 9.x.x|npm install eosjs@dawn3 (version 9)|tag: DAWN-2018-04-23-ALPHA|eosio/eos:DAWN-2018-04-23-ALPHA|
|tag: 8.x.x|npm install eosjs@8 (version 8 )| tag: dawn-v3.0.0|eosio/eos:dawn3x|
|branch: dawn2|npm install eosjs|branch: dawn-2.x|eosio/eos:dawn2x|

## 3. EOSJS例子
1. 调用上一章的vehicle合约
```
Eos = require('eosjs')

keyProvider = '5KaT5XKr3EFH3ai2zVcK3qH9U4cq4UFv2P5jwEWtoeqHZqPLE3R'   //下面vehicle帐号对应的私钥
eos = Eos({keyProvider})

eos.contract('vehicle').then(ctr => {
        ctr.add('5','Honda','white','3',{authorization:['vehicle']})    //调用vehicle合约的add方法
})
```

2. 查询上一章的vehicle合约数据
```
Eos = require('eosjs')

keyProvider = '5KaT5XKr3EFH3ai2zVcK3qH9U4cq4UFv2P5jwEWtoeqHZqPLE3R'
eos = Eos({keyProvider})

eos.getTableRows({"scope":"vehicle", "code":"vehicle", "table":"cars", "json": true})
          .then(result => {
	  	let rows = result.rows
          	let len = rows.length
          	for(let i = 0; i < len; i ++){
          	  var id = result.rows[i].id
          	  var type = result.rows[i].type
          	  var color = result.rows[i].color
          	  var year = result.rows[i].year

		  console.log("id = " + id + "; type = " + type + "; color = " + color + "; year = " + year);
	  	}
	  });
```

## 4. 部署API
剩下的工作就是通过NodeJS搭建Restful API服务器，让服务器去调用链上合约的功能，提供API给前端使用，和传统的开发一样的做法了，在这里就不多描述了。

P.S. 当然，也可以直接通过前端HTML页面直接引入EOSJS库直接访问链上数据。

## 5. 参考文献
[EOSJS 官方Github地址](https://github.com/EOSIO/eosjs)
