# 使用Python truffle 构建完整的智能合约
使用solidity的truffle框架开发智能合约，使用Python调用合约。


### 1. 合约编写、编译和部署

1. git clone https://github.com/jamess010/AIonChain
2. cd ./AIonChain && cd ./master/smartcontract
3. truffle develop (port: 9545)
4. compile
5. migrate (--reset)

### 2. 使用 python 调用
1. 安装 [web3.py](https://github.com/ethereum/web3.py) 或者使用 docker web3.py， 参见：[使用Docker Python 构建智能合约](https://www.aifirm.org/2019-08-01-Use-Docker-Python-Build-Smart-Contract/)
2. cd ./notebook
3. 将合约的 abi 编辑到 ./abis/ModelRepo.abi
4. jupyter notebook
5. 在 notebook 中打开 test_modelrepo.ipynb
6. 更改 HTTPProvider 的地址
7. 合约地址放到 contract_address 中
