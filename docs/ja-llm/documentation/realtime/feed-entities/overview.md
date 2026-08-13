# フィードエンティティ {: #feed-entities}


GTFS Realtime は、単一の realtime feed 内で組み合わせることができる、4つの異なる種類のリアルタイムデータをサポートしています。以下に概要を示します。完全なドキュメントは該当するセクションに記載されています。

## 便の更新(trip update) {: #trip-updates}

#### 「バスXは5分遅延しています」 {: #bus-x-is-delayed-by-5-minutes}


便の更新(trip update)は、時刻表の変動を表します。リアルタイム対応の、予定されているすべての便(trip)について、便の更新(trip update)を受信することが期待されます。これらの更新は、ルート・路線系統(route)上の停留所等(stop)における予測到着時刻または予測出発時刻を提供します。便の更新(trip update)は、便(trip)が運休となる、時刻表に追加される、あるいは経路変更されるといった、より複雑なシナリオにも対応できます。

[便の更新(trip update)の詳細...](../trip-updates)

## 運行情報(alert) {: #service-alerts}

#### 「Station Y は工事のため閉鎖されています」 {: #station-y-is-closed-due-to-construction}


運行情報は、特定のエンティティに関するより上位レベルの問題を表し、一般に運行障害のテキストによる説明の形式を取ります。

これらは、以下に関する問題を表すことができます。

*   駅
*   路線
*   ネットワーク全体
*   など

運行情報は通常、問題を説明するテキストで構成されます。また、詳細情報のための URL や、この運行情報が誰に影響するかを理解するのに役立つ、より構造化された情報も使用できます。

[運行情報の詳細...](../service-alerts)

## 車両位置情報(vehicle position) {: #vehicle-positions}

#### 「このバスは時刻Yに位置Xにいます」 {: #this-bus-is-at-position-x-at-time-y}


車両位置情報(vehicle position)は、ネットワーク上の特定の車両に関するいくつかの基本的な情報を表します。

最も重要なのは車両がいる緯度と経度ですが、車両から取得した現在の速度や走行距離計の読み取り値に関するデータも使用できます。

[Vehicle Position updatesの詳細...](../vehicle-positions)

## 便の変更(trip modification) {: #trip-modifications}

#### 「これらの便は特定の日に迂回の影響を受けます」 {: #these-trips-are-affected-by-a-detour-on-certain-days}


便の変更(trip modification)は、一連の便に影響する迂回を記述するために使用されます。 

便の変更(trip modification)では、特定の停留所等(stop)をキャンセルし、便の時刻を調整し、
便が通行する新しいルート形状(shape)を提供し、途中にある臨時
停留所等(stop)の位置を提供することができます。

[便の変更(trip modification)の詳細...](../trip-modifications)

## フィードタイプに関する歴史的注記 {: #historical-remark-on-feed-types}


GTFS Realtime Specification の初期バージョンでは、各フィードには単一タイプのエンティティのみを含めることが必須でした。統合されたスキーマからタイプごとのフィードスキーマへ変換するツールの例は、Bliksem Labs の [gtfsrt-examples](https://github.com/bliksemlabs/gtfsrt-examples/blob/master/split_by_entitytype.py) GitHub リポジトリにあります。
