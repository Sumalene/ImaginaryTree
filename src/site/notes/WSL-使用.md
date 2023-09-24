---
{"dg-publish":true,"permalink":"/WSL-使用/","title":"HomePage"}
---

## WSL的使用

```
wsl --install 
```
打开powershell，输入  wsl --install 

> 等待安装;重启电脑

> 输入用户名以及密码(密码不在命令行中显示,请注意)

•若要更改安装的发行版，请输入：wsl --install -d <Distribution Name>。 将 <Distribution Name> 替换为要安装的发行版的名称。

•若要查看可通过在线商店下载的可用 Linux 发行版列表，请输入：wsl --list --online 或 wsl -l -o。

•若要在初始安装后安装其他 Linux 发行版，还可使用命令：wsl --install -d <Distribution Name>。

---

•gcc与g++分别是linux默认的c和c++编译器。gcc/g++在执行编译的时候一般有下面4步：

•预处理，生成.i的文件 [预处理器]

•将预处理后的文件转换成汇编语言，生成文件.s [编译器]

•由汇编变为目标代码（机器代码）生成.o的文件 [汇编器]

•连接目标代码，生成可执行程序 [链接器ld]

•Ubuntu缺省情况下，并没有提供C/C++的编译环境，因此还需要手动安装,Ubuntu shell 下输入： sudo apt install build-essential

- 装cpp的,sudo apt install g++

> nano新建cpp文件,co E cx

•gcc hello.c –o hello (g++ hello.cpp -o hello)

•./hello

Hello World