---
draft: false
pin: false
date:
  created: 2025-10-02
title: GTFS Digest - September 2025 - New cEMV Field Adopted
description: The September 2025 GTFS Digest is here! This month, the GTFS community voted on a few proposals, two of which have been adopted into the specification, such as a new cEMV field.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2025年9月 - 新しいcEMVフィールドが採用されました {: #gtfs-digest-september-2025-new-cemv-field-adopted}


今月、GTFSコミュニティはいくつかの提案について投票を行い、そのうち2件が仕様に採用されました。これには、非接触型決済が受け付けられる場合を容易に示すための新しいcEMVフィールドが含まれます。GTFS-Realtimeの`SPECIAL_EVENT`フィールド、Fares V2のネットワークセット、およびGTFSファイルのホスティングに関するベストプラクティスについての議論も継続していますので、ぜひご参加ください！

<!-- more -->

GTFS Digestは、GTFSに関する動向の概要を提供するために[MobilityData](https://mobilitydata.org/)が毎月配信しているリソースです。

皆様からのフィードバックを大切にしており、新しいレイアウトについてどう思われるかを知りたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


*毎月、GTFSコミュニティによる貢献を紹介しています。今月は、以下の貢献を紹介します。*

| コントリビューター | 貢献 |
| :---- | :---- |
| **Sierra W.** | GTFS-Faresチャンネルでの初めての貢献 |
| **Masahiro Bessho, Matt Caywood, Masahiko Fukuda, Masaki Ito, M1LL3RD, BKK-Budapest** | PRへの初めての投票 |
| **ODPT** | PR# 545における最初のproducerおよび最初の投票 |

## 🗳️ 現在投票中 {: #currently-voting}


*以下は、現在投票が行われている提案の一覧です。ぜひご確認のうえ、投票プロセスにご参加ください。*

| 提案 | 提唱者 | 説明 | 投票締切 |
| :---- | :---- | :---- | :---- |
| [[GTFS Fares v2] Semantics clarification #561](https://github.com/google/transit/pull/561) | Tzujenchanmbd (MobilityData) | GTFS Fares v2 ファイルにおけるセマンティクスを明確化するための提案 | 10月6日 |

## 🚀 最近採択された提案 {: #recently-adopted}


*今月は、最終段階を通過した提案をお祝いします。以下をご確認ください。* 

| 提案 | 提唱者 | 説明 | 採択日 |
| :---- | :---- | :---- | :---- |
| [`agency.txt` および `routes.txt` に `cemv_support` フィールドを追加 #545](https://github.com/google/transit/pull/545) | Sergiodero (MobilityData) | この PR は、特定の事業者またはルート・路線系統(route)における交通サービスの利用時に、乗客が非接触型 Europay、Mastercard、Visa を使用できるかどうかを示すため、`agency.txt` および routes.txt に新しい cemv_support フィールドを導入します | 投票は9月29日に終了しました |
| [`stops.stop_access` フィールドを追加 #515](https://github.com/google/transit/pull/515) | tzujenchanmbd (MobilityData) | この PR は、特定の駅において停留所等(stop)へどのようにアクセスするかを示すため、`stops.txt` に `stop_access` フィールドを追加します。詳細については、[この提案](https://docs.google.com/document/d/1huTq9I6Bs38ZGtcG-7Cpns0kT1njV3PoUCjnjEE0Y1E/edit?tab=t.0#heading=h.4jjq7xol2izb)を参照してください。  | 投票は9月22日に終了しました |

## 📂 アクティブな提案 {: #active-proposals}


*これらの提案は活発に議論されており、皆様からのご意見を必要としています！* 

| 提案 | 提唱者 | 説明 | ステータス |
| :---- | :---- | :---- | :---- |
| [[GTFS Fares v2] `networks.txt` も存在する場合、`routes.txt` の `network_id` の更新を禁止 #581](https://github.com/google/transit/pull/581) | Skalexch (MobilityData) | このPRは、`routes.txt` における `network_id` の存在を禁止するファイルに networks.txt を追加します | 議論期間 |
| [GTFS および GTFS-realtime の意思決定プロセス #579](https://github.com/google/transit/pull/579)  | Ckraatz (SimplifyTransit) | このPRは、GTFS Schedule および Realtime のガバナンスプロセスを変更することを目的としています。 | 議論期間 |
| [ネットワークセットを追加し、`fare_leg_join_rules.txt` のネットワークに関する制約を緩和 #578](https://github.com/google/transit/pull/578) | Skalexch (MobilityData) | この提案は、`network_sets.txt` と `network_set_elements.txt` という2つの新しいファイルを追加するとともに、`fare_leg_join_rules.txt` の要件の一部を緩和します。これにより、複数のネットワークにまたがる有効運賃区間(effective fare leg)を照合できるようになります。 | 議論期間 |
| [`communication_period` および `impact_period` を追加 #546](https://github.com/google/transit/pull/546) | Skalexch (MobilityData) | このPRは、2つの新しいフィールド `communication_period` および `impact_period` を導入することで、GTFS Realtime Alert 仕様の `active_period` フィールドを明確化し、曖昧さを解消します。議論はユースケースおよび除外事項に焦点を当てています。 |  |
| [GTFS-realtime Service Alerts に新しい `SPECIAL_EVENT` Cause を追加 #577](https://github.com/google/transit/pull/577) | Ckraatz (SimplifyTransit) | この提案は、パレード、スポーツイベント、コンサートなどの混乱に適用可能な、「Special Event」と呼ばれる新しい Cause を GTFS-realtime Service Alerts に追加します。 | 議論期間 |
| [GTFS static の `trips.txt` に `trip_route_type` を追加 #572](https://github.com/google/transit/pull/572) | miklcct
(Jnction) | この提案は、`trip_route_type` という新しい任意フィールドを `trips.txt` に追加します。 | 議論期間  |
| [GTFS ファイルのホスティングに関するベストプラクティスを追加 #567](https://github.com/google/transit/pull/567) | doconnoronca (Transee) | この提案は、公開Webサーバーがブラウザ以外からのリクエストをブロックしたり、地域によってアクセスを制限したりしないことを推奨する、GTFSファイルのホスティングに関するベストプラクティスを導入します。代わりに、不正利用行為の防止に焦点を当てます。 | 議論期間 |

### その他のオープンな提案: {: #other-open-proposals}


* [[GTFS-Fares v2] 距離ベース運賃を追加 #556](https://github.com/google/transit/pull/556)  
* [gtfs-realtime.proto の誤字を修正 #541](https://github.com/google/transit/pull/541)  
* [original_trip_id により GTFS Schedule と Realtime を拡張 #534](https://github.com/google/transit/pull/534)  
* [停留所ごとの粒度で車両の輸送を指定するための乗車許可を導入 #533](https://github.com/google/transit/pull/533)  
* [仕様に event_based_trips.txt を追加 #527](https://github.com/google/transit/pull/527)  
* [過去の Stop time イベントは保持するべきです #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] `nonconsecutive_transfer_allowed` フィールドを追加し、`fare_transfer_type` を明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] チケット商品/チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🐙 Github で最も活発な会話 {: #most-active-conversations-on-github}


*Github Issues は、新機能のアイデアや仕様に関する質問など、会話を始めるのに最適な場所です。以下は今月最も活発な会話です。*

| 会話 | 著者 | 説明 |
| :---- | :---- | :---- |
| [Add Cause and DayTimePeriod to GTFS-TripModifications #580](https://github.com/google/transit/issues/580) | **Ckraatz (Simplify Trannsit)** | この議論は、変更の原因、および変更が実施される日と時間帯を伝達するために、Trip Modifications に新しい情報要素を追加する可能性を中心としています。 |

## 🔥 Slackで最も活発な会話 {: #most-active-conversations-on-slack}


*今月のGTFS Slackチャンネルで最も活発だった議論のまとめです。*

| 投稿者 | 説明 | Slackチャンネル |
| :---- | :---- | :---- |
| Stephen Miller | [GTFSについて事業者にインタビューするボランティア](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1758013110596709)を募集しました | #gtfs |
| Leonard Ehrenfried | [network_idおよびfrom/to_area_idを含むケースの解釈](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1759135967840069)について質問しました | #gtfs-fares |
| Lars Persson | [Transitデータを使用したアプリの作成に関する助言](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1759224364887579)を求めました | #gtfs-realtime |

## 📅 今後のイベント {: #upcoming-events}


| イベント | 日付 | 場所 |
| :---- | :---- | :---- |
| GTFS Fares V2 Working Group Meeting | 2025年10月28日 | [オンライン](https://mobilitydata.org/event/specifications-discussions-gtfs-fares-v2-monthly-meeting-8/2025-10-28/) |

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


<div class="grid cards" markdown>

- :simple-slack: [__Slack__](https://share.mobilitydata.org/slack) に参加し、コミュニティに自己紹介してください。

- :material-newspaper-variant: GTFS に関するあらゆる最新情報を毎月受け取るには、[__GTFS Digest__](https://gtfs.org/blog/) を購読してください。

- :fontawesome-solid-user-group: 開発に関する情報を得るために、[__GTFS Changes__](https://groups.google.com/g/gtfs-changes) Google Group に参加してください。 

- :simple-github: [__GitHub__](https://github.com/google/transit) にアクセスして、issue を投稿し、変更に関する議論に参加し、変更を提案してください。 

</div>

**今回の GTFS Digest をお読みいただき、ありがとうございます！2025年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
