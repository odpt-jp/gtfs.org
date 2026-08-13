# GTFS Realtime ベストプラクティス {: #gtfs-realtime-best-practices}

## はじめに {: #introduction}


これは、[GTFS Realtime](../reference) データ形式でリアルタイムの公共交通情報を記述するための推奨プラクティスです。

### ドキュメント構造 {: #document-structure}


推奨プラクティスは、主に2つのセクションに整理されています。

* __[メッセージ別に整理されたプラクティスの推奨事項](#practice-recommendations-organized-by-message):__ 推奨事項は、公式の GTFS Realtime リファレンスで説明されているものと同じ順序で、メッセージおよびフィールドごとに整理されています。
* __[ケース別に整理されたプラクティスの推奨事項](#practice-recommendations-organized-by-use-case):__ 頻度ベースのサービス（時刻表ベースのサービスとの対比）などの特定のケースでは、対応する GTFS Schedule データに加えて、複数のメッセージおよびフィールドにまたがってプラクティスを適用する必要がある場合があります。そのような推奨事項は、このセクションに集約されています。

### フィードの公開および一般的な実践 {: #feed-publishing-general-practices}


* フィードは、公開され永続的なURLで公開するべきです
* フィードにアクセスするためにログインを要求せず、URLに直接アクセスできるべきです。必要に応じてAPIキーを使用することができますが、APIキーの登録は自動化され、すべての人が利用できるべきです。
* GTFS Realtimeフィード内の永続的な識別子（idフィールド）（例: FeedEntity.id、VehicleDescriptor.id、CarriageDetails.id）を、フィードの更新間で維持してください。
* GTFS Realtimeフィードは、少なくとも30秒ごと、またはフィード内で表される情報（車両の位置）が変更されるたびのいずれかより頻繁な方で更新するべきです。VehiclePositionsは他のフィードエンティティよりも頻繁に変更される傾向があり、可能な限り頻繁に更新するべきです。コンテンツが変更されていない場合でも、その時点で情報が依然として有効であることを反映する新しい`FeedHeader.timestamp`でフィードを更新するべきです。
* GTFS Realtimeフィード内のデータは、便の更新(trip update)および車両位置情報(vehicle position)については90秒より古くならないようにし、運行情報(alert)については10分より古くならないようにするべきです。たとえば、producerが30秒ごとに`FeedHeader.timestamp`タイムスタンプを継続的に更新している場合でも、そのフィード内のVehiclePositionsの経過時間は90秒より古くならないようにするべきです。
* GTFS Realtimeデータをホストするサーバーは信頼性が高く、一貫して有効な形式のprotobufエンコード済みレスポンスを返すべきです。無効なレスポンス（protobufエラーまたは取得エラー）は、レスポンス全体の1%未満であるべきです。
* GTFS Realtimeデータをホストするweb-serverは、consumerが`If-Modified-Since` HTTPヘッダーを活用できるよう、ファイルの更新日時を正しく報告するように設定するべきです（HTTP/1.1 - Request for Comments 2616のSection 14.29を参照してください）。これにより、変更されていないフィードコンテンツの転送を回避し、producerとconsumerの帯域幅を節約できます。
* フィードは、指定されたURLに対するHTTPリクエストで照会された場合、デフォルトでprotocol bufferエンコード済みのフィードコンテンツを提供するべきです。consumerは、protocol-bufferエンコード済みコンテンツを受信するために特別なHTTP acceptヘッダーを定義する必要がないようにするべきです。
* protocol buffersが[optional values](https://developers.google.com/protocol-buffers/docs/proto#optional)をエンコードする方法により、consumerはGTFS Realtimeフィードからデータを読み取る前に、値を使用する前にprotocol bufferによって生成された`hasX()`メソッドを使用して値の存在を確認し、`hasX()`がtrueの場合にのみその値を使用するべきです（`X`はフィールド名です）。`hasX()`が`false`を返す場合、`gtfs-realtime.proto`値で定義されたそのフィールドのデフォルト値を想定するべきです。consumerが最初に`hasX()`メソッドを確認せずに値を使用する場合、producerが意図して公開していないデフォルトデータを読み取っている可能性があります。
* フィードの完全性を確保するため、フィードはHTTP（暗号化なし）ではなくHTTPSを使用するべきです。
* フィードは、対応する静的GTFSデータセットに含まれる便(trip)の大部分をカバーするべきです。特に、高密度かつ交通量の多い都市部、および混雑するルート・路線系統(route)のデータを含めるべきです。

## メッセージ別に整理された実践上の推奨事項 {: #practice-recommendations-organized-by-message}

### FeedHeader {: #feedheader}


| フィールド名 | 推奨事項 |
| --- | --- |
| `gtfs_realtime_version` | 現在のバージョンは「2.0」です。GTFS Realtime の初期バージョンでは、さまざまな交通状況を適切に表現するために必要なすべてのフィールドが必須ではなかったため、すべての GTFS Realtime フィードは「2.0」以上であるべきです。 |
| `timestamp` | この timestamp は、連続する2回のフィード反復の間で減少するべきではありません。 |
|  | フィード内容が変更された場合、この timestamp 値は常に変更されるべきです。ヘッダーの `timestamp` を更新せずにフィード内容を変更するべきではありません。<br><br>*よくある誤り* - ロードバランサーの背後に GTFS Realtime フィードの複数のインスタンスがある場合、各インスタンスはリアルタイムデータソースから情報を取得し、わずかに同期がずれた状態でコンシューマーに公開している可能性があります。GTFS Realtime コンシューマーが連続して2回リクエストを行い、各リクエストが異なる GTFS Realtime フィードインスタンスによって処理される場合、同じフィード内容が異なる timestamp とともにコンシューマーに返される可能性があります。<br><br>*考えられる解決策* - プロデューサーは `Last-Modified` HTTP ヘッダーを提供するべきであり、コンシューマーは古いデータの受信を避けるため、最新の `If-Modified-Since` HTTP ヘッダーを渡すべきです。<br><br>*考えられる解決策* - HTTP ヘッダーを使用できない場合、各コンシューマーが同じプロデューサーサーバーにルーティングされることを保証するために、スティッキーセッションなどの選択肢を使用することができます。 |

### FeedEntity {: #feedentity}


すべてのエンティティは、ユーザーにとって関連性がなくなった場合にのみ、GTFS Realtime feed から削除するべきです。feed はステートレスであると見なされます。つまり、各 feed は交通システムのリアルタイム状態全体を反映します。あるエンティティが1つの feed インスタンスで提供され、その後の feed 更新で削除された場合、そのエンティティにはリアルタイム情報がないと仮定するべきです。

| フィールド名 | 推奨事項 |
| --- | --- |
| `id` | 便(trip)の全期間にわたって安定して維持するべきです |

### TripUpdate {: #tripupdate}


便のキャンセルに関する一般的なガイドライン:

* 複数日にわたって便をキャンセルする場合、producer は、指定された `trip_ids` および `start_dates` を参照する `CANCELED` の TripUpdates と、同じ `trip_ids` および乗客にキャンセルの理由（例: 迂回）を説明するために表示できる `TimeRange` を参照する `NO_SERVICE` の Alert を提供するべきです。
* 便内のどの停留所等(stop)にも訪問しない場合、その便が `NEW` または `DUPLICATED` の便であり、その後キャンセルされた場合を除き、すべての `stop_time_updates` を `SKIPPED` としてマークするのではなく、その便を `CANCELED` とするべきです。 

| フィールド名 | 推奨事項 |
| --- | --- |
| `trip` | [message TripDescriptor](#tripdescriptor) を参照してください。 |
|  | 個別の `VehiclePosition` および `TripUpdate` フィードが提供される場合、2つのフィード間で [TripDescriptor](#tripdescriptor) と [VehicleDescriptor](#vehicledescriptor) の ID 値の組み合わせは一致するべきです。<br><br>例えば、ある `VehiclePosition` entity が `vehicle_id:A` および `trip_id:4` を持つ場合、対応する `TripUpdate` entity も `vehicle_id:A` および `trip_id:4` を持つべきです。いずれかの `TripUpdate` entity が `trip_id:4` と 4 以外の `vehicle_id` を持つ場合、これはエラーです。 |
| `vehicle` | [message VehicleDescriptor](#vehicledescriptor) を参照してください。 |
|  | 個別の `VehiclePosition` および `TripUpdate` フィードが提供される場合、2つのフィード間で [TripDescriptor](#tripdescriptor) と [VehicleDescriptor](#vehicledescriptor) の ID 値の組み合わせは一致するべきです。<br><br>例えば、ある `VehiclePosition` entity が `vehicle_id:A` および `trip_id:4` を持つ場合、対応する `TripUpdate` entity も `vehicle_id:A` および `trip_id:4` を持つべきです。いずれかの `TripUpdate` entity が `trip_id:4` と 4 以外の `vehicle_id` を持つ場合、これはエラーです。 |
| `stop_time_update` | 指定された `trip_id` の `stop_time_updates` は、`stop_sequence` の昇順で厳密に並べるべきであり、`stop_sequence` を繰り返してはいけません。 |
|  | 便が運行中である間、すべての `TripUpdates` には、将来の予測到着時刻または出発時刻を持つ `stop_time_update` を少なくとも1つ含めるべきです。[GTFS Realtime spec](../feed-entities/trip-updates/#stoptimeupdate) では、指定された便について将来の予定到着時刻を持つ停留所等(stop)を参照する過去の `StopTimeUpdate` を producer は削除するべきではないと述べていることに注意してください（すなわち、車両が予定より早くその停留所等(stop)を通過した場合）。そうしない場合、この停留所等(stop)には更新がないと判断されます。 |
| `timestamp` | この便に対するこの予測が更新された時刻を反映するべきです。 |
| `delay` | `TripUpdate.delay` は、予定からの逸脱、すなわち車両が予定よりどの程度早いか／遅れているかについて観測された過去の値を表すべきです。将来の停留所等(stop)に対する予測は、`StopTimeEvent.delay` または `StopTimeEvent.time` を通じて提供するべきです。 |

### TripDescriptor {: #tripdescriptor}


個別の `VehiclePosition` および `TripUpdate` フィードが提供される場合、2つのフィード間で [TripDescriptor](#tripdescriptor) と [VehicleDescriptor](#vehicledescriptor) の ID 値の対応付けは一致しなければなりません。

例えば、`VehiclePosition` エンティティに `vehicle_id:A` および `trip_id:4` がある場合、対応する `TripUpdate` エンティティにも `vehicle_id:A` および `trip_id:4` があるべきです。

| フィールド名 | 推奨事項 |
| --- | --- |
| `schedule_relationship` | `ADDED` 便(trip)の動作は規定されておらず、この列挙型の使用は推奨されません。<br/>便(trip)が元々運行予定ではない場合、既存の便(trip)の停車パターンに従わない場合は `NEW` を使用し、既存の便(trip)の複製である場合は `DUPLICATED` を使用してください。<br/>便(trip)が変更されたダイヤまたは停車で運行されるものの、GTFS static 内の元の予定便(trip)に関連付けることができる場合は、`REPLACEMENT` を使用し、変更された便(trip)の停車時刻(stop_time)の完全なリストを指定してください。 |

### TripProperties {: #tripproperties}


| フィールド名 | 推奨事項 |
| --- | --- |
| `trip_headsign` | `TripDescriptor.schedule_relationship` = `NEW` の便(trip)には常に提供するべきです。また、便(trip)が迂回する場合は、`TripDescriptor.schedule_relationship` = `REPLACEMENT` の便(trip)にも提供するべきです。 |

### VehicleDescriptor {: #vehicledescriptor}


個別の `VehiclePosition` および `TripUpdate` フィードが提供される場合、2つのフィード間で [TripDescriptor](#tripdescriptor) および [VehicleDescriptor](#vehicledescriptor) の ID 値の対応付けが一致するべきです。

例えば、`VehiclePosition` エンティティが `vehicle_id:A` および `trip_id:4` を持つ場合、対応する `TripUpdate` エンティティも `vehicle_id:A` および `trip_id:4` を持つべきです。

| フィールド名 | 推奨事項 |
| --- | --- |
| `id` | 便(trip)の全期間にわたって車両を一意かつ安定して識別するべきです |

### StopTimeUpdate {: #stoptimeupdate}


| フィールド名 | 推奨事項 |
| --- | --- |
| `stop_sequence` | 可能な限り `stop_sequence` を提供するべきです。これは、便(trip)内で複数回出現する可能性がある `stop_id`（例: ループ路線）とは異なり、`stop_times.txt` 内の GTFS 停車時刻(stop_time)を一意に特定するためです。 |
| `arrival` | 連続する停留所等(stop)間の到着時刻は増加するべきです。同一または減少してはいけません。 | 
|         | 到着 `time`（[StopTimeEvent](#stoptimeevent) で指定）は、停車または待機時間が想定される場合、同じ停留所等(stop)の出発 `time` より前であるべきです。それ以外の場合、到着 `time` は出発 `time` と同一であるべきです。 |
| `departure` | 連続する停留所等(stop)間の出発時刻は増加するべきです。同一または減少してはいけません。 |
|           | 出発 `time`（[StopTimeEvent](#stoptimeevent) で指定）は、停車または待機時間が想定されない場合、同じ停留所等(stop)の到着 `time` と同一であるべきです。それ以外の場合、出発 `time` は到着 `time` より後であるべきです。 |

### StopTimeEvent {: #stoptimeevent}


| フィールド名 | 推奨事項 |
| --- | --- |
| `delay` | `stop_time_update` の `arrival` または `departure` において `delay` のみが提供される場合（`time` は提供されない場合）、GTFS の [`stop_times.txt`](../../schedule/reference/#stop_timestxt) には、対応するこれらの停留所等(stop)の `arrival_times` および／または `departure_times` が含まれるべきです。GTFS の `stop_times.txt` ファイル内で加算する時刻がなければ、realtime feed 内の `delay` 値には意味がありません。 |
| `scheduled_time` | 便(trip)が新規または代替の便であり、その便が時刻表に従って運行される場合（代替便の場合は変更された時刻表であることがあります）、すべての timepoints に対して `scheduled_time` を提供するべきです。複製された便の運行時間または停車時間が元の便と異なる場合、それらを指定するために `scheduled_time` を使用することもできます。 |

### VehiclePosition {: #vehicleposition}


以下は、消費者に高品質なデータ（例: 予測の生成用）を提供するために、VehiclePostions feed に含めるべき推奨フィールドです。

| フィールド名 | 注記 |
| --- | --- |
| `entity.id` | 便(trip)の全期間にわたって安定して維持するべきです |
| `vehicle.timestamp` | 車両位置情報(vehicle position)が測定された時刻の timestamp を提供することを強く推奨します。そうしない場合、消費者は message timestamp を使用しなければならず、最後の message が個々の位置情報よりも頻繁に更新される場合、乗客にとって誤解を招く結果となる可能性があります。 |
| `vehicle.vehicle.id` | 便(trip)の全期間にわたって車両を一意かつ安定して識別するべきです |

### 位置情報 {: #position}


この `trip_id` に対して `DETOUR` の effect を持つ運行情報(alert)が存在する場合を除き、車両位置情報(vehicle position)は、現在の便(trip)に対応する GTFS `shapes.txt` データから200メートル以内にあるべきです。

### 運行情報(alert) {: #alert}


運行情報(alert)に関する一般的なガイドライン:

* `trip_id` および `start_time` が `exact_time=1` の間隔内にある場合、`start_time` は `headway_secs` の正確な倍数だけ間隔の開始時刻より後であるべきです。 
* 複数日にわたって便(trip)をキャンセルする場合、提供者は、指定された `trip_ids` および `start_dates` を参照する `CANCELED` のTripUpdatesと、同じ `trip_ids` および `TimeRange` を参照し、キャンセル（例: 迂回）を説明するために乗客に表示できる `NO_SERVICE` の運行情報(alert)を提供するべきです。
* 運行情報(alert)が路線上のすべての停留所等(stop)に影響する場合、停留所等(stop)ベースの運行情報(alert)ではなく、路線ベースの運行情報(alert)を使用してください。路線上のすべての停留所等(stop)に運行情報(alert)を適用してはいけません。
* 運行情報(alert)には文字数制限はありませんが、公共交通機関の乗客はモバイルデバイスで運行情報(alert)を閲覧することが多いです。簡潔にしてください。

| フィールド名 | 推奨事項 |
| --- | --- |
| `description_text` | 運行情報(alert)を読みやすくするために改行を使用してください。 |

## ユースケース別に整理された実践上の推奨事項 {: #practice-recommendations-organized-by-use-case}

### 頻度ベースの便 {: #frequency-based-trips}


頻度ベースの便は固定された時刻表に従わず、あらかじめ定められた運行間隔を維持しようとします。これらの便は、[GTFS frequency.txt](../../schedule/reference/#frequenciestxt) において `exact_times=0` を設定するか、`exact_times` フィールドを省略することで示されます（`exact_times=1` の便は頻度ベースの便では*ありません*。`exact_times=1` を含む `frequencies.txt` は、時刻表ベースの便をよりコンパクトな形式で保存するための便宜的な方法として使用されるだけです）。頻度ベースの便に対する GTFS Realtime フィードを構築する際には、留意すべきベストプラクティスがいくつかあります。

* [TripUpdate.StopTimeUpdate](#stoptimeupdate) では、頻度ベースの便は固定された時刻表に従わないため、`arrival` および `departure` の [StopTimeEvent](#stoptimeevent) に `delay` を含めるべきではありません。代わりに、到着／出発予測を示すために `time` を提供するべきです。

* 仕様で必須とされているとおり、[TripUpdate](#tripupdate) または [VehiclePosition](#vehicleposition) において [TripDescriptor](#tripdescriptor) を使用して `trip` を記述する場合、`trip_id`、`start_time`、および `start_date` のすべてを提供しなければなりません。さらに、`schedule_relationship` は `UNSCHEDULED` とするべきです。
 （例：増発便）。

## この文書について {: #about-this-document}

### 目的 {: #objectives}


GTFS Realtime Best Practicesを維持する目的は、以下のとおりです。

* 公共交通アプリにおけるエンドユーザーの顧客体験を向上させること
* ソフトウェア開発者がアプリケーション、製品、およびサービスを導入・拡張しやすくすること

### 公開済みの GTFS Realtime Best Practices を提案または改訂する方法 {: #how-to-propose-or-amend-published-gtfs-realtime-best-practices}


GTFS のアプリケーションおよび慣行は進化するため、この文書は随時改訂が必要になる場合があります。この文書の改訂を提案するには、[GTFS Realtime Best Practices GitHub repository](https://github.com/MobilityData/GTFS_Realtime_Best-Practices) で pull request を作成し、変更を提唱してください。

### このドキュメントへのリンク {: #linking-to-this-document}


GTFS Realtime データを正しく作成するためのガイダンスをフィード提供者に提供するため、ここにリンクしてください。個々の推奨事項にはアンカーリンクがあります。推奨事項をクリックすると、ページ内アンカーリンクの URL を取得できます。

GTFS Realtime を利用するアプリケーションが、ここで説明されていない GTFS Realtime データの運用に関する要件または推奨事項を設ける場合、これらの共通ベストプラクティスを補完するために、それらの要件または推奨事項を記載したドキュメントを公開することが推奨されます。
