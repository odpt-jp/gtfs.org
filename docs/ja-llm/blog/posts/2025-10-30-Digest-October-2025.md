---
draft: false
pin: false
date:
  created: 2025-10-30
title: GTFS Digest - October 2025 - Celebrating an Active Month for GTFS
description: The October 2025 GTFS Digest is here! This month was a particularly active one thanks to new faces and long-time contributors. Read on for the latest GTFS community news.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2025年10月 - GTFSにとって活発な月を祝して {: #gtfs-digest-october-2025-celebrating-an-active-month-for-gtfs}


今月、コントリビューターの皆様は仕様に積極的に取り組んでくださいました。小規模な編集上の変更による詳細の改善から、ブランドロゴを含めるための新機能の提案まで、コミュニティの創造性と協力は真に際立っていました。

<!-- more -->

GTFS Digestは、GTFSに関する進展の概要を提供するために[MobilityData](https://mobilitydata.org/)が毎月配信するリソースです。

皆様からのフィードバックを大変重視しており、新しいレイアウトについてどう思われるかを知りたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


*毎月、GTFSコミュニティによる貢献を紹介しています。今月は、以下の貢献を紹介します。*

| コントリビューター | 貢献 |
| :---- | :---- |
| Dominik Skarżyński | 新しいコントリビューターとして、[calendar.txt における重複しない duplicate service_ids](https://github.com/google/transit/issues/584)についての議論を開始しました |
| Felix Gündling | アプリ内で交通事業者のブランドロゴを表示することに関する、活発に議論されている[プルリクエスト](https://github.com/google/transit/pull/585)を作成しました。 |

## 🗳️ 現在投票中 {: #currently-voting}


現在、投票プロセス中の提案はありません。

## 🚀 最近採択された提案 {: #recently-adopted}


*今月は、最終段階を通過した提案をお祝いします。以下をご確認ください。* 

| 提案 | 提唱者 | 説明 | 採択日 |
| :---- | :---- | :---- | :---- |
| [[GTFS Fares v2] セマンティクスの明確化 #561](https://github.com/google/transit/pull/561) | Tzujenchanmbd (MobilityData) | GTFS Fares v2 ファイルのセマンティクスを明確化するための提案 | 2025年10月14日 |

## 📂 アクティブな提案 {: #active-proposals}


*これらの提案は活発に議論されており、皆様からのご意見を必要としています！* 

| 提案 | 提唱者 | 説明 | ステータス |
| :---- | :---- | :---- | :---- |
| [images.txt + agency logo #585 を追加](https://github.com/google/transit/pull/585) | Felixguendling (Transitous) | この提案では、`agency.txt` に新しい `agency_brand_image_url` 列を追加し、ブランドロゴを経路検索アプリケーションに表示できるようにします。 | 議論期間 |
| [GTFS-realtime Service Alerts に新しい SPECIAL_EVENT Cause を追加 #577](https://github.com/google/transit/pull/577) | Ckraatz (SimplifyTransit) | この提案では、パレード、スポーツイベント、コンサートなどの混乱に適用可能な、「Special Event」と呼ばれる新しい Cause を GTFS-realtime Service Alerts に追加します。 | 議論期間 |
| [GTFS ファイルのホスティングに関するベストプラクティスを追加 #567](https://github.com/google/transit/pull/567) | doconnoronca (Transee) | この提案では、GTFS ファイルのホスティングに関するベストプラクティスを導入し、公開 Web サーバーがブラウザー以外からのリクエストをブロックしたり、地域によってアクセスを制限したりすることを避け、代わりに不正利用行為の防止に重点を置くことを推奨します。 | 議論期間 |
| [[GTFS-Fares v2] 距離ベース運賃を追加 #556](https://github.com/google/transit/pull/556) | skalexch (MobilityData) | この PR では、`fare_leg_rules.txt` および `stop_times.txt` に複数の新しいフィールドを導入するとともに、新しい `fare_leg_distance_rules.txt` ファイルを追加することで、距離ベースの運賃体系をモデル化できる新機能を追加します。 | 議論期間 |
| [[GTFS Fares v2] network sets を追加し、fare_leg_join_rules.txt の networks に関する制約を緩和 #578](https://github.com/google/transit/pull/578) | skalexch (MobilityData) | この提案では、`network_sets.txt` と `network_set_elements.txt` という2つの新しいファイルを追加するとともに、`fare_leg_join_rules.txt` の要件の一部を緩和します。これにより、複数の networks にまたがる有効運賃区間(effective fare leg)を照合できるようになります。 | 議論期間 |

### その他のオープンな提案: {: #other-open-proposals}


* [不足しているスペースを追加 #587](https://github.com/google/transit/pull/587)  
* [GTFS および GTFS-realtime の意思決定プロセス #579](https://github.com/google/transit/pull/579)  
* [trip_route_type を GTFS static に追加 #572](https://github.com/google/transit/pull/572)  
* [communication_period および impact_period を追加 #546](https://github.com/google/transit/pull/546)  
* [original_trip_id により GTFS Schedule および Realtime を強化 #534](https://github.com/google/transit/pull/534)  
* [停留所ごとの粒度で車両の輸送を指定するための乗車許可を導入 #533](https://github.com/google/transit/pull/533)  
* [仕様に event_based_trips.txt を追加 #527](https://github.com/google/transit/pull/527)  
* [過去の Stop Time Events は保持するべきです #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドを追加し、fare_transfer_type を明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set の一致述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] チケット商品/チケットメディアの乗換動作 #423](https://github.com/google/transit/pull/423)

## 🐙 Github で最も活発な議論 {: #most-active-conversations-on-github}


*Github Issues は、新機能のアイデアや仕様に関する質問など、議論を始めるのに最適な場所です。以下は今月最も活発な議論です。*

| 議論 | 著者 | 説明 |
| :---- | :---- | :---- |
|  [寄生者を止めよう: オープントランジットデータを保護する](https://github.com/google/transit/issues/586) | Stefan de Konink |  ベンダーが追加する紹介リンクに関する、GTFS のライセンスおよび利用についての議論です。  |
| [提案: calendar.txt で重複しない duplicate service_ids を許可する](https://github.com/google/transit/issues/584) | Dominik Skarżyński | この提案は、calendar.txt 内の同一 service_id に対して複数の重複しない日付範囲を許可することで、GTFS におけるデータの重複を削減することを目的としています。 |

## 🔥 Slackで最も活発な会話 {: #most-active-conversations-on-slack}


*今月のGTFS Slackチャンネルで最も活発だった議論のまとめです。*

| 投稿者 | 説明 | Slackチャンネル |
| :---- | :---- | :---- |
| Gabriel Masiero | [GTFS Realtimeにおけるバス位置情報のプロジェクト](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1761134635290789)について質問しました。 | [#gtfs-realtime](https://mobilitydata-io.slack.com/archives/C3D321CKB) |
| Eva Leake | [便に対する「最大距離」制限の実装](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1761232949894399)について質問しました。 | [#gtfs-flex](https://mobilitydata-io.slack.com/archives/CSP7HDF37) |

## 📅 今後のイベント {: #upcoming-events}


| イベント | 日付 | 場所 |
| :---- | :---- | :---- |
| GTFS Fares V2 ワーキンググループ会議 | 2025年11月25日 | [オンライン](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-1230499460009) |

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


<div class="grid cards" markdown>

- :simple-slack: [__Slack__](https://share.mobilitydata.org/slack) に参加し、コミュニティに自己紹介してください。

- :material-newspaper-variant: GTFS に関するあらゆる月次更新情報を受け取るには、[__GTFS Digest__](https://gtfs.org/blog/) を購読してください。

- :fontawesome-solid-user-group: 開発に関する情報を得るために、[__GTFS Changes__](https://groups.google.com/g/gtfs-changes) Google Group に参加してください。 

- :simple-github: [__GitHub__](https://github.com/google/transit) を訪問して、issue を投稿し、変更に関する議論に参加し、変更を提案してください。 

</div>

**この版の GTFS Digest をお読みいただき、ありがとうございます！2025年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
