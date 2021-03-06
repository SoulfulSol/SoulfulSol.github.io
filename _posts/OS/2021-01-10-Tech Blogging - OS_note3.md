---
layout: post
title: "[OS] 운영체제의 구조"
subtitle: "Operating System Note 3"
background: '/img/bg_technology.jpg'
categories: technology-os

---


> 운영체제를 책을 빌려주는 '도서관'이다.
>
> 시스템 리소스(책)를 필요로 하는 시민(응용 프로그램)에게 적절한 책을 찾아서 제공하며,
>
> 대여기간이 다 되면, 책을 '회수'한다.



- 운영체제는 응용프로그램이 요청하는 **메모리를 허가, 분배한다.**
- 운영체제는 응용프로그램이 요청하는 **CPU시간을 제공한다.**
- 운영체제는 응용프로그램이 요청하는 **IO DEVICES 사용을 허가/제어한다.**



***사용자 --(요청)--> 응용프로그램 --(요청)--> OS --(제어, 허가)--> 하드웨어 리소스***



- 운영체제는 사용자 인터페이스를 제공한다 : **쉘(Shell)**이라는 프로그램이 사용자의 요청을 받아 그 요청을 운영체제에 넘겨준다.

- ![image_1](https://github.com/Sol-cito/OS/blob/main/img/Note3_1.png?raw=true)

  **ex) ls 명령어를 cmd창에 입력한다** : 운영체제에 '파일 리스트를 보여줘'라는 요청을 보내고, 그 결과 운영체제가 현재 경로의 파일 리스트를 cmd에 출력해준다.

  - Shell은 **터미널환경**과, **GUI환경**으로 구분된다.
  - Shell도 또한 하나의 **응용프로그램(Applciation)이다.**

  

- 운영체제는 **사용자뿐만 아니라 응용프로그램을 위해서도 인터페이스를 제공한다.**

  - 응용프로그램은 운영체제와 **프로그래밍 language**를 통하여 운영체제와 소통한다.

    ----> 특정 코드를 통해 **운영체제의 기능 사용을 요청한다.** 

  - OS가 제공하는 이러한 인터페이스를 **시스템 콜(System Call)**이라 한다.

    - 시스템콜은 직접 사용하기에 어려움이 있다 --> 일일히 세부사항을 직접 코딩하기가 어렵다.
    - 따라서 프로그래밍 언어들은 이 시스템콜을 편리하게 사용하기 위한 수단으로 여러가지 함수를 library 형태로 제공한다.
    - 운영체제가 응용프로그램에 제공하는 이 인터페이스를 **Application Programming Interface, API** 라고 한다.
    - **API**는 어떤 형태로 제공되는가 ? : **함수**의 형태로 제공하며, 일반적으로 **라이브러리**형태로 제공한다.

    ex) C언어의 open() 함수 : 파일을 연다. --> 파일은 저장매체(storage)에 있는 data이므로, **저장매체의 리소스를 제어할 수 있어야 한다(운영체제의 역할)**.

    ex2) C언어의 printf() 함수 : 모니터(IO device)에 문자열을 출력해야 한다.

    ​	(1) printf() 함수가 user mode에서 실행된다.

    ​	(2) printf() 함수의 실행은 stdio 라이브러리를 호출한다.

    ​	(3) stdio 라이브러리는 **시스템콜인 write() API 함수를 호출한다.**

    ​	(4) 실행의 흐름이 kernel mode로 전환되고, 커널이 호출을 실행하고 모니터에 글자를 출력한다.

    ​	(5) 실행의 흐름이 다시 user mode로 전환된다.

  - API는 함수의 집합이므로, 따라서 API는 **요청서의 집합**이라고 할 수 있다 : 응용프로그램이 필요로 하는 요청 종류(ex)파일 열기, 파일 닫기, 파일 쓰기, 화면 깜빡임.....)를 함수형태로 만들어 모아놓은 것이기 떄문이다.

  - 따라서, **Shell** 프로그램 또한 **API**를 통해 만들어졌음을 알 수 있다. Shell은 운영체제에 요청을 보내서 사용자가 필요한 결과를 얻어내게 하는 프로그램이므로, 운영체제의 API를 활용해야만 한다.

  > 응용프로그램은 OS가 제공하는 Interface(System Call)을 통해서만 자원을 사용할 수 있다.
  >
  > 운영체제는 응용프로그램이 자원을 활용할 수 있도록 함수 형태의 API를 제공한다. 
  >
  > 시스템 콜은 운영체제 기능을 호출하는 함수이고, API는 각 언어별(C, JAVA..) 운영체제 기능 호출 인터페이스 함수이다.



