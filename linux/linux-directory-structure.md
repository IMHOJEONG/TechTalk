---
description: '-'
---

# Linux Directory Structure

#### /usr

[https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/usr.html](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/usr.html)

* 일반적으로 시스템에서 가장 큰 데이터 공유를 포함
  * 모든 사용자 바이너리, 문서, 라이브러리, 헤더 파일 등을 포함하는 시스템에서 가장 중요한 디렉토리 중 하나
  * X 및 지원 라이브러리는 여기에서 찾을 수 있음
* `telnet`, `ftp` 등과 같은 사용자 프로그램도 여기에 배치됨
* 원래 Unix 구현에서 `/usr` 은 사용자의 홈 디렉토리가 있는 위치였음
  * 즉, `/usr/someone`은 현재 `/home/someone`으로 알려진 디렉토리
* 현재 Unix에서 `/usr`는 사용자 영역 프로그램 및 데이터가 있는 곳
  * 시스템 영역 프로그램 및 데이터와 반대
  * 이름은 바뀌지 않았지만, `사용자와 관련된 모든 것` 에서 `사용자가 사용할 수 있는 프로그램 및 데이터` 로 의미가 좁혀지고 길어졌음
* 따라서 일부 사람들은 이제 이 디렉토리를 원래 의도했던 `사용자`가 아니라 `사용자 시스템 리소스` 를 의미하는 것으로 참조할 수 있음
*
