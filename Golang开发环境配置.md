## 安装Go
**MAC OS**
```
brew install go
brew install gpg
```
* 更新终端初始化脚本(如`.bashrc`或`.zshrc`)，添加以下内容
```sh
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
export GOPROXY=https://mirrors.aliyun.com/goproxy/
```
**CentOS**
```
sudo yum group install "Development Tools"
sudo yum install -y wget libpng12
sudo rm -rf /usr/local/go
wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz
sudo tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```
* 更新终端初始化脚本(如`.bashrc`或`.zshrc`)，添加以下内容
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:/usr/local/go/bin
```
## GoLand
