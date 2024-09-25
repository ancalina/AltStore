
# AltStore

> AltStore는 탈옥하지 않은 iOS 기기를 위한 대체 앱 스토어입니다.
>  [Go to English](README.en.md)

[![Swift Version](https://img.shields.io/badge/swift-5.0-orange.svg)](https://swift.org/)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

AltStore는 사용자의 Apple ID로 다른 앱(.ipa 파일)을 사이드로드할 수 있게 해주는 iOS 애플리케이션입니다. AltStore는 사용자의 개인 개발 인증서로 앱을 재서명하고, 이를 데스크톱 앱인 AltServer로 전송하여 iTunes WiFi 동기화를 사용해 재서명된 앱을 기기에 다시 설치합니다. 앱이 만료되지 않도록 동일 WiFi 네트워크에 있는 경우 백그라운드에서 주기적으로 앱을 새로고침합니다.

초기 릴리스에서는 Delta(AltStore 개발자 Riley Testut님의 올인원 에뮬레이터)의 배포를 위한 견고한 기반을 구축하는 데 중점을 두었습니다. 이제 Delta가 출시되었으므로, 누구나 AltStore를 통해 자신의 앱을 나열하고 배포할 수 있도록 지원하는 작업을 시작하고 있습니다(기여 환영합니다 🙂).

## 주요 기능
- AltServer를 사용하여 WiFi를 통해 앱 설치
- Apple ID로 어떤 앱이든 재서명하고 설치
- 동일 WiFi에 있는 경우 백그라운드에서 주기적으로 앱 새로고침
- AltStore를 통해 직접 앱 업데이트 처리

## 최소 프로젝트 요구사항
- Xcode 15
- Swift 5.9
- iOS 14.0 (AltStore)
- macOS 11.0 (AltServer)

## 프로젝트 개요

### AltStore
AltStore는 대부분의 기능을 포함하고 있는 일반적인 샌드박스화된 iOS 앱입니다. AltStore 앱 타겟에는 AltStore를 통해 앱을 다운로드하고 업데이트하는 모든 로직이 포함되어 있습니다. AltStore는 대부분의 iOS 개발자가 익숙한 표준 iOS 프레임워크와 기술을 많이 사용합니다:
* Core Data
* Storyboards/Nibs
* Auto Layout
* Background App Refresh
* Network.framework (iOS 12에 새로 추가됨)

### AltServer
AltServer도 일반적인 샌드박스화된 macOS 애플리케이션입니다. AltServer는 AltStore보다 훨씬 덜 복잡하며, 그 때문에 파일 수가 적습니다.

### AltKit
AltKit은 AltStore와 AltServer 간의 공통 코드를 포함하는 공유 프레임워크입니다.

### AltSign
AltSign은 Apple 서버와 통신하고 앱을 재서명하는 데 사용되는 내부 프레임워크입니다. 자세한 내용은 [AltSign 레포지토리](https://github.com/rileytestut/altsign)를 참고하세요.

### Roxas
Roxas는 iOS 개발에서 자주 사용되는 다양한 작업을 간소화하기 위해 개발된 내부 프레임워크입니다. 자세한 내용은 [Roxas 레포지토리](https://github.com/rileytestut/roxas)를 참고하세요.

## 컴파일 방법
AltStore와 AltServer는 iOS 또는 macOS 개발자라면 비교적 간단하게 컴파일하고 실행할 수 있습니다. AltStore와/또는 AltServer를 컴파일하려면:

1. 레포지토리를 클론합니다:
    ``` 
    git clone https://github.com/rileytestut/AltStore.git
    ```
2. 서브모듈을 업데이트합니다: 
    ```
    cd AltStore 
    git submodule update --init --recursive
    ```
3. `AltStore.xcworkspace`를 열고 프로젝트 탐색기에서 AltStore 프로젝트를 선택합니다. `Signing & Capabilities` 탭에서 팀을 `Yvette Testut`에서 자신의 계정으로 변경합니다.
4. **(AltStore만 해당)** `Info.plist`의 `ALTDeviceID` 값을 자신의 기기 UDID로 변경합니다. 보통 AltServer가 설치 중에 AltStore의 Info.plist에 기기 UDID를 포함시킵니다. Xcode를 통해 실행할 때는 값을 직접 설정해야 합니다. 그렇지 않으면 AltStore가 올바른 기기에 대해 앱을 재서명하거나 설치하지 않습니다.
5. **(AltStore만 해당)** `Info.plist`의 `ALTServerID` 값을 자신의 AltServer의 serverID로 변경합니다. 이는 동일 네트워크에 여러 AltServer가 있는 경우 AltStore가 이를 구분하는 데 도움이 되며, Bonjour 브라우징 애플리케이션을 사용해 AltServer가 광고하는 serverID를 확인할 수 있습니다. 이는 필수 사항은 아니지만, 여러 AltServer가 실행 중인 경우 도움이 됩니다.
6. 앱을 빌드하고 실행합니다! 🎉

## 라이선스
AltStore는 일부 종속성의 라이선스 때문에 **AGPLv3 라이선스**에 따라 배포할 수밖에 없습니다. 그렇지만 AltStore의 목표는 누구나 제한 없이 사용할 수 있는 오픈 소스 프로젝트가 되는 것이므로, 이 프로젝트의 모든 원본 코드를 자유롭게 사용, 수정 및 배포할 수 있도록 명시적으로 허가합니다(종속성은 여전히 원래 라이선스에 따릅니다).

## 연락처

### 현지화
* 이메일: taekyung@ancal.me

### 개발자 (Riley Testut)
* 이메일: riley@altstore.io
* Mastodon(선호): [@rileytestut@mastodon.social](https://mastodon.social/@rileytestut)
* 트위터/X(현재 활동 적음): [@rileytestut](https://twitter.com/rileytestut)

AltStore에 대한 일반적인 질문이 있습니까? [FAQ](https://altstore.io/faq/)를 꼭 읽어보세요.
