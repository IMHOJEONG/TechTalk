---
description: '-'
---

# chatGPT로 블로그 리소스 생성하기

* chat.openai.com
  * 들어가서 회원가입 진행&#x20;
  * 첫 번째 페이지&#x20;
    * Our goal is to get external feedback in order to improve our systems and make them safer. 🚨 While we have safeguards in place, the system may occasionally generate incorrect or misleading information and produce offensive or biased content. It is not intended to give advice
    * ```
      우리의 목표는 시스템을 개선하고 더 안전하게 만들기 위해 외부 피드백을 받는 것입니다.
      🚨
      우리는 보호 장치를 갖추고 있지만 때때로 시스템이 부정확하거나 오해의 소지가 있는 정보를 생성하고 공격적이거나 편향된 콘텐츠를 생성할 수 있습니다. 조언을 드리기 위함이 아닙니다
      ```
  * 데이터 수집 방법&#x20;
    * 🦾 Conversations may be reviewed by our AI trainers to improve our systems. 🔐 Please don't share any sensitive information in your conversations.
    * ```
      시스템을 개선하기 위해 AI 트레이너가 대화를 검토할 수 있습니다.
      🔐
      대화에서 민감한 정보를 공유하지 마십시오.
      ```



<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 13-30-16.png" alt=""><figcaption><p>ChatGPT의 첫 화면 </p></figcaption></figure>

ChatGPT의 첫 화면&#x20;



* 질문을 구체적으로 작성하는 게 chatGPT를 잘 아는 방법&#x20;
* 요즘 미국 대학생들이 숙제로 이미 chatGPT에게 많이 일을 시킨다고 함&#x20;
  * Write a outline of report about pros and cons of youtube

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 13-34-44.png" alt=""><figcaption></figcaption></figure>

* 각 토픽에 대해 자세히 써 달라고 하면?

![](<../.gitbook/assets/Screenshot from 2023-01-25 13-36-58.png>)

* 이 맥락을 그대로 가져와서 답변을 해줌&#x20;
* 이 동일한 대화 내에서는 얘가 일관적인 맥락을 가지고 답변을 해줌&#x20;
  * 이 질문을 지속적으로 잘해주게 되면&#x20;
    * 한 대화 창 내에서 연속적으로 꼬리 질문을 하게 되면
    * 훨씬 더 퀄리티있는 답변을 얻어낼 수가 있음&#x20;
* 더 질문을 많이 하거나, 구체적인 요구사항을 물어보게 되면&#x20;
  * 더 정확한 답변, 더 유용하고 새로운 답변이 나올 수 있음&#x20;
* chatGPT를 이용하면 이렇게 글 작성, 리포트 작성, 논문 작성도 가능
  * 리서치할 때 주요 정보들을 정리해서 보여주는 것도 가능하고&#x20;
  * 아니면 긴 글 같은 것들을 요약하라고 시키면 또 요약도 잘해줌&#x20;
* 글 뿐만 아니고, 음악 랩 소설 => 이런 새로운 어떤 컨텐츠를 만드는 작업까지도 잘해준다고 함&#x20;

\---

* 자동 블로그를 만들어서 수익화?
  * 어떤 글을 쓸까 고민 => 트래픽을 많이 발생시킬 수 있는 블로그여야 될 거다?
  * 그렇다면 꼭 한글일 필요가 없고, 제 전문 분야일 필요가 없음&#x20;
  * 그래서 한글이 아니고 더 퀄리티있는 답변이 나오는 영어 블로그를 제가 한 번 써보려고 했음&#x20;
* 블로그 인기 순위에는 테크, 정치, 시사 관련 블로그가 많음&#x20;
  * chatGPT의 경우 => 2021년 데이터까지 학습된 챗봇이 응답을 해주는 것&#x20;
  * 아주 최근 데이터는 답변을 잘 해주지 못함&#x20;
  * 최근 트렌드를 반영한 컨텐츠는 잘 생성을 못함&#x20;
  * 그래서 시기적절성이 좀 중요하지 않은 주제를 좀 고민하다 보니
    * 라이프스타일, 건강, 육아 등&#x20;
    * 글 주제를 다양하고, 이미지를 쉽게 생성 가능한 블로그 생성하고자 함&#x20;

\---&#x20;

* 블로그 초기 세팅을 chatGPT 받아서 해보자&#x20;

1. 사이트 이름(브랜드 네임)을 추천받는 것&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 13-48-18.png" alt=""><figcaption><p>브랜드 이름 추천</p></figcaption></figure>

