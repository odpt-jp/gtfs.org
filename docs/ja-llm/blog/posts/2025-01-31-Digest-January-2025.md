---
draft: false
pin: false
date:
  created: 2025-01-31
title: GTFS Digest - January 2025 - A Vote on Rider Categories and a Proposal on Interoperable tripid see the light
description: The January GTFS Digest covers the ongoing vote on rider_categories.txt for GTFS-Fares v2, the adoption of agency_fare_url clarifications, and proposals like original_trip_id for trip referencing and boarding_permissions.txt for vehicle carriage rules. Discussions include GTFS Realtime versioning and best practices for unique IDs.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2025年1月 - 乗客カテゴリーに関する投票と、相互運用可能な「trip_id」に関する提案が日の目を見る {: #gtfs-digest-january-2025-a-vote-on-rider-categories-and-a-proposal-on-interoperable-trip_ids-see-the-light}


1月のGTFS Digestでは、GTFS-Fares v2向けのrider_categories.txtに関する継続中の投票、agency_fare_urlの明確化の採用、ならびに便参照のためのoriginal_trip_idや車両持ち込み規則のためのboarding_permissions.txtといった提案を取り上げます。議論には、GTFS Realtimeのバージョニングおよび一意なIDに関するベストプラクティスが含まれます。

<!-- more -->

GTFS Digestは、[MobilityData](https://mobilitydata.org/)が毎月作成する、GTFSに関する進展の概要を提供するリソースです。 

皆様からのフィードバックを大切にしており、本ツールがどの程度役立ったかを知りたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、本ツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**Harpreetkaur、Laurent、Stephanie Daniels、Wojciech、& Angela Teyvi**   
GTFS slack に参加し、貴重なスレッドを通じてコミュニティに貢献していただき、ありがとうございます。 

**David1234**  
google/transit で初めての Pull Request を投稿されたことに称賛を送ります。今後どのように発展していくかを楽しみにしています！ 

**Miklcct**  
投票が不成立となった後も、提案を継続されたことを称賛します。提案を支持することは難しい場合があり、合意形成には多くの忍耐が必要です。

## 🗳️ 現在投票中 {: #currently-voting}


[**[GTFS Fares v2] rider_categories.txt を追加 #511**](https://github.com/google/transit/pull/511)  
rider_categories.txt ファイルは、特定の運賃の対象となる乗客カテゴリをモデル化することを目的とした、GTFS-Fares v2 提案の一部です。

* *投票は 2025-02-13 23:59:59 UTC に終了します。*

## 🚀 最近採用された変更 {: #recently-adopted}


[**agency_fare_url の使用を明確化 #524**](https://github.com/google/transit/pull/524)  
この PR は、agency_fare_url の定義を、チケットの購入を可能にするページだけでなく、運賃情報を含む URL ページも含めるように拡張します。

## 📂 アクティブな提案 {: #active-proposals}


[**original_trip_id による GTFS Schedule および Realtime の拡張 #534**](https://github.com/google/transit/pull/534)  
Davidr1234 は、Google Transit 拡張の `original_trip_id` を GTFS Schedule と GTFS Realtime の両方で採用することを提案しています。これにより、GTFS と、同様の概念を持つ SIRI や NeTEx などの他の標準との間で、便(trip)をシームレスに参照できるようになります。

[**停留所ごとの粒度で車両の輸送を指定するための乗車許可の導入 #533**](https://github.com/google/transit/pull/533)  
Miklcct は、あらゆる種類の車両を公共交通サービスで輸送、乗車、または降車できるかどうかを、停留所ごとの粒度で指定するための汎用的なソリューションを提案しています。これは、stop_times.txt から参照される新しいファイル boarding_permissions.txt を導入することで実現されます。

[**booking_rules.txt における continuous pickup/dropoff の値およびフィールド型に関する明確化 #528**](https://github.com/google/transit/pull/528)  
この PR は、デマンド型サービスに特化した明確化を導入し、start_pickup_drop_off_window/end_pickup_drop_off_window が指定されている場合に continuous_pickup/continuous_drop_off の値として 1 を許可します。また、booking_rules.txt 内の4つのフィールドのデータ型を正の整数に更新します。

[**仕様への event_based_trips.txt の追加 #527**](https://github.com/google/transit/pull/527)  
この PR は issue [#526](https://github.com/google/transit/issues/526) に基づくものであり、該当する便(trip)がイベント後の臨時便であることをコンシューマーおよび顧客に示すため、event_based_trips.txt ファイルを追加することを提案しています。

[**stops.stop_access フィールドの追加 #515**](https://github.com/google/transit/pull/515)  
この PR は、特定の駅において停留所等(stop)へどのようにアクセスするかを示すため、stops.txt に stop_access フィールドを追加します。詳細については、[この提案](https://docs.google.com/document/d/1huTq9I6Bs38ZGtcG-7Cpns0kT1njV3PoUCjnjEE0Y1E/edit?tab=t.0#heading=h.4jjq7xol2izb)を参照してください。このフィールドの追加は、駅モデリングを改善するための3段階計画の第1段階でもあります。

[**TripUpdate.schedule_relationship = ADDED を非推奨とし、GTFS static のスケジュールに基づいて運行されない新規／置換便(trip)を指定するために TripUpdate.schedule_relationship = NEW / REPLACEMENT を追加 #504**](https://github.com/google/transit/pull/504)  
この PR の当初の提案は、投票が否決された後に変更されました。更新された提案は、GTFS Realtime における TripUpdate.schedule_relationship = ADDED を非推奨とし、GTFS Schedule に存在しない完全に新しい便(trip)を示すために TripUpdate.schedule_relationship = NEW に置き換えることに焦点を当てており、この新しい値は experimental としてフラグ付けされます。次回の投票前に変更がコミュニティによって十分に検討されるよう、できるだけ多くの方に議論への参加をお願いします。

**その他のオープンな提案:**

* [過去の Stop time イベントは保持するべきです #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドの追加および fare_transfer_type の明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [CANCELED/SKIPPED TripUpdates と NO_SERVICE Alerts の明確化 #482](https://github.com/google/transit/pull/482)  
* [[GTFS-Fares v2] チケット商品／チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🔥 最も活発な議論 {: #most-active-conversations}


[**[Discussion] GTFS Realtime Versioning #530**](https://github.com/google/transit/issues/530)  
GTFS Realtime に関する最近の議論では、コミュニティが合意形成に困難を抱えているようであり、最近の PR#504 でバージョニングの話題が提起されました。これに対応するため、GTFS Realtime のバージョニングについて議論することに特化したこの issue を公開します。

[**GTFS-RT TripUpdates feed における、GTFS-Static に存在しない trip_id を使用するアドホックな便 #529**](https://github.com/google/transit/issues/529)  
Mpaine-act は新しい GTFS-Realtime TripUpdates feed を設定しており、GTFS-Static に存在しない便の扱いに関するガイダンスを求めています。この issue では、後方互換性のために負の（偽の）trip_id を使用する方法と、trip_id を完全に省略する方法の2つのアプローチを比較しています。

[**GTFS Service Alerts に communication_period と impact_period を追加 #521**](https://github.com/google/transit/issues/521)  
GTFS Realtime Alert の active_period フィールドは、運行情報(alert)の表示期間または障害の継続期間のいずれを意味する可能性もあるため、曖昧です。提案では、その用途を明確にするために、運行情報(alert)の表示用として communication_period を、障害の期間用として impact_period を追加することが提案されています。

[**ベストプラクティス: 一意な ID における妥当な長さ #518**](https://github.com/google/transit/issues/518)  
この issue では、GTFS feed で使用されるあらゆる ID に推奨文字数上限を設定するベストプラクティスを導入し、値が 36 bytes を超えた場合に validator warning を発生させることを提案しています。

[**グローバル trip id #462**](https://github.com/google/transit/issues/462)  
SKI+ の David は、NeTEx や HRDF などの他の標準との統合を改善するため、GTFS Schedule および Realtime に新しいフィールド「trip_global_id」を提案しています。これは、1日を通して有効な便(trip)識別子の必要性に対応するものです。このグローバル ID により、異なるデータ形式間での旅行情報のマッピングが容易になります。

[**提案されたベストプラクティス: SCHEDULED 便(trip)では常に TripDescriptor に trip_id を含める #465**](https://github.com/google/transit/issues/465)  
この提案では、データ統合を簡素化するため、GTFS Realtime の SCHEDULED 便(trip)において TripDescriptor での `trip_id` の使用を必須とすることを提案しています。複数の識別子を使用する代替手法では、しばしば問題が発生するためです。

### #gtfs における Slack の会話 {: #slack-conversations-on-gtfs}


Guillaume は、[Google が乗車エリアを使用しているか](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1736191393238099?thread_ts=1719341926.148069&cid=C3FFFKX9C)を質問しました。

Stephanie は、[GTFS calendars のベストプラクティス](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1736198110045109)について質問しました。

Wojciech は、[GTFS における予定された便(trip)のキャンセル](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1736511997339379)について問い合わせました。

Aaron は、[GTFS 利用者の間での shape_dist _travelled に関する期待](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1736876535662269)について質問しました。

Angela は、[OSM をクエリして GTFS を生成する方法](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1737478885517559)を求めました。

Harpreetkaur は、[便(trip)の最終停留所等(stop)に対する GTFS-RT TripUpdates に関するベストプラクティス](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1737492251744489)について助言を求めました。

### #gtfs-realtime における Slack の会話 {: #slack-conversations-on-gtfs-realtime}


Michael は、[複数の GTFS-RT フィードを単一のフィードに統合するツール](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1737552364940779)を求めました。

Laurent は、[まだ開始していない便(trip)において遅延をどのように伝播させるか](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1737741170127629)について助言を求めました。

## 📅 今後のイベント {: #upcoming-events}


[**GTFS-Fares v2 – 月例会議**](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-1230505989539?utm-campaign=social&utm-content=attendeeshare&utm-medium=discovery&utm-term=listing&utm-source=cp&aff=ebdsshcopyurl) | 2025年2月25日 午前11時 EST

トピック : GTFS-Fares v2 extension Working Group Meeting

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


[**GitHub: google/transit**](https://github.com/google/transit): コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

[**GTFS-changes**](https://groups.google.com/g/gtfs-changes): 更新が発生したらすぐに受け取りましょう。GTFS-changes Google グループに参加して、新しい pull request と投票に関する情報を入手してください。 

[**GTFS-realtime**](https://groups.google.com/g/gtfs-realtime): Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

[**GTFS.org**](https://gtfs.org/): 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS に必要な情報について頻繁に更新されるリソースを見つけることができます。 

[**MobilityData Slack**](https://share.mobilitydata.org/slack): GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slack の会話に参加してください。これは、各チャンネルを利用する 1,300 人を超えるモビリティ愛好家から、質問への回答を迅速に得られる素晴らしい場所です。 

**この版の GTFS Digest をお読みいただき、ありがとうございます！2025 年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
