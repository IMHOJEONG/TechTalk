# Ansible

* ![](../../.gitbook/assets/ansible\_basic.svg)
* ####
* ####
*   #### Control Node (제어 노드)

    * Ansible이 설치된 시스템
    * 이곳에서 ansible 또는 ansible-inventory와 같은 Ansible 명령을 실행

    #### Managed Node (관리 노드)

    * Ansible이 제어하는 원격 시스템 또는 호스트

    #### Inventory (목록)

    * 논리적으로 구성된 관리 노드 목록
    * 제어 노드에서 Inventory를 생성해 Ansible에 대한 호스트 배포를 설정

    ***

    #### Inventory 구축

    * Inventory : Ansible에 시스템 정보 및 네트워크 위치를 제공하는 중앙 집중식 파일에 관리 노드를 구성
      * Inventory 파일을 사용해 Ansible은 단일 명령으로 많은 수의 호스트를 관리할 수 있음
      * `+` : 지정해야 하는 명령줄 옵션의 수를 줄여 Ansible을 보다 효율적으로 사용하는 데 도움이 됨
        * ex) Inventory에는 일반적으로 SSH 사용자가 포함됨
          * Ansible 명령을 실행할 때 `-u` 플래그를 포함할 필요가 없음
    * 지금까지 관리 노드를 `/etc/ansible/hosts` 파일에 직접 추가했다면
      * 유연성, 재사용 및 다른 사용자와의 공유를 위해 소스 제어에 추가할 수 있는 Inventory 파일을 생성
    * Inventory ⇒ Ansible이 배포하고 구성하는 관리 노드 또는 호스트의 목록
      * Inventory 생성의 목적 : 자동화할 서버 및 장치 목록을 추적하기 위한 Inventory 생성
      * 동적 Inventory를 사용해 지속적으로 시작 및 중지되는 서버 및 장치로 클라우드 서비스를 추적
      * 패턴을 사용해 Inventory의 특정 하위 집합을 자동화
      * Ansible이 인벤토리에 사용하는 연결 방법을 확장하고 개선
      *
      *
