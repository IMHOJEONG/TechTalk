---
description: '-'
---

# Media Source Extensions

* MSE(Media Source Extensions)
  * 오디오 또는 비디오 세그먼트에서 재생할 스트림을 빌드할 수 있는 Javascript API



* 비디오를 사이트에 삽입하려면 MSE를 이해해야 함&#x20;
  * 장치의 기능 및 네트워크 환경에 적응하는 또 다른 방법인 적응형 스트리밍(Adaptive Streaming)
  * 광고 삽입과 같은 Adaptive Splicing
  * 성능 및 다운로드 크기 제어&#x20;



<figure><img src="../.gitbook/assets/SMMXsJT0g2ivhkxfB9Rc (1).avif" alt=""><figcaption></figcaption></figure>

* MSE를 체인으로 생각할 수 있음&#x20;
* 다운로드한 파일과 미디어 요소 사이에는 여러 레이어가 있음 (그림과  같이)



* 미디어를 재생하기 위한 \<audio> 또는 \<video> element
* 미디어 element를 공급하기 위한 SourceBuffer가 있는 MediaSource 인스턴스&#x20;
* Response 객체에서 미디어 데이터를 검색하기 위한 fetch() 또는 XHR 호출&#x20;
* MediaSource.SourceBuffer를 공급하기 위한 Response.arrayBuffer()에 대한 호출





* Google Shaka Player와 같은 라이브러리를 사용하는 것도 권장하기 때문&#x20;





{% embed url="https://web.dev/media-mse-basics/" %}



