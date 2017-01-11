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

lua字节码指令是由4个字节32位确定, 代码是如何被翻译成对应的32位指令，同样这里我们先
不关心，我们关心只是一段lua代码编译出的字节码指令到底是什么。

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
print("hello world")
print("max ", max(10,8))
```

这里定义了两个函数，max函数和add函数，并进行简单实现，首先调用print函数输出hello world
然后调用自定义函数max，函数add是没有任何调用只是定义，关心的是这段代码生成的字节码的结
是什么，这里我们使用lua5.2.1版本lauc编译器，使用指令：luac -l test.lua进行编译。

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

### 指令解释：

上面的源码生成指令可以看出来，每一行是一个指令是由32位组成，每一行指令主要包含两部分
第一部分操作码opcode和二部分操作寄存器，惭怍寄存器可能是3个可能是2两个，也可能是3个
每一行指令前两部分不是指令具体内容，只是指令的辅助内容，第一个字段是指令数量，第二个
字段是是指令的对应的源码的行数。

通过上面的结果我们可以看出来，每一个lua函数，lua都会生成一块指令块，该指令块包含该函数
的内容指令。同时最关键的是lua源码Chunk默认会生成一个main函数，该指令块主要包含lua的执行
过程，以及调用函数过程。


## 指令分类

四种指令：iABC	iABx	iAsBx	iAx,代码中定义：enum OpMode {iABC, iABx, iAsBx, iAx};
lua所有指令前6位是操作码opcode,剩下组成部分如下：

```
Instructions can have the following fields:
	`A' : 8 bits
	`B' : 9 bits
	`C' : 9 bits
	'Ax' : 26 bits ('A', 'B', and 'C' together)
	`Bx' : 18 bits (`B' and `C' together)
	`sBx' : signed Bx
```

## 所有指令

这里的指令是5.2.1版本里面所有的指令都定义在lopcode.h头文件中定义，代码如下：

```
/*----------------------------------------------------------------------
name		args	description
------------------------------------------------------------------------*/
OP_MOVE,/*	A B	R(A) := R(B)					*/
OP_LOADK,/*	A Bx	R(A) := Kst(Bx)					*/
OP_LOADKX,/*	A 	R(A) := Kst(extra arg)				*/
OP_LOADBOOL,/*	A B C	R(A) := (Bool)B; if (C) pc++			*/
OP_LOADNIL,/*	A B	R(A), R(A+1), ..., R(A+B) := nil		*/
OP_GETUPVAL,/*	A B	R(A) := UpValue[B]				*/

OP_GETTABUP,/*	A B C	R(A) := UpValue[B][RK(C)]			*/
OP_GETTABLE,/*	A B C	R(A) := R(B)[RK(C)]				*/

OP_SETTABUP,/*	A B C	UpValue[A][RK(B)] := RK(C)			*/
OP_SETUPVAL,/*	A B	UpValue[B] := R(A)				*/
OP_SETTABLE,/*	A B C	R(A)[RK(B)] := RK(C)				*/

OP_NEWTABLE,/*	A B C	R(A) := {} (size = B,C)				*/

OP_SELF,/*	A B C	R(A+1) := R(B); R(A) := R(B)[RK(C)]		*/

OP_ADD,/*	A B C	R(A) := RK(B) + RK(C)				*/
OP_SUB,/*	A B C	R(A) := RK(B) - RK(C)				*/
OP_MUL,/*	A B C	R(A) := RK(B) * RK(C)				*/
OP_DIV,/*	A B C	R(A) := RK(B) / RK(C)				*/
OP_MOD,/*	A B C	R(A) := RK(B) % RK(C)				*/
OP_POW,/*	A B C	R(A) := RK(B) ^ RK(C)				*/
OP_UNM,/*	A B	R(A) := -R(B)					*/
OP_NOT,/*	A B	R(A) := not R(B)				*/
OP_LEN,/*	A B	R(A) := length of R(B)				*/

OP_CONCAT,/*	A B C	R(A) := R(B).. ... ..R(C)			*/

OP_JMP,/*	A sBx	pc+=sBx; if (A) close all upvalues >= R(A) + 1	*/
OP_EQ,/*	A B C	if ((RK(B) == RK(C)) ~= A) then pc++		*/
OP_LT,/*	A B C	if ((RK(B) <  RK(C)) ~= A) then pc++		*/
OP_LE,/*	A B C	if ((RK(B) <= RK(C)) ~= A) then pc++		*/

OP_TEST,/*	A C	if not (R(A) <=> C) then pc++			*/
OP_TESTSET,/*	A B C	if (R(B) <=> C) then R(A) := R(B) else pc++	*/

OP_CALL,/*	A B C	R(A), ... ,R(A+C-2) := R(A)(R(A+1), ... ,R(A+B-1)) */
OP_TAILCALL,/*	A B C	return R(A)(R(A+1), ... ,R(A+B-1))		*/
OP_RETURN,/*	A B	return R(A), ... ,R(A+B-2)	(see note)	*/

OP_FORLOOP,/*	A sBx	R(A)+=R(A+2);
			if R(A) <?= R(A+1) then { pc+=sBx; R(A+3)=R(A) }*/
OP_FORPREP,/*	A sBx	R(A)-=R(A+2); pc+=sBx				*/

OP_TFORCALL,/*	A C	R(A+3), ... ,R(A+2+C) := R(A)(R(A+1), R(A+2));	*/
OP_TFORLOOP,/*	A sBx	if R(A+1) ~= nil then { R(A)=R(A+1); pc += sBx }*/

OP_SETLIST,/*	A B C	R(A)[(C-1)*FPF+i] := R(A+i), 1 <= i <= B	*/

OP_CLOSURE,/*	A Bx	R(A) := closure(KPROTO[Bx])			*/

OP_VARARG,/*	A B	R(A), R(A+1), ..., R(A+B-2) = vararg		*/

OP_EXTRAARG/*	Ax	extra (larger) argument for previous opcode	*/
```



	