* chatGPT가 위처럼 질문하면 적절한 이름을 추천해주게 됨&#x20;
* chatGPT는 질문을 하더라도 매번 다른 답변을 해주게 됨&#x20;
* 바로 그냥 해보는 그런 생생한 느낌으로 하고 있음&#x20;
* 마음에 안 들어서 좀 더 유니크하고 심플한 이름을 추천해 달라고 해보자&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 13-52-25.png" alt=""><figcaption></figcaption></figure>

* 맥락을 기억하고 있어서, 이 주제에 맞는 블로그 이름이지만 앞에서 추천되었던 것 중, 이러한 조건이 추가된 새로운 이름을 추천해줄 것&#x20;
* 이것도 나쁘지 않지만, 은유를 이용해 좀 더 예술적인 이름을 추천해달라고 해보자&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 13-55-14.png" alt=""><figcaption></figcaption></figure>

* AI의 흐름을 따라 만들자&#x20;
* 제 블로그 주제에 따른 카테고리 별로 더 추천해주게 됨&#x20;
* 이렇게 해보고 아쉬운 부분이 있으면 수정하고&#x20;
  * 비슷한 할 만한 주제까지 chatGPT가 추천해주고 있음&#x20;
*

    <figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 13-59-36.png" alt=""><figcaption></figcaption></figure>

    * 사용자의 의도에 맞추어, 아예 그 사용자가 언급하지 않는 어떤 창조적인 활동을 하는 게
      * 이 인공지능 서비스가 굉장히 놀라운 부분&#x20;
    * 그리고 중간에 답변이 끊어지는 경우 그냥 계속 continue 해달라고 하면 됨&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 14-04-16.png" alt=""><figcaption></figcaption></figure>

\----

* API를 이용해서 완전히 자동화하려면?
* 지금까지 만든 블로그 제목과 추천해준 카테고리, 주제들을 csv 형태로 저장을 해서&#x20;
  * 자동화의 허브 역할을 할 에어테이블이라는 노코드 데이터베이스에 넣어줄 것&#x20;
  * 그 데이터를 기반으로 해서 이후에 API를 활용해서 글 쓰는 걸 자동화하는 거를 세팅을 해볼 것&#x20;



* CSV 형태로 저장하는 것도 chatGPT에게 명령을 해보도록 하자&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 14-08-58.png" alt=""><figcaption></figcaption></figure>

* 하다가 만드는 거 멈추면, 또 다시 continue 해달라고 하자&#x20;
* 그리고 나서 ubuntu의 기본 프로그램인 libre office에서&#x20;
  * paste special => Untitled Text&#x20;
  * 옵션에서 seperate Character를 comma로 나누어주면&#x20;
  * csv 파일 형식으로 텍스트를 구분해 줄 수 있습니다.&#x20;
