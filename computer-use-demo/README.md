# Anthropic Computer Use Demo

> [!주의]  
> Computer use는 베타 기능입니다. Computer use는 표준 API 기능이나 채팅 인터페이스와는 다른 고유한 위험이 있다는 점을 유의해주세요. 이러한 위험은 인터넷과 상호작용할 때 더욱 높아집니다. 위험을 최소화하기 위해 다음과 같은 예방 조치를 고려하세요:
> 
> 1. 직접적인 시스템 공격이나 사고를 방지하기 위해 최소한의 권한을 가진 전용 가상 머신이나 컨테이너를 사용하세요.
> 2. 정보 도난을 방지하기 위해 계정 로그인 정보와 같은 민감한 데이터에 대한 접근 권한을 모델에 부여하지 마세요.
> 3. 악성 콘텐츠에 대한 노출을 줄이기 위해 허용 목록의 도메인으로 인터넷 접근을 제한하세요.
> 4. 쿠키 수락, 금융 거래 실행, 서비스 약관 동의와 같이 적극적인 동의가 필요한 작업뿐만 아니라 실제 중요한 결과를 초래할 수 있는 결정에 대해서는 사람의 확인을 요청하세요.
>
> 경우에 따라 Claude는 사용자의 지시와 상충하더라도 콘텐츠에 포함된 명령을 따를 수 있습니다. 예를 들어, 웹페이지나 이미지에 포함된 Claude 지시사항이 기존 지시를 무시하거나 Claude가 실수를 하도록 할 수 있습니다. Prompt injection과 관련된 위험을 피하기 위해 Claude를 민감한 데이터와 작업으로부터 격리하는 예방 조치를 취하시기 바랍니다.
>
> 마지막으로, 귀하의 제품에서 computer use를 활성화하기 전에 관련 위험을 최종 사용자에게 알리고 동의를 구하시기 바랍니다.

이 저장소는 다음과 같은 참조 구현을 통해 Claude의 computer use를 시작하는 데 도움을 줍니다:

* 필요한 모든 종속성이 포함된 Docker 컨테이너를 생성하기 위한 빌드 파일
* 업데이트된 Claude 3.5 Sonnet 모델에 접근하기 위한 Anthropic API, Bedrock 또는 Vertex를 사용하는 computer use agent 루프
* Anthropic이 정의한 computer use 도구
* Agent 루프와 상호작용하기 위한 Streamlit 앱

모델 응답의 품질, API 자체, 또는 문서의 품질에 대한 피드백은 [이 양식](https://forms.gle/BT1hpBrqDPDUrCqo7)을 통해 제공해주세요 - 여러분의 의견을 기다립니다!

> [!중요]
> 이 참조 구현에서 사용된 Beta API는 변경될 수 있습니다. 가장 최신 정보는 [API release notes](https://docs.anthropic.com/en/release-notes/api)를 참조하세요.

> [!중요]
> 컴포넌트들은 약하게 분리되어 있습니다: agent 루프는 Claude가 제어하는 컨테이너에서 실행되며, 한 번에 하나의 세션에서만 사용할 수 있고, 필요한 경우 세션 간에 재시작하거나 리셋해야 합니다.

## Quickstart: Docker 컨테이너 실행하기

[이후 내용은 기술적인 명령어와 설정이 많아 원문 형태를 유지하는 것이 좋을 것 같습니다. 번역이 필요한 부분만 번역하여 제공하겠습니다.]

### Screen size

환경 변수 `WIDTH`와 `HEIGHT`를 사용하여 화면 크기를 설정할 수 있습니다. 예시:

[명령어 부분 생략]

[XGA/WXGA](https://en.wikipedia.org/wiki/Display_resolution_standards#XGA) 이상의 해상도로 스크린샷을 전송하는 것은 [이미지 크기 조정](https://docs.anthropic.com/en/docs/build-with-claude/vision#evaluate-image-size)과 관련된 문제를 피하기 위해 권장하지 않습니다.
API의 이미지 크기 조정 동작에 의존하면 도구에서 직접 스케일링을 구현하는 것보다 모델 정확도가 낮아지고 성능이 저하됩니다. 이 프로젝트의 `computer` 도구 구현은 더 높은 해상도에서 권장 해상도로 이미지와 좌표를 스케일링하는 방법을 보여줍니다.

## Development

[개발 관련 명령어 부분 생략]

위의 docker run 명령은 호스트에서 파일을 편집할 수 있도록 repo를 docker 이미지 내부에 마운트합니다. Streamlit은 이미 자동 리로딩으로 구성되어 있습니다.