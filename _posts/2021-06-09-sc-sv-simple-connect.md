---
title:  "SystemC를 SystemVerilog 연결하는 방법"
---

# 1. Interfacing to SystemC at Signal Level
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




