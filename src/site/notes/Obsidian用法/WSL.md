---
{"dg-publish":true,"permalink":"/Obsidian用法/WSL/","title":"测试"}
---

1.在任务栏搜索栏中搜索“命令提示符”，右键单击顶部结果，然后选择“以管理员身份运行”选项。

2.键入以下命令以查看所有正在运行的WSL发行版，然后按Enter：

wsl --list --verbose

3.输入以下命令以关闭Linux发行版，然后按Enter：

wsl -t DISTRO-NAME

在命令中，确保将DISTRO-NAME替换为要关闭的发行版的名称，例如，wsl -t Ubuntu-20.04。

4.(可选)键入以下命令以确认发行版不再运行，然后按Enter：

wsl --list --verbose

完成这些步骤后，在Linux 2的Windows子系统上运行的发行版将正常关闭。