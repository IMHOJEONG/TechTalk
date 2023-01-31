---
description: '-'
---

# Out of memory: Killed process 2243 (java) total-vm:5347792kB, anon-rss:1732152kB, file-rss:0kB, shme

* ![](<.gitbook/assets/Screenshot from 2023-01-18 11-07-10 (1).png>)
*   #### 현상

    * Nexus를 올리던 중 위와 같은 에러를 기반으로 docker container가 떨어지는 증상
    * 하나씩 자세히 알아보자
    * 뭐… 필요한 메모리를 늘리면 되는데, 그걸로 만족하는가?

    #### 문제 해결

    * 용어 정리가 필요
    * `total-vm`
    * `anon-rss`
    * `file-rss`
    * `shmem-rss`
    * `UID`
    * `pgtables`
    * `oom_score_adj`

    [https://ssup2.github.io/theory\_analysis/Linux\_OOM\_Killer/](https://ssup2.github.io/theory\_analysis/Linux\_OOM\_Killer/)

    #### OOM Killer

    * Out Of Memory Killer

    #### Linux는 실제 물리 Memory보다 많은 양의 가상 Memory 공간을 생성하고 Process에게 할당

    * Memory Overcommit

    [https://www.baeldung.com/linux/memory-overcommitment-oom-killer](https://www.baeldung.com/linux/memory-overcommitment-oom-killer)

    #### Memory Overcommit이란 정책은 무엇인가?

    * Linux 운영 체제에는 메모리를 관리하는 특정 방법이 있음
    * 정책 중 하나
      * 애플리케이션이 원하는 만큼의 메모리를 미리 예약할 수 있도록 허용하는 Overcommit
      * 그러나 약속한 메모리는 실제 사용 시 사용하지 못할 수 있음
      * 시스템은 메모리 부족을 방지하기 위한 특별한 방법을 제공해야 함
    * 시스템 안정성을 위해 애플리케이션을 제거하는 프로세스인
      * OOM(Out-Of-Memory) Killer에 대해 알아보자

    #### OOM Killer가 호출되었을 때

    * killer가 작동하려면 시스템에서 overcommit을 허용해야 함
    * 그런 다음 각 프로세스를 제거함으로써 시스템이 얻을 수 있는 이익에 따라 점수를 매김
    * 마지막으로 메모리 부족 상태가 되면 커널은 점수가 가장 높은 프로세스를 죽임
    * `/proc/PID/oom_score` 파일에서 `PID`로 프로세스의 점수를 찾을 수 있음
    * 이제 터미널을 시작하고 \$$ 변수에 PID가 있음



![](<.gitbook/assets/Screenshot from 2023-01-18 11-07-10 (5).png>)

```bash
#!/bin/bash

while read -r pid comm
do
    printf '%d\\t%d\\t%s\\n' "$pid" "$(cat /proc/$pid/oom_score)" "$comm"
done < <(ps -e -o pid= -o comm=) | sort -k2 -n
```

* `oom_score_reader` 스크립트를 사용해 `oom_score` 기준으로 가장 낮은 것부터 가장 높은 것까지 정렬하여
  * PID 및 이름과 함께 모든 프로세스를 나열해보자
* 프로세스 치환(대체)를? 사용해 PS의 결과를 read 명령에 제공
  * Process Substitution : 프로세스 치환
  * [http://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/process-sub.html](http://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/process-sub.html)

![](<.gitbook/assets/Screenshot from 2023-01-18 11-07-10 (6).png>)

* 점수는 0에서 1000 사이의 값을 가짐
  * `oom_score`의 값 0은 프로세스가 OOM killer로부터 안전하다는 것을 의미

#### OOM Killer로부터 프로세스 보호

* 프로세스 제거 가능성을 낮출 필요가 있습니다
* 수명이 긴 프로세스와 서비스에 특히 중요
  * 이러한 프로세스를 위해 `oom_score_adj` 매개변수를 설정해야 함
* 이 매개변수는 -1000에서 1000까지 범위의 값을 사용
  * 따라서 음수 값은 `oom_score`를 감소시켜 killer에게 프로세스를 덜 매력적으로 만들어줌
  * 반대로 양수 값은 점수를 상승시킴
  * `oom_score_adj = -1000` 인 프로세스는 종료에 영향을 받지 않음
* `/proc/PID/oom_score_adj` 파일에서 매개변수를 확인해야 함

```bash
cat /proc/$$/oom_score_adj
```

<figure><img src=".gitbook/assets/Screenshot from 2023-01-18 13-16-29.png" alt=""><figcaption></figcaption></figure>



* 터미널의 점수가 어느 방향으로도 조정되지 않았는지 확인

#### 수동으로 `oom_score_adj` 설정

* 가장 간단한 방법으로 `oom_score_adj` 파일에 직접 쓸 수 있음
* 먼저 프로세스의 점수를 확인
* PID를 얻으려면 pgrep이 필요

```bash
cat /proc/$(pgrep dockerd)/oom_score
```



<figure><img src=".gitbook/assets/Screenshot from 2023-01-18 13-20-22.png" alt=""><figcaption></figcaption></figure>

```bash
$ echo 500 > /proc/$(pgrep firefox)/oom_score_adj

$ cat /proc/$(pgrep firefox)/oom_score

$ sudo echo -30 > /proc/$(pgrep firefox)/oom_score_adj

$ cat /proc/$(pgrep firefox)/oom_score
```

* 조정 계수를 0 미만으로 줄이려면 `sudo` 권한이 필요함

#### choom 커멘드 존재

* choom을 사용해 점수를 보고하고 조정을 수정할 수 있음
* `util-linux` 패키지의 일부
  * 이제 p 스위치(`-p`)를 사용해 Firefox 프로세스를 한 번 더 확인해보자
  * `-n` 스위치와 함께 `oom_score_adj`의 새 값을 제공해 점수를 높임
  * `choom`을 사용하면 주어진 `oom_score_adj`로 바로 프로세스를 시작할 수 있음

```bash
$ choom -p $(pgrep firefox)

pid 3061's current OOM score: 40
pid 3061's current OOM score adjust value: -30

$ choom -p $(pgrep firefox) -n 300
pid 3061's OOM score adjust value changed from -30 to 300

$ choom -p $(pgrep firefox)
pid 3061's current OOM score: 371
pid 3061's current OOM score adjust value: 300

$ choom -n 300 firefox

$ choom -p $(pgrep firefox)
pid 3061's current OOM score: 346
pid 3061's current OOM score adjust value: 300
```

#### 서비스 구성

* 서비스의 경우, `/etc/systemd` 폴더에 있거나 링크된 서비스 구성에서 점수를 영구적으로 조정할 수 있음
* 서비스 부분에서, `OOMScoreAdjust` 항목을 편집해야 함

```bash
[Unit]
Description=Snap Daemon

# some output skipped

[Service]

# some output skipped

OOMScoreAdjust=-900
ExecStart=/usr/lib/snapd/snapd

# more output skipped
```

#### OOM\_SCORE는 어떻게 계산되는가?

* 점수 계산 방법은 커널 버전에 따라 다름
* 메모리 공간 외에도 이전 릴리스들은 실행 시간, nice level, root 권한이 반영되기도 했음
* 그러나 버전 5에서는 총 메모리 사용량만 중요
  * 이를 확인? `oom_kill.c` 소스 파일에서 `oom_badness` 함수를 검사해야 할 필요가 존재
* 먼저, 함수는 프로세스가 `immune` 인지 확인
  * 일반적으로 `oom_score_adj=-1000`인 덕분에 ⇒ 이 경우 작업은 0점을 얻음
  * 그렇지 않으면, 작업의 RAM, 가상 메모리 및 스왑 공간 크기가 합산됨
    * 그런 다음 결과를 사용가능한 총 메모리로 나눔
  * 마지막 이 함수는 비율을 1000으로 정규화시킴
    * 이 때, `oom_score_adj` 가 작동함
      * 따라서 점수에 추가됨
      * 음수 값은 실제로 이 값을 줄이게 됨

#### Overcommit을 제어하기 위한 시스템 전체 매개변수

* `overcommit_memory` 매개변수를 설정해 Linux 시스템의 Overcommit 정책을 변경할 수 있음
*   매개변수는 `/proc/sys/vm/overcommit_memory` 파일에 있음

    <figure><img src=".gitbook/assets/Screenshot from 2023-01-18 11-07-10.png" alt=""><figcaption></figcaption></figure>
* `0` 은 적당한 Overcommit을 허용 (기본 설정)
  * 그러나 무리한 메모리 할당은 실패
  * `1` : 항상 오버 커밋
  * `2` : 오버 커밋을 허용하지 않음
* 일반적으로 프로세스는 OOM killer에 의해 종료되지 않지만, 메모리 할당 시도는 오류를 반환할 수 있음
* 기본 정책 이외의 정책을 사용하면 응용 프로그램이 메모리를 처리하는 방식에 더 큰 요구 사항이 부과된다는 점



