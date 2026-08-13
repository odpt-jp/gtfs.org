---
draft: false
pin: false
date:
  created: 2025-01-07
title: GTFS Digest - December 2024 - 2 Votes and 1 Adoption to celebrate the Digest’s 1-year anniversary!
description: Let's celebrate the Digest's 1-year anniversary with 2 votes and an adoption to the specification. The community is currently voting on a proposal to clarify the use of agency_fare_url and a proposal that specifies the behavior of ADDED, and un-deprecate REPLACEMENT in TripUpdates. We also recommend that you take a look at the latest adoption that adds fare_leg_join_rules.txt to the specification.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2024年12月 - Digestの1周年を祝う2件の投票と1件の採用！ {: #gtfs-digest-december-2024-2-votes-and-1-adoption-to-celebrate-the-digests-1-year-anniversary}


Digestの1周年を、2件の投票と仕様への1件の採用で祝いましょう。コミュニティでは現在、agency_fare_url の使用を明確化する提案、および TripUpdates における ADDED の動作を規定し、REPLACEMENT の非推奨を解除する提案について投票を行っています。また、仕様に fare_leg_join_rules.txt を追加する最新の採用についてもご確認いただくことを推奨します。

<!-- more -->

GTFS Digest は、GTFSに関する進展の概要を提供するために [MobilityData](https://mobilitydata.org/) が毎月作成しているリソースです。 

皆様からのフィードバックを大切にしており、私たちの取り組みについてご意見を伺いたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 📢 お知らせ {: #announcements}


[**GTFS Governance Changes Proposal Document のレビューにご協力ください**](https://docs.google.com/document/d/1EyJFvgOXZ4Gq6d6GJ6Hibey8Gkwyh7M25ECGwarmsT8/edit?usp=sharing)  
Governance Working Group Meetings および 2023 MobilityData Workshops で行われた議論を踏まえ、本書はそれらのセッションから得られた知見を反映しています。2025年第1四半期に予定されている PR の公開を支援するため、レビューをお願いいたします。

* *レビュー期間は2025年1月17日に終了します*

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**Gcamp & Miklcct**   
それぞれの PR で投票を開始し、GTFS の発展に貢献してくださったことに大きな称賛を送ります。

**Jeffkessler-keolis**  
なんとよく書かれた issue でしょうか。しかも、これが初めてのものです！素晴らしいです！

## 🗳️ 現在投票中 {: #currently-voting}


[**agency_fare_url の使用を明確化 #524**](https://github.com/google/transit/pull/524)  
この PR は、チケットの購入を可能にするページだけでなく、運賃情報を含む URL ページも含めるように agency_fare_url の定義を拡張します。

* *投票期間は 1月20日 23:59:59 UTC に終了します。*

[**TripUpdate.schedule_relationship = ADDED の動作を規定し、REPLACEMENT の非推奨を解除 #504**](https://github.com/google/transit/pull/504)  
この PR は、追加または置換される旅程全体を規定する OpenTripPlanner の実装に基づき、ADDED の動作を規定し、REPLACEMENT の非推奨を解除します。完全に新しい便(trip)の完全な仕様をサポートするため、行先表示(headsign)、乗車／降車種別などの追加フィールドが必須として導入されます。

* *投票期間は 1月15日 23:59:59 UTC に終了します。*

## 🚀 最近採用された提案 {: #recently-adopted}


[**fare_leg_join_rules.txt を追加 #439**](https://github.com/google/transit/pull/439)  
この Pull Request は、用語定義に*有効運賃区間(effective fare leg)*の概念を導入し、有効運賃区間を定義するための fare_leg_join_rules.txt を追加するとともに、この新しいアプローチに合わせて fare_leg_rules.txt の network_id、from_area_id、to_area_id、from_timeframe_group_id、および to_timeframe_group_id フィールドを更新します。

* *この提案は Ito World によって作成され、Google によって利用されています。*

## 📂 アクティブな提案 {: #active-proposals}


[**[GTFS Fares v2] rider_categories.txt を追加 #511**](https://github.com/google/transit/pull/511)  
rider_categories.txt ファイルは GTFS-Fares v2 提案の一部であり、特定の運賃の対象となる乗客カテゴリをモデル化することを目的としています。

[**仕様への event_based_trips.txt の追加 #527**](https://github.com/google/transit/pull/527)
この PR は issue [#526](https://www.google.com/url?q=https://github.com/google/transit/issues/526&sa=D&source=docs&ust=1736197802148598&usg=AOvVaw05zhgBG-OjK_VKMYBNuHju) に基づくものであり、該当する便(trip)がイベント後の臨時便であることを利用者および顧客に示すため、event_based_trips.txt ファイルの追加を提案しています。


**その他のオープンな提案:**

* [過去の Stop time イベントは保持するべきです #502](https://github.com/google/transit/pull/502)  
* [stops.stop_access フィールドを追加 #515](https://github.com/google/transit/pull/515)  
* [[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドを追加し、fare_transfer_type を明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] チケット商品/チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🔥 最も活発な議論 {: #most-active-conversations}


[**GTFS Service Alerts に communication_period と impact_period を追加 #521**](https://github.com/google/transit/issues/521)  
GTFS Realtime の Alert active_period フィールドは、運行情報(alert)の表示期間と障害の継続期間のいずれを意味する可能性もあるため、曖昧です。提案では、その用途を明確にするため、運行情報(alert)の表示用に communication_period、障害の期間用に impact_period を追加することが提案されています。

[**イベントベースの便（「イベント終了後 x 分に出発する」便のモデリングをサポートするため）の規定を追加 #526**](https://github.com/google/transit/issues/526)
この issue では、復路の便がイベントの終了時刻に依存するイベントベースのルート・路線系統(route)をモデリングするための GTFS の方法が提案されています。適用対象の便(trip)がイベント終了後の臨時便であり、想定されるイベント終了時刻に基づいて運行されることをコンシューマーおよび顧客に示すため、event_based_trips.txt ファイルを追加することが提案されています。

[**ベストプラクティス: 一意な ID における妥当な長さ #518**](https://github.com/google/transit/issues/518)  
この issue では、GTFS feed で使用されるあらゆる ID に推奨文字数上限を設定するベストプラクティスを導入し、値が 36 bytes を超える場合に validator warning を発生させることが提案されています。

### #gtfs における Slack の会話 {: #slack-conversations-on-gtfs}


Leo は、[データセット同士をリンクするために GTFS データセット外で ids を定義する拡張機能](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1733497063149959)を探していました。

Evan は、[California Transit Data Guidelines の Version 4 の最終版](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1733962711549929?thread_ts=1728590417.755079&cid=C3FFFKX9C)を発表しました。

Raffael は、[parent_stations のベストプラクティス](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1734526282094799)について質問しました。

Michael は、[ダウンロードした GTFS を使用してオフラインで動作するアプリ](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1734569332694509)について問い合わせました。

Stephanie は、[スケジューリングのベストプラクティス](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1736198110045109)について質問しています。

### #gtfs-fares における Slack の会話 {: #slack-conversations-on-gtfs-fares}


Michael は、[分割チケットのモデリング](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1733233599140119)について質問しました。

Michael は、[駅の制限を伴う運賃モデリング](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1733258324963899)について問い合わせました。

Michael は、[ゾーン運賃における中間ゾーンの定義](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1733258582268919)について、より詳細な情報を求めました。

Guillaume は、[rider_categories PR を妨げているものがあるかどうか](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1734545159926479)を確認しました。

### #gtfs-realtime における Slack の会話 {: #slack-conversations-on-gtfs-realtime}


Michael は、[削除された代替便を復活させるための以前の PR](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1733354282636069) について質問しました。

## 💬 GTFS コミュニティに参加 {: #join-the-gtfs-community}


[**GitHub: google/transit**](https://github.com/google/transit): コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

[**GTFS-changes**](https://groups.google.com/g/gtfs-changes): 更新が発生したらすぐに受け取りましょう。GTFS-changes Google グループに参加して、新しい pull request と投票に関する情報を入手してください。 

[**GTFS-realtime**](https://groups.google.com/g/gtfs-realtime): Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

[**GTFS.org**](https://gtfs.org/): 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS に必要な情報について頻繁に更新されるリソースを見つけることができます。 

[**MobilityData Slack**](https://share.mobilitydata.org/slack): GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slack の会話に参加してください。これは、各チャンネルで活動する 1,300 人を超えるモビリティ愛好家から、質問への回答を迅速に得られる素晴らしい場所です。 

**GTFS Digest の本号をお読みいただき、ありがとうございます！2025 年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
