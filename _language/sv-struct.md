--- 
title: "System Verilog Struct 살펴보기"
---

일반적인 구조체의 형태는 


```systemverilog
struct { bit [7:0] opcode; bit [23:0] addr; } IR; // anonymous structure
IR.opcode = 1; 
```

이런 식인데 여기에 typedef를 선언해서 타입으로 사용한다. 

```systemverilog
typedef struct { 
bit [ 7:0 ] opcde ;
bit [23 : 0] addr;
} instruction; // named structure type 
instruction IR: // define variable

```


