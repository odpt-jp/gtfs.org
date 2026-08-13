---
draft: false
pin: false
date:
  created: 2024-02-01
title: GTFS Digest - January 2024
description: Read this month's digest and stay up to date on GTFS development.
authors: 
  - mobilitydata
categories:
  - GTFS Digest
---
# [GTFS Digest] 2024年1月 - 年初から最新情報を把握しましょう。 {: #gtfs-digest-january-2024-start-the-year-in-the-know}

可視性の向上と価値ある改善に関するコミュニティでの議論のため、保留中のベストプラクティスをGTFS仕様へ移行します。GTFS Governanceの改善、短期的なサービス変更、GTFS-realtimeの利用、およびEntity-Relationship Model（ERD）の採用に関する議論に参加してください。 


<!-- more -->

GTFS Digestは、GTFSに関する動向の概要を提供するために[MobilityData](https://mobilitydata.org/)が毎月作成するリソースです。 

皆様からのフィードバックを大切にしており、私たちの取り組みについてお聞かせいただきたいと考えています。[このフォーム](https://forms.gle/GGefktvemnJD5Q9g8)にご記入いただき、このツールの可能性を最大限に引き出すためにご協力ください。

## 🏅 コントリビューターへの称賛 {: #contributor-shoutouts}


**Martijn Vanallemeersch**

Slackに参加し、初日に[列車の編成短縮に関する活発な議論](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1704902620455499)を提起してくださいました。 

**Marcy Jaffe**

いつもながら、小規模な交通事業者に配慮し、[GTFS validatorに関する重要な点](https://mobilitydata-io.slack.com/archives/C03E10N96QL/p1703074450467169)を共有してくださり、ありがとうございます。 

**doconnoronca**

[短期的な運行変更について明確化を求めて](https://github.com/google/transit/issues/425)くださり、ありがとうございます。貴重なやり取りのきっかけとなりました。

## 📢 お知らせ {: #announcements}


**[未解決の Best Practices issue および PR の移行 #421](https://github.com/google/transit/issues/421) **

GTFS Best Practices の仕様への移行の一環として、MobilityData は未解決のすべての issue および PR を GTFS 仕様リポジトリへ移管しました。[Issue #421](https://github.com/google/transit/issues/421) では、移行プロセスおよび提案されている次のステップについて詳しく説明しています。これにより、保留中の Best Practices の提案により多くの注目を集め、それらに関する議論を再開し、コミュニティが価値を見出すあらゆる改善を推進できるようにしたいと考えています。

## 📂 アクティブな提案 {: #active-proposals}


**[GTFS-Flex #388](https://github.com/google/transit/pull/388)** 

GTFS-Flex 提案により、乗客は経路検索ツール上でデマンド型サービスを見つけることができます。この提案は複数回の議論を経ており、現在はコントリビューターによるレビュー中です。 



* 直近の GTFS-Flex ワーキンググループ会議は 2024年1月17日に開催されました。
    * location_groups の正規化を別ファイルに追加することについて合意しました。
    * ファイル名について合意しました。
        * locations.geojson - この名前を維持します。
        * 既存の location_groups.txt → location_group_stops.txt
        * 新しい正規化ファイル → location_groups.txt

**[GTFS Trip-Modifications #403](https://github.com/google/transit/pull/403) **

便の変更(trip modification)は、ルート形状(shape)を変更し、運行されない停留所等(stop)を削除し、必要に応じて一時的な停留所等(stop)を追加するために便(trip)に対して行われる変更です。[Trip-Modifications は主に迂回経路の可視化およびリアルタイム予測の更新に使用されます。](https://blog.transitapp.com/how-transit-and-swiftly-put-bus-detours-on-the-map/)

初回会議は 2024年1月10日に開催されました。議論された項目は以下のとおりです。



* GTFS-TripModifications と GTFS-NewTrips
    * TripModifications によるサイズ面の利点は十分に大きいため、このソリューションを進めるべきであるという合意がありました。
* 停留所等(stop)について stop sequence のみを選択子として維持するか、stop ids を使用して停留所等(stop)を選択できるよう変更するか
    * stop_id を使用することの有用性（および生成の容易さ）は十分に高いため、変更を行うべきであるという合意がありました。Transit は具体的な提案に取り組む予定です。

**[[GTFS-Fares v2] fare_leg_rules.txt に rule_priority を追加 #418](https://github.com/google/transit/pull/418)**

rule_priority フィールドは、一致するルールが乗車区間(leg)に適用される優先順位を定義し、特定のルールが他のルールより優先されるようにします。このフィールドの存在はトリガーとして機能し、空のセマンティクスを「anything except」から「一致に影響しない」へ変更します。

**[[GTFS-Fares v2] 同一チケット商品／チケットメディアの乗換動作](https://github.com/google/transit/pull/423)** \
2つの乗車区間(leg)間の特定の乗換ルールにおいて、同一のチケット商品／チケットメディアの使用が必要かどうかを区別するための仕組みです。この仕組みにより、「チケットベースのシステム」（すなわち、パスに類似した商品）と「ストアドバリューシステム」（すなわち、一般的な従量課金商品）を区別できます。

## 🔥 最も活発な議論 {: #most-active-conversations}


**[GTFSガバナンスの変更：段階的計画 #413](https://github.com/google/transit/issues/413)**

MobilityDataは、正式な改正プロセスとGTFSガバナンスの非公式な側面の両方を改善するための改良を提案しています。このissueには、コミュニティで特定された一般的な問題と、提案された段階的計画が含まれています。


* フェーズ1は完了しています： 
    * [GitHubでのIssueテンプレートの追加](https://github.com/google/transit/pull/417)
    * [GTFS Digestの公開](https://github.com/google/transit/issues/419)
    * [GitHubでのラベルの変更](https://github.com/google/transit/labels) 
* フェーズ2は進行中です： 
    * 投票提案を策定中

**[決定的な参照としてEntity-Relationship Modelを使用する #415](https://github.com/google/transit/issues/415) **

ほとんどの議論は、構造そのものを説明することよりも、データの形式に関するものです。提案は、誰もが参照できる明確かつ正式なモデルを用意し、議論を容易にして混乱を避けることです。dbdiagram.ioのようなツールを使用して、GTFSデータモデルを視覚的に表現し、形式化するという考えです。

**[短期間の運行変更をGTFSから除外することが推奨されているのはなぜですか？ #425](https://github.com/google/transit/issues/425)**

この議論は、GTFSが1週間以内の運行変更にGTFS-realtimeを使用することを提案している理由についてです。寄稿者は、このバッファによりコンシューマーがフィードを処理する時間を確保でき、データ品質の確保、エラーの修正、新しい時刻表の適時表示に役立つと述べています。また、多数のGTFS-realtime更新を管理することについても議論しています。

**[#gtfsでのSlack会話](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1704902620455499)**

この議論で、Martijnは鉄道業界における一般的な慣行、すなわち列車の分割、減車、連結をどのようにモデル化するかについて、コミュニティにアイデアを求めています。主に地域間または都市間の移動で使用されるこれらの慣行は、異なる目的地に対応するために車両を分離および／または連結するものです。

**[#gtfs-realtimeでのSlack会話](https://mobilitydata-io.slack.com/archives/C3D321CKB/p1704895869851189) **

Joelは、次の質問への回答にあなたの助けを必要としています：「前の停留所等(stop)からのSKIPPED stop updateは、それらの停留所等(stop)にデータ内の更新がない場合、次の停留所等(stop)へ伝播されるべきですか？」

**[#gtfsでのSlack会話](https://mobilitydata-io.slack.com/archives/C3FFFKX9C/p1705020453309289) **

Evanは次の質問をしています：「特にFrequency-based service（exact_times=0）に従うfrequencies.txtを含むフィードを生成／利用する人々について、皆さんは次の文言をどの程度厳密に解釈していますか？」

## 📅 今後のイベント {: #upcoming-events}


**[GTFS-Fares v2 月例会議](https://www.eventbrite.ca/e/specifications-discussions-gtfs-fares-v2-monthly-meetings-tickets-522966225057)**| 2024年1月23日 午前11時 EST

トピック：バックログのレビューおよび1つのルート・路線系統(route)の挙動

## GTFS コミュニティに参加する {: #join-the-gtfs-community}


**[GitHub: google/transit](https://github.com/google/transit)**: コミュニティとアイデアを共有しましょう！公式 GTFS GitHub リポジトリに参加してください。

**[GTFS-changes](https://groups.google.com/g/gtfs-changes)**: 更新が発生したらすぐに受け取りましょう。GTFS-changes Google グループに参加して、新しい pull request と投票に関する情報を入手してください。 

**[GTFS-realtime](https://groups.google.com/g/gtfs-realtime)**: Realtime に関するあらゆる話題について議論し、最新情報を把握しましょう。このグループでは、GTFS Realtime について議論し、質問を行い、変更を提案しています。

**[GTFS.org](https://gtfs.org/)**: 公式 GTFS ドキュメント Web サイトです。ここでは、GTFS に必要な情報について頻繁に更新されるリソースを見つけることができます。 

**[MobilityData Slack](https://share.mobilitydata.org/slack)**: GTFS について質問がありますか、またはコミュニティとつながる必要がありますか？GTFS Slack の会話に参加してください。1,300 人を超えるモビリティ愛好家が参加するチャンネルで、質問への回答を迅速に得られる素晴らしい場所です。 

**GTFS Digest 第2版をお読みいただき、ありがとうございます！2024年以降も、最新の GTFS 更新情報をお届けできることを楽しみにしています。**
