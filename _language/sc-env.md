---
title:  "SystemC 환경 잡기"
---

## SystemC Compile 하기

SystemC 코드를 컴파일 하기 위해서는 include path, library path, library 그리고 컴파일 옵션을 고려 해야 한다.

### SystemC Vendor 별 Compiler를 정리  

Cadence  : xmsc_run , 케이던스는 **xmsc_run** 이 있으며 
Sysnopsys  : 
Metor  :


### 컴파일러
컴파일러는 보통의 그것을 사용 해도 되지만  SystemC 라이브러리를 컴파일 했던 걸 사용 해야 한다. 

### Include Path

```shell
-I systemc include path 
-I systemc TLM (tlm_core ) path
-I systemc TLM (tlm_utils)  path 
```
기본 패스 포함 해야 되며 소스파일이 있는 곳이라면 거기도 포함 시켜야 하낟. 

### Library Path
```shell
-L systemc lib path 
``` 
sytemc Lib가 있는 패스 설정 해주기 

###  Add Library 
```shell
-lsystemc -ldl -lm -lpthread
```
일단 systemc lib 를 포함 하고 기본 필요한것들 포함 시킨다. 

### Option
```shell
-std=gnu++11
```
저거 외에 컴파일 최적화 옵션도.. 포함 해주면 되고 말그대로 옵션이다. 

### rpath
```shell
-Wl,-rpath compiler library path 
-WL,-rpath systemc library path 
```
rpath는  실행 파일을 만들때 라이브러리 의존성을 제거 해준다. rpath에 $ORIGIN 을 설정 하면 현재 폴더에 있는 라이브러릴 실행 할때 참조한다. 참고 .. 


## Interfacing to SystemC at Signal Level
SystemC 의 클래스와 같은 더미 SystemVerilog 모델을 만들어서 
그 System Verilog 모델과 SystemC 를 같이 컴파일 해서 하는 방법이다. 

알단 아래와 같은 SystemC 모듈이 있다고 했을때 

model.cpp
```c++
#include "systemc.h"

#SC_MODULE(model){
class model : public sc_module{
 sc_in<sc_logic> in;
 sc_out<sc_logic> out;
 SC_CTOR(model) : in("in"),out("out"){
  SC_METHOD(run);
  sensitive << in;
 }
 ~model() {}
 void run(){
   out.write(in.read());
 }
};

// register top SystemC module
SC_MODULe_EXPORT(sctop)
```

같은 이름의 SystemVerilog를 만들어 준다. (Verilog 가 될수도 있고 )

model.v
```verilog
// Note module name must match exactly name of sc_module class in SystemC
module model(in,out)
// Note that the forigen attribute string value must be "SystemC"
(* integer foregin = "SystemC";*);
// Note that port names must match exactly port names as they appear in 
// sc_module class in SystemC
// they must also match in order, mod and type
input in;
output out;
```

테스트를 위한 Top.v를 만들어 준다 . 
```verilog
module top;
reg in; 
wire out 
initial begin 
in = 1'b0;
#10 in = 1'b1;
#10 $finish;
end 

// Instantiate the SystemC HDL Shell
model test (in,out);

initial $monitor($time,,"in = %b, out = %b", in,out);

endmodule 
```

그리고 같이 컴파일 해준다. 
```shell
xrun -sysc +timescale=1ns/1ps *.v *.cpp -top top 
```

케이던스와 멘터꺼는 sc_module_export 해서 sc module 이름을 지정해서 rtl의 포함시킬수 있는데 Sysnopsys 꺼가 다른 방식으로 해야 한다는.. 컴파일 할때 sctop을 따로 지정 해서 적용 해야 한다는 다른점이 있음.. 

dpi를써서 인터페이스 하는 방법도 있다. 
혹은 TLM방식으로도 생각해 보자. 








 



