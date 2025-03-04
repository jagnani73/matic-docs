---
id: getting-started
title: PoS 브리지
sidebar_label: 소개
description: 폴리곤에서 다음 블록체인 앱을 설치합니다.
keywords:
  - docs
  - matic
image: https://matic.network/banners/matic-network-16x9.png
---

시작하려면 [Pos에 대한 최신 Matic.js 문서](../matic-js/get-started.md) 를 확인하십시오.

브리지는 기본적으로 루트 체인에서 하위 체인으로 자산을 이동하는 데 도움이 되는 일련의 컨트랙트입니다. 이더리움과 폴리곤 간에 자산을 이동하는 데는 주로 두 가지 브리지가 있습니다. 첫 번째는 Plasma Bridge이고 두 번째는 **PoS Bridge** 또는 **Proof of Stake bridge**라고 합니다. **Plasma bridge**는 플라즈마 인출 메커니즘으로 인해 향상된 보안 보장을 제공합니다.

그러나 하위 토큰에는 특정 제한 사항이 있으며 Plasma Bridge에서 폴리곤으로부터 이더리움으로의 모든 종료/출금과 관련된 7일의 출금 기간이 있습니다.

이것은 **유연성**과 **더 빠른 인출**이 필요한 DApp/사용자에게 상당히 고통스러운 일이며 강력한 외부 검증자 세트에 의해 보호되는 폴리곤 Proof-of-Stake 브리지가 제공하는 보안 수준에 만족합니다.

지분 증명 기반 자산은 PoS 보안을 제공하고 하나의 체크포인트 간격으로 더 빠른 인출을 제공합니다.

## PoS Bridge를 이용하는 단계

