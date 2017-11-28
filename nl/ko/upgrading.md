---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-06"

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

# 작업공간 업그레이드

Conversation 서비스는 기능을 정기적으로 추가하고 업데이트합니다. 이러한 변경사항 중 일부는 자동으로 작업공간에 적용되는 반면, 영향이 큰 업데이트에는 수동 업데이트가 필요합니다.
{: shortdesc}

2017년 2월 3일 전에 작성된 인스턴스가 있고 작업공간에 사용 가능한 업데이트가 있는 경우, 업그레이드 아이콘(![업그레이드 아이콘](images/upgrade.png))이 나타나 업그레이드가 사용 가능함을 표시합니다. 작업공간 업그레이드 대화 상자를 열려면 이 아이콘을 선택하십시오.

작업공간을 업그레이드하는 경우 최신 버전의 API가 도구에서 사용 가능하며 "시험" 패널이 최신 기능을 사용하도록 시작됩니다.

작업공간을 업그레이드한 후 작업공간을 이전 버전으로 되돌릴 수 없습니다.

## 작업공간 업그레이드
작업공간에서 기능 방해를 방지하려면 다음을 수행하십시오.

1.  [작업공간 복제](configure-workspace.html#exporting-and-copying-workspaces).
2.  복제 작업공간 업그레이드.
3.  업그레이드한 작업공간의 문제 테스트.

문제에 대한 테스트를 완료하는 경우, **2017-02-03** 이상을 사용하도록 메시지 API 호출을 변경하여 업그레이드를 애플리케이션에 적용하십시오.

## 점프 조치에서 변경
**점프** 조치 처리가 2017년 2월 3일 릴리스에서 변경되었습니다.

이전에 노드 조건으로 점프했고 이 노드와 해당 피어 노드에 true로 평가된 조건이 없는 경우, 시스템이 루트 레벨 노드로 점프하고 조건이 입력과 일치한 노드를 검색합니다. 어떤 상황에서는 이 처리를 통해 루프가 작성되어 대화 상자가 진행되지 않았습니다.

새 프로세스에서 대상 노드와 해당 피어가 true로 평가되지 않으면 대화 상자 턴이 종료됩니다. 생성된 응답이 사용자로 리턴되고 오류 메시지가 애플리케이션으로 리턴됩니다.

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

다음 사용자 입력은 대화 상자의 루트 레벨에서 처리됩니다.

이 업데이트는 조건이 false인 노드를 대상으로 지정하는 **점프** 조치가 있는 경우 대화 상자의 동작을 변경할 수 있습니다.

이전 처리 모델을 복원하려는 경우 조건이 `true`인 최종 피어 노드를 추가하십시오. 응답에서 대화 상자 트리의 루트 레벨에서 첫 번째 노드의 조건을 대상으로 지정하는 **점프** 조치를 사용하십시오.
