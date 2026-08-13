---
draft: false
pin: false
date:
  created: 2025-03-06
title: GTFS Digest - February 2025 - Rider Categories, Adopted!
description: This month’s update highlights the adoption of rider_categories.txt in GTFS-Fares v2, a clarification on continuous pickup/dropoff rules for demand-responsive transit currently up for a vote, and new discussions shaping the specification. The community is debating whether a null fare_media_id should act as a wildcard for transfer rules, whether the StopTimeEvent timestamp should shift from int64 to uint64, and why GTFS field names use singular forms even for multi-value relationships. 
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2025年2月 - Rider Categories、採択！ {: #gtfs-digest-february-2025-rider-categories-adopted}


今月の更新では、GTFS-Fares v2 における rider_categories.txt の採択、現在投票に付されているデマンド型交通における連続乗車／降車ルールの明確化、および仕様を形作る新たな議論を取り上げます。コミュニティでは、null の fare_media_id を乗換ルールのワイルドカードとして扱うべきか、StopTimeEvent の timestamp を int64 から uint64 に変更すべきか、また複数値の関係であっても GTFS のフィールド名が単数形を使用する理由について議論されています。 

<!-- more -->

GTFS Digest は、[MobilityData](https://mobilitydata.org/) が毎月作成する、GTFS に関する動向の概要を提供するリソースです。 

皆様からのフィードバックを大切にしており、今回の内容についてご意見をお聞かせいただきたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 コントリビューターへの感謝 {: #contributor-shoutouts}


**Max Buchholz**  
GTFS Realtime の proto file にあるいくつかの誤字を修正する[最初の PR](https://github.com/google/transit/pull/541)を投稿しました。ご貢献ありがとうございます！ 

**Felix Gündling & Scott Berkley**  
GTFS コミュニティへようこそ。GTFS Fares に関する質問や知見を共有していただき、ありがとうございます。

**Wojciech Kulesza**  
Fares に大きな関心を寄せ、Working Group Meeting に参加していただきありがとうございます。

**Jerome Le Lan & lolpro11**  
GTFS Github Repo に最初の issue を投稿していただいたことに感謝します。皆様のご貢献に感謝いたします！

## 🗳️ 現在投票中 {: #currently-voting}


[**booking_rules.txt における continuous pickup/dropoff の値およびフィールド型に関する明確化 #528**](https://github.com/google/transit/pull/528)  
この PR は、デマンド型サービス（DRT）向けに特化した明確化を導入し、start_pickup_drop_off_window/end_pickup_drop_off_window が指定されている場合に continuous_pickup/continuous_drop_off に値 1 を使用できるようにします。また、booking_rules.txt 内の4つのフィールドのデータ型を正の整数に更新します。

## 🚀 最近採用された項目 {: #recently-adopted}


[**[GTFS Fares v2] rider_categories.txt を追加 #511**](https://github.com/google/transit/pull/511)  
rider_categories.txt ファイルは GTFS-Fares v2 提案の一部であり、特定の運賃の対象となる乗客カテゴリをモデル化することを目的としています。

## 📂 アクティブな提案 {: #active-proposals}


[**gtfs-realtime.proto の誤字を修正 #541**](https://github.com/google/transit/pull/541)  
1Maxnet1 は、Realtime の Proto ファイル内にあるいくつかの誤字を指摘し、修正を提案しました。これはドキュメント保守の変更であり、マージ前に他のコミュニティメンバーによるレビューが有益となる可能性があります。 

[**original_trip_id による GTFS Schedule および Realtime の拡張 #534**](https://github.com/google/transit/pull/534)  
Davidr1234 は、Google Transit 拡張の `original_trip_id` を GTFS Schedule と GTFS Realtime の両方で採用することを提案しています。これにより、GTFS と、同様の概念を持つ SIRI や NeTEx などの他の標準との間で、便(trip)をシームレスに参照できるようになります。

[**stops.stop_access フィールドの追加 #515**](https://github.com/google/transit/pull/515)  
この PR は、特定の駅において停留所等(stop)へのアクセス方法を示すため、stops.txt に stop_access フィールドを追加します。詳細については、[この提案](https://docs.google.com/document/d/1huTq9I6Bs38ZGtcG-7Cpns0kT1njV3PoUCjnjEE0Y1E/edit?tab=t.0#heading=h.4jjq7xol2izb)を参照してください。このフィールドの追加は、駅モデリングを改善するための3段階計画の第1段階でもあります。

[**TripUpdate.schedule_relationship = ADDED を非推奨とし、GTFS static のスケジュールに基づいて運行されない新規／置換便を指定するために TripUpdate.schedule_relationship = NEW / REPLACEMENT を追加 #504**](https://github.com/google/transit/pull/504)  
この PR の当初の提案は、投票が否決された後に変更されました。更新された提案は、GTFS Realtime における TripUpdate.schedule_relationship = ADDED を非推奨とし、GTFS Schedule に存在しない完全に新しい便(trip)を示すために TripUpdate.schedule_relationship = NEW へ置き換えることに焦点を当てており、新しい値は experimental として示されています。次回の投票前にコミュニティが変更を十分に検討できるよう、できるだけ多くの方に議論への参加をお願いします。

**その他の未解決の提案:**

* [停留所等(stop)ごとの粒度で車両の輸送を指定するための乗車許可の導入 #533](https://github.com/google/transit/pull/533)  
* [仕様への event_based_trips.txt の追加 #527](https://github.com/google/transit/pull/527)  
* [過去の Stop Time Events は保持するべきです #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドの追加および fare_transfer_type の明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [CANCELED/SKIPPED TripUpdates と NO_SERVICE Alerts の明確化 #482](https://github.com/google/transit/pull/482)  
* [[GTFS-Fares v2] チケット商品／チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🔥 最も活発な議論 {: #most-active-conversations}


[**fare_transfer_rules の fare_media_id: ワイルドカードか明示的な列挙か？ #538**](https://github.com/google/transit/issues/538)   
Jerome は、null の fare_media_id が fare_products においてワイルドカードとして機能し、すべてのチケットメディア間で無料乗換を許可するのか、それとも各チケットメディアに対して明示的な重複記載が必要なのかについて明確化を求めています。

[**GTFS Realtime Stop Time Updates Timestamp 型 #537**](https://github.com/google/transit/issues/537)  
lolpro11 は、現在 int64 である StopTimeEvent の time フィールドを、他の GTFS Realtime の timestamp フィールドとの一貫性のために uint64 へ移行するべきかを質問しています。 

[**複数形のフィールド名？ #536**](https://github.com/google/transit/issues/536)  
Nina は、GTFS において「複数」の多重度(cardinality)を持つフィールドに、複数形ではなく単数形の名前が付けられている理由を質問しています。回答では、CSV の各行には単一の外部キー参照しか含まれないため、列名はコレクションを表すのではなく個々の行に適用されると説明されています。

[**ベストプラクティス: 一意な ID における妥当な長さ #518**](https://github.com/google/transit/issues/518)   
Stefan は、GTFS フィードで使用されるあらゆる ID に推奨の文字数上限を設定し、値が 36 バイトを超えた場合にバリデーターの警告を発生させるベストプラクティスを導入することを提案しています。

[**相乗り路線の統合 #430**](https://github.com/google/transit/issues/430)   
フランスの National Access Point (NAP) の Aurélien は、バス路線のように動作する相乗り路線をモデル化する可能性のある提案について議論するため、GitHub Issue を作成しました。この路線には正確な停留所等(stop)と目的地がありますが、この場合は、運賃から金銭的インセンティブを受け取ることができる一般の自動車運転者が運行するか、乗客と費用を分担します。議論のために、2つの異なる可能性のある解決策が提示されています。

### #gtfs における Slack の会話 {: #slack-conversations-on-gtfs}


Holger は、[OpenTransportMeetup で予定されているプレゼンテーション](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1738683307201049)を共有しました。

Matthew は、[Google Maps Transit Layer への GTFS および GTFS-RT の掲載](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1738781861988599)について質問しました。

Antoine は、[service_id と route_id の関連付けに関する議論](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1739215527531599)を提起しました。

Emma は、[GTFS-Pathways データを生成するソフトウェアベンダー](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1740067288244449)を探しました。

Weston は、[Google Maps に頻度ベースのサービスを含めることの影響](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1741026771607149)について質問しました。

### #gtfs-realtime における Slack の会話 {: #slack-conversations-on-gtfs-realtime}


Paul は、[GTFS-RT のマッチングに関する利用者の意見](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1738939691099549)を求めました。

Usamam は、[GTFS-RT データを保存するためのユーザーフレンドリーなツール](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1739988659801339)を探しました。

### #gtfs-flex における Slack の会話 {: #slack-conversations-on-gtfs-flex}


Isabelle は、[Flex をカープールに使用できるかどうか](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1739462251045339)についての質問を共有しました。

### #gtfs-fares における Slack 会話 {: #slack-conversations-on-gtfs-fares}


Wojciech は、[距離ベース運賃に関する詳細](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1740055815487029)を求めました。

Felix は、Fares v2 の実装に関連する [fare_leg_rules.txt](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1740687333979029)、[fare_transfer_rules.txt](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1740744467147399)、[fare_leg_join_rules.txt](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1740743816951809)、および [timeframes.txt](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1740387033618589) について複数の質問をしました。

Weston は、[Fares Working Group Meeting における距離ベース運賃に関する質問](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1740509127859989)について追加の質問をしました。

## 📅 今後のイベント {: #upcoming-events}


[**GTFS-Fares v2 – 月例会議**](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-1230505989539?utm-campaign=social&utm-content=attendeeshare&utm-medium=discovery&utm-term=listing&utm-source=cp&aff=ebdsshcopyurl) | 2025年3月25日 午前11時 EST

トピック : GTFS-Fares v2 extension Working Group Meeting

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


[**GitHub: google/transit**](https://github.com/google/transit): コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

[**GTFS-changes**](https://groups.google.com/g/gtfs-changes): 更新が発生したらすぐに受け取りましょう。GTFS-changes Google グループに参加して、新しい pull request と投票に関する情報を入手してください。 

[**GTFS-realtime**](https://groups.google.com/g/gtfs-realtime): Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

[**GTFS.org**](https://gtfs.org/): 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS に必要な情報について頻繁に更新されるリソースを見つけることができます。 

[**MobilityData Slack**](https://share.mobilitydata.org/slack): GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slack の会話に参加してください。これは、各チャンネルを利用する 1,300 人を超えるモビリティ愛好家から、質問への回答を迅速に得られる素晴らしい場所です。 

**この版の GTFS Digest をお読みいただき、ありがとうございます！2025 年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
