#  如何利用Truffle React框架构建完整的智能合约

使用solidity的truffle框架开发智能合约，前端使用react框架，最终完成智能合约从前端到后端，从开发到部署的完整流程。

### 1. 版本需求

* Truffle v5.0.28 (core: 5.0.28)
* Solidity v0.5.0 (solc-js)
* Node v8.11.2
* Web3.js v1.0.0-beta.37

### 2. 项目初始化

1. mkdir -p truffle  
2. cd truffle
3. truffle unbox react

### 3. 合约编写、编译和部署

1. 将[Github truffle-react](https://github.com/jamess010/AIOpen/tree/master/data/blockchain/truffle-react) 目录下的文件拷贝出来。
2. copy ./source/App.js to "./client/src/App.js"
3. copy ./source/truffle-config.js to ./
4. copy ./source/Migration.sol ./source/SimpleStorage.sol to "./contracts"
5. copy ./source/1_initial_migration.js	./source/2_deploy_contracts.js to "./migrations"
6. truffle develop (port: 8545)
7. compile
8. migrate (--reset)

### 4. 启动项目，查看效果
1. cd client && npm start
2. config metamask wallet to private chain on http://localhost:8545
3. visit http://localhost:3000
4. input number xxx in input box, then click "修改" button
5. in wallet , click comfirm button
6. in mainpage ,the The stored value is: xxx(you enter number above)

### 5. 总结
一个完整的覆盖前后端的DAPP实际上就两点，跟传统互联网项目的前后端类似：

合约编写、部署
前端调用
基于以太坊开发DAPP实际上比较简单，重点是在合约的逻辑性、安全性上。从这也可以看出来以太坊生态的强大和完整，便捷完备的开发语言、工具，确实是目前最牛的项目之一。
