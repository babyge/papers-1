堆栈管理字,用于管理堆栈中的数据

DUP (n --> n n)
复制栈顶元素

SWAP ( n1 n2 --> n2 n1)
交换栈顶上两元素

DROP (n --> )
移去栈顶元素

OVER (n1 n2 --> n1 n2 n1)
复制次栈顶元素

TUCK (n1 n2 --> n2 n1 n2)
复制栈顶元素到次栈顶之下，相当于SWAP OVER

ROT (n1 n2 n3 --->n2 n3 n1)
旋转堆栈上的三个元素，原来的第三个元素变成了第一个元素

-ROT (n1 n2 n3 --> n3 n1 n2)
反向旋转堆栈顶部的三个元素，栈顶元素被旋转到第三个位置

NIP (n1 n2 --> n2)
从栈顶上移去第二个元素，等效于SWAP DROP

2DUP (n1 n2 --> n1 n2 n1 n2)
复制栈顶的两个元素

2SWAP (n1 n2 n3 n4 -->n3 n4 n1 n2)
把栈顶的两个元素和第三个第四个元素交换

2DROP (n1 n2 --> )
从栈顶上去掉栈顶的两个元素

PICK
把从栈顶开始算的第n个位置复制到栈顶，栈顶为0.
0 PICK 等效于 DUP
1 PICK 等效于 OVER

ROLL
旋转位置n到栈顶，n必须大于0
1 ROLL 相当于SWAP
2 ROLL 等效于ROT

2. 其他forth字

MOD (n1 n2 --> n3)
n1除以n2并把余数n3留在堆栈上

/MOD (n1 n2 --> n3 n4)
n1 除以n2，n3为余数，n4为商

MIN (n1 n2 --> n3)
把n1和n2之中最小的一个放到栈顶

MAX(n1 n2 --> n3)
把n1和n2之中大的项放到栈顶

NEGATE (n1 --> -n1)
改变n1的符号

ABS 
取栈顶的绝对值

2* 
通过算术移位把n1乘2 

2/ 
通过算术移位把n1除以2

U2/
执行16位逻辑右移

(8*
通过3位位移实现将栈顶乘以8) - SP-Forth不支持

1+
栈顶元素增1

1-
栈顶元素减1 

2+
栈顶元素加2

2-
栈顶元素减2

3. 定义forth字2
: <name>
... 
;

变量
variable my.name

! (n addr)：把值n存入addr变量中
@ (addr --> n): 读取n的值到堆栈上

堆栈变量

系统变量SP0包含有一个空的堆栈的堆栈指针。

depth: 返回堆栈的元素个数
 
常量
constant 


forth冒号定义
： 用于在字典中创建一个新字

数组

allot在字典中分配n个字节

返回栈
>R: 从参数栈出弹出顶层元素，压入返回栈 
R>: 从返回栈中弹出参数压入参数栈

CODE字

Forth字典

表

字符和字节数据

查找字典地址

名字字段

分支指令和循环

if else then 
do loop
begin until
begin while repeat
begin again

=
<>
<=
>=
0<
0>
0=
0<>
0<=
0>=
U<
U>
U<=
U>=

Forth逻辑操作

and

or

xor

leave
在do中退出循环

until循环

while循环