*   [https://airtable.com/](https://airtable.com/)

    * 에어테이블이라는 데이터베이스에 업로드?
    * 에어테이블을 쓰는 이유 => 향후에 블로그 발행을 자동화하기에 액셀이나, 구글 시트에 비해서 편하기 때문&#x20;



![](<../.gitbook/assets/Screenshot from 2023-01-25 14-30-42.png>)



*   주제(타이틀)를 기반으로 해서 이제 글을 생성하는 것도 chatGPT를 통해서 할 것&#x20;

    * 블로그 작성을 시키기 위해서 요구사항을 구체적으로 적어주는 것&#x20;
    * 배경과 해야 하는 일, 그리고 그 일의 구체적인 형식에 대해서 이렇게 언급을 해주면&#x20;
      * 이것에 맞춰서 이렇게 글을 생성해줍니다.&#x20;


* I am writing blog about ideas on how to adapt to new changes and trends in lifestyle, health, food, career and personal growth. I'd like to give useful information in life to my readers
  * Write a blog about "Mindful living tips". Its category is lifestyle.
  * Write a long blog articles around 2000 words in markdown format, and include subtitles and detail description, and do not include main title.



* 위의 문장을 chat에 넣어 블로그 글을 생성한다&#x20;
  * 요약도 가능&#x20;
  * 나중에 블로그를 자동 발행할 때 필요해서 제가 생성하라고 한 것&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 14-41-59.png" alt=""><figcaption></figcaption></figure>

* 또한, 해시태그들도 생성이 가능합니다&#x20;

![](<../.gitbook/assets/Screenshot from 2023-01-25 14-43-05.png>)

* 10개의 해시태그를 콤마로 연결해서 써달라?

![](<../.gitbook/assets/Screenshot from 2023-01-25 14-44-53.png>)

* 해시태그를 10개 만드는데, 콤마를 구분자로 넣어서 활용해줄 수 있음&#x20;



* 이미지를 첨부해서 활용이 가능하다&#x20;
  * 이런 식으로 이미지를 마크다운으로 넣어주고 바로 이미지를 넣어줌&#x20;
  * unsplash라는 이미지를 저작권 없이 쓸 수 있는 고해상도 이미지를 제공해주는 사이트의 API를 이용해가지고&#x20;
  * 여기에 적절한 이미지를 추가해주어라 이런식으로도 쓸 수가 있음&#x20;
* \[INFO: you can add images to the reply by Markdown, Write the image in Markdown without backticks and without using a code block. Use the Unsplash API ([https://source.unsplash.com/1600x900/](https://source.unsplash.com/1600x900/)?\<PUT YOUR QUERY HERE>). the query is just some tags that describes the image] ## DO NOT RESPOND TO INFO BLOCK ##nnmy Next prompt is \[INSERT]\

* Give me a picture using for blog cover fits to this article
* 이것을 이용해서 블로그 커버로 쓸 이미지를 추천해달라\~\~
  * 영상 그대로 하면 오류가 발생하는데 아마도 \n표시의 오류인듯
* \[INFO : you can add images to the reply by Markdown, Write the image in Markdown without backticks and without using a code block. Use the Unsplash API ([https://source.unsplash.com/1600x900/?)](https://www.youtube.com/redirect?event=comments\&redir\_token=QUFFLUhqbTRvV3pxMmVLTmNqWlVwZ25HWTlrLUo1eGlnd3xBQ3Jtc0tsSjNKZkw5LWpXb2VmNW5mV1BMblNHMlN6NnZFMkk0OFFzdC1RV3E0NkhSamRRNWJVd1laUmgxdnBzS2xmNGloMUlHWDYwMW9YTGl2YUZESnZZbWdBTTBXV3FNUG1GSEpGaXg1MGR5OTBLeG9zUHNyQQ\&q=https%3A%2F%2Fsource.unsplash.com%2F1600x900%2F%3F%29\&stzid=UgxzkFewzrqdv08Msn54AaABAg). the query is just some tags that describes the image] ## DO NOT RESPOND TO INFO BLOCK ##\n\nmy Next prompt is give me a picture of \[원하시는 그림 설명]
* 이 chat을 입력하면 됩니다.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 14-55-14.png" alt=""><figcaption></figcaption></figure>

* API라는 것을 이용해 요렇게 이미지까지 생성을 해줌&#x20;
  * 이런식으로 chatGPT의 프롬프트를 이용해서 텍스트도 만들고 서머리도 만들고, 해시태그도 만들고, 이미지도 만듬&#x20;
  * [https://www.reddit.com/r/ChatGPTPromptGenius/comments/zyf4l2/chatgpt\_unsplash\_beta/](https://www.reddit.com/r/ChatGPTPromptGenius/comments/zyf4l2/chatgpt\_unsplash\_beta/)

\---



*   자동화 동작을 zapier를 통해서 한번만 세팅해주면&#x20;

    * 여기 에어테이블에 기본적인 소스가 되는 타이틀 정도만 세팅을 해주면&#x20;
    * 그 뒤 여러가지 서머리, 본문 텍스트, 해시태그, 이미지 같은 것도 다 생성을 해주는 것&#x20;


* 에어테이블에서 같은 데이터가 들어갈 빈 필드들을 미리 생성을 해둠&#x20;
* 특정 레코드를 생성했을 대 오토메이션 뷰에 들어가면서&#x20;
  * 이 뷰에 들어간 걸 트리거로 해서 이 내용들이 채워지도록&#x20;
  * 자동화할 필요가 있음&#x20;
* 이런식으로 zapier라는 자동화 툴을 통해서&#x20;
  * 요 블로그에 들어갈 컨텐츠를 자동으로 생성하자
* 그렇게 하려면 chatGPT에서 했던 그 프롬프트로 질문하는 내용을&#x20;
  * zapier라는 자동화 툴을 통해서 진행하고&#x20;
  * 그 결과물을 이제 내 에어테이블에 업데이트하는 게 필요함&#x20;
* 그렇게 하기 위해서 이 OpenAI의 API라는 것에 접근 권한을 받아야 함&#x20;
* zapier에 연결을 하고 싶으면 OpenAI의 API를 신청해야 한다
  * 아주 약간의 크레딧을 주고 playground에서 질문을 점검할 수 있음&#x20;
  * chatGPT보다는 좀 더 뭐 디테일한 설정을 할 수 있음&#x20;



* API를 사용한 만큼 비용을 내게 됨&#x20;



* OpenAI의 API를 이용해서 프롬프트를 날리고&#x20;
  * 생성된 컨텐츠를 제 에어테이블로 가져오도록 하자&#x20;
  *

<figure><img src="../.gitbook/assets/Screenshot from 2023-01-25 15-25-31.png" alt=""><figcaption></figcaption></figure>

* Airtable에 대한 zapier의 연결 설정&#x20;
* 하나를 넣어주어야 zapier 테스트를 통과합니다&#x20;



* 프롬프트 날리는 동작을 openAI 앱을 이용해서 옵션을 통해 하는 것&#x20;



* 최대 길이가 몇 개의 내 유료 토큰을 소비해가지고&#x20;
  * 얼마나 길게 컨텐츠를 생성할 것이냐 물어보는 것&#x20;



* zap의 액션이 별개로 가면은 chatGPT에서처럼 그 맥락을 가져오는 게 아니고&#x20;
  * 별도의 대화가 시작되는 걸로 이 프롬프트 자동화는 동작을 함&#x20;
  * 그렇기 때문에 이전 결과를 prompt에 넣어주어야 한다&#x20;
* 이미지를 가져와서 쓰려면 최종적으로는 이미지 URL이 필요함&#x20;
* GPT-3 API로 생성할 때 앞단에 빈 여백을 얘가 포함한 채로 이 텍스트를 생성해 줌&#x20;



*   이 태그를 집어넣을 때도 맨 앞칸의 한 칸에 띄어쓰기에 있기 때문에

    * 제대로 안 들어가는 문제가 발생한 것&#x20;


* 이것을 제대로 하기 위해&#x20;
  * 두 가지 작업을 하심&#x20;
  * 이 태그로 생성한 얘를 zapier의 Formatter라는 모듈을 이용해서&#x20;
    * 공백을 없애도록 처리를 해주심&#x20;
* 띄어쓰기가 앞에 있어서 제대로 쉼표별로 분리해서 태그가 들어가게 됩니다
* 위처럼 모든 tag를 zapier에서 하게 되면 과금이 너무 심함&#x20;
  * 그래서 값이 업데이트가 되면은 이 summary나 body나 image URL의 공백이 제거되도록&#x20;
  * Modify-Trim 기능을 이용해서 처리&#x20;



* Trim-Whitespace를 GPT3 API 프롬프트에다가 시켜봤는데&#x20;
  * Whitespace를 정리하라고 시켜도 못함
* 그래서 어쩔 수 없이 이 zapier의 formatter 모듈과 에어테이블 오토메이션으로 자동화처리를 함&#x20;



* 애석하게도 이 메인이미지에 이 이미지 URL을 가지고&#x20;
  * 바로 이미지를 집어넣는 게 불가능&#x20;
  * 그래서 별도의 필드를 생성해서 이 부분에다가&#x20;
  * 여기다 생성한 URL을 집어넣고 이 URL을 기반으로 해서 메인 이미지를 업데이트하는 방식&#x20;
    *   한 번의 프로세스를 더 거치는 방식&#x20;


* 정상적으로 업데이트가 되었으면 성공 필드에다가 TRUE 체크 표시를 하도록 설정&#x20;
  * 실패 시 파악하기 위함&#x20;



* 메인 이미지로 최종적으로 파일 형태로 집어넣기 위해선&#x20;
  * AirTable extension 중에 URL을 attachment로 바꾸는 이런 스크립트가 있어서&#x20;
  * 최종적으로 요 이미지가 파일로 들어가는 것까지 자동화를 함&#x20;



*   처음 하시는 분들은 따라하기 힘들 정도의 에러케이스가 있었음&#x20;

    * GPT3 API를 사용하는데 약간 어려움이 있더라&#x20;


* Webflow를 이용해 이 자동으로 생성된 블로그 컨텐츠들을 발행까지 해보자

\---

webflow를 붙이면서 만난 문제들&#x20;

* Image 처리를 위해 붙인 문제&#x20;
  * [https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch](https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch)
* 이미지 삭제하려면 postman으로 요청을 보내야&#x20;
  * [https://docs.htmlcsstoimage.com/getting-started/using-the-api/#authentication](https://docs.htmlcsstoimage.com/getting-started/using-the-api/#authentication)









[https://www.youtube.com/watch?v=oVUHrs83S34](https://www.youtube.com/watch?v=oVUHrs83S34)
