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
# [GTFS Digest] 2025年9月号 - 新しい cEMV フィールドが採用されました {: #gtfs-digest-september-2025-new-cemv-field-adopted}

今月、GTFS コミュニティではいくつかの提案について投票が行われ、そのうち2件が仕様に採用されました。その中には、非接触型決済が利用可能であることを簡単に示すための新しい cEMV フィールドも含まれています。GTFS-Realtime における `SPECIAL_EVENT` フィールド、Fares V2 におけるネットワークセット、そして GTFS ファイルのホスティングに関するベストプラクティスについての議論も引き続き行われていますので、ぜひご参加ください。

<!-- more -->

GTFS Digest は、[MobilityData](https://mobilitydata.org/) によって毎月配信されるリソースで、GTFS に関する最新の動向をまとめたものです。

私たちは皆さまからのフィードバックを大切にしており、新しいレイアウトについてのご意見をお聞かせいただきたいと考えています。[こちらのフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 貢献者への感謝 {: #contributor-shoutouts}


*毎月、GTFS コミュニティによる貢献を紹介しています。今月は以下の貢献を特に取り上げたいと思います。*

| 貢献者 | 貢献内容 |
| :---- | :---- |
| **Sierra W.** | GTFS-Fares チャンネルでの初めての貢献 |
| **Masahiro Bessho, Matt Caywood, Masahiko Fukuda, Masaki Ito, M1LL3RD, BKK-Budapest** | PR に対する初めての投票 |
| **ODPT** | 初めてのプロデューサーとして、また PR#545 に対する初めての投票 |

## 🗳️ 現在投票中 {: #currently-voting}


*以下は現在投票中の提案の一覧です。ぜひご覧いただき、投票プロセスにご参加ください。*

| 提案 | 提案者 | 説明 | 投票期限 |
| :---- | :---- | :---- | :---- |
| [[GTFS Fares v2] セマンティクスの明確化 #561](https://github.com/google/transit/pull/561) | Tzujenchanmbd (MobilityData) | GTFS Fares v2 ファイルにおけるセマンティクスを明確化する提案 | 10月6日 |

## 🚀 最近採択された提案 {: #recently-adopted}


*今月は、最終段階を通過した提案を祝福します。以下をご覧ください。* 

| 提案 | 提唱者 | 説明 | 採択日 |
| :---- | :---- | :---- | :---- |
| [Add `cemv_support` field in `agency.txt` and `routes.txt` #545](https://github.com/google/transit/pull/545) | Sergiodero (MobilityData) | このPRでは、`agency.txt` および `routes.txt` に新しい `cemv_support` フィールドを導入し、特定の事業者またはルートにおいて、乗客がコンタクトレスの Europay、Mastercard、Visa を使用して交通サービスを利用できるかどうかを示します。 | 9月29日に投票終了 |
| [Add `stops.stop_access` field #515](https://github.com/google/transit/pull/515) | tzujenchanmbd (MobilityData) | このPRでは、`stops.txt` に `stop_access` フィールドを追加し、特定の駅における停留所等(stop)へのアクセス方法を示します。詳細については [この提案](https://docs.google.com/document/d/1huTq9I6Bs38ZGtcG-7Cpns0kT1njV3PoUCjnjEE0Y1E/edit?tab=t.0#heading=h.4jjq7xol2izb) を参照してください。 | 9月22日に投票終了 |

## 📂 アクティブな提案 {: #active-proposals}


*これらの提案は活発に議論されており、皆様のご意見をお待ちしています！* 

| 提案 | 提唱者 | 説明 | ステータス |
| :---- | :---- | :---- | :---- |
| [[GTFS Fares v2] `networks.txt` が存在する場合に `routes.txt` 内の `network_id` を禁止する更新 #581](https://github.com/google/transit/pull/581) | Skalexch (MobilityData) | このPRは、`routes.txt` 内での `network_id` の存在を禁止するファイルの一覧に `networks.txt` を追加します。 | 議論中 |
| [GTFS および GTFS-realtime の意思決定プロセス #579](https://github.com/google/transit/pull/579)  | Ckraatz (SimplifyTransit) | このPRは、GTFS Schedule および Realtime のガバナンスプロセスを変更することを目的としています。 | 議論中 |
| [`fare_leg_join_rules.txt` 内のネットワークに関する制約を緩和し、ネットワークセットを追加 #578](https://github.com/google/transit/pull/578) | Skalexch (MobilityData) | この提案では、新しい2つのファイル `network_sets.txt` および `network_set_elements.txt` を追加し、同時に `fare_leg_join_rules.txt` に関するいくつかの要件を緩和します。これにより、複数のネットワークにまたがる有効運賃区間を照合できるようになります。 | 議論中 |
| [`communication_period` および `impact_period` の追加 #546](https://github.com/google/transit/pull/546) | Skalexch (MobilityData) | このPRは、GTFS Realtime Alert 仕様の `active_period` フィールドを明確化し、曖昧さを解消するために、`communication_period` および `impact_period` という2つの新しいフィールドを導入します。議論はユースケースと除外条件に焦点を当てています。 |  |
| [GTFS-realtime Service Alerts に新しい `SPECIAL_EVENT` の原因を追加 #577](https://github.com/google/transit/pull/577) | Ckraatz (SimplifyTransit) | この提案では、パレード、スポーツイベント、コンサートなどのイベントによる運行影響に対応するため、GTFS-realtime Service Alerts に「Special Event」という新しい原因(Cause)を追加します。 | 議論中 |
| [`trips.txt` に `trip_route_type` を追加 (GTFS static) #572](https://github.com/google/transit/pull/572) | miklcct (Jnction) | この提案では、`trips.txt` に `trip_route_type` という新しい任意フィールドを追加します。 | 議論中  |
| [GTFS ファイルのホスティングに関する新しいベストプラクティスの追加 #567](https://github.com/google/transit/pull/567) | doconnoronca (Transee) | この提案では、GTFS ファイルのホスティングに関するベストプラクティスを導入し、公共のWebサーバーが非ブラウザリクエストをブロックしたり、地域によってアクセスを制限したりすることを避け、代わりに不正利用防止に重点を置くことを推奨しています。 | 議論中 |

### その他の公開中の提案 {: #other-open-proposals}


* [[GTFS-Fares v2] 距離ベース運賃の追加 #556](https://github.com/google/transit/pull/556)  
* [gtfs-realtime.proto の誤字修正 #541](https://github.com/google/transit/pull/541)  
* [original_trip_id による GTFS Schedule および Realtime の拡張 #534](https://github.com/google/transit/pull/534)  
* [停留所単位で車両の搭載可否を指定するための乗車許可の導入 #533](https://github.com/google/transit/pull/533)  
* [仕様への event_based_trips.txt の追加 #527](https://github.com/google/transit/pull/527)  
* [過去の Stop Time Events を保持するべき #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] `nonconsecutive_transfer_allowed` フィールドの追加および `fare_transfer_type` の明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set のマッチング述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] チケット商品／メディアの乗り継ぎ動作 #423](https://github.com/google/transit/pull/423)

## 🐙 Githubで最も活発な議論 {: #most-active-conversations-on-github}


*Github Issues は、新機能のアイデアや仕様に関する質問など、議論を始めるのに最適な場所です。以下は今月最も活発な議論です。*

| 議論 | 作成者 | 説明 |
| :---- | :---- | :---- |
| [GTFS-TripModifications に Cause と DayTimePeriod を追加する #580](https://github.com/google/transit/issues/580) | **Ckraatz (Simplify Trannsit)** | この議論は、便の変更(Trip Modifications)に新しい情報要素を追加し、変更の原因および変更が行われる日や時間帯を伝達できるようにする可能性を中心にしています。 |

## 🔥 Slack上で最も活発な会話 {: #most-active-conversations-on-slack}


*今月の GTFS Slack チャンネルで最も活発だった議論のまとめです。*

| 投稿者 | 説明 | Slack チャンネル |
| :---- | :---- | :---- |
| Stephen Miller | [GTFS について事業者にインタビューするボランティアを募集](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1758013110596709) | #gtfs |
| Leonard Ehrenfried | [network_id および from/to_area_id を含むケースの解釈](https://mobilitydata-io.slack.com/archives/C01KL7PR170/p1759135967840069) について質問 | #gtfs-fares |
| Lars Persson | [Transit データを使用したアプリ作成に関するアドバイス](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1759224364887579) を求めた | #gtfs-realtime |

## 📅 今後のイベント {: #upcoming-events}


| イベント | 日付 | 開催場所 |
| :---- | :---- | :---- |
| GTFS Fares V2 ワーキンググループ会議 | 2025年10月28日 | [オンライン](https://mobilitydata.org/event/specifications-discussions-gtfs-fares-v2-monthly-meeting-8/2025-10-28/) |

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


<div class="grid cards" markdown>

- :simple-slack: [__Slack__](https://share.mobilitydata.org/slack) に参加し、コミュニティに自己紹介してください。

- :material-newspaper-variant: [__GTFS Digest__](https://gtfs.org/blog/) を購読して、GTFS に関する毎月の最新情報を受け取りましょう。

- :fontawesome-solid-user-group: [__GTFS Changes__](https://groups.google.com/g/gtfs-changes) Google グループに参加して、開発に関する最新情報を入手してください。

- :simple-github: [__GitHub__](https://github.com/google/transit) を訪問して、課題を投稿したり、変更に関する議論に参加したり、変更を提案したりしてください。

</div>

**GTFS Digest のこの号をお読みいただきありがとうございます！2025年以降も最新の GTFS 情報をお届けできることを楽しみにしています。**
