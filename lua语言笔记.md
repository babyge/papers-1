Lua中，每条语句结束的分号是可选的

在交互模式下，dofile函数可以加载一个文件
dofile("lib1.lua")

## 全局变量
全局变量不需要声明。直接给一个变量赋值就定义了全局变量。
如果想删除一个全局变量，就给它赋一个nil值就好了

## 保留字：以下字符为Lua的保留字，不能当作标识符。
and        break      do         else       elseif
end        false      for        function   if
in         local      nil        not        or
repeat     return     then       true       until
while

Lua是大小写敏感的
单行注释：--
多行注释: --[[ --]]

## 类型和值
Lua有8种基本类型：nil, boolean, number, string, userdata, function, thread和table

type函数可以用来获取变量或值的类型

boolean:布尔值有两种取值，true和false，除了false和nil外，其他值都相当于true.
number: Lua没有整数只支持浮点数。
strings：是字符的序列，是8位字节，不可修改。自动分配内存和释放。
转义序列：
\a bell
\b back space               -- 后退
\f form feed                -- 换页
\n newline                  -- 换行
\r carriage return          -- 回车
\t horizontal tab           -- 制表
\v vertical tab
\\ backslash                 -- "\"
\" double quote             -- 双引号
\' single quote             -- 单引号
\[ left square bracket      -- 左中括号
\] right square bracket     -- 右中括号

可以使用单引号，也可以使用双引号，还可以使用[[ ]]。[[ ]]不解释转义序列
tonumber()函数将字符串转成数字，数字转字符串用tostring()
function: 函数可以保存在变量里。Lua可以调用Lua或者是C编写的函数。
userdata：可以将C的数据存放在变量中

# 3.表达式

## 3.1算术运算符
二元运算符：+ - * / ^  (加减乘除幂)
一元运算符：-  (负值)
这些运算符的操作数都是实数。

## 3.2关系运算符
<      >      <=     >=     ==     ~=
~=不等于

## 3.3 逻辑运算符
and    or     not
逻辑运算符认为false和nil是假（false），其他为真，0也是true.
and和or的运算结果不是true和false，而是和它的两个操作数相关。
a and b       -- 如果a为false，则返回a，否则返回b
a or  b        -- 如果a为true，则返回a，否则返回b

not的结果只返回false或者true
一个很实用的技巧：如果x为false或者nil则给x赋初始值v
x = x or v
等价于
if not x then
    x = v
end
and的优先级比or高。
 
C语言中的三元运算符
a ? b : c
在Lua中可以这样实现：
(a and b) or c

## 3.4 连接运算符
..         --两个点
字符串连接，如果操作数为数字，Lua将数字转成字符串。

## 3.5 优先级

从高到低的顺序：
^
not    - (unary)
*      /
+      -
..
<      >      <=     >=     ~=     ==
and
or

## 3.6 表的构造
构造器是创建和初始化表的表达式。表是Lua特有的功能强大的东西。最简单的构造函数是{}，用来创建一个空表。可以直接初始化数组:
days = {"Sunday", "Monday", "Tuesday", "Wednesday",
              "Thursday", "Friday", "Saturday"}
Lua将"Sunday"初始化days[1]（第一个元素索引为1），用"Monday"初始化days[2]...
print(days[4])       --> Wednesday

如果想初始化一个表作为record使用可以这样：
a = {x=0, y=0}       <-->       a = {}; a.x=0; a.y=0

list风格初始化和record风格初始化是这种一般初始化的特例:
{x=0, y=0}        <-->       {["x"]=0, ["y"]=0}
{"red", "green", "blue"}        <-->
                  {[1]="red", [2]="green", [3]="blue"}
如果真的想要数组下标从0开始：
days = {[0]="Sunday", "Monday", "Tuesday", "Wednesday",
              "Thursday", "Friday", "Saturday"}
