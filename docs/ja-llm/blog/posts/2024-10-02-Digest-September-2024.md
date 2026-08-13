---
draft: false
pin: false
date:
  created: 2024-10-02
title: GTFS Digest - September 2024 - Loads of new conversations
description: The September 2024 GTFS Digest highlights recent contributions, including adopted proposals on validity rules for polygons and clarifications for from/to\_stop\_id in transfers.txt. It also features active proposals like adjustments to trip modifications and updates to nonconsecutive transfers in GTFS-Fares v2. This edition showcases a surge of new conversations, sparking discussions on HTTPS best practices, route deviation, and more.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2024年9月 - 多数の新たな議論 {: #gtfs-digest-september-2024-loads-of-new-conversations}


2024年9月のGTFS Digestでは、ポリゴンの有効性ルールに関する採択済み提案や、transfers.txt における from/to_stop_id の明確化を含む、最近の貢献を取り上げます。また、便の変更に対する調整や、GTFS-Fares v2 における連続しない乗換の更新など、活発な提案も紹介します。本号では、新たな議論が急増しており、HTTPS のベストプラクティス、ルート逸脱などに関する議論が始まっています。

<!-- more -->

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**Uriel Fojas**  
初めての Slack スレッドを投稿するために時間を割いていただき、ありがとうございます！

**Evan Siroki、Stefan de Konink、& Weston Shippy**  
先月、それぞれ2件の新しい issue を投稿し、GTFS の開発を継続的に支援していただいたことに称賛を送ります！

**Philip Cline**  
GTFS merge tooling について素晴らしい議論を始めてくださいました！これが何かにつながることを願っています。

## 🚀 最近採択された項目 {: #recently-adopted}


[**locations.geojson のポリゴンに有効性ルールを追加 #476**](https://github.com/google/transit/pull/476)  
この PR は、GTFS においてポリゴンが無効となり得るすべての方法を記述する代わりに、flex ポリゴンの有効性ルールを改善するため、OpenGIS Simple Features Specification のセクション 6.1.11 を参照することを提案しています。

[**[明確化] transfers.txt における from/to_stop_id および from/to_trip_id の説明 #455**](https://github.com/google/transit/pull/455)  
この PR は、元の意味を変更することなく、transfers.txt における from/to_stop_id および from/to_trip_id の説明を明確化しています。

## 📂 アクティブな提案 {: #active-proposals}


[**便の変更に対する調整 #497**](https://github.com/google/transit/pull/497)  
この PR は、Trip Modifications に関する GTFS Realtime ドキュメントを明確化し、frequency based services のサポートを追加し、.proto ファイル内で不足している extensions フィールドを追加するとともに、異なるファイル間でドキュメントをより包括的かつ一貫したものにします。

[**[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドの追加および fare_transfer_type の明確化 #498**](https://github.com/google/transit/pull/498)  
この PR は、乗換ルールが連続しない乗換に適用できるかどうかを示すために fare_transfer_rules.txt に nonconsecutive-transfer-allowed フィールドを追加し、複数回の乗換を含む旅程(journey)における fare_transfer_type の説明も明確化します。

[**過去の Stop time イベントは保持するべきです #502**](https://github.com/google/transit/pull/502)  
この PR は、StopTimeUpdates における説明を、過去の停車時刻(stop_time)の保持を「許可する」ものから、保持を「推奨する」ものへ変更します。

[**TripUpdate.schedule_relationship = ADDED の動作を指定し、REPLACEMENT の非推奨を解除する #504**](https://github.com/google/transit/pull/504)  
この PR は、旅程(journey)全体を追加または置換することを指定する OpenTripPlanner の実装に基づき、ADDED の動作を指定し、REPLACEMENT の非推奨を解除します。完全に新しい便(trip)の完全な仕様をサポートするため、行先表示(headsign)、pickup / drop-off types などの追加フィールドが必須として導入されます。

**その他のオープンな提案:**

* [[GTFS Fares v2] Area Set のマッチング述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] fare_leg_join_rules.txt の追加（第1イテレーション） #439](https://github.com/google/transit/pull/439)  
* [GTFS real time FeedHeader への Feed Version および GTFS url の追加 #434](https://github.com/google/transit/pull/434)  
* [[GTFS-Fares v2] チケット商品／チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🔥 最も活発な議論 {: #most-active-conversations}


[**HTTPS の使用、または少なくとも良好な SSL 健全性を推奨するベストプラクティスの追加 #496**](https://github.com/google/transit/issues/496)  
Evan は、SSL 証明書の健全性が良好な HTTPS の使用を推奨するベストプラクティスを追加することを提案しています。

[**Flex によって提供される現在のルート逸脱の解決策は十分ですか？ #499**](https://github.com/google/transit/issues/499)  
Weston は、ポリゴンデータを作成せずに、逸脱したルートを stop_times.txt 内で直接モデル化する方法を提案しています。

[**transfer_type = 1 #500**](https://github.com/google/transit/issues/500)  
Stefan は、transfers.txt における transfer_type = 1 の説明について、明確化の可能性を指摘し、このアプローチで見落とされている可能性のあるユースケースを尋ねています。

[**timeframes.txt に開始日と終了日を追加できるようにする #506**](https://github.com/google/transit/issues/506)  
Evan は、無料運賃の週末などの一時的な運賃ポリシーをモデル化するために、timeframes.txt に start_date および end_date 列を追加することを提案しています。

[**continuous_pickup/continuous_drop_off の禁止に関する文言はどのように解釈されますか？値 1 を明示的に許可するよう調整するべきですか？ #507**](https://github.com/google/transit/issues/507)  
Weston は、stop_times.end_pickup_drop_off_window を使用するデマンド型サービスをモデル化する際の stop_times.txt における continuous_pickup/continuous_drop_off の使用について、継続的な乗車または降車が許可されないことをデータ作成者が指定できるよう、明確化することを提案しています。

[**既存の GTFS において「停留所等(stop)/駅の概念的なグループ化」を定義する機能の不足 #438**](https://github.com/google/transit/issues/438)  
MobilityData は、駅のモデル化を改善するための[3 段階の計画](https://github.com/google/transit/issues/438#issuecomment-2289511429)を提案しており、この計画に対するフィードバックを歓迎しています。大きな異論がなければ、まもなく第 1 段階を進めます。

[**trips.txt に cars_allowed フィールドを追加する #466**](https://github.com/google/transit/issues/466)  
最近のコメントでは、trips.txt に cars_allowed を追加する代わりに boarding_permissions.txt を使用する方向に傾いています。一方で、各停留所等(stop)で車両の乗車および降車が許可されているかを把握できる利便性は、特別な処理の必要性を正当化し得ることも指摘されています。

### #gtfs における Slack の会話 {: #slack-conversations-on-gtfs}


Walter Jenkins が、[**St-Louis 市における bike buses の GTFS での実装**](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1725502423179069)について質問しています。

Philip Cline が質問しています: [**事業者は、timepoints を失ったり、デフォルトで block IDs を変更したりすることなく、サービス変更のために GTFS feeds をマージするには何を使用していますか？**](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1726493345923399)

Uriel Fojas が、[**州全体の計画のために、州レベルで停留所等(stop)の設備データを一元化するプロジェクト。**](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1726786680320179)について助言を求めています。

Josh Druker が質問しています: [**通常運行から緊急時／減便運行（気象緊急事態などの場合）へサービスを切り替える方法について、何か考えはありますか？**](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1726925502813659)

### #gtfs-fares における Slack の会話 {: #slack-conversations-on-gtfs-fares}


Evan Siroki は次のように述べています: [**「運賃無料週間」をモデル化できるようにいくつかの例外を追加したいのですが、そのためには新しい service_id を作成する必要があり、結果として Fares v2 の範囲外のファイルに干渉することになるようです。**](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1727195591059179)

### #gtfs-flex における Slack の会話 {: #slack-conversations-on-gtfs-flex}


Daniel Michalov が質問しています: [**OTP において flex trips の最大長を制限している可能性のある、ほかの要因はありますか？**](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1725553302355079)

Marcy Jaffe が質問しています: [**有効な GTFS-schedule を持つ事業者が、GTFS-Flex を使用して新しい route も生成する場合（Spare Labs、ありがとうございます）、2つのデータセットを1つに統合する必要がありますか？一致させなければならないデータ要素に関するチュートリアルはありますか？**](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1725714575720539)

Fabian Braun が質問しています: [**GTFS-Flex に関する Google の計画について、何かわかっていますか？見逃している公開発表はありましたか？**](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1726481999646599)

## 📅 今後のイベント {: #upcoming-events}


[**GTFS-Fares v2 – 月例会議**](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-769939809697) | 2024年10月22日 午前11時 EDT

トピック : GTFS-Fares v2 extension Working Group Meeting

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


[**GitHub: google/transit**](https://github.com/google/transit): コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

[**GTFS-changes**](https://groups.google.com/g/gtfs-changes): 更新が発生したらすぐに受け取りましょう。GTFS-changes google groups に参加して、新しい pull requests と投票に関する情報を入手してください。 

[**GTFS-realtime**](https://groups.google.com/g/gtfs-realtime): Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

[**GTFS.org**](https://gtfs.org/): 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS に必要な情報について頻繁に更新されるリソースを見つけることができます。 

[**MobilityData Slack**](https://share.mobilitydata.org/slack): GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS slack の会話に参加してください。これは、各チャンネルで活動する 1,300 人を超えるモビリティ愛好家から、質問への回答を迅速に得られる素晴らしい場所です。 

**GTFS Digest の本号をお読みいただき、ありがとうございます！2024 年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
