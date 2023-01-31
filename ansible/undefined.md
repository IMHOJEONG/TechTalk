---
description: '-'
---

# 연결 방법 및 세부 사항

* [https://docs.ansible.com/ansible/latest/inventory\_guide/connection\_details.html](https://docs.ansible.com/ansible/latest/inventory\_guide/connection\_details.html)
* Ansible은 ssh 연결 플러그인(기본값)을 사용할 때, ssh 키를 해독하기 위해
  * 암호를 수동으로 수락하기 위해 사용자와 ssh 프로세스 간의 통신을 허용하는 채널을 노출하지 않음
  * ssh-agent를 사용하는 것이 좋음
* 일단! 간단한 연결 방법 ⇒ 제가 임의로 찾아보고 작성한 부분
  * [https://magnuxx.tistory.com/entry/ansible-에러-Failed-to-connect-to-the-host-via-ssh-Permission-denied](https://magnuxx.tistory.com/entry/ansible-%EC%97%90%EB%9F%AC-Failed-to-connect-to-the-host-via-ssh-Permission-denied)
  * ssh 키 파일을 가지고 있지 않는 경우에 복사해서 처리
