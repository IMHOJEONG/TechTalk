---
description: '-'
---

# Inventory Basics : formats, hosts, groups (형식, 호스트 및 그룹)

[https://docs.ansible.com/ansible/latest/inventory\_guide/intro\_inventory.html#id4](https://docs.ansible.com/ansible/latest/inventory\_guide/intro\_inventory.html#id4)

* 가지고 있는 Inventory 플러그인에 따라 다양한 형식 중 하나로 Inventory 파일을 생성할 수 있음
  * 가장 일반적인 형식 : `INI`, `YAML`

```jsx
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

* `[]` : 괄호 안의 제목 ⇒ 호스트를 분류하고 어떤 호스트를 언제 어떤 목적으로 제어할지 결정하는 데 사용되는 그룹
  * 그룹 이름 : [유효한 변수 이름 만들기](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_variables.html#valid-variable-names) 와 동일한 규칙을 따라야 함

```yaml
all:
	hosts:
		mail.example.com:
	children:
		webservers:
			hosts:
				foo.example.com:
				bar.example.com:
		dbservers:
			hosts:
				one.example.com:
				two.example.com:
				three.example.com:
```

#### Default Groups : 기본 그룹

* Inventory 파일에 그룹을 정의하지 않은 경우에도 Ansible은 두 개의 기본 그룹(`all` 및 `ungrouped`)을 생성함
  * `all` 그룹 : 모든 호스트가 포함
  * `ungrouped` 그룹 : 다른 그룹이 없는 모든 호스트가 포함됨
* `all` 호스트는 항상 최소 2개의 그룹에 속함 (all & ungrouped 또는 all & some other group)
  * ex) [mail.example.com](http://mail.example.com) ⇒ `all` 과 `ungrouped` 그룹에 속함
  * ex) [two.example.com](http://two.example.com) ⇒ `all`과 `dbservers` 그룹에 속함
* all과 ungrouped는 항상 존재하지만 암시적일 수 있으며 group\_names와 같은 그룹 목록에 나타나지 않음

#### Hosts in multiple Groups : 여러 그룹의 호스트

* 각 호스트를 둘 이상의 그룹에 넣을 수 있음
* ex) 애틀렌타의 데이터 센터에 있는 프로덕션 웹 서버 ⇒ \[prod], \[atlanta], \[webservers]라는 그룹에 포함될 수 있음
* 대상 : 애플리케이션, 스택, 마이크로서비스(데이터베이스 서버, 웹 서버 등)
* 위치 : 로컬 DNS, 스토리지 등과 통신하기 위한 데이터 센터 또는 지역(west or east)
* 시점 : 프로덕션 리소스(ex-prod, test)에 대한 테스트를 피하기 위한 개발 단계

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    test:
      hosts:
        bar.example.com:
        three.example.com:
// [one.example.com](<http://one.example.com>) 을 dbservers, east, prod 그룹에서 볼 수 있음 
```

#### Grouping groups: parent/child group relationships

* 그룹을 그룹화 ⇒ 부모/자식 그룹 관계
* 그룹 간에 부모/자식 관계를 만들 수 있음
  * 상위 그룹 : 그룹의 그룹, 중첩 그룹이라고도 함
* ex) 모든 production 호스트가 이미 atlanta\_prod 및 denver\_prod와 같은 그룹에 있는 경우
  * 이러한 소규모 그룹을 포함하는 Production 그룹을 생성 가능
* 이 접근 방식 : 하위 그룹을 편집해 상위 그룹에서 호스트를 추가 or 제거할 수 있음 ⇒ 유지 관리를 줄임

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      children:
        east:
    test:
      children:
        west:
```

* Child groups(하위 그룹)에는 몇 가지 속성이 존재
  * 하위 그룹의 구성원인 모든 호스트는 자동으로 상위 그룹의 구성원이 됨
* Groups는 여러 부모와 자식을 가질 수 있지만 순환 관계는 아님
  * 호스트는 여러 그룹에 있을 수도 있지만, 런타임에는 호스트 인스턴스가 하나만 있음
  * Ansible은 여러 그룹의 데이터를 병합함

#### Adding ranges of hosts

* 유사한 패턴을 가진 호스트가 많은 경우 각 호스트 이름을 별도로 나열하지 않고 범위로 추가할 수 있음

```yaml
...
	webservers:
     hosts:
				www[01:50].example.com:
