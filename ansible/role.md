---
description: '-'
---

# Role

[https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_reuse\_roles.html](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_reuse\_roles.html)

* Role을 사용하면 알려진 파일 구조를 기반으로 관련 변수, 파일, 태스크, 핸들러 및 기타 Ansible 아티팩트를 자동으로 로드할 수 있음
* Content를 Role 별로 그룹화한 후에는 Content를 쉽게 재사용하고 다른 사용자와 공유할 수 있음

#### Role 디렉터리 구조

* Ansible Role에는 8개의 주요 표준 디렉토리가 있는 정의된 디렉토리 구조가 존재
  * 각 Role에 이러한 디렉터리 중 하나 이상을 포함해야 함
* Role이 사용하지 않는 디렉터리는 생략할 수 있음

```yaml
# playbooks
site.yml
webservers.yml
fooservers.yml
```

```yaml
roles/
    common/               # 이 계층 구조는 Role을 나타냄 
        tasks/            #
            main.yml      #  <-- 작업 파일은 보장되는 경우 더 작은 파일을 포함할 수 있음 
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- 템플릿 리소스와 함께 사용할 파일
            ntp.conf.j2   #  <------- 템플릿은 .j2로 끝남 
        files/            #
            bar.txt       #  <-- 복사 리소스와 함께 사용할 파일 
            foo.sh        #  <-- 스크립트 리소스와 함께 사용할 스크립트 파일
        vars/             #
            main.yml      #  <-- 이 역할과 관련된 변수
        defaults/         #
            main.yml      #  <-- 이 역할에 대한 기본 낮은 우선 순위 변수 
        meta/             #
            main.yml      #  <-- 역할 종속성
        library/          # 역할에는 사용자 정의 모듈도 포함될 수 있음 
        module_utils/     # 역할에는 사용자 정의 module_utils도 포함될 수 있음 
        lookup_plugins/   # 또는 이 경우 조회와 같은 다른 유형의 플러그인

    webtier/              # "공통"과 같은 종류의 구조는 webtier 역할에 대해 수행됨
    monitoring/           # ""
    fooapp/               # ""
```

* 기본적으로 Ansible은 관련 콘텐츠(또한 main.yaml 및 main)에 대한 main.yml 파일의 역할 내의 각 디렉터리를 찾음
  * `tasks/main.yaml` : 역할이 실행하는 작업의 기본 목록
  * `handlers/main.yml` : 이 역할 내부 또는 외부에서 사용할 수 있는 핸들러
  * `library/my_moduel.py` : 이 역할 내에서 사용될 수 있는 모듈
  * `defaults/main.yml` : 역할의 기본 변수
    * 이러한 변수는 사용 가능한 변수 중 가장 낮은 우선 순위를 가지며
    * 인벤토리 변수를 비롯한 다른 변수로 쉽게 재정의할 수 있음
  * `vars/main.yml` : 역할에 대한 기타 변수
  * `files/main.yml` : 역할이 배포하는 파일
  * `templates/main.yml` : 역할이 배포하는 템플릿
  * `meta/main.yml` : 지원되는 플랫폼과 같은 선택적 Galaxy 메타데이터 및 역할 종속성을 포함하는 역할에 대한 메타데이터
* 일부 디렉터리에 다른 YAML 파일을 추가할 수 있음
  * 플랫폼별 작업을 별도의 파일에 배치하고 `tasks/main.yml` 파일에서 참조할 수 있음

```yaml
# roles/example/tasks/main.yml
- name: Install the correct web server for RHEL
  import_tasks: redhat.yml
  when: ansible_facts['os_family']|lower == 'redhat'

- name: Install the correct web server for Debian
  import_tasks: debian.yml
  when: ansible_facts['os_family']|lower == 'debian'

# roles/example/tasks/redhat.yml
- name: Install web server
  ansible.builtin.yum:
    name: "httpd"
    state: present

# roles/example/tasks/debian.yml
- name: Install web server
  ansible.builtin.apt:
    name: "apache2"
    state: present
```

