---
title:  "SystemC Compile 하기"
---

SystemC 코드를 컴파일 하기 위해서는 include path, library path, library 그리고 컴파일 옵션을 고려 해야 한다.

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








 



