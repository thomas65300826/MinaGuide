# Mina 节点部署指导
本篇指导是写给想要部署Mina节点参与Testworld测试网的。

## 节点要求:
1. 服务器要求:
```
CPU: 至少 8 核 (或者 8 vCPU)
RAM: 至少 16 Gb (推荐 32Gb或者以上)
操作系统: Ubuntu 18.04
```
2. 需要收到Mina的邀请邮件，内含私钥下载链接。

## 租用服务器
* 谷歌云：https://console.cloud.google.com/
* 亚马逊云： https://aws.amazon.com/

## 下载并安装Mina
```
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl mina-testnet-postake-medium-curves=0.2.6-5c08d6d --allow-downgrades
```
使用以下命令查看Mina版本
```
coda version
```
可以在控制台看到以下结果：
```
read Commit [DIRTY]5c08d6d51111d31269e77f3c62a544b3262ee4c8 on branch HEAD.
```
## 设置私钥

1. 创建私钥文件夹并下载私钥

```<keypair-download-url>```在你收到的邀请邮件里面有。

```
mkdir ~/keys
wget -O ~/keys/new-keys.zip <keypair-download-url>
```
2. 解压缩私钥

```
cd ~/keys
unzip new-keys.zip
```

3. 重命名私钥并修改权限
```
mv <testworld-keypair> my-wallet
mv <testworld-keypair>.pub my-wallet.pub
chmod 700 ~/keys
chmod 600 ~/keys/my-wallet
```
其中，替换```<testworld-keypair>```为你解压缩出来的私钥文件，替换```<testworld-keypair>.pub```为你解压缩出来的.pub文件.

## 获取网络信息

1. 下载Peer列表
```
wget -O ~/peers.txt https://raw.githubusercontent.com/MinaProtocol/coda-automation/bug-bounty-net/terraform/testnets/testworld/peers.txt
```
2. 设置节点
首先创建环境文件
```
vi ~/.mina-env
```
在文件里写入以下信息，并把邀请邮件中的密码替换进去
```
CODA_PRIVKEY_PASS="邀请邮件中的密码"
EXTRA_FLAGS=" -file-log-level Debug -super-catchup"
```
关闭文件，并输入以下命令：
```
systemctl --user daemon-reload
systemctl --user start mina
systemctl --user enable mina
sudo loginctl enable-linger
```
以下命令可以用来跟Mina交互：
* 查看Mina运行状态
``` 
systemctl --user status mina 
```
* 停止运行Mina
``` 
systemctl --user stop mina 
```
* 重启动Mina
``` 
systemctl --user restart mina 
```
* 查看运行日子
``` 
journalctl --user -u mina -n 1000 -f 
```
## 查看节点连接状态
使用以下命令查看连接状态
```
coda client status
```
很可能会看到以下信息
```
...
Peers:                                         Total: 4 (...)
...
Sync Status:                                   Bootstrap
```
再等待一段时间，你就可以看到节点同步的信号。
```
Sync Status: Synced
```

## 导入账号
使用以下命令导入之前下载的私钥
```
coda accounts import -privkey-path ~/keys/my-wallet
```
输入密码后，你会看到以下输出信息
```
Imported account!
Public key: B62qjaA4N9843FKM5FZk1HmeuDiojG42cbCDyZeUDQVjycULte9PFkC
```
将Mina的地址写入环境变量,替换 ```<YOUR-PUBLIC-KEY> ```为你的公钥地址。
```
export MINA_PUBLIC_KEY=<YOUR-PUBLIC-KEY>
export CODA_PUBLIC_KEY=<YOUR-PUBLIC-KEY>
```
## 发送转账
1. 先解锁你的账户
```
coda accounts unlock -public-key $MINA_PUBLIC_KEY
```
2. 使用以下命令发送转账
```
coda client send-payment \
  -amount 0.5 \
  -receiver B62qndJi5mnRoBZ8SAYDM1oR2SgAk5WpZC8hGpJUZ4e64kDHGbFMeLJ \
  -fee 0.1 \
  -sender $MINA_PUBLIC_KEY
 ```
* ```amount``` 为你发送的Mina数量
* ```receiver``` 为对方的地址
* ```fee``` 为你为这笔转账支付的网络费用
* ```sender``` 为你自己的公钥地址
发送完转账，你会看到类似以下的输出信息
```
Dispatched payment with ID 3XCgvAHLAqz9VVbU7an7f2L5ffJtZoFega7jZpVJrPCYA4j5HEmUAx51BCeMc232eBWVz6q9t62Kp2cNvQZoNCSGqJ1rrJpXFqMN6NQe7x987sAC2Sd6wu9Vbs9xSr8g1AkjJoB65v3suPsaCcvvCjyUvUs8c3eVRucH4doa2onGj41pjxT53y5ZkmGaPmPnpWzdJt4YJBnDRW1GcJeyqj61GKWcvvrV6KcGD25VEeHQBfhGppZc7ewVwi3vcUQR7QFFs15bMwA4oZDEfzSbnr1ECoiZGy61m5LX7afwFaviyUwjphtrzoPbQ2QAZ2w2ypnVUrcJ9oUT4y4dvDJ5vkUDazRdGxjAA6Cz86bJqqgfMHdMFqpkmLxCdLbj2Nq3Ar2VpPVvfn2kdKoxwmAGqWCiVhqYbTvHkyZSc4n3siGTEpTGAK9usPnBnqLi53Z2bPPaJ3PuZTMgmdZYrRv4UPxztRtmyBz2HdQSnH8vbxurLkyxK6yEwS23JSZWToccM83sx2hAAABNynBVuxagL8aNZF99k3LKX6E581uSVSw5DAJ2S198DvZHXD53QvjcDGpvB9jYUpofkk1aPvtW7QZkcofBYruePM7kCHjKvbDXSw2CV5brHVv5ZBV9DuUcuFHfcYAA2TVuDtFeNLBjxDumiBASgaLvcdzGiFvSqqnzmS9MBXxYybQcmmz1WuKZHjgqph99XVEapwTsYfZGi1T8ApahcWc5EX9
Receipt chain hash is now A3gpLyBJGvcpMXny2DsHjvE5GaNFn2bbpLLQqTCHuY3Nd7sqy8vDbM6qHTwHt8tcfqqBkd36LuV4CC6hVH6YsmRqRp4Lzx77WnN9gnRX7ceeXdCQUVB7B2uMo3oCYxfdpU5Q2f2KzJQ46
```
## 设置SNARK工作
使用以下命令可以设置SNARk工人
```
coda client set-snark-work-fee <FEE>
coda client set-snark-worker -address $MINA_PUBLIC_KEY
```
其中```FEE```为你设置的SNARK的价格.


## 其他信息链接
* Mina区块浏览器 - minaexplorer.com
* 官方网站 - minaprotocol.com
