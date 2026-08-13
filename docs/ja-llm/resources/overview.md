# リソースの概要 {: #resources-overview}


!!! info "このセクションはレビュー中です" 

    このセクションは現在、awesome-transit リストの内容を反映しています。一部の外部リンクが古くなっている可能性があることにご注意ください。このリストの内容に関するフィードバックを提供するには、[awesome-transit repository](https://github.com/MobilityData/awesome-transit) で issue または pull request を作成してください。このセクションのレビュー済みバージョンは、今後公開されます。

### 公共交通のオープンソース技術に関するデータ標準、API、アプリ、ツール、データセット、および研究のコミュニティリスト {: #community-list-of-data-standards-apis-apps-tools-datasets-and-research-around-open-source-technology-of-public-transit}


オープン技術は、さまざまな関係者が公共交通の改善に向けて協力する機会を提供します。

オープン技術の要素には、以下が含まれます。
- オープン標準
- オープンデータ
- オープンソースソフトウェア（OpenTripPlanner のような利用者向けアプリと、GTFS Validator のような開発者向けツールの両方）

このリストは、公共交通のためのオープン技術エコシステムに焦点を当てています。掲載される技術は、それ自体がオープンソースである場合、および／またはオープン標準および／またはオープンデータに依存する場合があります。

追加または変更したい内容がありますか？ [MobilityData/awesome-transit](https://github.com/MobilityData/awesome-transit) で [pull request](https://github.com/MobilityData/awesome-transit/pulls) または [issue](https://github.com/MobilityData/awesome-transit/issues) を作成してください。


[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome) [![RSS](https://img.shields.io/badge/Subscribe-RSS-blue.svg)](https://github.com/MobilityData/awesome-transit/commits/master.atom)

------------------------------

## 目次 {: #table-of-contents}

- [データの作成](../producing-data)
  - [GTFS](../producing-data/#gtfs)
    - [GTFS ライブラリ](../producing-data/#gtfs-libraries)
    - [GTFS コンバーター](../producing-data/#gtfs-converters)
    - [GTFS データ収集・保守ツール](../producing-data/#gtfs-data-collection-and-maintenance-tools)
    - [GTFS マージツール](../producing-data/#gtfs-merge-tools)
    - [GTFS 分析ツール](../producing-data/#gtfs-analysis-tools)
    - [GTFS 時刻表公開ツール](../producing-data/#gtfs-timetable-publishing-tools)
    - [GTFS バリデーター](../producing-data/#gtfs-validators)
  - [GTFS Realtime](../producing-data/#gtfs-realtime)
    - [GTFS Realtime ライブラリ・デモアプリ](../producing-data/#gtfs-realtime-libraries--demo-apps)
    - [GTFS Realtime バリデーター](../producing-data/#gtfs-realtime-validators)
    - [GTFS Realtime（およびその他のリアルタイム API）アーカイブツール](../producing-data/#gtfs-realtime-and-other-real-time-api-archival-tools)
    - [GTFS Realtime コンバーター](../producing-data/#gtfs-realtime-convertors)
    - [GTFS Realtime ユーティリティ](../producing-data/#gtfs-realtime-utilities)
  - [SIRI](../producing-data/#siri)
  - [その他のマルチモーダルデータ形式](../producing-data/#other-multimodal-data-formats)
- [データの共有](../sharing-data)
- [データの利用](../using-data)
  - [利用者向けアプリ](../using-data/#consumer-apps)
    - [Web アプリ（オープンソース）](../using-data/#web-apps-open-source)
    - [Web アプリ（クローズドソース）](../using-data/#web-apps-closed-source)
    - [ネイティブアプリ（オープンソース）](../using-data/#native-apps-open-source)
    - [ネイティブアプリ（クローズドソース）](../using-data/#native-apps-closed-source)
  - [ハードウェア](../using-data/#hardware)
  - [API 作成用ソフトウェア](../using-data/#software-for-creating-apis)
  - [SDK](../using-data/#sdks)
  - [可視化](../using-data/#visualizations)
  - [事業者向けツール](../using-data/#agency-tools)
- [その他のリソース](../other)
  - [コミュニティ](../other/#community)

## ライセンス {: #license}


[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

法律上可能な範囲で、[MobilityData](https://mobilitydata.org/)、[University of South Florida](http://www.usf.edu/) の [Center for Urban Transportation Research](https://www.cutr.usf.edu/)、および [Luqmaan Dawoodjee](https://github.com/luqmaan) は、本著作物に関するすべての著作権および関連する権利または隣接権を放棄しています。

## 概要 {: #about}

これは情報提供のみを目的としたコミュニティリソースです。プロジェクト／製品の掲載は、推奨を意味するものではありません。

このリストは、皆様のようなオープンソースコミュニティの貢献者によって作成・維持されています！[MobilityData](https://mobilitydata.org/) がこのプロジェクトを管理しています。 

Awesome-transit は当初、[Luqmaan Dawoodjee](https://github.com/luqmaan) によって作成され、プロジェクトが MobilityData に移管されるまでの数年間、[University of South Florida](http://www.usf.edu/) の [Center for Urban Transportation Research](https://www.cutr.usf.edu/) によって管理されていました。
