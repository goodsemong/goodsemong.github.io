---
title:  "SystemC를 SystemVerilog 연결하는 방법"
---

# 1. Interfacing to SystemC at Signal Level
SystemC 의 클래스와 같은 더미 SystemVerilog 모델을 만들어서 
그 System Verilog 모델과 SystemC 를 같이 컴파일 해서 하는 방법이다. 

알단 아래와 같은 SystemC 모듈이 있다고 했을때 
```c++
#SC_MODULE(model){
clas model : public sc_model{
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
```

같은 이름의 SystemVerilog를 만들어 준다. (Verilog 가 될수도 있고 )
```verilog
// Note module name must match exactly name of sc_module class in SystemC
module model(in,out)
// Note that the forigen attribute string value must be "SystemC"
(* integer foregin = "SystemC";*)
// Note that port names must match exactly port names as they appear in 
// sc_module class in SystemC
// they must also match in order, mod and type
input in;
output out;
```

그리고 같이 컴파일 해준다. 
```shell
xrun -sysc -log_xmsim xmsim.log *.v *.cpp -input xmsim.in -top top 
```


# 2. Interfacing to SystemC via Method Calls











 