# Loops

* Ansible은 작업을 여러 번 실행하기 위해 `with_<lookup>` 및 키워드까지 루프를 제공&#x20;
  * 일반적으로 사용되는 루프의 예
  * 파일 모듈을 사용해 여러 파일 및 디렉토리에 대한 소유권 변경&#x20;
  * 사용자 모듈을 사용해 여러 사용자 만들기&#x20;
  * 특정 결과에 도달할 때까지 폴링 단계 반복 등&#x20;



* `loop` 키워드를 2.5 버전 이후부터 추가했고, `with_<lookup>`을 완전히 대체하지는 않지만 대부분의 사용 사례에 권장함&#x20;

### \`loop\` 키워드와 \`with\_\*\` 의 비교&#x20;

* `with_lookup` Lookup plugin에 의존
  * 심지어 item도 조회
* loop 키워드는 with\_list와 동일하며 간단한 루프에 적합&#x20;
* loop 키워드는 문자열을 입력으로 허용하지 않음&#x20;
* 일반적으로 with\_X에서 루프로 마이그레이션에서 다룬 with\_\*의 모든 사용은 루프를 사용하도록 업데이트할 수 있음&#x20;
* with\_items는 암시적 단일 수준 병합을 수행&#x20;
  * with\_items를 루프로 변경할 때 주의
  * 정확한 결과와 같으려면 loop와 함께, flatten을 사용해야 할 수도 있음&#x20;

```yaml
with_items:
    - 1
    - [2,3]
    - 4
    
loop: "{{ [1, [2, 3], 4] | flatten(1) }}"
```

* loop 내에서 조회를 사용해야 하는 with\_\*문
  * 루프 키워드를 사용하도록 변환하면 안 됨&#x20;

```yaml
loop: "{{ lookup('fileglob', '*.txt
', wantlist=True)}}"

with_fileglob: '*.txt'
```

### 기본 루프&#x20;

#### 간단한 리스트의 순회

* 반복되는 작업은 간단한 문자열 목록에 대한 표준 루프로 작성할 수 있음&#x20;
  * 작업에서 직접 목록을 정의할 수 있음&#x20;

```yaml
- name: Add several users
  ansible.builtin.user:
    name: "{{ item }}"
    state: present
    groups: "wheel"
  loop:
     - testuser1
     - testuser2
```

*
*
*
*
* [https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_loops.html](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_loops.html)
