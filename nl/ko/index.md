---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-27"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 정보

{{site.data.keyword.conversationfull}} 서비스를 사용하면 자연어 입력을 이해하고 머신 러닝을 사용하여 인간 간의 대화를 시뮬레이션하는 방식으로 고객에게 응답하는 솔루션을 빌드할 수 있습니다.
{: shortdesc}

## 서비스 사용 방법

이 다이어그램에서는 완전한 솔루션의 전체 아키텍처를 표시합니다.![서비스의 플로우 다이어그램](images/conversation_arch_overview.png)

- 사용자는 구현된 사용자 **인터페이스**를 통해 애플리케이션과 상호작용합니다. 예를 들어, 단순 대화 창이나 모바일 앱 또는 음성 인터페이스를 사용하는 로봇입니다.

- **애플리케이션**은 사용자 입력을 {{site.data.keyword.conversationshort}} 서비스로 보냅니다.
    - 애플리케이션은 대화 상자 플로우 및 훈련 데이터의 컨테이너인 *작업공간*에 연결됩니다.
    - 서비스는 사용자 입력을 해석하고 대화의 플로우를 지시하고 필요한 정보를 수집합니다.
    - 추가 {{site.data.keyword.watson}} 서비스를 연결하여 사용자 입력을 분석할 수 있습니다(예: {{site.data.keyword.toneanalyzershort}} 또는 {{site.data.keyword.speechtotextshort}}).

- 애플리케이션은 사용자 인텐트 및 추가 정보를 기반으로 **백엔드 시스템**과 상호작용할 수 있습니다. 예를 들어, 질문에 응답, 티켓 열기, 계정 정보 업데이트 또는 주문 제출 등이 있습니다. 수행할 수 있는 작업에는 한계가 없습니다.

## 구현

대화 구현 방법은 다음과 같습니다.

- **작업공간을 구성하십시오.** 사용하기 쉬운 그래픽 환경을 사용하여 대화를 위한 훈련 데이터 및 대화 상자를 설정하십시오.

    훈련 데이터는 다음 아티팩트로 구성됩니다.
    - **인텐트**: 사용자가 서비스와 상호작용할 때 갖는 목적입니다. 사용자 입력에서 식별할 수 있는 목적마다 하나의 인텐트를 정의하십시오. 예를 들어, 상점 운영 시간에 대한 질문에 응답하는 인텐트 *store_hours*를 정의할 수 있습니다. 인텐트마다 고객이 필요한 정보를 요청하는 데 사용할 수 있는 입력을 반영하는 샘플 표현을 추가합니다(예: "What time do you open?").
    - **엔티티**: 엔티티는 인텐트의 컨텍스트를 제공하는 용어나 오브젝트를 나타냅니다. 예를 들어, 엔티티는 대화 상자에서 사용자가 운영 시간을 알고 싶어하는 상점을 구분하는 데 도움이 되는 도시 이름이 될 수 있습니다.

      훈련 데이터를 추가하면 자연어 클래스류가 자동으로 작업공간에 추가되며 서비스가 청취하고 응답해야 한다고 표시한 요청 유형을 이해하도록 훈련됩니다.

    대화 상자 도구를 사용하여 인텐트와 엔티티를 통합하는 대화 상자 플로우를 빌드하십시오. 대화 상자 플로우는 그래픽으로 도구에 트리로 표시됩니다. 분기를 추가하여 서비스에서 처리할 각 인텐트를 처리할 수 있습니다. 그런 다음 다른 요인(예: 애플리케이션 또는 다른 외부 서비스에서 서비스로 전달된 사용자 입력 또는 정보에 있는 엔티티)에 따라 요청에 대한 가능한 많은 순열을 처리하는 분기 노드를 추가할 수 있습니다.

- **작업공간을 배치하십시오.** 프론트 엔드 사용자 인터페이스, 소셜 미디어 또는 메시징 채널에 연결하여 사용자에 구성된 작업공간을 배치하십시오.

자세히 보려면 다음 링크를 따르십시오.

- [인텐트 및 엔티티 플랜](intents-entities.html#planning-your-entities)
- [대화 상자 개요](dialog-build.html#overview)
- [배치 개요](deploy.html)

## 언어 지원

기능을 통한 언어 지원은 [지원되는 언어](lang-support.html) 주제에 자세히 설명되어 있습니다.

## 다음 단계

- 서비스를 [시작하십시오](getting-started.html).
- 몇 가지 [데모](sample-applications.html)를 시도하십시오.
- [SDK(![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘"))](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} 목록을 보십시오.
