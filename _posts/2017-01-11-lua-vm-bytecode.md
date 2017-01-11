---
layout: post
title:  "lua 字节码(bytecode)"
categories: lua
tags: lua
---

* content
{:toc}

## lua字节码

lua虚拟机最终执行的是经过lua编译器编译的字节码，我们暂且不关系Chunk生成字节码过程，
只关系字节码本身，字节码的格式到底是什么？具体含义是是什么？

## 字节码格式

### 源码案例：

```
--test function 1
function max(num1, num2)
   if (num1 > num2) then
	  r = num1
   else
	  r = num2
   end
   return r
end

--test function 2
function add(num1, num2)
	return num1 + num2
end

console("hello world")
console("max ", max(10,8))
```

### 对应字节码：

```
main <test.lua:0,0> (15 instructions at 0000000000180370)
0+ params, 5 slots, 1 upvalue, 0 locals, 7 constants, 2 functions
	1	[9]	CLOSURE  	0 0	; 0000000000180640
	2	[2]	SETTABUP 	0 -1 0	; _ENV "max"
	3	[14]	CLOSURE  	0 1	; 00000000001809C0
	4	[12]	SETTABUP 	0 -2 0	; _ENV "add"
	5	[16]	GETTABUP 	0 0 -3	; _ENV "console"
	6	[16]	LOADK    	1 -4	; "hello world"
	7	[16]	CALL     	0 2 1
	8	[17]	GETTABUP 	0 0 -3	; _ENV "console"
	9	[17]	LOADK    	1 -5	; "max "
	10	[17]	GETTABUP 	2 0 -1	; _ENV "max"
	11	[17]	LOADK    	3 -6	; 10
	12	[17]	LOADK    	4 -7	; 8
	13	[17]	CALL     	2 3 0
	14	[17]	CALL     	0 0 1
	15	[17]	RETURN   	0 1

function <test.lua:2,9> (8 instructions at 0000000000180640)
2 params, 3 slots, 1 upvalue, 2 locals, 1 constant, 0 functions
	1	[3]	LT       	0 1 0
	2	[3]	JMP      	0 2	; to 5
	3	[4]	SETTABUP 	0 -1 0	; _ENV "r"
	4	[4]	JMP      	0 1	; to 6
	5	[6]	SETTABUP 	0 -1 1	; _ENV "r"
	6	[8]	GETTABUP 	2 0 -1	; _ENV "r"
	7	[8]	RETURN   	2 2
	8	[9]	RETURN   	0 1

function <test.lua:12,14> (3 instructions at 00000000001809C0)
2 params, 3 slots, 0 upvalues, 2 locals, 0 constants, 0 functions
	1	[13]	ADD      	2 0 1
	2	[13]	RETURN   	2 2
	3	[14]	RETURN   	0 1
```

### 具体含义：

```
C		LN		OPCODE			A	B	C
C		LN		OPCODE			A	Bx
C		LN		OPCODE			A	sBx
备注C-代表指令数量,LN代表指令的行号
```


## 指令分类

种类：iABC	iABx	iAsBx	iAx

代码中定义：enum OpMode {iABC, iABx, iAsBx, iAx};  /* basic instruction format */

## iABC指令