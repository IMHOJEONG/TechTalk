---
description: '-'
---

# Jinja2

* Ansible은 Jinja2 템플릿을 사용해 동적 표현과 변수 및 팩트에 대한 액세스를 가능하게 함&#x20;
* 템플릿 모듈과 함께 템플릿을 사용할 수 있음&#x20;
  * ex) 구성 파일에 대한 템플릿을 생성한 다음, 해당 구성 파일을 여러 환경에 배포하고 각 환경에 대한 올바른 데이터
    * IP주소, 호스트 이름, 버전을 제공할 수 있음&#x20;
* 작업 이름 등을 템플릿으로 지정하여 playbook에서 직접 template을 사용할 수도 있음&#x20;
* Jinja2에 포함된 모든 표준 필터와 테스트를 사용할 수 있음&#x20;
* Ansible에는 데이터 선택 및 변환을 위한 추가 특수 필터, 템플릿 표현식 평가를 위한 테스트, 템플릿에 사용할 파일, API 및 데이터베이스와 같은 외부 소스에서 데이터를 검색하기 위한 조회 플러그인이 포함되어 있음&#x20;



* 모든 템플릿은 task가 대상 시스템에서 전송 및 실행되기 전에 Ansible 컨트롤러에서 발생함&#x20;
  * 대상의 패키지 요구 사항을 최소화함&#x20;
  * jinja2는 컨트롤러에만 필요함&#x20;
* 또한, Ansible이 대상 시스템에 전달하는 데이터의 양을 제한&#x20;
* Ansible은 컨트롤러의 모든 데이터를 전달하고 대상에서 구문 분석하는 대신&#x20;
  * 컨트롤러에서 템플릿을 구문 분석하고 각 작업에 필요한 정보만 대상 머신에 전달&#x20;





[https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_templating.html](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_templating.html)

[https://watch-n-learn.tistory.com/88](https://watch-n-learn.tistory.com/88)



