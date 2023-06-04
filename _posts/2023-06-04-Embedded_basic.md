---
title:  "Embedded 기본적인것들"
tags: 
    - embedded
    - startup
    - compile

---

## Embedded 기본 내용 정리 

### Startup 파일에 대해서 
처음 시스템이 부팅 하고 나서 SRAM이나. DDR, 등이 초기화가 안되어 있는 상태이다. 이때 빠르게 동작 하거나 System을 초기 설정 해야 하는 경우가 있으며 C 언어로 프로그램 하다보면 함수등 때문에 Stack 부분을 써야 하는데 이는 아직 SRAM등이 초기화 되지 않아서 사용을 할 수가 없는 경우 ,  이때 Startup.S 파일에 Interupt, 메모리나 클럭 PLL 등 초기화가 필요한 것들을 수행한다. 맨 마지막은 main 함수로의 점프로 마무리 한다. 

익셉션벡터/메모리컨트롤러 설정/ 스택 영역 할당/ 메인함수로 Jump 이렇게 정리가 될려나. 

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers tags lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;url&quot;:&quot;https://raw.githubusercontent.com/goodsemong/goodsemong.github.io/main/assets/compileandlink.drawio&quot;}"></div>
<script type="text/javascript" src="https://viewer.diagrams.net/embed2.js?&fetch=https%3A%2F%2Fraw.githubusercontent.com%2Fgoodsemong%2Fgoodsemong.github.io%2Fmain%2Fassets%2Fcompileandlink.drawio"></script>


### Bootloader / Bootstrap 의 차이점

일단 차이점을 ChatGPT에서 구해보면 


A `bootloader`, `startup code`, and `bootstrap loader` are all related but have slightly different functions in the boot process of a computer or device.A bootloader is a program that runs on a device before the operating system starts. It is responsible for loading the operating system into memory and starting it. The bootloader can also provide options for selecting the operating system or performing other tasks.Startup code, also known as BIOS or firmware, is a small program that runs before the bootloader. It is responsible for initializing the hardware and providing basic input/output services.A bootstrap loader is a program that is used to load the bootloader into memory. It is typically stored in a ROM or other non-volatile memory and is executed first when the device is powered on.
{: .notice}


### RO/RW/ZI 구분해보기 

{% include figure image_path="/assets/images/rorwzi.jpg" alt="예. ro rw zi" caption="rw ro zi 영역 예시" %}

전역 변수들은 초기화(RW) 여부 0으로 초기화 혹은 초기화 안한 (ZI), 코드랑, const 변수(RO) 나눠지고 main 함수 안의 static은 RW/Zi로 가고 main 함수 안의 local 변수는 stack으로 간다. 그리고 메모리 할당은 Heap에 위치 한다. 
RW = .data | ZI = .bss | RO = .constadata + .text 

### Entry Point 설정하기 
처음 실행해주는거는 `linker.ld` 에서 설정한다. 

```c
 :
 :
 ENTRY(_start)
```
