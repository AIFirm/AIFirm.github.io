# 使用私有IPFS网络测试 IPFS Python Client

### 1. 部署私有IPFS网络
1. 建立3个节点的IPFS网络，在 docker-compose.yml 文件中写入下面内容：
```
version: '3'
services:
    ipfs-node1:
      image: ipfs/go-ipfs
      restart: unless-stopped
      ports:
       - "7000:4001"
       - "9000:5001"
       - "8080:8080"
      volumes:
       - ./node1:/home

    ipfs-node2:
      image: ipfs/go-ipfs
      restart: unless-stopped
      ports:
       - "7001:4001"
       - "9001:5001"
       - "8081:8080"
      volumes:
       - ./node2:/home

    ipfs-node3:
      image: ipfs/go-ipfs
      restart: unless-stopped
      ports:
       - "7002:4001"
       - "9002:5001"
       - "8082:8080"
      volumes:
       - ./node3:/home
```
2. docker-compose up -d
3. 随便进入一个node进行测试，dokcer-compose exec ipfs-node1 sh
4. 使用 ipfs swarm peers 查看连接状态
5. echo ‘This is node1!’ > node1.txt
6. ipfs add node1.txt 得到hash值 xxx
7. ipfs cat "node1.txt hash xxx" 查看文件

### 2. 使用 IPFS Python Client 测试

1. pip install ipfsapi
2. python 测试：
3. import ipfsapi
4. api = ipfsapi.connect('xxx.xxx.xxx.xxx', 9001)
5. api.cat("node1.txt hash xxx")  
6. api.add("hello.txt")
7. api.id() # 查看 id 信息等
8. api.get("node1.txt hash xxx") # 下载文件
9. api.swarm.peers() # 查看连接状态

### 3. 参考资料

1. [A short trip to Jupyter via the Inter-planetary File System](https://hackernoon.com/a-short-trip-to-jupyter-via-the-inter-planetary-file-system-92c36c1e5b45)

2. [IPFS](https://github.com/jamess010/AIOpen#术语解释)
