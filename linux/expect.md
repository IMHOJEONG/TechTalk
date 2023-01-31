---
description: '- spawn 명령어도 여기서?'
---

# expect?

* expect
  * 스크립트에 따라 다른 대화식 프로그램과 "대화" 하는 프로그램&#x20;
  * 스크립트에 따라 Expect는 프로그램에서 기대할 수 있는 것과 올바른 응답이 무엇인지 알고 있음&#x20;
  * 해석된 언어는 분기 및 높은 수준의 제어 구조를 제공해 대화를 지시&#x20;
  * 또한, 사용자는 원할 대 직접 제어하고 상호 작용할 수 있으며&#x20;
    * 나중에 스크립트로 제어를 반환할 수 있음&#x20;
* expect는 C 또는 C++에서 직접 사용할 수 있음&#x20;
  * TCl 없이?
* Expect라는 이름은 uucp, kermit 및 기타 모뎀 제어 프로그램에 의해 대중화된&#x20;
  * send/expect 시퀀스라는 아이디어에서 유래됨&#x20;
  * uucp와는 달리 Expect는 모든 프로그램과 작업을 염두에 두고 사용자 수준 명령으로 실행할 수 있도록 일반화되었음&#x20;
* Expect는 실제로 동시에 여러 프로그램과 대화할 수 있음&#x20;

ex) 1. 전화 요금을 지불하지 않고 로그인할 수 있도록 컴퓨터에서 다시 전화를 거는 경우&#x20;

2\. 게임(예: 로그)을 시작하고 최적의 구성이 나타나지 않으면 나타날 때까지 다시 시작한다음 제어권을 넘겨줌&#x20;

3\. fsck를 실행하고 질문에 대한 응답으로 예, 아니오로 대답하거나 미리 결정된 기준에 따라 제어 권한을 다시 부여&#x20;

4\. 다른 네트워크나 BBS(예: MCI Mail, CompuServe)에 연결하고 메일이 원래 로컬 시스템으로 전송된 것처럼 나타나도록 자동으로 메일을 검색&#x20;

5\. rlogin, telnet, tip, su, chgrp 등을 통해 환경 변수, 현재 디렉토리 또는 모든 종류의 정보를 전달&#x20;



* 쉘이 이러한 작업을 수행할 수 없는 데는 여러가지 이유가 존재&#x20;
  * 모든 것이 Expect로 가능&#x20;
* 일반적으로 Expect는 프로그램과 사용자 간의 상호 작용이 필요한 모든 프로그램을 실행하는 데 유용&#x20;
  * 필요한 것 : 상호 작용이 프로그래밍 방식으로 특성화될 수 있다는 것&#x20;
* 또한 Expect는 원하는 경우 사용자가 다시 제어할 수 잇음&#x20;
  * 제어 중인 프로그램을 중단하지 않고&#x20;
* 마찬가지로 사용자는 언제든지 스크립트에 제어권을 반환할 수 있음&#x20;

#### Linux Expect with Example

* 시스템 관리자는 반복적인 작업을 항상 자동화함&#x20;
* Bash 스크립트는 시스템 자동화 작업을 수행하기 위한 많은 프로그램과 기능을 제공&#x20;
* expect 명령은 계속하려면 사용자 입력이 필요한 대화형 응용 프로그램을 제어하는 방법 제공&#x20;

#### Linux expect Command Syntax&#x20;

```bash
expect [options] [commands/command file]
```



* Spawn : 새로운 프로세스 생성&#x20;
* Send : 프로그램에 응답 전송&#x20;
* Expect : 결과를 기다림&#x20;
* Interact : 프로그램과 상호작용을 가능하게 함&#x20;



* Expect는 TCL(Tool Command Language)을 사용해 프로그램 흐름과 필수 상호 작용을 제어&#x20;
* 일부 시스템에서는 기본적으로 Expect가 포함되어 있지 않음&#x20;
* Debian 기반 시스템에서 apt로 설치하려면&#x20;

```bash
sudo apt install expect
```

```bash
yum install expect 
```

* `-c` : 스크립트 전에 실행할 명령을 지정
* `-d` : 간략한 diagnostic 출력을 제공&#x20;
* `-D` : 대화형 디버거
* `-f` : 읽을 파일을 지정&#x20;
* `-i` : 대화식으로 명령을 prompt함&#x20;
* `-b` : 파일을 한 줄씩 읽음(버퍼)
* `-v` : 버전 출력&#x20;
*







* [https://linux.die.net/man/1/expect](https://linux.die.net/man/1/expect)
* [https://phoenixnap.com/kb/linux-expect](https://phoenixnap.com/kb/linux-expect)