注意：不推荐数组下标从0开始，否则很多标准库不能使用。
在构造函数的最后的","是可选的，可以方便以后的扩展。
a = {[1]="red", [2]="green", [3]="blue",}
在构造函数中域分隔符逗号（","）可以用分号（";"）替代，通常我们使用分号用来分割不同类型的表元素。
{x=10, y=45; "one", "two", "three"}

# 4. 语句
## 4.1 赋值语句
Lua可以对多个变量同时赋值，变量列表和值列表的各个元素用逗号分开，赋值语句右边的值会依次赋给左边的变量。
a, b = 10, 2*x       <-->       a=10; b=2*x
遇到赋值语句Lua会先计算右边所有的值然后再执行赋值操作，所以我们可以这样进行交换变量的值：
x, y = y, x                     -- swap 'x' for 'y'
a[i], a[j] = a[j], a[i]         -- swap 'a[i]' for 'a[i]'
当变量个数和值的个数不一致时，Lua会一直以变量个数为基础采取以下策略：
a. 变量个数 > 值的个数             按变量个数补足nil
b. 变量个数 < 值的个数             多余的值会被忽略
## 4.2 局部变量和代码块
local定义局部变量
do ... end定义代码块

## 4.3 流程控制语句
控制结构的条件表达式结果可以是任何值，Lua认为false和nil为假，其他值为真。
### if语句，有三种形式：

```lua
if conditions then
    then-part
end;
 
if conditions then
    then-part
else
    else-part
end;
 
if conditions then
    then-part
elseif conditions then
    elseif-part
..            --->多个elseif
else
    else-part
end;
```
### while语句：

```lua
while condition do
    statements;
end;
```
### repeat-until语句：
```lua
repeat
    statements;
until conditions;
```

## 4.4 break和return语句

break语句用来退出当前循环（for、repeat、while）。在循环外部不可以使用。
return用来从函数返回结果，当一个函数自然结束时，结尾会有一个默认的return。（这种函数类似pascal的过程（procedure））
Lua语法要求break和return只能出现在block的结尾一句（也就是说：作为chunk的最后一句，或者在end之前，或者else前，或者until前），例如：
local i = 1
while a[i] do
    if a[i] == v then break end
    i = i + 1
end
有时候为了调试或者其他目的需要在block的中间使用return或者break，可以显式的使用do..end来实现：
# 5. 函数
函数有两种用途：1.完成指定的任务，这种情况下函数作为调用语句使用；2.计算并返回值，这种情况下函数作为赋值语句的表达式使用。
语法：
function func_name (arguments-list)
    statements-list;
end;

## 5.1 多返回值
函数多值返回的特殊函数unpack，接受一个数组作为输入参数，返回数组的所有元素。unpack被用来实现范型调用机制，在C语言中可以使用函数指针调用可变的函数，可以声明参数可变的函数，但不能两者同时可变。在Lua中如果你想调用可变参数的可变函数只需要这样：
f(unpack(a))
unpack返回a所有的元素作为f()的参数

## 5.2 可变参数
Lua函数可以接受可变数目的参数，和C语言类似在函数参数列表中使用三点（...）表示函数有可变的参数。Lua将函数的参数放在一个叫arg的表中，除了参数以外，arg表中还有一个域n表示参数的个数。
## 6.3 正确的尾调用

## 8.1 require函数

Lua提供高级的require函数来加载运行库。粗略的说require和dofile完成同样的功能但有两点不同：
1.       require会搜索目录加载文件
2.       require会判断是否文件已经加载避免重复加载同一文件。由于上述特征，require在Lua中是加载库的更好的函数。

# 9. 协同程序

## 9.1 协同的基础

Lua的所有协同函数存放于coroutine table中。create函数用于创建新的协同程序，其只有一个参数：一个函数，即协同程序将要运行的代码。若一切顺利，返回值为thread类型，表示创建成功。通常情况下，create的参数是匿名函数：
协同有三个状态：挂起态（suspended）、运行态（running）、停止态（dead）。当我们创建协同程序成功时，其为挂起态，即此时协同程序并未运行。我们可用status函数检查协同的状态





