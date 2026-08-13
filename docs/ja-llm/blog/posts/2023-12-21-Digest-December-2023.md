---
draft: false
pin: false
date:
  created: 2023-12-21
title: GTFS Digest - March 2024
description: Read this month's digest and stay up to date on GTFS development.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2023年12月 - 2024年を迎える前に知っておくべきこと {: #gtfs-digest-december-2023-what-you-need-to-know-before-2024}

今回の版では、主要な貢献者への謝辞、最近採択された提案の詳細、進行中の議論、今後のイベントをご紹介します。また、主要なツールとGTFSコミュニティとのつながりを維持する方法にも焦点を当てます。2024年に向けて、最新の更新情報を把握するためにぜひご覧ください。


<!-- more -->

GTFSの動向をお知らせするために[MobilityData](https://mobilitydata.org/)が作成する月次リソース、**GTFS Digest**へようこそ。

GTFSはコミュニティからの貢献に依存しています。次回のdigestにこれらの更新情報を掲載できるよう、皆様の動向を[specifications@mobilitydata.org](mailto:specifications@mobiltydata.org)まで共有してください。

この初回リリースに関する皆様のフィードバックを大切にしており、このツールを改善してその可能性を最大限に引き出すため、[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)へのご記入をお願いいたします。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**gcamp**

* [GTFS Trip-Modifications](https://github.com/google/transit/pull/403) の提案を Pull Request 段階へ移行し、投票段階への進行を促進するためにコミュニティと協力して取り組みました。これは効果的なアドボカシーの実践です！

**barbagus**

* 初めての [issue #409](https://github.com/google/transit/issues/409) を、非常に明確かつ詳細に作成したことに称賛を送ります。とても洗練されています！

**abyrd**

* 活発な Pull Requests に対して綿密なレビューを提供したことに敬意を表します。[これはまさに貴重な貢献です](https://github.com/google/transit/pull/403#issuecomment-1841006022)！

**ponlawat-w**

* 初めての PR [#412](https://github.com/google/transit/pull/412) において、編集上の修正をコミュニティの注意に喚起することで貢献したことに称賛を送ります。

## 🚀 最近採択されたもの {: #recently-adopted}


**[ベストプラクティス: すべてのファイルに対するDataset PublishingガイドラインおよびPractice Recommendationsの追加 #406](https://github.com/google/transit/pull/406)**

この提案は、[Dataset Publishing & General Practiceガイドライン](https://gtfs.org/schedule/best-practices/#dataset-publishing-general-practices)および[すべてのファイルに対するPractice Recommendations](https://gtfs.org/schedule/best-practices/#all-files)を、GTFS仕様の参照ファイルに追加することに焦点を当てています。これは、issue [#396](https://github.com/google/transit/issues/396)で概説されている、Best PracticesをGTFS仕様へ統合する第2段階を表しています。

**[[GTFS-Fares v2] networks.txtおよびroute_networks.txtの追加 #405](https://github.com/google/transit/pull/405)**

GTFS-ScheduleおよびGTFS-Fares v2データは、常に同じ組織・部門によって作成されるとは限らず、常に同じ頻度で更新されるとも限りません。この提案により、運賃データとスケジュールデータを別々に作成することができます。

## 🗳️ 現在投票中 {: #currently-voting}


**[仕様に統合されたベストプラクティスの内容を削除（フェーズ2） #60](https://github.com/MobilityData/GTFS_Schedule_Best-Practices/pull/60)**

[PR #406](https://github.com/google/transit/pull/406) により、特定のベストプラクティスが GTFS 仕様に直接統合されたことを受け、この提案では、単一の信頼できる情報源に集約するため、Best Practices 文書から重複する内容を削除します。**投票は 2023-12-26 に締め切られます。**

## 📂 アクティブな提案 {: #active-proposals}


**[GTFS-Flex #388](https://github.com/google/transit/pull/388)**

GTFS-Flex の提案により、乗客は経路検索ツール上でデマンド型サービスを見つけることができます。この提案は複数回の議論を経ており、現在はコントリビューターによるレビューが行われています。

* 2023年11月1日に開催された直近の GTFS-Flex ワーキンググループ会議:
    * issue (google/transit#398) に対応するため、stop_times.txt に専用の外部キーを設けることで合意しました。
    * location_groups.txt の使用に戻し、location.geojson ids への参照を削除しました (google/transit#397)。
    * hail-and-ride/taxi-like のユースケースに対する pickup_type=3 の制限を維持しました (google/transit#400)。
* [最後に残っている課題はこちら](https://github.com/google/transit/pull/388#issuecomment-1805958429)および[こちら](https://github.com/google/transit/pull/388#pullrequestreview-1766734999)で確認してください。

**[GTFS Trip-Modifications #403](https://github.com/google/transit/pull/403)**

Trip-Modifications は、ルート形状(shape)を変更し、運行されない停留所等(stop)を削除し、必要に応じて一時的な停留所等(stop)を追加するために便(trip)に対して行われる変更です。[Trip-Modifications は主に迂回経路の可視化およびリアルタイム予測の更新に使用されます。](https://blog.transitapp.com/how-transit-and-swiftly-put-bus-detours-on-the-map/)

**[[GTFS-Fares v2] Add rule_priority to fare_leg_rules.txt #418](https://github.com/google/transit/pull/418)**

rule_priority フィールドは、一致するルールが乗車区間(leg)に適用される優先順位を定義し、特定のルールを他のルールより優先させることができます。このフィールドの存在はトリガーとして機能し、空の値の意味を「anything except」から「一致に影響しない」へ変更します。

## 🔥 最も活発な議論 {: #most-active-conversations}


**[GTFS ガバナンスの変更: 段階的導入計画 #413](https://github.com/google/transit/issues/413)**

MobilityData は、正式な改正プロセスと GTFS ガバナンスの非公式な側面の両方を改善するための改良を提案しています。この issue には、コミュニティで共通して認識されている問題と、提案された段階的導入計画が含まれています。

**[routes レベルで必須の交通手段の種類には柔軟性が欠ける可能性があります（複数モードのルート・路線系統(route)） #409](https://github.com/google/transit/issues/409)**

一部のルート・路線系統(route)は、便(trip)に応じて列車とバスの両方で運行されるよう設計されています。現在の GTFS 仕様では、「交通手段の種類」は routes の属性であるため、このようなユースケースを適切に扱うことができません。

**[#mobilitydata での Slack 会話](https://mobilitydata-io.slack.com/archives/C0J4TJY8L/p1702318548285739)**

Melinda は、フィード利用者およびアプリ開発者向けのベストプラクティスについて、皆さんの知見をぜひ聞きたいと考えています。彼女は次のように尋ねています。「これまでのすべてのベストプラクティスは GTFS 作成者を対象としていますよね？ GTFS データセットを扱う開発者向けに、別のベストプラクティス集を作ることを考えた人はいますか？」

**[#gtfs での Slack 会話](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1701735882006169)**

Steve は、次の質問に答えるために皆さんの助けを必要としています。「他の形式に変換することなく、GTFS ファイルを地図上で可視化できるツールまたはサービスを知っている方はいますか？」

## 📅 今後のイベント {: #upcoming-events}


**[Trip-Modifications - 提案についての議論（会議）](https://www.eventbrite.ca/e/gtfs-flex-working-group-meeting-implementation-testing-tickets-780184602147)** | 2024年1月10日 @ EST 午前11時

**[GTFS-Flex - ワーキンググループ会議](https://www.eventbrite.ca/e/gtfs-flex-working-group-meeting-implementation-testing-tickets-780184602147)** | 2024年1月17日 @ EST 午前10時

## 🛠️ ツールの更新 {: #tools-update}


**[新リリース: Canonical GTFS Schedule Validator](https://gtfs-validator.mobilitydata.org/)**

最新の仕様追加をサポートしており、検証レポートには新しいサマリーセクションが追加されています。これには、フィードに Blocks、Frequencies、または Fares v2 などの GTFS コンポーネントが含まれているかを確認するためのタグが含まれます。

## GTFSコミュニティに参加する {: #join-the-gtfs-community}


**[GitHub: google/transit](https://github.com/google/transit)**: コミュニティとアイデアを共有しましょう！公式GTFS GitHubリポジトリに参加してください。

**[GTFS-changes](https://groups.google.com/g/gtfs-changes)**: 更新が発生したらすぐに受け取りましょう。GTFS-changes Googleグループに参加して、新しいpull requestや投票に関する情報を入手してください。

**[GTFS-realtime](https://groups.google.com/g/gtfs-realtime)**: Realtimeに関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtimeについて議論し、質問を行い、変更を提案しています。

**[GTFS.org](https://gtfs.org/)**: 公式GTFSドキュメントWebサイトです。ここでは、GTFSに必要なリソースが頻繁に更新されています。

**[MobilityData Slack](https://share.mobilitydata.org/slack)**: GTFSについて質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slackの会話に参加してください。1,300人を超えるモビリティ愛好家がチャンネルを利用しており、質問への回答を迅速に得られる素晴らしい場所です。

**最初のGTFS Digestをお読みいただき、ありがとうございます！2024年以降も、最新のGTFS更新情報をお届けできることを楽しみにしています。**
