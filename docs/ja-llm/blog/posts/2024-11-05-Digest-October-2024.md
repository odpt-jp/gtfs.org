---
draft: false
pin: false
date:
  created: 2024-11-05
title: GTFS Digest - October 2024 - The Global GTFS Community united in Montreal 
description: Key updates from the October 2024 GTFS Digest include a vote on adding a `feed_version` field to GTFS Realtime feeds, the adoption of trip modification adjustments for Realtime, and ongoing proposals like enhancements to station modeling and rider categories in GTFS-Fares v2. Conversations across GTFS channels focused on implementation questions, validator updates, and evolving best practices. The 2024 International Mobility Data Summit in Montreal also brought together global GTFS community members for impactful exchanges and collaboration.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2024年10月 - モントリオールに集結した世界のGTFSコミュニティ {: #gtfs-digest-october-2024-the-global-gtfs-community-united-in-montreal}


2024年10月のGTFS Digestにおける主な更新には、GTFS Realtimeフィードへの`feed_version`フィールド追加に関する投票、Realtime向けの便の変更(trip modification)調整の採用、ならびに駅モデリングの強化やGTFS-Fares v2における乗客カテゴリーなどの継続中の提案が含まれます。GTFSチャネル全体での議論では、実装に関する質問、validatorの更新、進化するベストプラクティスに焦点が当てられました。モントリオールで開催された2024 International Mobility Data Summitでは、世界中のGTFSコミュニティメンバーも集まり、有意義な交流と協働が行われました。

<!-- more -->

GTFS Digestは、GTFSに関する進展の概要を提供するために[MobilityData](https://mobilitydata.org/)が毎月作成しているリソースです。 

皆様からのフィードバックを大切にしており、本ツールの成果についてお聞かせいただきたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、本ツールの可能性を最大限に引き出すためにご協力ください。

## 📢 お知らせ {: #announcements}


[**2024 International MobilityData Summit は成功を収めました！**](https://mobilitydata.org/mobilitydata-strengthens-montreals-sustainable-mobility-ecosystem/)  
先週モントリオールで開催された2024 International Mobility Data Summitには、世界各地から180名を超える参加者が集まりました。参加者は、[GTFS Track](https://mobilitydata.org/the-2024-international-mobility-data-summit-new/)で提案されたパネルやワークショップに積極的に参加するとともに、ネットワークを構築し、長期にわたる関係のための強固な基盤を築く機会を得ました。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**Caltrans の Evan Siroki**  
Evan は今月、issue 516 と 512 の起票方法について私たちに見事な手本を示してくれました。また、複数の slack チャンネルでも非常に活発に活動しています。🙇

**SKI+ (SBB) の David Rudi**  
PR #434 における最初の producer になっていただき、ありがとうございます！コミュニティはあなたの献身に感謝しています！

**Transee の Darwin O'Connor**   
あなたは PR #434 の実現に着実に近づいており、testing phase における最初の consumer でもありました。素晴らしい仕事です！

**UIUC の Saipraneeth Devunuri**  
validator の問題の原因を発見し、issue #1912 を起票しました！素晴らしい仕事です！

## 🗳️ 現在投票中 {: #currently-voting}


[**GTFS real time FeedHeader に GTFS Feed Version を追加 #434**](https://github.com/google/transit/pull/434)  
この Realtime 提案では、GTFS real time データとともに使用する GTFS schedule ファイルに関する情報を提供するため、フィードヘッダーに新しいフィールドを追加します。feed_version は、GTFS の feed_info.txt ファイル内の feed_version と一致します。

この提案は SKI+（SBB）によって作成され、TransSee によって利用されています。

* *投票期間は11月16日 23:59:59 UTC に終了します。*

## 🚀 最近採用された項目 {: #recently-adopted}


[**便の変更に対する調整 #497**](https://github.com/google/transit/pull/497)  
この PR は、Trip Modifications に関する GTFS Realtime ドキュメントを明確化し、frequency based services のサポートを追加し、.proto ファイル内で不足していた extensions フィールドを追加するとともに、異なるファイル間でより包括的かつ一貫したドキュメントにしています。

## 📂 アクティブな提案 {: #active-proposals}


[**[GTFS Fares v2] rider_categories.txt を追加 #511**](https://github.com/google/transit/pull/511)  
rider_categories.txt ファイルは GTFS-Fares v2 提案の一部であり、特定の運賃の対象となる乗客カテゴリをモデル化することを目的としています。

[**stops.stop_access フィールドを追加 #515**](https://github.com/google/transit/pull/515)  
この PR は、特定の駅において停留所等(stop)へのアクセス方法を示すため、stops.txt に stop_access フィールドを追加します。詳細については、[この提案](https://docs.google.com/document/d/1huTq9I6Bs38ZGtcG-7Cpns0kT1njV3PoUCjnjEE0Y1E/edit?tab=t.0#heading=h.4jjq7xol2izb)を参照してください。このフィールドの追加は、駅のモデリングを改善するための3段階の計画における第1段階でもあります。

[**TripUpdate.schedule_relationship = ADDED の動作を規定し、REPLACEMENT の非推奨を解除 #504**](https://github.com/google/transit/pull/504)  
この PR は、追加または置換される旅程(journey)全体を規定する OpenTripPlanner の実装に基づき、ADDED の動作を規定し、REPLACEMENT の非推奨を解除します。完全に新しい便(trip)の完全な仕様をサポートするため、行先表示(headsign)、乗車／降車タイプなどの追加フィールドが必要に応じて導入されます。

**その他のオープンな提案:**

* [過去の Stop Time Events は保持するべきです #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] fare_leg_join_rules.txt を追加（第1イテレーション） #439](https://github.com/google/transit/pull/439)  
* [GTFS real time FeedHeader に Feed Version および GTFS url を追加 #434](https://github.com/google/transit/pull/434)  
* [[GTFS-Fares v2] チケット商品／チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🔥 最も活発な議論 {: #most-active-conversations}


[**timeframes.txt に開始日と終了日を追加できるようにする #506**](https://github.com/google/transit/issues/506)  
Evan は、特別日の運賃変更に対する開始日と終了日を追加することを提案しています。この提案は、GTFS-Fares v2 Working Group の10月のセッションで議論されました。詳細については、[こちら](https://docs.google.com/document/d/1d3g5bMXupdElCKrdv6rhFNN11mrQgEk-ibA7wdqVLTU/edit?usp=sharing)の会議メモを参照してください。

[**仕様書内で、休日情報を含めるべきであることを明示的に記載する #512**](https://github.com/google/transit/issues/512)  
この提案は、事前に判明しており、通常は毎年繰り返される休日運行を、すべての GTFS Schedule フィードに明示的に期待される要素とすることを、GTFS Specification の説明文内で明確にすることを目的としています。

### #gtfs における Slack の会話 {: #slack-conversations-on-gtfs}


Walter Jenkins は、[セントルイス市向けに GTFS で bike buses を実装すること](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1725502423179069)について質問しています。

Evan Siroky は、[California's Transit Data Guidelines のバージョン 4](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1728590417755079)に関するフィードバックを求めています。

Pablo B は、複数の事業者が存在する地域における[停留所等(stop)の定義](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1729072406562529)について質問しています。

Alvaro T は、[乗換ハブおよび異なる乗車制限を伴う国際バスルートをモデル化すること](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1730128316025739)に関するヒントを求めています。

### #gtfs-flex における Slack の会話 {: #slack-conversations-on-gtfs-flex}


Josh Drucker は、[Flex をパラトランジット向けにも構築するべきか](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1728316827975629)を質問しています。

Josh Drucker は、[オンデマンドサービスに関するいくつかの質問](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1728931281007179)を共有しました。

### #gtfs-validators における Slack の会話 {: #slack-conversations-on-gtfs-validators}


Evan Siroky が、[fare_transfer_rules におけるエッジケース](https://mobilitydata-io.slack.com/archives/C03E10N96QL/p1728080625895539)について質問しています。

Jeff Maki は、コミュニティで議論され、Praneeth によって次回の validator リリース 7.0 で対処すべき issue として提起された、[GTFS validator の問題](https://mobilitydata-io.slack.com/archives/C03E10N96QL/p1729270266359879)を抱えています。

Pablo B が、[GTFS 公開のベストプラクティス](https://mobilitydata-io.slack.com/archives/C03E10N96QL/p1729589637103559)について質問しています。

## 📅 今後のイベント {: #upcoming-events}


[**GTFS-Fares v2 – 月例会議**](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-769939809697) | 2024年11月26日 午前11時 EST

トピック : GTFS-Fares v2 extension ワーキンググループ会議

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
