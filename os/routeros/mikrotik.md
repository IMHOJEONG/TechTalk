---
description: '- How to limit Netflix, Facebook & Youtube bandwidth using Mangle Rules'
---

# mikrotik



* WinBox에서 진행 => IP => Firewall&#x20;
  * Mangle, Address lists
  * chain은 prerouting으로 자동으로 만들어지는듯&#x20;
  * New Mangle Rule => Action&#x20;
    * Action : add dst to address list&#x20;
    * Address List : Facebook Servers&#x20;
    * Timeout : 30d 00:00:00
    * comment for New Mangle Rule => Facebook IPs
    *
    * comment for New Mangle Rule => Netflix IPs
    * Timeout : 30d 00:00:00
    * Address List : Netflix Servers&#x20;
    *
  * Queue List?&#x20;
    * Queue Tree애다가 Traffic을 정의하고 Download, Upload 트래픽의 제약을 정의함&#x20;



* L3에서 제약이 가능한 것으로 테스트 => Mangle?



[https://www.youtube.com/watch?v=sGKU\_rb8XxI](https://www.youtube.com/watch?v=sGKU\_rb8XxI)