* Role에는 라이브러리라는 디렉토리에 모듈 및 기타 플러그인 유형이 포함될 수도 있음

#### Role 저장 및 찾기

* 기본적으로 Ansible은 다음 위치에서 Role을 찾음
  * Collection에서 사용하는 경우
  * playbook 파일에 상대적인 `roles/` 라는 디렉토리에 있음
  * 구성된 `roles_path`에서 기본 검색 경로는 `~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles` 입니다.
  * 플레이북 파일이 있는 디렉토리에
* Role을 다른 위치에 저장하는 경우 Ansible이 역할을 찾을 수 있도록 `roles_path` 구성 옵션을 설정합니다
* 공유 역할을 단일 위치로 확인하면 여러 플레이북에서 더 쉽게 사용할 수 있음
* 또한 정규화된 경로를 사용해 Role을 호출할 수 있음

```yaml
---
- hosts: webservers
  roles:
    - role: '/path/to/my/roles/common'
```

#### Role 사용

* 세 가지 방법으로 Role을 사용할 수 있음
  * Role 옵션이 있는 play 수준
    * Play에서 Role을 사용하는 고전적인 방법
  * `include_role`을 사용하는 작업 수준
    * `include_role`을 사용해 play의 tasks 섹션 어디에서나 역할을 동적으로 재사용할 수 있음
  * `import_role`을 사용하는 작업 수준
    * `import_role`을 사용하여 play의 tasks 섹션 어디에서나 정적으로 역할을 재사용할 수 있음

#### Play 수준에서 Role을 사용

* Role을 사용하는 고전적인 방법은 주어진 Play에 대한 Role 옵션을 사용하는 것

```yaml
---
- hosts: webservers
  roles:
    - common
    - webservers
```

* 각 Role `x`에 대해 Play 수준에서 Role 옵션을 사용하는 경우
* `roles/x/tasks/main.yml` 이 있으면 Ansible은 해당 파일의 작업을 Play에 추가
* `roles/x/handlers/main.yml`이 있으면 Ansible은 해당 파일의 핸들러를 Play에 추가
* `roles/x/vars/main.yml` 이 있으면 Ansible은 해당 파일의 변수를 Play에 추가
* `roles/x/defaults/main.yml` 이 있으면 Ansible은 해당 파일의 변수를 Play에 추가
* `roles/x/meta/main.yml`이 있는 경우 Ansible은 해당 파일의 모든 Role 종속성을 Role 목록에 추가
* 복사, 스크립트, 템플릿 또는 포함 작업(Role 내)은 상대적으로 또는 절대적으로 경로를 지정할 필요 없이
  * `roles/x/{files,templates,tasks}/(작업에 따라 dir이 다름)` 의 파일을 참조할 수 있음
* Play 레벨에서 Role 옵션을 사용하는 경우
  * Ansible은 Role을 정적 가져오기로 취급하고 Playbook 구문 분석 중에 처리함
  * Ansible은 다음 순서로 각 Play를 실행
    * Play에 정의된 모든 `pre_tasks`
    * `pre_tasks`에 의해 트리거된 모든 핸들러
    * Role에 나열된 각 Role (나열된 순서대로)
      * Role의 `meta/main.yml`에 정의된 모든 Role 종속성은 태그 필터링 및 조건에 따라 먼저 실행됨
    * Play에 정의된 모든 작업
    * Role 또는 task에 의해 트리거되는 핸들러
    * Play에 정의된 모든 `post_tasks`
    * `post_tasks`에 의해 트리거된 모든 핸들러

***

* **참고! Role의 Task와 함께 Tag를 사용하는 경우**
  * `pre_tasks`, `post_tasks` 및 Role 종속성에도 태그를 지정하고 함께 전달해야 함
  * 특히 사전/사후 작업 및 Role 종속성이 중단 기간 제어
    * 또는 로드 밸런싱을 모니터링 하는데 사용되는 경우 더욱 그러함
