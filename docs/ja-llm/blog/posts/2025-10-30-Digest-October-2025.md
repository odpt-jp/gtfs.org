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
# [GTFS ダイジェスト] 2025年10月 - 活発なGTFSの1か月を祝して {: #gtfs-digest-october-2025-celebrating-an-active-month-for-gtfs}

今月、私たちのコントリビューターは仕様への取り組みに精力的に活動してきました。小さな編集上の変更による詳細の改善から、ブランドロゴを含めるための新機能の提案に至るまで、コミュニティの創造性と協調性が際立っています。

<!-- more -->

GTFS ダイジェストは、[MobilityData](https://mobilitydata.org/) によって毎月配信されるリソースであり、GTFS に関する最新の動向を概観するものです。

私たちは皆さまからのフィードバックを非常に重視しており、新しいレイアウトについてのご意見を伺いたいと考えています。ぜひ [このフォーム](https://forms.gle/GGefktvemnJD5Q9g8) にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 貢献者への感謝 {: #contributor-shoutouts}


*毎月、GTFS コミュニティによる貢献を紹介しています。今月は以下の貢献を特に取り上げたいと思います。*

| 貢献者 | 貢献内容 |
| :---- | :---- |
| Dominik Skarżyński | 新しい貢献者であり、[calendar.txt における重複しない service_id についての議論](https://github.com/google/transit/issues/584)を開始しました。 |
| Felix Gündling | [アプリ内で交通事業者のブランドロゴを表示するための pull request](https://github.com/google/transit/pull/585)を作成し、活発な議論を引き起こしました。 |

## 🗳️ 現在の投票中の提案 {: #currently-voting}

現在、投票プロセス中の提案はありません。

## 🚀 最近採択された提案 {: #recently-adopted}


*今月は、最終段階を通過した提案を祝福します。以下をご覧ください。* 

| 提案 | 提案者 | 説明 | 採択日 |
| :---- | :---- | :---- | :---- |
| [[GTFS Fares v2] セマンティクスの明確化 #561](https://github.com/google/transit/pull/561) | Tzujenchanmbd (MobilityData) | GTFS Fares v2 ファイルにおけるセマンティクスを明確化する提案 | 2025年10月14日 |

## 📂 アクティブな提案 {: #active-proposals}


*これらの提案は活発に議論されており、皆様のご意見をお待ちしています！* 

| 提案 | 提唱者 | 説明 | ステータス |
| :---- | :---- | :---- | :---- |
| [images.txt と agency logo の追加 #585](https://github.com/google/transit/pull/585) | Felixguendling (Transitous) | この提案は、`agency.txt` に新しいカラム `agency_brand_image_url` を追加し、ブランドロゴを経路検索アプリケーションに表示できるようにするものです。 | 議論中 |
| [GTFS-realtime Service Alerts に新しい SPECIAL_EVENT の Cause を追加 #577](https://github.com/google/transit/pull/577) | Ckraatz (SimplifyTransit) | この提案は、パレード、スポーツイベント、コンサートなどの混乱に対応するため、GTFS-realtime Service Alerts に「Special Event」という新しい Cause を追加するものです。 | 議論中 |
| [GTFS ファイルのホスティングに関する新しいベストプラクティスの追加 #567](https://github.com/google/transit/pull/567) | doconnoronca (Transee) | この提案は、GTFS ファイルのホスティングに関するベストプラクティスを導入し、公共の Web サーバーが非ブラウザリクエストをブロックしたり、地域によってアクセスを制限したりするのを避け、代わりに不正利用の防止に焦点を当てることを推奨するものです。 | 議論中 |
| [[GTFS-Fares v2] 距離ベース運賃の追加 #556](https://github.com/google/transit/pull/556) | skalexch (MobilityData) | このプルリクエストは、`fare_leg_rules.txt` および `stop_times.txt` に複数の新しいフィールドを導入し、さらに新しいファイル `fare_leg_distance_rules.txt` を追加することで、距離ベースの運賃システムをモデル化できる新機能を追加するものです。 | 議論中 |
| [[GTFS Fares v2] network sets の追加および fare_leg_join_rules.txt における network 制約の緩和 #578](https://github.com/google/transit/pull/578) | skalexch (MobilityData) | この提案は、`network_sets.txt` および `network_set_elements.txt` という2つの新しいファイルを追加し、同時に `fare_leg_join_rules.txt` に関するいくつかの要件を緩和するものです。これにより、複数のネットワークにまたがる有効運賃区間を対応付けることが可能になります。 | 議論中 |

### その他の公開中の提案 {: #other-open-proposals}


* [Add missing spaces #587](https://github.com/google/transit/pull/587)  
* [GTFS および GTFS-realtime の意思決定プロセス #579](https://github.com/google/transit/pull/579)  
* [GTFS static に trip_route_type を追加 #572](https://github.com/google/transit/pull/572)  
* [communication_period および impact_period の追加 #546](https://github.com/google/transit/pull/546)  
* [original_trip_id による GTFS Schedule および Realtime の拡張 #534](https://github.com/google/transit/pull/534)  
* [停留所単位で車両の搭載可否を指定するための乗車許可の導入 #533](https://github.com/google/transit/pull/533)  
* [仕様への event_based_trips.txt の追加 #527](https://github.com/google/transit/pull/527)  
* [過去の Stop Time Events を保持するべき #502](https://github.com/google/transit/pull/502)  
* [[GTFS Fares v2] nonconsecutive_transfer_allowed フィールドの追加および fare_transfer_type の明確化 #498](https://github.com/google/transit/pull/498)  
* [[GTFS Fares v2] Area Set のマッチング述語 #483](https://github.com/google/transit/pull/483)  
* [[GTFS-Fares v2] Fare product/media の乗り継ぎ動作 #423](https://github.com/google/transit/pull/423)

## 🐙 Githubで最も活発な議論 {: #most-active-conversations-on-github}


*Github Issuesは、新機能のアイデアや仕様に関する質問など、議論を始めるのに最適な場所です。以下は今月最も活発な議論です。*

| 議論 | 作成者 | 説明 |
| :---- | :---- | :---- |
| [寄生を止めよう：オープンな交通データを保護する](https://github.com/google/transit/issues/586) | Stefan de Konink | ベンダーによって追加されたリファラリンクに関する、GTFSのライセンスおよび利用についての議論です。 |
| [提案：calendar.txtで重複しないservice_idの重複を許可する](https://github.com/google/transit/issues/584) | Dominik Skarżyński | この提案は、同じservice_idに対してcalendar.txt内で複数の重複しない日付範囲を許可することで、GTFS内のデータ重複を減らすことを目的としています。 |

## 🔥 Slackで最も活発な会話 {: #most-active-conversations-on-slack}


*今月のGTFS Slackチャンネルで最も活発だったディスカッションのまとめです。*

| 投稿者 | 説明 | Slackチャンネル |
| :---- | :---- | :---- |
| Gabriel Masiero | [GTFS Realtimeにおけるバス位置情報のプロジェクト](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1761134635290789)について質問しました。 | [#gtfs-realtime](https://mobilitydata-io.slack.com/archives/C3D321CKB) |
| Eva Leake | [便(trip)に対する「最大距離」制限の実装](https://mobilitydata-io.slack.com/archives/CSP7HDF37/p1761232949894399)について質問しました。 | [#gtfs-flex](https://mobilitydata-io.slack.com/archives/CSP7HDF37) |

## 📅 今後のイベント {: #upcoming-events}


| イベント | 日付 | 開催場所 |
| :---- | :---- | :---- |
| GTFS Fares V2 ワーキンググループ会議 | 2025年11月25日 | [オンライン](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-1230499460009) |

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


<div class="grid cards" markdown>

- :simple-slack: [__Slack__](https://share.mobilitydata.org/slack) に参加し、コミュニティに自己紹介してください。

- :material-newspaper-variant: [__GTFS Digest__](https://gtfs.org/blog/) を購読して、GTFS に関する毎月の最新情報を受け取りましょう。

- :fontawesome-solid-user-group: [__GTFS Changes__](https://groups.google.com/g/gtfs-changes) Google グループに参加して、開発に関する最新情報を入手してください。

- :simple-github: [__GitHub__](https://github.com/google/transit) を訪問して、課題を投稿したり、変更に関する議論に参加したり、変更を提案したりしてください。

</div>

**GTFS Digest のこの号をお読みいただきありがとうございます！2025 年以降も最新の GTFS 情報をお届けできることを楽しみにしています。**
