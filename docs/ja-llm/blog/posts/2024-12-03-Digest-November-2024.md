---
draft: false
pin: false
date:
  created: 2024-12-03
title: GTFS Digest - November 2024 - Governance Proposal and Rider Categories - Your Input Needed
description: November’s highlights include the Governance Changes Proposal, a new GTFS Realtime feature linking feed versions to schedules, and voting on fare leg join rules. Active proposals and discussions tackled rider categories, flexible pathways, and demand-based frequencies,
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2024年11月 - ガバナンス提案と乗客カテゴリー: ご意見をお寄せください {: #gtfs-digest-november-2024-governance-proposal-and-rider-categories-your-input-needed}


11月の主な内容には、ガバナンス変更提案、フィードバージョンをスケジュールに関連付ける新しい GTFS Realtime 機能、および運賃区間結合ルールに関する投票が含まれます。進行中の提案および議論では、Fares の乗客カテゴリー、柔軟な構内通路(pathway)、および需要ベースの運行頻度が取り上げられました。

<!-- more -->

GTFS Digest は、GTFS に関する進展の概要を提供するために [MobilityData](https://mobilitydata.org/) が毎月作成しているリソースです。 

皆様からのフィードバックを大切にしており、本ツールについてどのように感じられたかをお聞きしたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、本ツールの可能性を最大限に引き出すためにご協力ください。

## 📢 お知らせ {: #announcements}


[**GTFS Governance Changes Proposal Document のレビューにご協力ください**](https://docs.google.com/document/d/1EyJFvgOXZ4Gq6d6GJ6Hibey8Gkwyh7M25ECGwarmsT8/edit?usp=sharing)  
Governance Working Group Meetings および 2023 MobilityData Workshops で行われた議論を踏まえ、この文書にはそれらのセッションで得られた知見が反映されています。2025年第1四半期に予定されている PR の公開を支援するため、レビューをお願いいたします。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**Saipraneeth Devunuri（University of Illinois Urbana-Champaign）**  
Saipraneeth氏の[研究](https://findingspress.org/article/116694-a-survey-of-errors-in-gtfs-static-feeds-from-the-united-states)では、MobilityDataのGTFS validatorを使用して、米国のGTFSフィードにおけるエラーの調査を作成しました。

**miklcct**   
11月に、ルート形状(shape)と構内通路(pathway)に関するissueを3件オープンしました！ 

**Konstantinos**  
初めてSlackに貢献し、ギリシャにおける[フェリーに関するユースケース](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1732189321118949)を共有しました。  

**Steven White**   
GitHub（[#514](https://github.com/google/transit/issues/514)および[#482](https://github.com/google/transit/pull/482#issuecomment-2498646379)）とSlackで、コミュニティメンバーを支援するための詳細な回答

## 🗳️ 現在投票中 {: #currently-voting}


[**fare_leg_join_rules.txt を追加 #439**](https://github.com/google/transit/pull/439)  
この Pull Request は、用語定義に*有効運賃区間(effective fare leg)*の概念を導入し、有効運賃区間を定義するために fare_leg_join_rules.txt を追加し、この新しいアプローチに合わせて fare_leg_rules.txt の network_id、from_area_id、to_area_id、from_timeframe_group_id、および to_timeframe_group_id フィールドを更新します。

この提案は Ito World によって作成され、Google によって利用されています。

* *投票期間は UTC の12月3日 23:59:59 に終了します。*

## 🚀 最近採用された項目 {: #recently-adopted}


[**GTFS real time FeedHeader に GTFS Feed Version を追加 #434**](https://github.com/google/transit/pull/434)  
この Realtime 提案では、GTFS real time データとともに使用する GTFS schedule ファイルに関する情報を提供するため、フィードヘッダーに新しいフィールドを追加します。feed_version は、GTFS の feed_info.txt ファイル内の feed_version と一致します。

## 📂 アクティブな提案 {: #active-proposals}


[**[GTFS Fares v2] rider_categories.txt を追加 #511**](https://github.com/google/transit/pull/511)  
rider_categories.txt ファイルは、特定の運賃の対象となる乗客カテゴリをモデル化することを目的とした、GTFS-Fares v2 提案の一部です。

[**stops.stop_access フィールドを追加 #515**](https://github.com/google/transit/pull/515)  
この PR は、特定の駅において停留所等(stop)へのアクセス方法を示すため、stops.txt に stop_access フィールドを追加します。詳細については、[この提案](https://docs.google.com/document/d/1huTq9I6Bs38ZGtcG-7Cpns0kT1njV3PoUCjnjEE0Y1E/edit?tab=t.0#heading=h.4jjq7xol2izb)を参照してください。このフィールドの追加は、駅のモデリングを改善するための3段階計画の第1段階でもあります。

**その他の公開中の提案:**

* [過去の Stop time events は保持するべきです #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドを追加し、fare_transfer_type を明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] チケット商品/チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🔥 最も活発な議論 {: #most-active-conversations}


[GTFS Service Alerts に communication_period と impact_period を追加 #521](https://github.com/google/transit/issues/521)  
GTFS Realtime の Alert active_period フィールドは、運行情報(alert)の表示期間または障害の継続期間のいずれを意味する可能性もあるため、曖昧です。提案では、その用途を明確にするために、運行情報(alert)の表示用として communication_period を、障害の期間用として impact_period を追加することが示されています。

[freqencies.txt の主キー: end_time、headway_secs、exact_times を追加しますか？ #514](https://github.com/google/transit/issues/514)  
この課題では、時間帯が重複し headway_secs が異なる便(trip)（ラッシュアワー中の追加バスサービスなど）、または exact_times が異なる便(trip)を対象とするため、frequencies.txt の主キーを拡張することが提案されています。

[外部からはアクセスできないが、乗換目的ではアクセス可能なプラットフォームに対する wheelchair_boarding の指定 #516](https://github.com/google/transit/issues/516)  
この課題では、構内通路(pathway)がアクセス可能かどうかを明示的に示し、駅構内の車椅子でアクセス可能な経路を特定できるようにするため、pathways.txt に wheelchair_accessible フィールドを設けることが提案されています。

[pathways.txt にロックされたプラットフォームが存在しないという要件を削除 #517](https://github.com/google/transit/issues/517)  
この課題では、すべてのプラットフォームが構内通路(pathway)の連鎖を介して少なくとも1つの入口/出口に接続されなければならないという要件を削除することで、構内通路(pathway)に関する要件をより柔軟にすることが提案されています。

[ベストプラクティス: 一意な ID における妥当な長さ #518](https://github.com/google/transit/issues/518)  
この課題では、GTFS フィードで使用されるあらゆる ID に推奨文字数上限を設定するベストプラクティスを導入し、値が36バイトを超える場合にバリデーター警告を発生させることが提案されています。

[GTFS フィードにライセンス情報を追加 #519](https://github.com/google/transit/issues/519)  
ライセンス情報は、GTFS の作成者による法的義務および制限に直接アクセスできるようにすることで、GTFS フィードを取り込むプロセスを迅速化できます。

[shapes.txt でトンネルなどの特徴を指定 #520](https://github.com/google/transit/issues/520)  
この課題では、トンネルなど、ルート上の特徴を指定するために shapes.txt に新しいフィールドを追加することが提案されています。さまざまな貢献者からの回答では、これを GTFS で表現するための複数の選択肢が指摘されています。

[freqencies.txt の exact_times フィールドにデマンドベースの頻度を追加 #522](https://github.com/google/transit/issues/522)  
この課題では、カイロのマイクロバスなどの例を用いて、乗客需要を含む要因に基づいて出発する便(trip)を指定するため、exact_times に新しい選択肢を追加することが提案されています。

["header.incrementality: DIFFERENTIAL" のコンシューマー/プロデューサーの動作を定義 #84](https://github.com/google/transit/issues/84)  
この再開された課題は、TripUpdates における CANCELED/SKIPPED と、Alerts における NO_SERVICE の役割を明確にすることに焦点を当てています。最新の議論では、CANCELED が経路探索に影響を与える一方で、NO_SERVICE が乗客に障害を通知することを確実にする点に焦点が当てられています。

### #gtfs における Slack の会話 {: #slack-conversations-on-gtfs}


Weston は、[静的な便(trip)計画における timepoint の使用](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1731105469421269)について質問しました。

Konstantinos は、[agency.txt ファイルへの運賃 URL の追加](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1732189321118949)について支援を必要としていました。

Elias は、Statistics Canada に関する [GTFS を活用した研究](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1732119975757499)を共有しました。

### #gtfs-flex における Slack の会話 {: #slack-conversations-on-gtfs-flex}


Matthew は、Via のフィーから [GTFS-Flex を生成する](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1730991630563329)ためのヒントを求め、[GTFS-Flex 用のバリデーター](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1731012710942629)が存在するかについて質問しました。

### #gtfs-validators における Slack 会話 {: #slack-conversations-on-gtfs-validators}


Jeff は、[GTFS validator が Flex format を完全に認識しているか](https://mobilitydata-io.slack.com/archives/C03E10N96QL/p1730911388951229)を質問しました。

Krysttian は、GTFS Validator を使用してエラーの調査を作成した Praneeth の[記事を共有しました](https://mobilitydata-io.slack.com/archives/C03E10N96QL/p1730988293903099)。

### #gtfs-fares における Slack の会話 {: #slack-conversations-on-gtfs-fares}


Evan は面白い[良い犬・悪い犬の運賃ポリシー](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1731565332639069)を共有しました。

Daniel は、[fare rule が紐付いていない fare attribute の扱い方](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1732231109425179)について助言を求めました。

### #gtfs-realtime における Slack の会話 {: #slack-conversations-on-gtfs-realtime}


Graeme は、[pacebus の GTFS-Realtime ソース](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1732734051456289)を探していました。

Holger には、[GTFS-Realtime のベストプラクティスに関するいくつかの質問と提案](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1732871254059609)がありました。

## 🛠️ ツールの更新 {: #tools-update}


[**新しい GTFS Schedule Validator リリース: Flex の部分的サポート**](https://github.com/MobilityData/gtfs-validator/releases/tag/v6.0.0)  
GTFS Schedule Validator 6.0 リリースでは、GTFS-Flex フィードに対する誤検知の通知を削除し、テキストに無効な文字が含まれる場合の新しいエラーを追加し、timepoints および translations に関する新しい仕様の明確化に対応して検証ルールを更新しました。 

[質問やフィードバックは、こちらのディスカッションスレッドで共有してください。](https://github.com/MobilityData/gtfs-validator/discussions/1909)

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


[**GitHub: google/transit**](https://github.com/google/transit): コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

[**GTFS-changes**](https://groups.google.com/g/gtfs-changes): 更新が発生したらすぐに受け取りましょう。GTFS-changes Google グループに参加して、新しい pull request と投票に関する情報を入手してください。 

[**GTFS-realtime**](https://groups.google.com/g/gtfs-realtime): Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

[**GTFS.org**](https://gtfs.org/): 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS に必要な情報について頻繁に更新されるリソースを見つけることができます。 

[**MobilityData Slack**](https://share.mobilitydata.org/slack): GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slack の会話に参加してください。この場所では、各チャンネルで活動する 1,300 人を超えるモビリティ愛好家から、質問への回答を迅速に得ることができます。 

**GTFS Digest の本号をお読みいただき、ありがとうございます！2024 年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
