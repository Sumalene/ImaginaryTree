---
{"dg-publish":true,"permalink":"/CSAPP计组/csapp_BOMBLAB/","title":"CSAPP-Bomb"}
---


## 概述

> 运行一个二进制文件 bomb，它包括六个"阶段(phase)"，每个阶段要求学生通过 stdin 输入一个特定的字符串。

### 拆弹方法

1. 使用 gdb 或 ddd 调试器，对二进制文件进行反汇编
2. 在每个阶段前设置断点

一共有三个文件: bomb bomb.c RM.md

RM : ```This is an x86-64 bomb for self-study students.```

bomb.c : (仪涵化)
```cpp
/***************************************************************************
 * 邪恶博士的阴险炸弹，1.1版
 * 2011 年，邪恶博士公司版权所有。保留所有权利。
 *
 * 许可证：
 *
 * 邪恶博士公司（发布者）特此授予你（受害者）使用此炸弹（"炸弹"）的明确许可。
 * 受害者）使用此炸弹（本炸弹）的明确许可。 这是一个
 * 有时间限制的许可，在受害者死亡时失效。
 * 清除者对损害、挫折不承担任何责任、
 * 对受害者造成的损害、挫折、精神错乱、虫眼、腕管综合症、失眠或其他
 * 对受害者造成的伤害概不负责。 除非施虐者想邀功、
 * 也就是说。 受害者不得将此炸弹源代码分发给
 受害者不得将此炸弹源代码分发给 * 清除者的任何敌人。 受害者不得调试
 * 逆向工程、运行 "字符串"、反编译、解密或使用任何
 * 其他技术获取炸弹知识并拆除炸弹。 炸弹
 * 处理本程序时不得穿戴防弹服。 程序
 清除者不会为清除者的拙劣*幽默感而道歉。
 * 幽默感而道歉。 在法律禁止使用炸弹的地方，本许可无效。
 * 法律禁止的地方，本许可证无效。
 ***************************************************************************/
/*
 * 自己注意： 记得删除这个文件，这样我的受害者就不会
 * 知道发生了什么事，这样他们就会在一场
 * 然后他们就会被炸得粉身碎骨 -- 邪恶博士
 */

文件 *infile；

int main(int argc, char *argv[])
{
    char *input；

    /* 给自己的提示：记得把这个炸弹移植到 Windows 上，并在上面安装一个漂亮的图形用户界面。
     * 梦幻般的图形用户界面。*/

    /* 运行时没有参数，炸弹会从标准输入端读取输入行。
     * 从标准输入中读取。*/
    if (argc == 1) {
        infile = stdin；
    }

    /* 当运行时有一个参数 <file>, 炸弹从 <file> 读取 * 直到 EOF.
     * 直到 EOF，然后切换到标准输入。因此，在
     * 可以将其拆除字符串添加到 <file> 中，避免重新输入。
     * 避免重新输入。*/
    else if (argc == 2) {
        if (!(infile = fopen(argv[1], "r")) {
            printf("%s: Error: Couldn't open %s\n", argv[0], argv[1])；
            exit(8)；
        }
    }

    /* 你不能使用一个以上的命令行参数来调用炸弹。*/
    else {
        printf("Usage: %s [<input_file>]\n", argv[0])；
        exit(8)；
    }

    /* 执行各种秘密任务，使炸弹更难拆除。*/
    initialize_bomb()；

    printf("Welcome to my fiendish little bomb. You have 6 phases with\n")；
    printf("which to blow yourself up. Have a nice day!\n")；

    /* 嗯...  六个阶段一定比一个阶段更安全！*/
    input = read_line(); /* 获取输入 */
    phase_1(input); /* 运行阶段 */
    phase_defused(); /* 糟糕！ 他们想出来了！
                                      * 让我知道他们是怎么做到的。*/
    printf("Phase 1 defused. How about the next one?\n")；

    /* 第二阶段更难。 没有人会知道
     * 如何拆除这个装置... */
    input = read_line()；
    phase_2(input)；
    phase_defused()；
    printf("That's number 2. Keep going!\n")；

    /* 我想到目前为止这还太简单了。 一些更复杂的代码会
     * 会让人感到困惑。*/
    input = read_line()；
    phase_3(input)；
    phase_defused()；
    printf("Halfway there!\n")；

    /* 哦，是吗？ 你的数学水平如何？ 试试这个俏皮的问题！*/
    input = read_line()；
    phase_4(input)；
    phase_defused()；
    printf("So you got that one. Try this one.\n")；

    /* 我们在内存中转啊转，转到哪里停哪里，炸弹就爆炸了！*/
    input = read_line()；
    phase_5(input)；
    phase_defused()；
    printf("Good work! On to the next...\n")；

    /* 这个阶段将永远不会被使用，因为没有人会通过前面的
     * 前面的阶段。 但为了以防万一，这个阶段要特别难。*/
    input = read_line()；
    phase_6(input)；
    phase_defused()；

    /* 哇，他们成功了！ 但是不是......少了点什么？ 也许
     * 他们忽略了什么？ Mua ha ha ha ha！*/
     * (第七个炸弹)

    return 0；
}
```

