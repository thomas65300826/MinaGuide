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
``` systemctl --user status mina ```
* 停止运行Mina
``` systemctl --user stop mina ```
* 重启动Mina
``` systemctl --user restart mina ```
* 查看运行日子
``` journalctl --user -u mina -n 1000 -f ```
## 查看