```

* Hosts의 숫자 범위를 정의할 때 ⇒ 보폭(Sequence 번호 사이의 증분)을 지정할 수 있음

```yaml
webservers:
     hosts:
				www[01:50:2].example.com:
```

* 숫자 패턴의 경우 원하는 대로 선행 0을 포함하거나 제거할 수 있음
* 알파벳 범위도 정의할 수 있음

```yaml
[databases]
db-[a:f].example.com
```

#### Passing multiple inventory sources (여러 인벤토리 소스 전달)

* 명령줄에서 여러 인벤토리 매개변수를 제공 or ANSIBLE\_INVENTORY를 구성해 여러 인벤토리 소스(디렉토리, 동적 인벤토리 스크립트 또는 인벤토리 플러그인에서 지원하는 파일)을 동시에 타켓팅할 수 있음
  * 스테이징 및 프로덕션과 같이 일반적으로 분리된 환경을 특정 작업에 대해 동시에 대상으로 지정하려는 경우에 유용

```bash
ansible-playbook get_logs.yml -i staging -i production
```

#### Organizing inventory in a directory (디렉토리에서 인벤토리 구성)

* 단일 디렉토리에 여러 인벤토리 소스를 통합할 수 있음
  * 간단한 버전 : 단일 인벤토리 파일 대신 여러 파일이 있는 디렉토리
* 단일 파일이 너무 길어지면 유지 관리가 어려워짐
* 여러 팀과 여러 자동화 프로젝트가 있는 경우
  * 팀 또는 프로젝트 당 하나의 인벤토리 파일이 있으면 ⇒ 모든 사람이 자신에게 중요한 호스트와 그룹을 쉽게 찾을 수 있음
* 인벤토리 디렉토리에서 여러 인벤토리 소스 유형을 결합할 수도 있음
  * 정적 및 동적 호스트를 결합하고 하나의 인벤토리로 관리하는 데 유용할 수 있음

```bash
# 디렉터리 구조 
inventory/
  openstack.yml          # configure inventory plugin to get hosts from OpenStack cloud
  dynamic-inventory.py   # add additional hosts with dynamic inventory script
  on-prem                # add static hosts and groups
  parent-groups          # add static hosts and groups
```

* 아래와 같은 명령어로 inventory 디렉터리를 선택할 수 있음

```bash
ansible-playbook example.yml -i inventory
```

* 또한, `ansible.cfg` 파일에서 인벤토리 디렉토리를 구성할 수도 있음

#### Managing inventory load order (Inventory 로드 순서를 관리)

* Ansible 은 파일 이름에 따라 ASCII 순서로 인벤토리 소스를 로드
* 한 파일 또는 디렉토리에서 상위 그룹을 정의
  * 다른 파일 또는 디렉토리에서 하위 그룹을 정의하는 파일을 먼저 로드해야 함
* 상위 그룹이 먼저 로드되면`Unable to parse /path/to/source_of_parent_groups as an inventory source` 에러가 발생합니다
* ex) `on-prem` 파일에 정의된 하위 그룹이 있는 `production`을 정의하는 `group-of-groups`라는 파일이 있는 경우
  * Ansible은 `production` 그룹을 구문 분석할 수 없음
  * 이 문제를 방지하려면 파일에 접두사를 추가해 로드 순서를 제어해야 함

```bash
inventory/
  01-openstack.yml          # configure inventory plugin to get hosts from OpenStack cloud
  02-dynamic-inventory.py   # add additional hosts with dynamic inventory script
  03-on-prem                # add static hosts and groups
  04-groups-of-groups       # add parent groups