gdb基本命令
#### 运行

- `gdb bomb` 使用 gdb 调试可执行文件 `bomb` √
- `r` run，运行程序，遇到断点处停止，等待用户输入下一步指令 √
- `c` continue，继续执行，到下一个断点(或程序结束) √
- `q` quit, 退出 gdb 调试 √
- `si` 单指令执行，即每次只执行一条指令，结合"交互模式下直接回车的作用是重复上一指令，对于单步调试非常方便" √
- `n` next，单步跟踪程序，遇到函数调用时，不会进入函数体内部
- `s` step，单步调试，遇到函数调用时，会进入函数体内部
- `until` 当你厌倦了在一个循环体内部单步跟踪时，这个命令可以运行程序直到退出循环体
- `until + 行号` 运行至行号处

#### 设置断点

- `b n` break n，在第 n 行设置断点 √
- `b func` 在函数 func() 的入口处设置断点 √
- `b *地址值` 在地址值处设置断点，例如，`b *0x401460` √
- `i b` info b, 显示当前程序的断点设置情况，会给出各个断点的序号，类型等信息 √
- `delete 断点号n` 删除第 n 个断点(从 `info b` 中得出断点序号) √
- `disable 断点号n` 暂停第 n 个断点
- `enable 断点号n` 开启第 n 个断点
- `delete breakpoints` 删除所有断点

#### 分割窗口

- `layout regs` 显示寄存器和反汇编窗口 √ 
- `layout asm` 显示反汇编窗口 √
- `Ctrl + X + A` 即可关闭当前活动的窗口
- `Ctrl + X + O` 可以在不同的窗口间进行切换
#### 具体的操作符的含义

1. 常量以符号`$`开头：`$-42`， `$0x15213`（一定要注意十进制还是十六进制）
2. 寄存器以符号`%`开头：`%esi`，`%rax`（可能存的是值或者地址）
3. 内存地址用括号括起来：如(`%rbx`)，括号实际上是去寻址的意思

#### 不同寄存器的常用方法
- 自行读书,请(100,200)

## 第二阶段 栈元素的比较

设置断点,进入第二阶段,要求我们输入六个数字
![Pasted image 20231106194135.png](/img/user/Pasted%20image%2020231106194135.png)
`lea`（Load Effective Address）指令将一个地址的有效偏移量加载到目标寄存器中，而不是将实际的内存数据加载到寄存器中。

`lea 0x10(%rsi), %rax`，它的含义是将寄存器 `rsi` 中的值加上 `0x10`（十六进制中的十六）作为偏移，然后将结果存储在 `rax` 寄存器中。

在 x86 汇编语言中，`%rsp` 寄存器是栈指针寄存器，用于指示栈的顶部地址。栈是一种后进先出（LIFO）的数据结构，用于存储函数调用期间的局部变量和返回地址等信息。

在汇编代码中，通过对 `%rsp` 的操作来分配和释放栈空间，例如使用 `sub` 指令减少 `%rsp` 的值，可以为新的局部变量或其他数据分配空间。

1. **指令语境：** 在汇编代码中，看到对 `%rsp` 的操作，比如 `sub`（减法）或 `add`（加法）操作，通常表明它在栈操作中扮演着重要角色。
    
2. **函数调用：** 在函数的开头和结尾，可能会看到对 `%rsp` 的操作，这用于函数的栈帧（stack frame）分配和清理，即在函数调用过程中用于存储局部变量、参数和返回地址的空间。
![Pasted image 20231106195348.png](/img/user/Pasted%20image%2020231106195348.png)

