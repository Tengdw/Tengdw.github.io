---
title: 选择&循环
date: 2017-10-15 16:19:05
tags: Java
category: Java学习
---
### 单分支选择结构( if )
**if选择结构是根据条件判断之后再做处理**

```flow
st=>start: 代码
op=>operation: 代码块
cond=>condition: 条件?
e=>end: 代码

st->cond->op->e
cond(yes)->op
cond(no)->e
```

### if-else多重选择结构

```flow
st=>start: 代码...
op=>operation: 代码块1
op1=>operation: 代码块2
cond=>condition: 条件?
e=>end: 代码...

st->cond->op->e
cond(yes)->op
cond(no)->op1->e
```

### while循环

```flow
st=>start: 开始循环
cond=>condition: 循环条件
op=>operation: 循环操作
e=>end: 循环结束

st->cond->op->cond
cond(yes)->op
cond(no)->e
```

### do...while循环

```flow
st=>start: 开始循环
cond=>condition: 循环条件
op=>operation: 循环操作
e=>end: 循环结束

st->op->cond
cond(yes)->op
cond(no)->e
```


### for循环
```flow
st=>start: 开始循环
cond=>condition: 判断条件
op=>operation: 循环体
op1=>operation: 循环变量增（减）值
op2=>operation: 赋循环变量初值
e=>end: 代码...

st->op2->cond->op->op1->op2
cond(yes)->op
cond(no)->e
```

### continue与break
- 在程序中，如果程序执行过程中碰到break，则跳出循环。
- 在程序执行过程中，如果碰到continue，则结束本次循环，继续下一轮循环。
