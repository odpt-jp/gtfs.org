# GTFS Realtime バインディング {: #gtfs-realtime-bindings}

## はじめに {: #introduction}


[GTFS Realtime](https://github.com/google/transit/tree/master/gtfs-realtime) は、公共交通システムに関するリアルタイム情報を伝達するためのデータ形式です。GTFS Realtime データは、高速かつ効率的な処理のために設計されたコンパクトなバイナリ表現である [Protocol Buffers](https://developers.google.com/protocol-buffers/) を使用してエンコードおよびデコードされます。データスキーマ自体は、[gtfs-realtime.proto](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto) で定義されています。

GTFS Realtime データを扱うために、開発者は通常、`gtfs-realtime.proto` スキーマを使用して、任意のプログラミング言語でクラスを生成します。これらのクラスは、GTFS-realtime データモデルオブジェクトを構築し、それらをバイナリデータとしてシリアライズするため、または逆方向に、バイナリデータをデータモデルオブジェクトとして解析するために使用できます。

`gtfs-realtime.proto` スキーマから GTFS Realtime データモデルクラスを生成することは非常に一般的な作業ですが、初めての開発者にとっては混乱を招くこともあるため、このプロジェクトは、最も一般的なプログラミング言語のいくつかについて、事前生成済みの GTFS Realtime 言語バインディングを提供することを目的としています。可能な場合、これらの言語バインディングは、他のプロジェクトでの利用を容易にするため、パッケージとして公開されます。

## サポートされている言語 {: #supported-languages}


* [.NET](dotnet.md)
* [Java](java.md)
* [JavaScript / TypeScript / Node.js](nodejs.md)
* [Python](python.md)
* [Golang](golang.md)
* ~~[Ruby](ruby.md)~~ *(2019年初頭をもって非推奨)*
* ~~[PHP](php.md)~~ *(2019年初頭をもって非推奨)*

## その他の言語 {: #other-languages}


C++ 用の生成済みコードは提供していません。公式の protoc compiler を使用してください（[こちら](https://developers.google.com/protocol-buffers/docs/downloads) または[こちら](https://github.com/google/protobuf)から入手できます）。

お好みの言語が不足していますか？貢献をご検討ください。

1. [CONTRIBUTING.md](https://github.com/MobilityData/gtfs-realtime-bindings/blob/master/CONTRIBUTING.md) をお読みください。
2. ご希望の言語で pull request を作成してください。更新手順（理想的には、スクリプト）を含めてください。また、その言語のエコシステムに適したパッケージングも提供してください。

## プロジェクトの履歴 {: #project-history}


このプロジェクトは当初 Google によって作成され、MobilityData は2019年初頭にこのプロジェクトの保守を開始しました。 

バインディングライブラリの旧バージョンは、現在も Google の名前で公開されています。Google が公開した最終バージョンを確認するには、各言語のドキュメントを[こちら](https://github.com/MobilityData/gtfs-realtime-bindings/tree/final-google-version)で参照してください。