首先栈顶指针寄存器 `%rsp` 减去 `0x18` ，在内存中开辟了一块 24 字节的栈空间  
图中我们能看到，调用 `read_six_numbers` 前先将栈顶指针减去了 `0x28` ，然后把栈顶指针值赋给 `%rsi`

当栈指针 `%rsp` 被减去一个值时，实际上是为新数据或变量在栈中分配内存空间的过程。在 x86 架构中，栈的增长方向是向低地址方向，所以当栈指针被减小时，栈会往低地址扩展，分配更多的空间。

 `0x401480` 我们遇见了一个内存地址 `0x4025c3` ，使用 `x/s 0x4025c3` 打印一下内容
![Pasted image 20231106202132.png](/img/user/Pasted%20image%2020231106202132.png)

再结合前面的函数名为 `read_six_numbers` ，可以知道我们需要输入六个整型数，且用空格分开  
`0x40148f` 处指令 `cmp $0x5,%eax` 会判断输入数字是否是六个，不是六个直接引爆炸弹，故我们先使用 `q` 退出当前调试，重新输入六个整数

#### 一些汇编语句与实际命令的转换 :  不需要了

#### 显示内容

- `x/[count][format] [address]` 打印内存值，从所给地址(address)处开始，以指定格式(format)显示 count 个值，例如 `x/100i foo` 反汇编并打印出从 foo 处开始的 100 条指令 √  
    常见 format 有 `d` decimal, `x` hex, `t` binary, `f` floating point, `i` instruction, `c` character, `s` string(以 8bit 字符串形式显示数据)
- `i r 寄存器名` , info registers 寄存器名，查看当前某个寄存器的内容，寄存器名前不需要 `%` ，例如，`i r rsi` √
- `Ctrl + L` 清屏，由于 gdb 没有专门的清屏命令，所以用 Linux 自带的。(有的时候不退出 gdb 重新 r 会导致显示内容覆盖，使用这个命令清屏，有可能不好使) √


#### 源码逆向

![Pasted image 20231118000151.png](/img/user/Pasted%20image%2020231118000151.png)
关键:1.栈顶是1(cmpl那个); 2.下一个元素等于倍增自己(add 那个)  3.逆序入栈(看方向)

```cpp
int phase_2(int input_array[]) {
    int i;
    int* rsp = &input_array[0];
    if (input_array[0] != 1) std::cout << "explode_bomb();";
    for (i = 1; i < 6; i++) {
        if (input_array[i] != 2 * input_array[i - 1])
            std::cout << "explode_bomb();";
    }
    return 0;
}
```

更新ans: 1 2 4 ....

---


##  第四阶段 递归

![Pasted image 20231118013212.png](/img/user/Pasted%20image%2020231118013212.png)


如果输入的数字不超过两个，程序会引爆炸弹。

func4 逆向
```cpp
int func4(int ecx, int edi) {
    if (ecx <= edi) {
        if (ecx == edi) {
            return 7;
        } else {
            return func4(ecx + 1, edi) * func4(ecx + 1, edi) - x;
        }
    } else {
        return func4(ecx - 1, edi) + func4(ecx - 1, edi) + x;
    }
}

```

## 第五阶段 缓冲区加密

"Maduiersnfotvbyl所以你认为你可以用Ctrl -C来阻止炸弹，是吗？" 

- 对于 "f"，对应的 ASCII 码是 0x66，取低四位得到 "6"。
- 对于 "l"，对应的 ASCII 码是 0x6C，取低四位得到 "C"。
- 对于 "y"，对应的 ASCII 码是 0x79，取低四位得到 "9"。
- 对于 "e"，对应的 ASCII 码是 0x65，取低四位得到 "5"。
- 对于 "r"，对应的 ASCII 码是 0x72，取低四位得到 "2"。
- 对于 "s"，对应的 ASCII 码是 0x73，取低四位得到 "3"。

所以，用户需要输入的字符 x 应该是将 "6532" 这四个数字拼接起来，得到的结果与 "flyers" 对应的数字串相匹配。在这个例子中，选择的小写字母 "ionefg" 分别对应数字 "6532"。

## 六与七 链表与树

![Pasted image 20231118021600.png](/img/user/Pasted%20image%2020231118021600.png)

理解就好.