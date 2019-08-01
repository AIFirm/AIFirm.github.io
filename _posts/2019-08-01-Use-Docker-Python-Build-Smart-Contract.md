# 使用Docker Python 构建智能合约

使用solidity的truffle框架开发智能合约，使用Docker Python调用合约。

### 1. 合约编写、编译和部署

1. 将 https://github.com/jamess010/AIOpen/tree/master/data/blockchain/docker-web3-python 内容拷贝下来
2. cd ./docker-web3-python
3. mkdir - p smartcontract
4. cd ./smartcontract
5. truffle init (port: 9545)
6. truffle compile
7. truffle migrate (--reset)

### 2. 使用 jupyter notebook 进行 python 调用
1. 打开浏览器，输入 http://localhost:8828
2. 打开 test_modelrepo.ipynb
3. 按照 notebook 步骤进行