문서의 이 섹션을 시작하기 전에 브리지를 사용하려고 하는 동안 이러한 용어들과 상호 작용할 수 있으므로 이러한 용어들을 완전히 이해하는 것이 도움이 될 수 있습니다. [매핑](https://docs.polygon.technology/docs/develop/ethereum-polygon/submit-mapping-request/) 및 [상태 동기화 메커니즘](https://docs.polygon.technology/docs/pos/state-sync/state-sync/)

해당 링크를 완료하시겠습니까? 그런 다음 업무흐름에 대한 높은 수준의 개요를 계속하겠습니다.

- PoS 브리지를 사용하는 첫 번째 단계는 **Root Token**과 **Child Token**을 매핑하는 것입니다. 이것은 복잡한 것이 아닙니다. 이는 루트 체인의 토큰 컨트랙트와 하위 체인의 토큰 컨트랙트가 서로 자산을 전송하기 위해 연결(매핑이라고 함)을 유지해야 함을 의미합니다. 매핑 요청을 제출하는 데 관심이 있다면, [여기](https://docs.polygon.technology/docs/develop/ethereum-polygon/submit-mapping-request/)에서 하십시오.

더 낮은 수준에서 더 자세히 설명하면 다음과 같습니다.

### 입금

- 자산 토큰의 소유자는 조건부 컨트랙트를 승인하여 예치할 토큰의 수량을 잠급니다. 이 승인 거래가 확인되면 자산 토큰 소유자는 **RootChainManager** 컨트랙트와 상호 작용하여 입금을 완료합니다.

- 다음으로 자산은 **상태 동기화 매카니즘**으로 보관됩니다. State Sync Mechanism이 무엇인지 알아보지 못했다면 이더리움 네트워크에서 폴리곤 네트워크로 데이터를 보내는 가장 단순한 형태의 기본 메커니즘입니다. 메커니즘 자체의 내부 작동은 **ChildChainManager** 컨트랙트를 트리거하는 **RootChainManager**로 이루어진 함수 호출로 구성됩니다.

이것을 비디오 포맷으로 보고 싶습니까? 아래에서 확인해주세요.

<video loop autoplay width="70%" height="70%" controls="true">
  <source type="video/mp4" src="/img/matic-to-eth/deposit-eth-matic.mp4"></source>
  <p>귀하의 브라우저는 비디오 요소를 지원하지 않습니다.</p>
</video>

### 출금

- 자산 인출은 PoS 브리지를 사용하면 간단합니다. 폴리곤 체인에서 자산 토큰을 소각하고 이 소각 트랜잭션의 트랜잭션 해시를 수집하여 **RootChainManager**에 제출하는 것만 큼 간단합니다. 그런 다음 **RootChainManager**는 이더리움 체인에 잠긴 자금을 해제하기 위해 조건자 컨트랙트를 호출합니다.

- 폴리곤 체인에서 소각 트랜잭션이 검증되면 이 소각 트랜잭션이 체크포인트되는 데 30분에서 3시간이 걸립니다. 체크포인트는 폴리곤 트랜잭션을 이더리움 블록체인에 병합하는 프로세스입니다.

- 다음으로 이 소각 트랜잭션의 증거는 exit 함수를 호출하여 **RootChainManager**에 제출됩니다. 이 함수 호출은 체크포인트 포함을 확인하기 위해 burnHash를 받은 다음에만 예치된 자금을 잠금 해제하고 교부하는 Predicate Contract(조건부 컨트랙트)를 트리거합니다.

이 모든 것을 비디오 형식으로 보고 싶으십니까? 아래에서 확인해주세요.

- 매핑이 완료되면 **matic.js SDK**를 사용하여 컨트랙트와 상호 작용하거나 SDK 없이도 동일한 작업을 수행할 수 있습니다. 그러나 matic.js SDK는 자산 전송 메커니즘을 모든 애플리케이션과 매우 쉽게 통합할 수 있도록 매우 사용자 친화적인 방식으로 설계되었습니다.

- **이더리움에서 폴리곤으로 자산을 전송한 다음 다시 이더리움으로 자산을 전송하는 전체 주기**는 이 자습서를 통해 설명됩니다. 간단히 말해서 프로세스는 다음과 같이 요약될 수 있습니다:

  1. 자산**(ERC20/ERC721/ERC1155)** 토큰 소유자는 양도할 토큰 수량을 사용하기 위해 PoS 브리지에서 특정 컨트랙트를 승인해야 합니다. 이 특정 컨트랙트는 **예치할 토큰의 수량을** 실제로 **잠그는** **조건부 컨트랙트**(Ethereum 네트워크에 배포됨)이라고 합니다.
  2. 승인이 완료되면 다음 단계는 **자산을 예치**하는 것입니다. **RootChainManager** 컨트랙트에서 함수 호출을 수행해야 폴리곤 체인에서 **ChildChainManager** 컨트랙트를 차례로 트리거합니다.
  3. 이것은 [여기](https://docs.polygon.technology/docs/pos/state-sync/state-sync/)에서 자세히 이해할 수 있는 상태 동기화 메커니즘을 통해 발생합니다.
  4. **ChildChainManager**는 내부적으로 하위 토큰 컨트랙트의 **deposit** 함수를 호출하고 해당 금액의 자산 토큰이 **사용자 계정에 발행됩니다**. ChildChainManager만 하위 토큰 컨트랙트의 입금 함수에 액세스할 수 있다는 점에 유의하는 것이 중요합니다.
  5. 사용자가 토큰을 받으면, **폴리곤 체인에서 무시할 수 있는 수수료로 거의 즉시 전송할 수 있습니다**.
  6. 자산을 이더리움으로 다시 인출하는 것은 자산 토큰을 **먼저 폴리곤 체인에서 소각**한 다음 **이 소각 트랜잭션의 증명을** 이더리움 체인에서 **제출되어야 하는** 2단계 프로세스입니다.
  7. 소각 트랜잭션이 이더리움 체인에 체크포인트되기까지 약 20분에서 3시간이 걸립니다. 이것은 지분 증명 검증자가 수행합니다.
  8. 트랜잭션이 체크 포인트에 추가되면 **exit** 함수를 호출하여 이더리움의 **RootChainManager** 컨트랙트에 소각 트랜잭션의 증거를 제출할 수 있습니다.
  9. 이 함수 호출은 **체크포인트 포함을 확인**한 다음 자산이 초기에 예치되었을 때 자산 토큰을 잠근 조건부 컨트랙트를 트리거합니다.
  10. 마지막 단계로, **조건부 컨트랙트는 잠긴 토큰을 해제**하고 이더리움의 사용자 계정으로 환불합니다.
