# GTFS-Realtime 運行情報(alert)ガイダンス {: #gtfs-realtime-service-alerts-guidance}

## はじめに {: #introduction}

[GTFS-Realtime Service Alerts](https://gtfs.org/documentation/realtime/reference/#message-alert) により、プロデューサーはネットワーク上で障害が発生した際に更新情報を提供することができます。Service Alerts は、以下を含む複数の種類のサービス障害、インシデント、および情報を表現するために使用できます。

* 閉鎖されたルート・路線系統(route)およびルート・路線系統(route)区間  
* 閉鎖された停留所等(stop)および駅  
* 迂回  
* サービスの重大な遅延  
* アクセシビリティの問題（例: エレベーターが稼働していない）  
* サービスに関する一般的なお知らせ

Service Alerts は複数のコンシューマーで使用できます。

* 乗換案内アプリ（例: Google Maps、Transit、Apple Maps、Citymapper、Moovit）  
* 公共交通情報ウェブサイト  
* デジタルサイネージ（バス停、ディスプレイなど）  
* 情報チャネル（メール、SMS など）

Service Alerts は、公共交通システムにおいてリアルタイムのインシデントおよび障害が現在または将来発生していることを乗客に伝えるために使用されます。乗換案内は、運行情報(alert)に基づいて、障害のあるサービスを提案しないことも選択できます。

以下のスクリーンショットは、GTFS-RT Service Alerts に基づく乗換案内の可能な動作を示しています。実際には、運行情報(alert)は乗換案内において以下の目的で使用されます。

* サービス変更および障害について乗客に通知する  
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

GTFS-Realtime Service Alerts フィードを検証するには、[GTFS Realtime Validator](https://github.com/MobilityData/gtfs-realtime-validator)を参照してください。

## 章 {: #chapters}


この文書では、運行情報を提供するためにService Alertsを最大限に活用する方法に関する、いくつかの推奨事項を詳述します。

1. [ガイドライン](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/guidelines)では、特定のフィールドの望ましい使用方法と望ましくない使用方法の可能性を検討します。  
2. [実際のユースケース](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/real-life-use-cases)では、考えられる各ユースケースを適切なService Alertの設定に対応付けます。  
3. [実世界の例](../../../../resources/mobilitydata-recommendations/gtfs-realtime-service-alerts/real-world-examples)では、実際のService Alertsの例をいくつか取り上げて評価し、改善案を提案します。