* 다른 키워드를 Role 옵션에 전달할 수 있음

```yaml
---
- hosts: webservers
  roles:
    - common
    - role: foo_app_instance
      vars:
        dir: '/opt/a'
        app_port: 5000
      tags: typeA
    - role: foo_app_instance
      vars:
        dir: '/opt/b'
        app_port: 5001
      tags: typeB
```

* Role 옵션에 Tag를 추가하면 Ansible이 Role 내의 모든 작업에 태그를 적용함
* Playbook의 역할
  * Section 내에서 `vars:` 를 사용하면 변수가 Play 변수에 추가되어 Role 전후의 Play 내의 모든 작업에서 사용할 수 있음
  * 이 동작은 `DEFAULT_PRIVATE_ROLE_VARS`로 변경할 수 있음

#### Including roles: dynamic reuse (Role을 포함, 동적 재사용)

* `include_role` 을 사용하여 Play의 작업 섹션 어디에서나 Role을 동적으로 재사용할 수 있음
* Role 섹션에 추가된 역할은 play의 다른 job보다 먼저 실행되지만 포함된 Role은 정의된 순서대로 실행됨
  * `include_role` 작업 전에 다른 작업이 있으면 다른 작업이 먼저 실행됨
* Role을 포함하려면

```yaml
---
- hosts: webservers
  tasks:
    - name: Print a message
      ansible.builtin.debug:
        msg: "this task runs before the example role"

    - name: Include the example role
      include_role:
        name: example

    - name: Print a message
      ansible.builtin.debug:
        msg: "this task runs after the example role"
```

* Role을 포함할 때, 변수 및 태그를 비롯한 다른 키워드를 전달할 수 있음

```yaml
---
- hosts: webservers
  tasks:
    - name: Include the foo_app_instance role
      include_role:
        name: foo_app_instance
      vars:
        dir: '/opt/a'
        app_port: 5000
      tags: typeA
  ...
```

* `include_role` 작업에 태그를 추가하면 Ansible은 include된 자체에만 태그를 적용
  * 즉, 해당 작업 자체에 `include` 문과 동일한 태그가 있는 경우 `--tags`를 전달하여 Role에서 선택한 작업만 실행할 수 있음
  * Role을 조건부로 포함할 수 있음

```yaml
---
- hosts: webservers
  tasks:
    - name: Include the some_role role
      include_role:
        name: some_role
      when: "ansible_facts['os_family'] == 'RedHat'"
```

#### Importing roles: static reuse (역할 가져오기: 정적 재사용)

* `import_role`을 사용하여 Play의 작업 섹션 어디에서나 Role을 정적으로 재사용할 수 있음
* 동작은 `roles` 키워드를 사용하는 것과 동일

```yaml
---
- hosts: webservers
  tasks:
    - name: Print a message
      ansible.builtin.debug:
        msg: "before we run our role"

    - name: Import the example role
      import_role:
        name: example

    - name: Print a message
      ansible.builtin.debug:
        msg: "after we ran our role"
```

* Role을 가져올 때 변수 및 태그를 비록한 다른 키워드를 전달할 수 있음

```yaml
---
- hosts: webservers
  tasks:
    - name: Import the foo_app_instance role
      import_role:
        name: foo_app_instance
      vars:
        dir: '/opt/a'
        app_port: 5000
  ...
```

* `import_role`문에 태그를 추가하면 Ansible은 Role 내의 모든 작업에 태그를 적용

#### Role Argument Validation (Role 인수 검증)

* 버전 2.11부터 인수 사양을 기반으로 Role 인수 유효성 검사를 활성화하도록 선택할 수 있음
* 이 사양은 `meta/argument_specs.yml` 파일에 정의됨
* 이 인수 사양이 정의되면 사양에 대해 Role에 대해 제공된 매개변수의 유효성을 검사하는
  * Role 실행 시작 부분에 새 task가 삽입됨
* 매개변수가 유효성 검사에 실패하면 Role 실행이 실패함