```

#### Adding variables to inventory (인벤토리에 변수 추가)

* 인벤토리의 특정 호스트 또는 그룹과 관련된 변수 값을 저장할 수 있음
* 단순화를 위해 기본 인벤토리 파일에 변수 추가 가능
  * 별도의 호스트 및 그룹 변수 파일에 변수를 저장하는 것이 시스템 정책을 설명하는 보다 강력한 접근 방식

#### Organizing host and group variables

* list와 hash 데이터를 호스트 및 그룹 변수 파일에서 사용할 수 있지만, 기본 인벤토리 파일에선 수행할 수 없음
* 호스트 및 그룹 변수 파일은 YAML 구문을 사용해야 함
* Ansible은 인벤토리 파일 또는 playbook 파일과 관련된 경로를 검색해 호스트 및 그룹 변수 파일을 로드
  * `/etc/ansible/hosts` 의 인벤토리 파일에 `raleigh` , `webservers` 의 두 그룹에 속하는 `foosball`이라는 호스트가 포함된 경우
    * 해당 호스트는 다음 위치에 있는 YAML 파일의 변수를 사용

```bash
/etc/ansible/group_vars/raleigh # can optionally end in '.yml', '.yaml', or '.json'
/etc/ansible/group_vars/webservers
/etc/ansible/host_vars/foosball
```

* 예를 들어, 인벤토리의 호스트를 데이터 센터 별로 그룹화하고,
  * 각 데이터 센터가 자체 NTP 서버와 데이터베이스 서버를 사용하는 경우
    * `/etc/ansible/group_vars/raleight` 라는 파일을 생성해 `raleigh` 그룹에 대한 변수를 저장할 수 있음

```bash
ntp_server: acme.example.org
database_server: storage.example.org
```

*   그룹 또는 호스트 이름을 딴 디렉토리를 만들 수 있음

    * Ansible은 이러한 디렉토리의 모든 파일을 사전순으로 읽음

    ```bash
    /etc/ansible/group_vars/raleigh/db_settings
    /etc/ansible/group_vars/raleigh/cluster_settings
    ```

    * `raleight` 그룹의 모든 호스트는 이러한 파일에 정의된 변수를 사용할 수 있음
      * 단일 파일이 너무 커지거나 일부 그룹 변수에 Ansible Vault를 사용하려는 경우 변수를 체계적으로 유지하는 데 매우 유용할 수 있음
* ansible-playbook의 경우 group\_vars/ 및 host\_vars/ 디렉토리를 플레이북 디렉토리에 추가할 수도 있음
  * 다른 Ansible 명령
    * ansible, ansible-console 등 ⇒ 인벤토리 디렉토리에서 group\_vars/ 및 hosts\_vars/ 만 찾음
  * 다른 명령으로 플레이북 디렉터리에서 그룹 및 호스트 변수를 로드하려면
    * 명령줄에 `--playbook-dir` 옵션을 제공해야 함
  * 플레이북 디렉터리와 인벤토리 디렉토리 모두에서 인벤토리 파일을 로드하면
    * 플레이북 디렉토리의 변수가 인벤토리 디렉터리에 설정된 변수보다 우선함
    * 플레이북 디렉토리의 변수가 인벤토리 디렉터리에 설정된 변수보다 우선함
* 인벤토리 파일 및 변수를 git repo에 보관하는 것은 인벤토리 및 호스트 변수의 변경 사항을 추적하는 훌륭한 방법

#### How variables are merged (변수가 병합되는 방법)

* 기본적으로 변수는 play가 실행되기 전에 특정 호스트에 병합/평면화됨
  * 이렇게 하면 Ansible이 호스트와 작업에 집중할 수 있음
    * Group은 Inventory 및 host 일치 외부에서 실제로 살아남지 못함
* 기본적으로 Ansible은 그룹 및/또는 호스트에 대해 정의된 변수를 포함해 변수를 덮어씀
* DEFAULT\_HASH\_BEHAVIOR 참조?
  * [https://docs.ansible.com/ansible/latest/reference\_appendices/config.html#default-hash-behaviour](https://docs.ansible.com/ansible/latest/reference\_appendices/config.html#default-hash-behaviour)
* 순서 / 우선순위는 (낮은 ⇒ 높은)
  * all group ⇒ parent group ⇒ child group ⇒ host
* 기본적으로 Ansible은 동일한 상위/하위 수준의 그룹을 ASCII 순서로 병합
  * 마지막으로 로드된 그룹의 변수가 이전 그룹의 변수를 덮어씀
  *
