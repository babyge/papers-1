(load "d:/working/lisp/hello.lisp") ; 加载lisp文件
(compile-file "d:/working/lisp/hello.lisp") ; 编译lisp文件
符号NIL是唯一的假值，其他所有的值都是真值。
()是NIL的等价表示。
Lisp提供四个等价谓词:
EQ: 用来测试对象标识
EQL: 除EQ功能外，保证数字和字符情况下的等价值
equal: 在递归上具有相同结构和内容的列表视为等价
equalp: 在比较字符串时，忽略大小写的区别
注释
;;;; 四个分号用于文件头注释
;;; 三个分号的注释用于段落注释
;; 两个分号的注释应用于接下来的代码上
; 只用于本行

# 函数

## 1.定义新函数

(defun name (parameter*)
"Optional documentation string"
body-form*)
几乎任何符号都可以用作函数名，通常仅包含字典字符和连字符。
一个defun的主体可由任意数量的lisp表达式构成，它们将在函数被调用时依次求值，而最后一个表达式的值将被用作整个函数的值返回。

## 2.形式参数列表

## 3.可选形参

可以通过&optional关键字来定义可选参数。
(defun foo (a b &optional c d) (list a b c d))
可以通过一个变量来查询是否使用了默认值。
(defun foo2 (a b &optional (c 3 c-supplied-p))
  (list a b c c-supplied-p))

## 4.剩余形参

可以用&rest关键字定义一揽子形参。

## 5.关键字形参

在任何必要的&optional和&rest形参之后，可以加上&key以及任意数量的关键字形参标识符
(defun foo3 (&key a b c) (list a b c))
如果一个给定的关键字没有出现在实参列表中，那么对应的形参将被赋予默认值.
如同可选形参那样，关键字形参也可以提供一个默认值形式以用一个supplied-p变量名。

## 6.混合不同的形参类型

声明顺序：首先是必要形参，其次是可选形参，再次是剩余形参，最后是关键字形参。

## 7.函数返回值

可以使用return-from操作符以任何值从函数中间返回。

## 8.作为数据的函数-高阶函数

操作符function提供了用来获取一个函数对象的方法。
#'是function的语法糖。
一旦得到了函数对象，就只剩一件事情可以做-调用它。Common Lisp提供了两个函数用来通过函数对象调用函数：funcall和apply.
funcall用于在编写代码时确切知道传递函数多少实参时。
apply支持函数参数是一个列表。

## 9.匿名函数

觉得没必要用defun来定义一个新函数时，可以使用lambda表达式创建匿名的函数。
(lambda (parameters) body)
lambda表达式的另一项重要用途是制作闭包closure，即捕捉了其创建时环境信息的函数。

# 变量

Common Lisp支持两种类型的变量：词法lexical变量和动态dynamic变量。这两种变量类型分别对应于其他语言中的局部变量和全局变量，不过也只能说大致如此。

## 1.变量基础知识

可以通过let操作符引入新变量。
(let (variable*) body-form*)
其中每一个variable都是一个变量初始化形式。每一个初始化形式要么是一个含有变量名和初值形式的列表，要么就是一个简单的变量名-作为将变量初始化到nil的简略写法。
(let ((x 10) (y 20) z))
另一个绑定形式是let的变体let*. let*中，每个变量的初始值形式，都可以引用到那些在变量列表中早先引入的变量。
例如，
```lisp
(let* ((x1 10) (y1 (+ x1 10))) (list x1 y1))
```
相当于，
```lisp
(let ((x2 10))
  (let ((y2 (+ x2 10)))
    (list x2 y2)))
```
## 2.词法变量和闭包

默认情况下，Common Lisp中所有的绑定形式都将引入词法作用域变量。
```lisp
(let ((count 0)) #'(lambda () (setf count (1+ count))))
```
上面的匿名函数称为一个闭包，因为它“封闭包装”了由let创建的绑定。

## 3.动态变量

词法作用域的绑定通过限制作用域（其中给定的名字只具有字面含义）使代码易于理解。
Common Lisp提供了两种创建全局变量的方式：defvar和defparameter. 两种形式都接受一个变量名、一个初始值以及一个可选的文档字符串。在被defvar和defparameter定义以后，该名字可用于任何位置来指向全局的当前绑定。
两种形式的区别在于，defparameter总是将初始值赋给命名的变量，而defvar只有当变量未定义时才这样做。defvar形式也可以不带初始值来使用。
## 4.常量
(defconstant name initial-value-form [documentation-string])
建议采用+x+的方式来命名常量
## 5.赋值
通过赋值操作符来进行赋值操作
(setf place value)
setf也可用于依次对多个位置赋值，如：
(setf x 1 y 2)
## 6.广义赋值
## 7.其他修改位置的方式
(incf x) 相当于 (setf x (+ x 1))
(decf x) 相当于 (setf x (- x 1))
(incf x 10) 相当于(setf x (+ x 10))
rotatef 宏用于轮换值
shiftf 宏用于向左移动值

#宏

## 1.when和unless
最基本的条件执行形式是由if操作符提供的，其基本形式为：
(if condition then-form [else-form])
condition被求值，如果其值非nil，那么then-form会被求值并返回其结果，否则则执行else-form.
progn操作符用于执行任意数量的形式并返回最后一个形式的值。
(defmacro my-when (condition &rest body)
  `(if ,condition (progn ,@body)))
(defmacro my-unless (condition &rest body)
  `(if (not ,condition) (progn ,@body)))
## 2.cond
(cond
 (test-1 form*)
 ...
 (test-N form*))
一般在最后加上一个default条件,
(cond
 (a (do-x))
 (b (do-y))
 (t (do-z)))
## 3. and, or和not
not函数接受单一参数对其真值取反，当参数为nil返回t，否则返回nil.
and和or是宏以便支持短路特性。
## 4.循环
所有的循环控制构造都是构建在tagbody和go两个特殊操作符上。
## 5.dolist和dotimes
dolist在一个列表的元素上循环操作。
(dolist (var list-form)
 body-form*)
如果想在列表结束之前中断一个dolist循环，则可以使用return
dotimes是用于循环计数的高级循环构造。
(dotimes (var count-form)
body-form*)
其中的count-form必须要能求值为一个整数。
例：
(dotimes (i 4) (print i))
## 6.do
(do (variable-definition*)
(end-test-form result-form*)
statement)
## 7.强大的loop
简化版的loop宏就是一个不绑定任何变量的无限循环。
(loop
body-form*)
# 如何自定义宏

1. 宏展开期和运行期
2.defmacro
```lisp
(defmacro name (parameter*)
"Optional documentation string."
body-form*)
```
反引用表达式范例：
```lisp
`(a (+ 1 2) c) : (list 'a '(+ 1 2) 'c) : (a (+ 1 2) c)
```
实践：建立单元测试框架

