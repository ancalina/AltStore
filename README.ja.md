# AltStore

> AltStoreは脱獄していないiOSデバイス向けの代替アプリストアです。</br>
> [Go to English 🇺🇸](README.en.md)</br>
> [한국어로 이동 🇰🇷](README.md)</br>
[![Swift Version](https://img.shields.io/badge/swift-5.0-orange.svg)](https://swift.org/)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

AltStoreは、Apple IDを使用して他のアプリ(.ipaファイル)をiOSデバイスにサイドロードできるiOSアプリケーションです。AltStoreは、個人の開発証明書でアプリを再署名し、デスクトップアプリであるAltServerに送信し、iTunes WiFi同期を使用して再署名されたアプリをデバイスにインストールします。同じWiFiに接続しているときは、バックグラウンドで定期的にアプリを更新し、期限切れを防ぎます。

初回リリースでは、Delta（[私のiOS用オールインワンエミュレーター](https://github.com/rileytestut/Delta)）の配布に向けた堅実な基盤を構築することに焦点を当てました。Deltaがリリースされた今、誰でも自分のアプリをAltStoreを通じてリストし、配布できるようにする作業に取り組んでいます（貢献大歓迎！🙂）。

## 特徴
- AltServerを使用してWiFi経由でアプリをインストール
- Apple IDで任意のアプリを再署名してインストール
- 同じWiFiに接続している場合、バックグラウンドで定期的にアプリを更新して期限切れを防ぐ
- AltStoreを介して直接アプリを更新

## 最低プロジェクト要件
- Xcode 15
- Swift 5.9
- iOS 14.0 (AltStore)
- macOS 11.0 (AltServer)

## プロジェクト概要

### AltStore
AltStoreは、通常のサンドボックス化されたiOSアプリケーションです。AltStoreアプリのターゲットには、AltStoreを介してアプリをダウンロードおよび更新するためのロジックがすべて含まれています。AltStoreは、多くのiOS開発者が馴染みのある標準的なiOSフレームワークとテクノロジーを多用しています:
* Core Data
* Storyboards/Nibs
* Auto Layout
* Background App Refresh
* Network.framework（iOS 12で追加）

### AltServer
AltServerも通常のサンドボックス化されたmacOSアプリケーションです。AltStoreよりもはるかに複雑ではなく、ファイル数も少ないです。

### AltKit
AltKitは、AltStoreとAltServer間の共通コードを含む共有フレームワークです。

### AltSign
AltSignは、Appleのサーバーと通信し、アプリを再署名するために使用される内部フレームワークです。詳細は[AltSignリポジトリ](https://github.com/rileytestut/altsign)を確認してください。

### Roxas
Roxasは、iOS開発で一般的に使用されるさまざまなタスクを簡素化するために開発された内部フレームワークです。詳細は[Roxasリポジトリ](https://github.com/rileytestut/roxas)を確認してください。

## コンパイル手順
iOSまたはmacOS開発者であれば、AltStoreおよびAltServerをコンパイルして実行するのは比較的簡単です。AltStoreおよび/またはAltServerをコンパイルするには:

1. リポジトリをクローンします:
    ``` 
    git clone https://github.com/rileytestut/AltStore.git
    ```
2. サブモジュールを更新します:
    ```
    cd AltStore 
    git submodule update --init --recursive
    ```
3. `AltStore.xcworkspace`を開き、プロジェクトナビゲータでAltStoreプロジェクトを選択します。`Signing & Capabilities`タブで、チームを`Yvette Testut`から自分のアカウントに変更します。
4. **(AltStoreのみ)** `Info.plist`の`ALTDeviceID`の値をデバイスのUDIDに変更します。通常、AltServerはインストール中にデバイスのUDIDをAltStoreのInfo.plistに埋め込みますが、Xcodeで実行する場合は自分で設定する必要があります。設定しないと、AltStoreは適切なデバイスに対してアプリを再署名またはインストールしません。
5. **(AltStoreのみ)** `Info.plist`の`ALTServerID`の値をAltServerのserverIDに変更します。これは、同じネットワーク上に複数のAltServerがある場合、AltStoreがそれらを区別するのに役立ちます。Bonjourブラウジングアプリケーションを使用して、AltServerが広告しているserverIDを確認できます。必須ではありませんが、複数のAltServerが実行されている場合に役立ちます。
6. アプリをビルドして実行します！🎉

## ライセンス
AltStoreは、いくつかの依存関係のライセンスにより、**AGPLv3ライセンス**の下で配布せざるを得ません。とはいえ、AltStoreの目標は誰でも制限なく使用できるオープンソースプロジェクトにすることです。そのため、このプロジェクトのオリジナルコードは、誰でも自由に使用、修正、配布できることを明示的に許可します（依存関係は依然として元のライセンスに従います）。

## 連絡先

### 翻訳者 (TaekyungAncal, ChatGPT)
* メール: taekyung@ancal.me

### 開発者 (Riley Testut)
* メール: riley@altstore.io
* Mastodon（推奨）: [@rileytestut@mastodon.social](https://mastodon.social/@rileytestut)
* Twitter（現在はあまり活動していません）: [@rileytestut](https://twitter.com/rileytestut)

AltStoreに関する質問ですか？[FAQ](https://altstore.io/faq/)を必ずお読みください。
