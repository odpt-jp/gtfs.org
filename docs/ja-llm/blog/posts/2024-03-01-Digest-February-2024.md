---
draft: false
pin: false
date:
  created: 2024-03-01
title: GTFS Digest - February 2024
description: Read this month's digest and stay up to date on GTFS development.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2024年2月 - 今年最初の投票の時です！ {: #gtfs-digest-february-2024-its-time-for-the-first-vote-of-the-year}

ガバナンスの強化、言語コード標準、相乗り路線、リアルタイムの洞察 - GTFSコミュニティで最も活発な議論に参加しましょう！


<!-- more -->

GTFS Digestは、GTFSに関する進展の概要を提供するために[MobilityData](https://mobilitydata.org/)が毎月作成しているリソースです。 

皆様からのフィードバックを大切にしており、私たちの取り組みについてご意見を伺いたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**AurelienC**

[相乗り路線の統合](https://github.com/google/transit/issues/430)について、フランスのNational Access Pointを代表して初めてのissueを投稿しました。よく書かれ、よく考えられたissueです！ 

**Weston and Darwin**

これらの活発なコントリビューターは、さらに一歩進んで初めてのPR（[PR#432](https://github.com/google/transit/pull/432)および[PR#434](https://github.com/google/transit/pull/434)）を投稿しました。皆様の貢献には常に感謝しています！

**Santiago Toso**

MobilityDataのslackに参加し、複数の会話を活性化しました。ご参加いただけてうれしく思います。 

**Kayla Firestack**

なんというスーパースターでしょう！Kaylaは[初めてのPR](https://github.com/google/transit/pull/431)で編集上の誤りを指摘し、google/transitにおける40人目のユニークなPR作成者となりました！

**Juliet Eldred**

[初めてのissue](https://github.com/google/transit/issues/435)でGTFSドキュメント内の不整合を指摘していただき、ありがとうございます！皆様のような貢献が、仕様をより利用しやすくする助けとなります。

## 📢 お知らせ {: #announcements}


**[GTFS/GBFS Communications Survey](https://form.typeform.com/to/cSlJFtVc)**

簡単な GTFS/GBFS Community Survey に参加して、未来を形作ることにご協力ください！

わずか4分お時間をいただければ、GTFS/GBFS コミュニティの未来に大きな影響を与えることができます！

## 🗳️ 現在投票中 {: #currently-voting}


**[GTFS Trip-Modifications #403](https://github.com/google/transit/pull/403)**

Trip-Modifications は、便(trip)のルート形状(shape)を変更し、運行されない停留所等(stop)を削除し、必要に応じて一時的な停留所等(stop)を追加するために便(trip)に対して行われる変更です。[Trip-Modifications は主に、迂回運行の可視化およびリアルタイム予測の更新に使用されます。](https://blog.transitapp.com/how-transit-and-swiftly-put-bus-detours-on-the-map/)



* 実験的フィールドに関する投票は、2024-03-06 23:59:59 UTC に終了します。

## 📂 アクティブな提案 {: #active-proposals}


**[GTFS-Flex [投票版] #433 ](https://github.com/google/transit/pull/433)**

GTFS-Flex の提案により、乗客は経路検索ツール上でデマンド型サービスを見つけることができます。この提案は複数回の議論を経ており、現在はコントリビューターによるレビューが行われています。



* この PR は **PR#388 の「投票版」**であり、PR#388 までの変更と完全に整合する変更を含みます。唯一の違いは、この PR が現行の仕様に基づいている一方、PR#388 は昨年7月時点の仕様に基づいていることです。
* 投票に向けた初期採用者からの支援リソースは、まもなく公開されます。

**[Feed Version および GTFS url を GTFS real time FeedHeader に追加 #434](https://github.com/google/transit/pull/434)**

この Realtime の提案では、GTFS real time データとともに使用する GTFS schedule ファイルに関する情報を提供するため、フィードヘッダーに2つの新しいフィールドを追加します。feed_version は GTFS の feed_info.txt ファイル内の feed_version と一致し、GTFS_url は GTFS ファイルの url を指します。この変更を実装する意思のあるプロデューサーが、進行のために必須です。

**[ルートベースの fare_rules における stops.zone_id の条件付き要件を明確化 #432](https://github.com/google/transit/pull/432)** \
この提案は、停留所等(stop)が、fare_id と route_id のみが定義された fare_rules.txt レコード（ルートベースの運賃）に route_id が存在する trip_ids にのみ割り当てられているシナリオを許可するため、stops.zone_id の条件付き要件を変更します。

### その他のオープンな提案 {: #other-open-proposals}


[[GTFS-Fares v2] fare_leg_rules.txt に rule_priority を追加 #418](https://github.com/google/transit/pull/418)

[[GTFS-Fares v2] 同一チケット商品/チケットメディアの乗換挙動 #423](https://github.com/google/transit/pull/423)<span style="text-decoration:underline;"> </span>

[[GTFS-Fares v2] 明確化: チケット商品の定義 #426](https://github.com/google/transit/pull/426)

## 🔥 最も活発な議論 {: #most-active-conversations}


**[[Governance] フェーズ2: 投票とレビューの強化 #436](https://github.com/google/transit/issues/436)**

この GitHub Issue は、GTFS Governance を強化する継続的な取り組みの一部であり、特に2023年12月に公開された[段階的計画](https://github.com/google/transit/issues/413)のフェーズ2で概説された仕様改定プロセスに焦点を当てています。変更には以下が含まれます。



* より早い投票段階の追加
    * MobilityData は、議論対象となる投票の代替案を2つ提案します
* 後の投票段階における多数決投票の導入
* 投票要件を3票から5票へ増加
* レビューガイドラインの正式化
* Pull Request 前の手順の正式化
* 主要な役割と責任の正式化

**[translations.txt で使用される言語コードデータ標準に関する明確化 #435](https://github.com/google/transit/issues/435)**

Trillium の Juliet Eldred は、翻訳で使用されるコードデータ標準に関するドキュメントの不整合を指摘し、正しい情報源がどこにあるかを明確にするため、コミュニティに協力を求めています。 

**[相乗り路線の統合 #430](https://github.com/google/transit/issues/430)**

フランスの National Access Point (NAP) の Aurélien は、バス路線のように振る舞う相乗り路線をモデル化する可能性のある提案について議論するため、GitHub Issue を作成しました。このような路線には正確な停留所等(stop)と目的地がありますが、この場合は、運賃による金銭的インセンティブを受け取ることができる一般の自動車運転者が運行するか、乗客と費用を分担します。議論のために、2つの異なる可能性のある解決策が提示されています。

**[#gtfs-realtime での Slack 会話](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1706193156057089)**

Stefan Begerad は、2つの具体的なケースを念頭に置き、TripUpdate エンティティをフィード内でどのくらいの期間利用可能な状態にしておくべきかについて、コミュニティに指針を求めました。コミュニティは仕様を参照し、Stefan を支援するための有用な洞察を提供しました。 

**[#gtfs-realtime での Slack 会話 (2)](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1708286157675929)**

Marcy は、旅行計画ツール間における Realtime の運行情報(alert)変数の表示に関する参照表または図表を求めています。また、これらの変数を理解するためのベストプラクティスと追加リソースも探しています。コミュニティは、最も近いリソースとして Transit と Google のドキュメントを参照しました。

**[#gtfs-realtime での Slack 会話](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1707413698834939)**

Santiago Toso は次のように質問しています。「CTA、Chicago の gtfs-rt データが利用可能かどうか、ご存じの方はいらっしゃいますか？」コミュニティは貴重なリソースへの複数のリンクで回答し、CTA の Will Anderson は支援を申し出ました。

## 📅 今後のイベント {: #upcoming-events}


**[GTFS-Fares v2 月例会議](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-522966225057)**| 2024年3月26日 @ 午前11時 EDT

トピック : 未定

**[GTFS ガバナンス - 投票およびレビューの強化](https://www.eventbrite.ca/e/gtfs-governance-enhancing-voting-and-reviews-11-am-edt-tickets-852341726047)** (1) | 2024年3月13日 @ 午前11時 EDT  

トピック: 提案された変更の概要および議論

**[GTFS ガバナンス - 投票およびレビューの強化](https://www.eventbrite.ca/e/gtfs-governance-enhancing-voting-and-reviews-8-pm-edt-tickets-852357152187)** (2) | 2024年3月13日 @ 午後8時 EDT  

トピック: 提案された変更の概要および議論

## 💬 GTFS コミュニティに参加する {: #join-the-gtfs-community}


**[GitHub: google/transit](https://github.com/google/transit)**: コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

**[GTFS-changes](https://groups.google.com/g/gtfs-changes)**: 更新が発生したらすぐに受け取りましょう。GTFS-changes Google グループに参加して、新しい pull request と投票に関する情報を入手してください。 

**[GTFS-realtime](https://groups.google.com/g/gtfs-realtime)**: Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

**[GTFS.org](https://gtfs.org/)**: 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS のニーズに対応するための、頻繁に更新されるリソースを見つけることができます。 

**[MobilityData Slack](https://share.mobilitydata.org/slack)**: GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slack の会話に参加してください。1,300 人を超えるモビリティ愛好家がチャンネルで活動しており、質問への回答を迅速に得られる素晴らしい場所です。 

**GTFS Digest 第3版をお読みいただき、ありがとうございます！2024年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