- 운영체제의 개발 순서
  - Kernel 개발 --> 시스템 콜 개발 --> 각 언어별 API 개발 --> Shell 개발 --> 응용프로그램 개발
  - 시스템 콜을 개발할 때, 시스템 콜을 정의한 표준이 존재한다 : 
    - (유닉스계열)POSIX API, 윈도우 API



### CPU Protection Rings

- CPU도 **권한모드**를 가지고 있다 : **사용자모드 / 커널모드**

  

  > Kernel 이란 ? 사전적 의미는 '핵심', '알맹이'이다. OS에서의 '핵심 소프트웨어'를 커널이라 한다.
  >
  > 즉, 커널모드는 OS가 CPU를 사용할 때 쓰는 모드이다.
  >
  > Shell 이란? 커널의 껍데기를 뜻한다.

  

  - 여기서 커널모드는 **특권 명령어 실행과 원하는 작업 수행을 위한 자원 접근을 가능케 하는 모드**이다 : 특별한 자원을 접근해야하는 경우, 커널모드가 아니면 실행되지 않도록 하는 것.

  ![image_1](https://github.com/Sol-cito/OS/blob/main/img/Note3_2.png?raw=true)

  ​	(출처 : https://en.wikipedia.org/wiki/Protection_ring )
  - CPU는 ring level별로 나누어져 있는데, **커널은 Ring 0, 응용프로그램은 Ring 3를 쓴다.**
  - 즉 **핵심 자원에 접근하는 특권 명령어를 실행하려면 커널모드가 되어야 하고, 사용자 모드에서는 접근할 수 없다.**

  ![image_1](https://github.com/Sol-cito/OS/blob/main/img/Note3_3.png?raw=true)
  - 우리가 만드는 Application은 **맨 윗 단에서 돌고 있다.**
  - 예를 들어, 다음과 같은 명령을 실행한다고 해 보자.
    - 1~100까지 for문으로 더한 결과를
    - 특정 파일의 data를 불러와서
    - 그 data값과 다시 더하고 출력한다.
  - 이 때, for문으로 수를 더하고 출력하는 것은 **응용프로그램**레벨에서 수행되지만, **특정 파일의 data를 불러오는 것**은 Disk에 접근하여 file을 읽어야 하기 때문에 **OS에 자원에 접근해달라는 요청(시스템 콜)을 보내야 하고, 따라서 Kernel 모드를 켜야 한다.**
  - 따라서, 어떤 프로그램이 실행될 때 **어떤 때는 CPU가 사용자 영역에서 실행되지만, 어떤 때는 Kernel 영역에서 실행된다.**

```c
int main(){
    fd = open("data.txt", O_RDONLY);
    if(fd == -1){
        printf("파일을 열지 못했음");
        return 1
    }else{
        printf("파일을 열었음");
        return ();
    }
}
// main함수는 '사용자모드'이다.
// 위 코드에서 open() 함수는 파일을 여는 '시스템 콜'이다. --> 'Kernel 모드'로 변경된다.
// open() 함수가 끝난 후 if절부터는 다시 '사용자 모드'이다.
```



> 따라서, System Call은 Kernel 모드에서 실행된다. 어플리케이션은 사용자 모드에서 실행된다.

- 커널 모드에서만 실행 가능한 기능들이 있으며(하드웨어 자원 접근), 
- 커널 모드로 실행하려면 **반드시 시스템 콜을 사용해야(거쳐야) 한다.**
- 시스템 콜은 **운영체제가 제공한다.**

> 컴퓨터는 응용프로그램 단과 Kernal(시스템) 단으로 나누어져 작동하기 때문에,
>
> 프로그래머도 '응용프로그래머'와 '시스템프로그래머'로 나누어진다.


---
위 내용은 '패스트캠퍼스'의 컴퓨터공학 강좌 내용을 요약 정리한 것임을 밝힙니다.
(https://www.fastcampus.co.kr/)