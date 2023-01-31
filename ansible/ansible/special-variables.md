---
description: '-'
---

# Special Variables ( 특별한 변수들 )

#### Magic Variables

[https://docs.ansible.com/ansible/latest/reference\_appendices/special\_variables.html#magic-variables](https://docs.ansible.com/ansible/latest/reference\_appendices/special\_variables.html#magic-variables)

* 이러한 변수는 사용자가 직접 설정할 수 없음
* Ansible은 내부 상태를 반영하기 위해 항상 재정의함

#### Facts

* 현재 호스트(inventory\_hostname)와 관련된 정보를 포함하는 변수
* 먼저 수집한 경우에만 사용할 수 있음

#### Connection variables

* 연결 변수 : 일반적으로 대상에서 작업을 실행하는 방법에 대한 세부 사항을 설정하는 데 사용됨
* 대부분은 연결 플러그인에 해당하지만 모든 플러그인이 해당 플러그인에 해당하는 것은 아님
* shell, terminal 및 be와 같은 다른 플러그인이 일반적으로 관련됨
* 각 연결/become/shell/etc 플러그인이 자체 재정의 및 특정 변수를 정의할 수 있음
  * 일반적인 항목만 설명됨
