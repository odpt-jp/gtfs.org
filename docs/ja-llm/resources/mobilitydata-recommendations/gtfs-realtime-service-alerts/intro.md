# GTFS-Realtime 運行情報(alert)ガイダンス {: #gtfs-realtime-service-alerts-guidance}

## はじめに {: #introduction}

[GTFS-Realtime Service Alerts](https://gtfs.org/documentation/realtime/reference/#message-alert) により、プロデューサーはネットワーク上で障害が発生した際に更新情報を提供することができます。Service Alerts は、以下を含む複数の種類の運行障害、インシデント、および情報を表現するために使用できます。

* 運休中のルート・路線系統(route)およびルート区間  
* 閉鎖中の停留所等(stop)および駅  
* 迂回  
* 深刻な運行遅延  
* アクセシビリティの問題（例: エレベーターが稼働していない）  
* 運行に関する一般的なお知らせ

Service Alerts は複数のコンシューマーで使用できます。

* 経路検索アプリ（例: Google Maps、Transit、Apple Maps、Citymapper、Moovit）  
* 交通情報ウェブサイト  
* デジタルサイネージ（バス停、ディスプレイなど）  
* 情報配信チャネル（メール、SMS など）

Service Alerts は、公共交通システムにおいてリアルタイムのインシデントおよび障害が現在発生している、または将来発生することを乗客に伝えるために使用されます。経路検索は、Alert に基づいて障害のあるサービスを提案しないことも選択できます。

以下のスクリーンショットは、GTFS-RT Service Alerts に基づく経路検索の可能な動作を示しています。実際には、alerts は経路検索において以下の目的で使用されます。

* 運行変更および障害について乗客に通知する  
* 関連性のない、または障害のあるルート・路線系統(route)および停留所等(stop)を除外するよう旅程(journey)の提案に影響を与える

<p align="center">
  <img src="../../../../assets/rt-alerts-guide-transit-screenshot.png" width="27%" height="400">
  <img src="../../../../assets/rt-alerts-guide-citymapper-screenshot.png" width="27.5%" height="400">
  <img src="../../../../assets/rt-alerts-guide-citymapper-screenshot-2.png" width="26%" height="400">
</p>
<p align="center"><em>Service Alerts を使用している TransitApp（左）および Citymapper（中央および右）のスクリーンショット。</em></p>

詳細については、以下を参照してください。

* [GTFS-Realtime Service Alerts リファレンスページ](https://gtfs.org/documentation/realtime/reference/)  
* [Service Alerts エンティティページ](https://gtfs.org/documentation/realtime/feed-entities/service-alerts/)

GTFS-Realtime Service Alerts フィードを検証するには、[GTFS Realtime Validator](https://github.com/MobilityData/gtfs-realtime-validator) を参照してください。

## 章 {: #chapters}


この文書では、運行情報を提供するために、Service Alerts を最大限に活用する方法に関するいくつかの推奨事項を詳述します。

1. [Guidelines](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/guidelines) では、特定のフィールドの望ましい使用方法と望ましくない使用方法の可能性を検討します。  
2. [Real-life Use Cases](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/real-life-use-cases) では、考えられる各ユースケースを適切な Service Alert の設定に対応付けます。  
3. [Real-world Examples](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/real-world-examples) では、実際の Service Alerts の例をいくつか取り上げ、それらを評価し、改善案を提案します。
