# GTFS Realtime リファレンス {: #gtfs-realtime-reference}


GTFS Realtime feed により、交通事業者は、サービスの障害（駅の閉鎖、路線の運休、重大な遅延など）、車両の位置、および予想到着時刻に関するリアルタイム情報を利用者に提供できます。

feed 仕様のバージョン 2.0 について、このサイトで説明および文書化しています。有効なバージョンは「2.0」、「1.0」です。

## 用語の定義 {: #term-definitions}

### 必須 {: #required}


GTFS-realtime v2.0 以降では、*必須* 列は、交通データが有効であり、利用アプリケーションにとって意味をなすために、配信データ提供者が提供しなければならないフィールドを説明します。

*必須* フィールドでは、以下の値が使用されます。

*   **必須**: このフィールドは、GTFS-realtime 配信データ提供者によって提供されなければなりません。
*   **条件付き必須**: このフィールドは、フィールドの *Description* に示される特定の条件下で必須です。これらの条件外では、このフィールドは任意です。
*   **条件付き禁止**: このフィールドは、フィールドの *Description* に示される特定の条件下で禁止されています。これらの条件外では、このフィールドは任意です。
*   **任意**: このフィールドは任意であり、配信データ提供者が実装する必須はありません。ただし、基盤となる自動車両位置情報システムでデータが利用可能な場合（例: VehiclePosition `timestamp`）、配信データ提供者は可能な場合にこれらの任意フィールドを提供するべきです。

*意味論的要件は GTFS-realtime バージョン 1.0 では定義されていなかったため、`gtfs_realtime_version` が `1` の配信データはこれらの要件を満たさない場合があります（詳細については、[意味論的要件に関する提案](https://github.com/google/transit/pull/64)を参照してください）。*

### 多重度(cardinality) {: #cardinality}


*多重度(cardinality)*は、特定のフィールドに対して指定できる要素数を表し、以下の値があります。

* **1つ** - このフィールドには単一の1つの要素を指定することができます。これは、[Protocol Buffer の *required* および *optional* の多重度](https://developers.google.com/protocol-buffers/docs/proto#simple)に対応します。
* **複数** - このフィールドには複数の要素（0、1、またはそれ以上）を指定することができます。これは、[Protocol Buffer の *repeated* の多重度](https://developers.google.com/protocol-buffers/docs/proto#simple)に対応します。

フィールドが必須、条件付き必須、または任意である場合を確認するには、必ず *Required* および *Description* フィールドを参照してください。Protocol Buffer の多重度については、[`gtfs-realtime.proto`](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto)を参照してください。

### Protocol Buffer データ型 {: #protocol-buffer-data-types}


以下の protocol buffer データ型は、フィード要素を記述するために使用されます。

*   **message**: 複合型
*   **enum**: 固定値のリスト

### 実験的フィールド {: #experimental-fields}


**experimental** とラベル付けされたフィールドは変更される可能性があり、まだ仕様に正式に採用されていません。**experimental** フィールドは、将来正式に採用される可能性があります。

## 要素インデックス {: #element-index}


*   [FeedMessage](#message-feedmessage)
    *   [FeedHeader](#message-feedheader)
        *   [Incrementality](#enum-incrementality)
    *   [FeedEntity](#message-feedentity)
        *   [TripUpdate](#message-tripupdate)
            *   [TripDescriptor](#message-tripdescriptor)
                *   [ScheduleRelationship](#enum-schedulerelationship_1)
            *   [VehicleDescriptor](#message-vehicledescriptor)
                *   [WheelchairAccessible](#enum-wheelchairaccessible)
            *   [StopTimeUpdate](#message-stoptimeupdate)
                *   [StopTimeEvent](#message-stoptimeevent)
                *   [ScheduleRelationship](#enum-schedulerelationship)
                *   [StopTimeProperties](#message-stoptimeproperties)
            *   [TripProperties](#message-tripproperties)
        *   [VehiclePosition](#message-vehicleposition)
            *   [TripDescriptor](#message-tripdescriptor)
                *   [ScheduleRelationship](#enum-schedulerelationship_1)
                *   [ModifiedTripSelector](#message-modifiedtripselector)
            *   [VehicleDescriptor](#message-vehicledescriptor)
                *   [WheelchairAccessible](#enum-wheelchairaccessible)
            *   [Position](#message-position)
            *   [VehicleStopStatus](#enum-vehiclestopstatus)
            *   [CongestionLevel](#enum-congestionlevel)
            *   [OccupancyStatus](#enum-occupancystatus)
            *   [CarriageDetails](#message-carriagedetails)
        *   [Alert](#message-alert)
            *   [TimeRange](#message-timerange)
            *   [EntitySelector](#message-entityselector)
                *   [TripDescriptor](#message-tripdescriptor)
                    *   [ScheduleRelationship](#enum-schedulerelationship_1)
            *   [Cause](#enum-cause)
            *   [Effect](#enum-effect)
            *   [TranslatedString](#message-translatedstring)
                *   [Translation](#message-translation)
            *   [SeverityLevel](#enum-severitylevel)
        *   [Shape](#message-shape)
        *   [Stop](#message-stop)
            *   [WheelchairBoarding](#enum-wheelchairboarding)
        *   [TripModifications](#message-tripmodifications)
            *   [Modification](#message-modification)
            *   [ReplacementStop](#message-replacementstop)

## 要素 {: #elements}

### _message_ FeedMessage {: #message-feedmessage}


フィードメッセージの内容です。ストリーム内の各メッセージは、適切な HTTP GET リクエストへの応答として取得されます。realtime feed は常に既存の GTFS feed との関係で定義されます。すべての entity id は GTFS feed を基準として解決されます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
|**header** | [FeedHeader](#message-feedheader) | 必須 | 1つ | この feed および feed message に関するメタデータです。 |
|**entity** | [FeedEntity](#message-feedentity) | 条件付き必須 | 複数 | フィードの内容です。交通システムで利用可能な real-time information がある場合、このフィールドを提供しなければなりません。このフィールドが空の場合、コンシューマーはシステムで利用可能な real-time information がないと想定するべきです。 |

### _message_ FeedHeader {: #message-feedheader}


フィードに関するメタデータであり、フィードメッセージに含まれます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **gtfs_realtime_version** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | フィード仕様のバージョンです。現在のバージョンは2.0です。 |
| **incrementality** | [Incrementality](#enum-incrementality) | 必須 | 1つ |
| **timestamp** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | このタイムスタンプは、このフィードのコンテンツが作成された時点（サーバー時刻）を示します。POSIX時刻（すなわち、1970年1月1日00:00:00 UTCからの秒数）です。リアルタイム情報を生成および消費するシステム間の時刻ずれを避けるため、タイムサーバーからtimestampを取得することを強く推奨します。数秒までの時刻差は許容可能であるため、Stratum 3またはそれより低い階層のサーバーを使用することも完全に許容されます。 |
| **feed_version** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | リアルタイムデータの基となるGTFSフィードの`feed_info.feed_version`と一致する文字列です。コンシューマーはこれを使用して、現在どのGTFSフィードが有効であるか、または新しいフィードがダウンロード可能になった時点を特定できます。 |

### _enum_ Incrementality {: #enum-incrementality}


現在の取得が増分であるかどうかを決定します。

*   **FULL_DATASET**: このフィード更新は、フィードに関するそれ以前のすべてのリアルタイム情報を上書きします。したがって、この更新は、既知のすべてのリアルタイム情報の完全なスナップショットを提供することが期待されます。
*   **DIFFERENTIAL**: 現在、このモードは**サポートされておらず**、このモードを使用するフィードの動作は**未規定**です。DIFFERENTIAL モードの動作を完全に規定することについて、[GTFS Realtime mailing list](http://groups.google.com/group/gtfs-realtime) で議論が行われており、それらの議論が最終決定された時点でドキュメントが更新されます。

**値**

| _**値**_ |
|-------------|
| **FULL_DATASET** |
| **DIFFERENTIAL** |

### _message_ FeedEntity {: #message-feedentity}


交通フィード内のエンティティの定義（または更新）です。エンティティが削除されない場合、'trip_update'、'vehicle'、'alert'、'shape'、'stop'、または'trip_modification'フィールドのうち、ちょうど1つを設定するべきです。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | このエンティティのフィード内で一意の識別子です。idは増分サポートを提供するためにのみ使用されます。フィードによって参照される実際のエンティティは、明示的なセレクタで指定しなければなりません（詳細については、以下のEntitySelectorを参照してください）。 |
| **is_deleted** | [bool](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | このエンティティを削除するかどうかです。IncrementalityがDIFFERENTIALであるフィードに対してのみ提供するべきです。IncrementalityがFULL_DATASETであるフィードに対しては、このフィールドを提供してはいけません。 |
| **trip_update** | [TripUpdate](#message-tripupdate) | 条件付き必須 | 1つ | 便(trip)のリアルタイムの出発遅延に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらのフィールドをすべて空にすることはできません。 |
| **vehicle** | [VehiclePosition](#message-vehicleposition) | 条件付き必須 | 1つ | 車両のリアルタイム位置に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらのフィールドをすべて空にすることはできません。 |
| **alert** | [Alert](#message-alert) | 条件付き必須 | 1つ | リアルタイムの運行情報(alert)に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらのフィールドをすべて空にすることはできません。 |
| **shape** | [Shape](#message-shape) | 条件付き必須 | 1つ | 迂回時などに追加されるリアルタイムのルート形状(shape)に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらのフィールドをすべて空にすることはできません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **stop** | [Stop](#message-stop) | 条件付き必須 | 1つ | フィードに動的に追加される新しい停留所等(stop)です。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **trip_modifications** | [TripModifications](#message-tripmodifications) | 条件付き必須 | 1つ | 迂回など、特定の変更の影響を受ける便(trip)のリストです。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

### _message_ TripUpdate {: #message-tripupdate}


便(trip)に沿った車両の進行状況に関するリアルタイム更新です。[便の更新(trip update)エンティティ](../../../documentation/realtime/feed-entities/trip-updates)に関する一般的な説明も参照してください。
upd
ScheduleRelationship の値に応じて、TripUpdate は以下を指定できます。

*   スケジュールに従って運行する便(trip)。
*   ルート・路線系統(route)に沿って運行するが、固定スケジュールを持たない便(trip)。
*   スケジュールに対して追加または削除された便(trip)。
*   static GTFS 内の既存の便(trip)を置き換える便(trip)。
*   static GTFS 内の既存の便(trip)の複製である新しい便(trip)。TripProperties で指定された運行日(service day)と時刻に運行されます。

更新は、将来の予測到着・出発イベントに対するもの、または既に発生した過去のイベントに対するものとすることができます。ほとんどの場合、過去のイベントに関する情報は計測値であるため、その uncertainty 値は 0 にすることが推奨されます。ただし、これが当てはまらない場合もあるため、過去のイベントについて 0 以外の uncertainty 値を持つことが許可されています。更新の uncertainty が 0 でない場合、その更新は、完了していない便(trip)に対する概算予測であるか、計測が正確でないか、またはイベント発生後に検証されていない過去に対する予測でした。

車両が同じ block 内で複数の便(trip)を運行する場合（便(trip)および block の詳細については、[GTFS trips.txt](../../schedule/reference/#tripstxt)を参照してください）:

* フィードには、車両が現在運行している便(trip)の TripUpdate を含めるべきです。提供者は、これらの将来の便(trip)の予測品質に確信がある場合、この車両の block において現在の便(trip)の後に続く 1つ以上の便(trip)の TripUpdate を含めることが推奨されます。同じ車両に対して複数の TripUpdate を含めることで、車両がある便(trip)から別の便(trip)へ移行する際の乗客の予測情報の「突然の表示」を回避し、また下流の便(trip)に影響する遅延（例: 既知の遅延が便(trip)間の計画された待機時間を超える場合）を乗客に事前通知できます。
* 各 TripUpdate エンティティは、block 内でスケジュールされている順序と同じ順序でフィードに追加する必要はありません。例えば、すべて 1つの block に属する `trip_ids` 1、2、3 の便(trip)があり、車両が便(trip) 1、次に便(trip) 2、次に便(trip) 3 を運行する場合、`trip_update` エンティティは任意の順序で表示できます。例えば、便(trip) 2、次に便(trip) 1、次に便(trip) 3 を追加することが許可されます。

更新は、既に完了した便(trip)を記述することもできることに注意してください。この目的のためには、便(trip)の最後の停留所等(stop)に対する更新を提供するだけで十分です。最後の停留所等(stop)への到着時刻が過去である場合、クライアントは便(trip)全体が過去であると判断します（重要ではありませんが、先行する停留所等(stop)に対する更新も提供できます）。この選択肢は、スケジュールより早く完了したものの、スケジュール上は現在時刻においてまだ運行中である便(trip)に最も関連します。この便(trip)の更新を削除すると、クライアントはその便(trip)がまだ運行中であると想定する可能性があります。フィード提供者は過去の更新を削除することが許可されていますが、削除する必要はないことに注意してください。これは、それが実用上有用となるケースの 1つです。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip** | [TripDescriptor](#message-tripdescriptor) | 必須 | 1つ | このメッセージが適用される便(trip)です。実際の各便(trip)インスタンスに対して、TripUpdate エンティティは最大 1つ存在できます。存在しない場合、予測情報が利用できないことを意味します。便(trip)がスケジュールどおりに進行していることを意味するものでは*ありません*。 |
| **vehicle** | [VehicleDescriptor](#message-vehicledescriptor) | 任意 | 1つ | この便(trip)を運行する車両に関する追加情報です。 |
| **stop_time_update** | [StopTimeUpdate](#message-stoptimeupdate) | 条件付き必須 | 複数 | 便(trip)の StopTimes に対する更新です（将来、すなわち予測に対するものと、一部の場合では過去、すなわち既に発生したものの両方）。更新は stop_sequence でソートしなければならず、次に指定された stop_time_update までの便(trip)の後続するすべての停留所等(stop)に適用されます。<br>trip.schedule_relationship が SCHEDULED または UNSCHEDULED の場合、便(trip)には少なくとも 1つの stop_time_update を提供しなければなりません。<br>trip.schedule_relationship が NEW または REPLACEMENT の場合、過去の時刻を持つ停留所等(stop)を含め、新規または置換便(trip)のすべての停留所等(stop)に stop_time_updates を提供しなければならず、static GTFS の停車時刻(stop_time)は使用されません。<br>便(trip)がキャンセルまたは削除された場合、stop_time_updates を提供する必要はありません。キャンセルまたは削除された便(trip)に stop_time_updates が提供される場合、trip.schedule_relationship はすべての stop_time_updates および関連する schedule_relationship より優先されます。便(trip)が複製される場合、新しい便(trip)のリアルタイム情報を示すために stop_time_updates を提供することができます。 |
| **timestamp** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 将来の StopTimes を推定するために、車両のリアルタイム進行状況が最後に計測された最新の時点です。過去の StopTimes が提供される場合、到着・出発時刻はこの値より前であることがあります。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。 |
| **delay** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 便(trip)の現在のスケジュールからの乖離です。Delay は、GTFS 内の既存スケジュールを基準として予測が提供される場合にのみ指定するべきです。<br>Delay（秒単位）は正（車両が遅れていることを意味します）または負（車両がスケジュールより早いことを意味します）にすることができます。Delay が 0 の場合、車両は正確に定時です。<br>StopTimeUpdates 内の Delay 情報は便(trip)レベルの Delay 情報より優先されるため、便(trip)レベルの Delay は、StopTimeUpdate の Delay 値が指定された便(trip)上の次の停留所等(stop)までのみ伝播されます。<br>フィード提供者は、データの鮮度を評価するため、Delay 値が最後に更新された時点を示す TripUpdate.timestamp 値を提供することが強く推奨されます。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来、正式に採用される場合があります。|
| **trip_properties** | [TripProperties](#message-tripproperties) | 任意 | 1つ | 便(trip)の更新されたプロパティを提供します。<br><br>**注意:** このメッセージはまだ**実験的**であり、変更される可能性があります。将来、正式に採用される場合があります。 |

### _message_ StopTimeEvent {: #message-stoptimeevent}


単一の予測イベント（到着または出発のいずれか）の時刻情報です。時刻情報は、遅延および/または推定時刻、不確実性で構成されます。NEW、REPLACEMENT、または DUPLICATED の便(trip)には、予定時刻を追加することもできます。

*   予測が GTFS 内の既存の時刻表を基準として示される場合、delay を使用するべきです。
*   予測時刻表が存在するかどうかにかかわらず time を指定するべきであり、新規または置換の便(trip)では指定しなければなりません。time と delay の両方が指定されている場合、time が優先されます（ただし通常、予定された便(trip)に対して time が指定される場合、GTFS の予定時刻 + delay と等しくなるべきです）。
*   便(trip)が新規、置換、または複製の場合、scheduled_time を指定することができます。

不確実性は time と delay の両方に等しく適用されます。不確実性は、実際の遅延における予想誤差をおおよそ示します（ただし、その正確な統計的意味はまだ定義されていないことに注意してください）。例えば、コンピュータによる時刻制御の下で運行される列車では、不確実性を 0 にすることが可能です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **delay** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 遅延（秒単位）は、正（車両が遅れていることを意味します）または負（車両が予定より早いことを意味します）にすることができます。遅延が 0 の場合、車両は正確に定時です。<br>StopTimeUpdate.schedule_relationship が NO_DATA の場合は**禁止**です。<br>それ以外の場合、time が指定されていなければ**必須**です。 |
| **time** | [int64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 絶対時刻としての推定または実際のイベントです。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。<br>StopTimeUpdate.schedule_relationship が NO_DATA の場合は**禁止**です。<br>それ以外の場合、delay が指定されていなければ**必須**です。 |
| **scheduled_time** | [int64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き禁止 | 1つ | 予定時刻です。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。<br>TripUpdate.schedule_relationship が NEW、REPLACEMENT、または DUPLICATED の場合は**任意**であり、それ以外の場合は**禁止**です。 |
| **uncertainty** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | uncertainty が省略された場合、不明として解釈されます。完全に確実な予測を指定するには、uncertainty を 0 に設定してください。<br>StopTimeUpdate.schedule_relationship が NO_DATA の場合は**禁止**です。 |

### _message_ StopTimeUpdate {: #message-stoptimeupdate}


便(trip)上の特定の停留所等(stop)における到着イベントおよび／または出発イベントのRealtime更新です。[TripDescriptor](#message-tripdescriptor)および[便の更新エンティティ](../../../documentation/realtime/feed-entities/trip-updates)のドキュメントにある停車時刻(stop_time)の更新に関する一般的な説明も参照してください。

更新は、過去および将来のイベントの両方に対して提供することができます。producerは必須ではありませんが、過去のイベントを削除することが許可されています。ただし、`TripUpdate.schedule_relationship`がNEWまたはREPLACEMENTの場合は、車両が運行中の便を定義するため、便全体が終了するまで過去の停留所等(stop)を削除してはいけません。
更新はstop_sequenceまたはstop_idを通じて特定の停留所等(stop)に関連付けられるため、これらのフィールドのいずれか1つを必ず設定しなければなりません。同じstop_idが1つの便(trip)内で複数回訪問される場合、その便(trip)上の当該stop_idに対するすべてのStopTimeUpdateでstop_sequenceを提供するべきです。

新規または置換便(trip)では、更新はGTFS Static内の既存の便(trip)を参照せずに、その便(trip)が訪問する停留所等(stop)を指定するために使用されます。このような便(trip)では、`stop_id`、`stop_sequence`、`departure`、`arrival`をすべて設定しなければなりません。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | 対応するGTFS feedのstop_times.txt内の値と同じでなければなりません。StopTimeUpdate内では、stop_sequenceまたはstop_idのいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。stop_sequenceは、同じstop_idを複数回訪問する便(trip)（例: ループ）において、予測がどの停留所等(stop)に対するものかを明確にするために必須です。`StopTimeProperties.assigned_stop_id`が設定されている場合、`stop_sequence`を設定しなければなりません。`TripUpdate.schedule_relationship`がNEWまたはREPLACEMENTの場合は**必須**であり、その値は便(trip)に沿って増加しなければなりません。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | 対応するGTFS feedのstops.txt内の値と同じでなければなりません。StopTimeUpdate内では、stop_sequenceまたはstop_idのいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。`StopTimeProperties.assigned_stop_id`が設定されている場合、stop_idを省略し、stop_sequenceのみを使用することが推奨されます。`StopTimeProperties.assigned_stop_id`と`stop_id`が設定されている場合、stop_idは`assigned_stop_id`と一致しなければなりません。`TripUpdate.schedule_relationship`がNEWまたはREPLACEMENTの場合は**必須**です。 |
| **arrival** | [StopTimeEvent](#message-stoptimeevent) | 条件付きで必須 | 1つ | schedule_relationshipが空であるかSCHEDULEDの場合、StopTimeUpdate内ではarrivalまたはdepartureのいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。schedule_relationshipがSKIPPEDの場合、arrivalとdepartureの両方を空にすることができます。`TripUpdate.schedule_relationship`がNEWまたはREPLACEMENTの場合は**必須**です。 |
| **departure** | [StopTimeEvent](#message-stoptimeevent) | 条件付きで必須 | 1つ | schedule_relationshipが空であるかSCHEDULEDの場合、StopTimeUpdate内ではarrivalまたはdepartureのいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。schedule_relationshipがSKIPPEDの場合、arrivalとdepartureの両方を空にすることができます。`TripUpdate.schedule_relationship`がNEWまたはREPLACEMENTの場合は**必須**です。 |
| **departure_occupancy_status** | [OccupancyStatus](#enum-occupancystatus) | 任意 | 1つ | 指定された停留所等(stop)を出発した直後の、車両の予測乗客混雑状態です。提供する場合、stop_sequenceを提供しなければなりません。Realtimeの到着予測または出発予測を提供せずにdeparture_occupancy_statusを提供するには、このフィールドを設定し、StopTimeUpdate.schedule_relationship = NO_DATAを設定します。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |
| **schedule_relationship** | [ScheduleRelationship](#enum-schedulerelationship) | 任意 | 1つ | デフォルトの関係はSCHEDULEDです。 |
| **stop_time_properties** | [StopTimeProperties](#message-stoptimeproperties) | 任意 | 1つ | GTFS stop_times.txt内で定義される特定のプロパティに対するRealtime更新<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |

### _enum_ ScheduleRelationship {: #enum-schedulerelationship}


この StopTime と静的スケジュールとの関係です。

**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **SCHEDULED** | 車両は、必ずしもスケジュールの時刻どおりではありませんが、静的な停留所等(stop)のスケジュールに従って運行しています。これは**デフォルト**の動作です。arrival と departure の少なくとも1つを指定しなければなりません。頻度ベースの便(trip)（exact_times = 0 の GTFS frequencies.txt）は、SCHEDULED 値を持つべきではなく、代わりに UNSCHEDULED を使用するべきです。 |
| **SKIPPED** | 停留所等(stop)はスキップされます。すなわち、車両はこの停留所等(stop)に停車しません。arrival と departure は任意です。`SKIPPED` が設定されても、同じ便(trip)内の後続の停留所等(stop)には伝播されません（すなわち、それらの停留所等(stop)にも `schedule_relationship: SKIPPED` を含む `stop_time_update` がない限り、車両は便(trip)内の後続の停留所等(stop)に停車します）。便(trip)内の前の停留所等(stop)からの遅延は、`SKIPPED` の停留所等(stop)を越えて伝播*します*。言い換えると、`SKIPPED` の停留所等(stop)の後にある停留所等(stop)に対して `arrival` または `departure` の予測を含む `stop_time_update` が設定されていない場合、`SKIPPED` の停留所等(stop)より前の予測が、`SKIPPED` の停留所等(stop)の後の停留所等(stop)および、後続の停留所等(stop)に対する `stop_time_update` が提供されるまでの便(trip)内の後続の停留所等(stop)に伝播されます。  |
| **NO_DATA** | この停留所等(stop)にはリアルタイムデータが提供されません。これは、利用可能なリアルタイム時刻情報がないことを示します。NO_DATA が設定されると後続の停留所等(stop)に伝播されるため、これはリアルタイム時刻情報を持たない停留所等(stop)を指定する推奨の方法です。NO_DATA が設定されている場合、`TripDescriptor.schedule_relationship` が `NEW` または `REPLACEMENT` でない限り、arrival または departure を指定してはいけません。その場合は、予測ではなくスケジュール時刻のみを指定しなければなりません。`TripDescriptor.schedule_relationship` が `NEW` または `REPLACEMENT` の場合、StopTimeUpdate は便(trip)の停留所等(stop)リストを定義するため、arrival と departure には引き続きスケジュール時刻を指定しなければなりません。この場合、スケジュールが静的 GTFS とは無関係であるものの、リアルタイム予測がまだ利用できないことを示します。 |
| **UNSCHEDULED** | 車両は頻度ベースの便(trip)（exact_times = 0 の GTFS frequencies.txt）を運行しています。この値は、GTFS frequencies.txt で定義されていない便(trip)、または exact_times = 1 の GTFS frequencies.txt 内の便(trip)に使用するべきではありません。`schedule_relationship: UNSCHEDULED` を含む `stop_time_updates` を持つ便(trip)は、TripDescriptor にも `schedule_relationship: UNSCHEDULED` を設定しなければなりません。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |

### _message_ StopTimeProperties {: #message-stoptimeproperties}


GTFS stop_times.txt 内で定義される特定のプロパティに対するRealtime更新です。

**注意:** このメッセージは現在も**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。<br> 

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **assigned_stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | リアルタイムの停留所等(stop)割り当てをサポートします。GTFS `stops.txt` で定義されている `stop_id` を参照します。<br> 新しい `assigned_stop_id` は、GTFS `stop_times.txt` で定義された `stop_id` と比較して、エンドユーザーにとって著しく異なる便(trip)体験をもたらすべきではありません。言い換えると、追加のコンテキストなしにアプリ内で新しい停留所等(stop)が提示された場合、エンドユーザーはこの新しい `stop_id` を「通常とは異なる変更」と見なすべきではありません。たとえば、このフィールドは、GTFS `stop_times.txt` で元々定義された停留所等(stop)と同じ駅に属する `stop_id` を使用して、プラットフォームの割り当てに使用することを意図しています。<br> リアルタイムの到着または出発予測を提供せずに停留所等(stop)を割り当てるには、このフィールドを設定し、`StopTimeUpdate.schedule_relationship = NO_DATA` を設定します。<br> このフィールドが設定されている場合、`StopTimeUpdate.stop_sequence` は設定しなければならず、`StopTimeUpdate.stop_id` は設定するべきではありません。停留所等(stop)の割り当ては、他のGTFS-realtimeフィールド（例: `VehiclePosition.stop_id`）にも反映するべきです。<br><br>**注意:** このフィールドは現在も**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **stop_headsign** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 停留所等(stop)における車両の更新された行先表示(headsign)です。<br><br>**注意:** このフィールドは現在も**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **drop_off_type** | [DropOffPickupType](#enum-dropoffpickuptype) | 任意 | 1つ | 停留所等(stop)における車両の更新された降車です。<br><br>**注意:** このフィールドは現在も**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **pickup_type** | [DropOffPickupType](#enum-dropoffpickuptype) | 任意 | 1つ | 停留所等(stop)における車両の更新された乗車です。<br><br>**注意:** このフィールドは現在も**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |

## _enum_ DropOffPickupType {: #enum-dropoffpickuptype}


**値**

| _**値**_                   | _**コメント**_                                         |
|----------------------------|--------------------------------------------------------|
| **REGULAR**                | 定期的に予定された乗車・降車です。                     |
| **NONE**                   | 利用可能な乗車・降車はありません。                     |
| **PHONE_AGENCY**           | 乗車・降車を手配するために事業者へ電話しなければなりません。 |
| **COORDINATE_WITH_DRIVER** | 乗車・降車を手配するために運転手と調整しなければなりません。 |

### _message_ TripProperties {: #message-tripproperties}


便(trip)の更新されたプロパティを定義します

**注意:** このメッセージはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。<br>.

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | (CSV) GTFS trips.txt で定義されている既存の便(trip)の複製であるものの、異なる運行日(service day)および/または時刻（`TripProperties.start_date` および `TripProperties.start_time` を使用して定義されます）に開始する新しい便(trip)の識別子を定義します。(CSV) GTFS における `trips.trip_id` の定義を参照してください。その値は、(CSV) GTFS で使用されている値とは異なっていなければなりません。`schedule_relationship` が `DUPLICATED` の場合、このフィールドは必須です。それ以外の場合、このフィールドに値を設定してはいけません。また、コンシューマーによって無視されます。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。 |
| **start_date** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 複製された便(trip)が運行される運行日(service day)です。YYYYMMDD 形式で指定しなければなりません。`schedule_relationship` が `DUPLICATED` の場合、このフィールドは必須です。それ以外の場合、このフィールドに値を設定してはいけません。また、コンシューマーによって無視されます。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。 |
| **start_time** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 便(trip)が複製される際の出発開始時刻を定義します。(CSV) GTFS における `stop_times.departure_time` の定義を参照してください。複製された便(trip)の予定到着時刻および出発時刻は、元の便(trip)の `departure_time` とこのフィールドの差分に基づいて計算されます。たとえば、GTFS の便(trip)に `departure_time` が `10:00:00` の停留所等(stop) A と、`departure_time` が `10:01:00` の停留所等(stop) B があり、このフィールドに `10:30:00` の値が設定されている場合、複製された便(trip)の停留所等(stop) B の予定 `departure_time` は `10:31:00` になります。リアルタイム予測の `delay` 値は、この計算された予定時刻に適用され、予測時刻を決定します。たとえば、停留所等(stop) B に対して `30` の出発 `delay` が指定されている場合、予測出発時刻は `10:31:30` です。リアルタイム予測の `time` 値には差分は適用されず、指定された予測時刻を示します。たとえば、10:31:30 を表す出発 `time` が停留所等(stop) B に対して指定されている場合、予測出発時刻は `10:31:30` です。`schedule_relationship` が `DUPLICATED` の場合、このフィールドは必須です。それ以外の場合、このフィールドに値を設定してはいけません。また、コンシューマーによって無視されます。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。 |
| **trip_headsign** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 元の便(trip)と異なる場合に、この便(trip)の行先表示(headsign)を指定します。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。 |
| **trip_short_name** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 元の便(trip)と異なる場合に、この便(trip)の名称を指定します。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。 |
| **shape_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 便(trip)のルート形状(shape)が (CSV) GTFS で指定されたルート形状(shape)と異なる場合、または、乗客の需要に基づいて異なる経路を通る車両など、(CSV) GTFS で提供されていない場合にリアルタイムで指定するため、車両走行経路のルート形状(shape)の識別子を指定します。(CSV) GTFS における `trips.shape_id` の定義を参照してください。<br>(CSV) GTFS にもリアルタイムにもルート形状(shape)が定義されていない場合、ルート形状(shape)は不明と見なされます。このフィールドは、(CSV) GTFS の shapes.txt で定義されたルート形状(shape)、または同じ (protobuf) リアルタイムフィード内の `Shape` を参照できます。この便(trip)の停留所等(stop)の順序（stop sequences）は、(CSV) GTFS と同じままでなければなりません。同じリアルタイムフィード内の `Shape` エンティティを参照する場合、このフィールドの値は、エンティティ内の `shape_id` の値であるべきであり、`FeedEntity` の `id` では_ありません_。<br>迂回が発生した場合など、元の便(trip)の一部であるものの今後停車しない停留所等(stop)は、schedule_relationship=SKIPPED としてマークするべきです。または、`TripModifications` メッセージを通じて詳細を提供できます。<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用される可能性があります。 |

### _message_ VehiclePosition {: #message-vehicleposition}


指定された車両のRealtime位置情報。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip** | [TripDescriptor](#message-tripdescriptor) | 任意 | 1つ | この車両が運行している便(trip)。車両を特定の便のインスタンスと識別できない場合は、空または部分的にすることができます。 |
| **vehicle** | [VehicleDescriptor](#message-vehicledescriptor) | 任意 | 1つ | この便を運行している車両に関する追加情報。各エントリには**一意の**vehicle idを含めるべきです。 |
| **position** | [Position](#message-position) | 任意 | 1つ | この車両の現在位置。 |
| **current_stop_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 現在の停留所等(stop)の停留所順序インデックス。current_stop_sequenceの意味（すなわち、参照先の停留所等(stop)）はcurrent_statusによって決まります。current_statusがない場合、IN_TRANSIT_TOが仮定されます。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 現在の停留所等(stop)を識別します。値は、対応するGTFS feed内のstops.txtと同じでなければなりません。`stop_id`の割り当てに`StopTimeProperties.assigned_stop_id`が使用される場合、このフィールドにも`stop_id`の変更を反映するべきです。 |
| **current_status** | [VehicleStopStatus](#enum-vehiclestopstatus) | 任意 | 1つ | 現在の停留所等(stop)に対する車両の正確な状態。current_stop_sequenceがない場合は無視されます。 |
| **timestamp** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両位置が測定された時点。POSIX time（すなわち、1970年1月1日00:00:00 UTCからの秒数）。 |
| **congestion_level** | [CongestionLevel](#enum-congestionlevel) | 任意 | 1つ |
| **occupancy_status** | [OccupancyStatus](#enum-occupancystatus) | _任意_ | 1つ | 車両または車両の車両編成における乗客の混雑状態。multi_carriage_detailsに車両編成ごとのOccupancyStatusが設定されている場合、このフィールドは乗客を受け入れるすべての車両編成を考慮した車両全体を表すべきです。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される場合があります。|
| **occupancy_percentage** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両内の乗客の混雑度を示す百分率値。値100は、座席および立席の収容力の両方と、現行の運用規則で許容される車両設計上の最大収容人数全体を表すべきです。乗客数が設計上の最大収容人数を超える場合、値は100を超えることがあります。occupancy_percentageの精度は、個々の乗客の乗車または降車を追跡できない程度に低くするべきです。multi_carriage_detailsに車両編成ごとのoccupancy_percentageが設定されている場合、このフィールドは乗客を受け入れるすべての車両編成を考慮した車両全体を表すべきです。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される場合があります。 |
| **multi_carriage_details** | [CarriageDetails](#message-carriagedetails) | 任意 | 複数 | この指定された車両の複数の車両編成の詳細。最初の出現は、**現在の進行方向を前提として**、車両の最初の車両編成を表します。multi_carriage_detailsフィールドの出現数は、車両の車両編成数を表します。また、エンジン、保守用車両編成などの乗車できない車両編成も含まれます。これらは、プラットフォーム上のどこに立つべきかについて乗客に有用な情報を提供するためです。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される場合があります。 |

### _enum_ VehicleStopStatus {: #enum-vehiclestopstatus}


**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **INCOMING_AT** | 車両はまもなく停留所等(stop)に到着します（停留所等の表示では、通常、車両記号が点滅します）。 |
| **STOPPED_AT** | 車両は停留所等(stop)に停車しています。 |
| **IN_TRANSIT_TO** | 車両は前の停留所等(stop)を出発し、移動中です。 |

### _enum_ CongestionLevel {: #enum-congestionlevel}


この車両に影響している混雑レベルです。

**値**

| _**値**_ |
|-------------|
| **UNKNOWN_CONGESTION_LEVEL** |
| **RUNNING_SMOOTHLY** |
| **STOP_AND_GO** |
| **CONGESTION** |
| **SEVERE_CONGESTION** |

### _enum_ OccupancyStatus {: #enum-occupancystatus}


車両または車両編成の乗客混雑状況です。

個々の提供者は、すべての OccupancyStatus 値を公開しない場合があります。したがって、利用者は OccupancyStatus 値が線形スケールに従うと想定してはいけません。利用者は、提供者によって示され意図された状態として OccupancyStatus 値を表現するべきです。同様に、提供者は実際の車両混雑状況に対応する OccupancyStatus 値を使用しなければなりません。

線形スケールで乗客混雑度を記述するには、`occupancy_percentage` を参照してください。

**注意:** このフィールドは依然として**実験的**であり、変更される場合があります。将来的に正式に採用される可能性があります。

***値***

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| _**EMPTY**_ | _車両は、ほとんどの基準で空車と見なされ、乗車中の乗客はほとんど、またはまったくいませんが、引き続き乗客を受け入れています。_ |
| _**MANY_SEATS_AVAILABLE**_ | _車両または車両編成には、多数の利用可能な座席があります。このカテゴリに該当するほど十分に多いと見なされる、総座席数に対する空席数は、提供者の裁量で決定されます。_ |
| _**FEW_SEATS_AVAILABLE**_ | _車両または車両編成には、少数の利用可能な座席があります。このカテゴリに該当するほど十分に少ないと見なされる、総座席数に対する空席数は、提供者の裁量で決定されます。_ |
| _**STANDING_ROOM_ONLY**_ | _車両または車両編成は、現在、立っている乗客のみを収容できます。_ |
| _**CRUSHED_STANDING_ROOM_ONLY**_ | _車両または車両編成は、現在、立っている乗客のみを収容でき、それらの乗客のためのスペースは限られています。_ |
| _**FULL**_ | _車両は、ほとんどの基準で満員と見なされますが、引き続き乗客の乗車を許可している場合があります。_ |
| _**NOT_ACCEPTING_PASSENGERS**_ | _車両または車両編成は乗客を受け入れていません。この車両または車両編成は通常、乗客の乗車を受け入れます。_ |
| _**NO_DATA_AVAILABLE**_ | _車両または車両編成には、その時点で利用可能な混雑状況データがありません。_ |
| _**NOT_BOARDABLE**_ | _車両または車両編成は乗車できず、乗客を受け入れることはありません。特殊な車両または車両編成（機関車、保守用車両など）に役立ちます。_ |

### _message_ CarriageDetails {: #message-carriagedetails}


複数の車両で構成される車両に使用する、車両固有の詳細です。

**注意:** このメッセージはまだ **experimental** であり、変更される可能性があります。将来、正式に採用される可能性があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両の識別子です。車両ごとに一意であるべきです。<br><br>**注意:** このフィールドはまだ **experimental** であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **label** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 乗客が車両を識別するのに役立つよう表示されることがある、利用者に表示されるラベルです。例: "7712"、"Car ABC-32" など。<br>**注意:** このフィールドはまだ **experimental** であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **occupancy_status** | [OccupancyStatus](#enum-occupancystatus) | 任意 | 1つ | この車両内の当該車両の混雑状況です。デフォルトは `NO_DATA_AVAILABLE` に設定されます。<br><br>**注意:** このフィールドはまだ **experimental** であり、変更される可能性があります。将来、正式に採用される可能性があります。|
| **occupancy_percentage** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | この車両内の当該車両の混雑率です。"VehiclePosition.occupancy_percentage" と同じ規則に従います。当該車両のデータが利用できない場合は -1 を使用してください。<br><br>**注意:** このフィールドはまだ **experimental** であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **carriage_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 車両の CarriageStatus リストにおいて、他の車両に対するこの車両の順序を識別します。進行方向の先頭車両は、値 1 を持たなければなりません。2番目の値は進行方向の2番目の車両に対応し、値 2 を持たなければなりません。以降も同様です。たとえば、進行方向の先頭車両は値 1 を持ちます。進行方向の2番目の車両が値 3 を持つ場合、コンシューマはすべての車両のデータ（すなわち、multi_carriage_details フィールド）を破棄します。データのない車両は、有効な carriage_sequence 番号で表現しなければならず、データのないフィールドは省略するべきです（代替として、それらのフィールドを含めて「データなし」の値に設定することもできます）。<br><br>**注意:** このフィールドはまだ **experimental** であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

### _message_ Alert {: #message-alert}


公共交通ネットワークにおける何らかの事象を示す運行情報(alert)。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **active_period** | [TimeRange](#message-timerange) | 任意 | 複数 | 運行情報(alert)をユーザーに表示する期間です。指定されない場合、運行情報(alert)はfeedに存在する限り表示されます。複数の期間が指定された場合、運行情報(alert)はそのすべての期間中に表示されます。 |
| **communication_period** | [TimeRange](#message-timerange) | 任意 | 複数 | 情報提供のみを目的として運行情報(alert)をユーザーに表示する期間です。指定されない場合、利用側のアプリケーションは、表示するのが適切なタイミングを決定することができます。 
| **impact_period** | [TimeRange](#message-timerange) | 任意 | 複数 | サービスが運行情報(alert)の影響を受ける期間です。communication_periodが指定されている場合、impact_period内のすべての時間間隔は、communication_period内の少なくとも1つの時間間隔に完全に含まれなければなりません。|
| **informed_entity** | [EntitySelector](#message-entityselector) | 必須 | 複数 | この運行情報(alert)を通知するべきユーザーを持つエンティティです。少なくとも1つのinformed_entityを指定しなければなりません。 |
| **cause** | [Cause](#enum-cause) | 条件付き必須 | 1つ | cause_detailが含まれる場合、Causeも含まれなければなりません。
| **cause_detail** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 事業者固有の表現を使用でき、Causeよりも具体的な、運行情報(alert)の原因の説明です。cause_detailが含まれる場合、Causeも含まれなければなりません。<br><br>**注意:** このフィールドは現在も**experimental**であり、変更される可能性があります。将来的に正式に採用されることがあります。
| **effect** | [Effect](#enum-effect) | 条件付き必須 | 1つ | effect_detailが含まれる場合、Effectも含まれなければなりません。
| **effect_detail** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 事業者固有の表現を使用でき、Effectよりも具体的な、運行情報(alert)の影響の説明です。effect_detailが含まれる場合、Effectも含まれなければなりません。<br><br>**注意:** このフィールドは現在も**experimental**であり、変更される可能性があります。将来的に正式に採用されることがあります。
| **url** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 運行情報(alert)に関する追加情報を提供するURLです。 |
| **header_text** | [TranslatedString](#message-translatedstring) | 必須 | 1つ | 運行情報(alert)のヘッダーです。このプレーンテキスト文字列は、たとえば太字で強調表示されます。 |
| **description_text** | [TranslatedString](#message-translatedstring) | 必須 | 1つ | 運行情報(alert)の説明です。このプレーンテキスト文字列は、運行情報(alert)の本文として整形されます（または、ユーザーによる明示的な「展開」要求時に表示されます）。説明に含まれる情報は、ヘッダーの情報を補完するべきです。 |
| **tts_header_text** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 読み上げ実装で使用する、運行情報(alert)のヘッダーを含むテキストです。このフィールドはheader_textの読み上げ用バージョンです。header_textと同じ情報を含むべきですが、読み上げ可能な形式で整形するべきです（たとえば、略語を除去し、数字を綴りで表記するなど）。 |
| **tts_description_text** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 読み上げ実装で使用する、運行情報(alert)の説明を含むテキストです。このフィールドはdescription_textの読み上げ用バージョンです。description_textと同じ情報を含むべきですが、読み上げ可能な形式で整形するべきです（たとえば、略語を除去し、数字を綴りで表記するなど）。 |
| **severity_level** | [SeverityLevel](#enum-severitylevel) | 任意 | 1つ | 運行情報(alert)の重大度です。 |
| **image** | [TranslatedImage](#message-translatedimage) | 任意 | 1つ | 運行情報(alert)のテキストとともに表示するTranslatedImageです。迂回、駅の閉鎖などによる運行情報(alert)の影響を視覚的に説明するために使用されます。画像は運行情報(alert)の理解を促進するべきであり、重要な情報が掲載される唯一の場所であってはいけません。次の種類の画像は推奨されません: 主にテキストを含む画像、追加情報を提供しないマーケティング画像またはブランド画像。<br><br>**注意:** このフィールドは現在も**experimental**であり、変更される可能性があります。将来的に正式に採用されることがあります。 |
| **image_alternative_text** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | `image`フィールド内のリンクされた画像の外観を説明するテキストです（たとえば、画像を表示できない場合や、アクセシビリティ上の理由でユーザーが画像を見られない場合）。HTMLの[alt画像テキストの仕様](https://html.spec.whatwg.org/#alt)を参照してください。<br><br>**注意:** このフィールドは現在も**experimental**であり、変更される可能性があります。将来的に正式に採用されることがあります。 |

### _enum_ Cause {: #enum-cause}


この運行情報の原因です。

**値**

| _**値**_ |
|-------------|
| **UNKNOWN_CAUSE** |
| **OTHER_CAUSE** |
| **TECHNICAL_PROBLEM** |
| **STRIKE** |
| **DEMONSTRATION** |
| **ACCIDENT** |
| **HOLIDAY** |
| **WEATHER** |
| **MAINTENANCE** |
| **CONSTRUCTION** |
| **POLICE_ACTIVITY** |
| **MEDICAL_EMERGENCY** |
| **SPECIAL_EVENT** |

### _enum_ Effect {: #enum-effect}


影響を受けるエンティティに対する、この問題の影響です。

**値**

| _**値**_ |
|-------------|
| **NO_SERVICE** |
| **REDUCED_SERVICE** |
| **SIGNIFICANT_DELAYS** |
| **DETOUR** |
| **ADDITIONAL_SERVICE** |
| **MODIFIED_SERVICE** |
| **OTHER_EFFECT** |
| **UNKNOWN_EFFECT** |
| **STOP_MOVED** |
| **NO_EFFECT** |
| **ACCESSIBILITY_ISSUE** |

### _enum_ SeverityLevel {: #enum-severitylevel}


運行情報(alert)の重大度です。

**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。

**値**

| _**値**_ |
|-------------|
| **UNKNOWN_SEVERITY** |
| **INFO** |
| **WARNING** |
| **SEVERE** |

### _message_ TimeRange {: #message-timerange}


時間間隔です。時刻 `t` が開始時刻以上かつ終了時刻未満である場合、この間隔は時刻 `t` において有効であるとみなされます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **start** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）による開始時刻です。存在しない場合、間隔は負の無限大から開始します。TimeRange が指定されている場合、start または end のいずれかを指定しなければなりません。両方のフィールドを空にすることはできません。 |
| **end** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）による終了時刻です。存在しない場合、間隔は正の無限大で終了します。TimeRange が指定されている場合、start または end のいずれかを指定しなければなりません。両方のフィールドを空にすることはできません。 |

### _message_ Position {: #message-position}


車両の地理的位置です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **latitude** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | WGS-84座標系における北緯の度数です。 |
| **longitude** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | WGS-84座標系における東経の度数です。 |
| **bearing** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 真北から時計回りの度数による方位です。すなわち、0は北、90は東です。これはコンパス方位、または次の停留所等(stop)もしくは中間地点に向かう方向とすることができます。これは、クライアントが以前のデータから計算できる、以前の位置の順序から推定するべきではありません。 |
| **odometer** | [double](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | メートル単位の走行距離計の値です。 |
| **speed** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両によって測定された、メートル毎秒単位の瞬間速度です。 |

### _message_ TripDescriptor {: #message-tripdescriptor}


`schedule_relationship` が `NEW` である場合を除き、GTFS の便(trip)の単一のインスタンスを識別する記述子です。`NEW` の場合は、追加する新しい便(trip)のインスタンスを指定します。

単一の便(trip)インスタンスを指定するには、多くの場合、`trip_id` だけで十分です。ただし、単一の便(trip)インスタンスに解決するために、以下のケースでは追加情報が必要です。

* frequencies.txt で定義される便(trip)の場合、`trip_id` に加えて `start_date` および `start_time` が必須です。
* 便(trip)が24時間を超えて継続する場合、または翌日に予定されている便(trip)と重複するほど遅延している場合、`trip_id` に加えて `start_date` が必須です。
* `trip_id` フィールドを提供できない場合、`route_id`、`direction_id`、`start_date`、および `start_time` をすべて提供しなければなりません。

すべての場合において、`trip_id` に加えて `route_id` が提供される場合、`route_id` は GTFS trips.txt で指定された便(trip)に割り当てられた `route_id` と同じでなければなりません。

`trip_id` フィールドは、単独でも他の TripDescriptor フィールドとの組み合わせでも、複数の便(trip)インスタンスを識別するために使用してはいけません。たとえば、GTFS frequencies.txt の exact_times=0 の便(trip)について、特定の時刻に開始する単一の便(trip)インスタンスに解決するには start_time も必須であるため、TripDescriptor が trip_id だけを指定するべきではありません。TripDescriptor が単一の便(trip)インスタンスに解決されない場合（つまり、0件または複数の便(trip)インスタンスに解決される場合）、エラーと見なされ、誤った TripDescriptor を含むエンティティはコンシューマーによって破棄されることがあります。

trip_id が不明な場合、TripUpdate 内の駅順序 ID だけでは不十分であり、stop_id も提供しなければならないことに注意してください。さらに、絶対的な到着時刻・出発時刻を提供しなければなりません。

TripDescriptor.route_id は、ルート・路線系統(route)のすべての便(trip)に影響するルート・路線系統(route)全体の運行情報(alert)を指定するために Alert EntitySelector 内で使用することはできません。代わりに EntitySelector.route_id を使用してください。

`schedule_relationship` が `NEW` の場合、`trip_id` は GTFS フィードに記載されていない値に設定しなければならず、便(trip)をルート・路線系統(route)に関連付けるため、`route_id` は GTFS static の `routes.txt` に記載されている値に設定しなければなりません。`start_date` を設定するべきであり、新しい便(trip)に対して `direction_id` を設定することができます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクターが参照する GTFS フィードの trip_id です。非頻度ベースの便(trip)（GTFS frequencies.txt で定義されない便(trip)）の場合、このフィールドだけで便(trip)を一意に識別できます。GTFS frequencies.txt で定義される頻度ベースの便(trip)の場合、trip_id、start_time、および start_date がすべて必須です。スケジュールベースの便(trip)（GTFS frequencies.txt で定義されない便(trip)）の場合、便(trip)が route_id、direction_id、start_time、および start_date の組み合わせによって一意に識別でき、かつそれらのフィールドがすべて提供される場合にのみ、trip_id を省略できます。schedule_relationship が NEW の場合、GTFS static で定義されていない一意の値を指定しなければなりません。schedule_relationship が REPLACEMENT の場合、trip_id は置き換えられる static GTFS の便(trip)を識別します。TripUpdate 内で schedule_relationship が DUPLICATED の場合、trip_id は複製される static GTFS の便(trip)を識別します。VehiclePosition 内で schedule_relationship が DUPLICATED の場合、trip_id は新しい複製便(trip)を識別し、対応する TripUpdate.TripProperties.trip_id の値を含まなければなりません。 |
| **route_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクターが参照する GTFS の route_id です。trip_id が省略される場合、便(trip)インスタンスを識別するには route_id、direction_id、start_time、および schedule_relationship=SCHEDULED をすべて設定しなければなりません。TripDescriptor.route_id は、ルート・路線系統(route)のすべての便(trip)に影響するルート・路線系統(route)全体の運行情報(alert)を指定するために Alert EntitySelector 内で使用するべきではありません。代わりに EntitySelector.route_id を使用してください。schedule_relationship が NEW の場合、新しい便(trip)が属するルート・路線系統(route)の route_id を指定しなければなりません。 |
| **direction_id** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクターが参照する便(trip)の進行方向を示す、GTFS フィードの trips.txt ファイルの direction_id です。trip_id が省略される場合、direction_id を提供しなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用されることがあります。<br>|
| **start_time** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | この便(trip)インスタンスの当初予定されていた開始時刻です。trip_id が非頻度ベースの便(trip)に対応する場合、このフィールドは省略するか、GTFS フィード内の値と等しくするべきです。trip_id が GTFS frequencies.txt で定義される頻度ベースの便(trip)に対応する場合、start_time は必須であり、便の更新(trip update)および車両位置情報(vehicle position)に指定しなければなりません。便(trip)が exact_times=1 の GTFS レコードに対応する場合、start_time は対応する時間帯について frequencies.txt の start_time より後の headway_secs の整数倍（ゼロを含む）でなければなりません。便(trip)が exact_times=0 に対応する場合、その start_time は任意に設定でき、当初は便(trip)の最初の出発時刻であることが想定されます。一度確立された後、この頻度ベースの exact_times=0 の便(trip)の start_time は、最初の出発時刻が変更された場合でも不変と見なすべきです。この時刻変更は、代わりに StopTimeUpdate に反映することができます。trip_id が省略される場合、start_time を提供しなければなりません。フィールドの形式および意味論は GTFS/frequencies.txt/start_time のものと同じです。たとえば、11:15:35 または 25:15:35 です。 |
| **start_date** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | YYYYMMDD 形式の、この便(trip)インスタンスの開始日です。予定された便(trip)（GTFS frequencies.txt で定義されない便(trip)）について、このフィールドは、翌日に予定されている便(trip)と重複するほど遅延した便(trip)を区別するために提供しなければなりません。たとえば、毎日 8:00 と 20:00 に出発する列車が12時間遅延した場合、同じ時刻に2つの異なる便(trip)が存在することになります。このような重複が不可能なスケジュールでは、このフィールドを提供できますが必須ではありません。たとえば、1時間遅れた車両がもはやスケジュールに関連しているとは見なされない、毎時運行のサービスなどです。このフィールドは、GTFS frequencies.txt で定義される頻度ベースの便(trip)に必須です。trip_id が省略される場合、start_date を提供しなければなりません。 |
| **schedule_relationship** | [ScheduleRelationship](#enum-schedulerelationship_1) | 任意 | 1つ | この便(trip)と static schedule の関係です。TripDescriptor が運行情報(alert)の `EntitySelector` で提供される場合、`schedule_relationship` フィールドは、一致する便(trip)インスタンスを識別する際にコンシューマーによって無視されます。 |
| **modified_trip** | [ModifiedTripSelector](#message-modifiedtripselector) | 任意 | 1つ | この便(trip)に行われた変更（ルート形状(shape)の変更、停留所等(stop)の削除または追加）へのリンクです。このフィールドが提供される場合、`ModifiedTripSelector` 値を参照しないコンシューマーの混乱を避けるため、`TripDescriptor` の `trip_id`、`route_id`、`direction_id`、`start_time`、`start_date` フィールドは空のままにしなければなりません。

### _enum_ ScheduleRelationship {: #enum-schedulerelationship}


この便(trip)と静的スケジュールとの関係です。一時的なスケジュールに従って新しい便(trip)が運行され、GTFSに反映されていない場合は、SCHEDULEDとしてマークするべきではなく、NEWとしてマークするべきです。変更されたスケジュールに従って便(trip)が運行され、GTFSに反映されていない場合は、SCHEDULEDとしてマークするべきではなく、REPLACEMENTとしてマークするべきです。

**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **SCHEDULED** | GTFSスケジュールに従って運行している便(trip)、または予定された便(trip)に関連付けられるのに十分近い便(trip)です。 |
| **ADDED** | *注: 動作が規定されていなかったため、この値は非推奨となりました。開始日または時刻を除いて予定された便(trip)と同一である追加の便(trip)には**DUPLICATED**を、既存の便(trip)に関連しない追加の便(trip)には**NEW**を使用してください。* |
| **UNSCHEDULED** | 関連付けられたスケジュールなしで運行している便(trip)です。この値は、exact_times = 0でGTFS frequencies.txtに定義された便(trip)を識別するために使用されます。GTFS frequencies.txtに定義されていない便(trip)、またはexact_times = 1でGTFS frequencies.txtに定義された便(trip)を説明するために使用するべきではありません。`schedule_relationship: UNSCHEDULED`を持つ便(trip)は、すべてのStopTimeUpdatesにも`schedule_relationship: UNSCHEDULED`を設定しなければなりません。 |
| **CANCELED** | スケジュール内に存在していたものの、削除された便(trip)です。 |
| **REPLACEMENT** | 変更されたスケジュールまたは迂回した経路などにより、既存の予定された便(trip)を置き換える便(trip)です。置換便(trip)の完全な旅程(journey)は`StopTimeUpdate`を通じて指定しなければならず、置き換えられるインスタンスにはGTFS staticの元のスケジュールは使用されません。<br>`REPLACEMENT`は、便(trip)が改訂されたスケジュールで運行している場合に使用することができますが、車両が静的GTFSの`stop_times.txt`に記載されたスケジュールに従うことを目的としている場合、リアルタイムのスケジュール逸脱（予測）を伝達するために使用してはいけません。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **DUPLICATED** | 運行開始日および時刻を除き、既存の予定された便(trip)と同一である新しい便(trip)です。`TripUpdate.TripProperties.trip_id`、`TripUpdate.TripProperties.start_date`、および`TripUpdate.TripProperties.start_time`とともに使用し、静的GTFSから既存の便(trip)をコピーして、異なる運行日および/または時刻に開始します。(CSV) GTFS（`calendar.txt`または`calendar_dates.txt`内）において元の便(trip)に関連する運行が今後30日以内に運行している場合、便(trip)の複製が許可されます。複製される便(trip)は`TripUpdate.TripDescriptor.trip_id`を通じて識別されます。<br><br> この列挙値は`TripUpdate.TripDescriptor.trip_id`により参照される既存の便(trip)を変更しません。producerが元の便(trip)をキャンセルしたい場合、CANCELEDの値を持つ別個の`TripUpdate`を公開しなければなりません。`exact_times`が空であるか`0`に等しいGTFS `frequencies.txt`で定義された便(trip)は複製できません。新しい便(trip)の`VehiclePosition.TripDescriptor.trip_id`には、`TripUpdate.TripProperties.trip_id`から対応する値を含めなければならず、`VehiclePosition.TripDescriptor.ScheduleRelationship`も`DUPLICATED`に設定しなければなりません。  <br><br>*複製された便(trip)を表すためにADDED列挙値を使用していた既存のproducerおよびconsumerは、DUPLICATED列挙値へ移行するために[移行ガイド](../../realtime/examples//migration-duplicated)に従わなければなりません。* |
| **NEW** | 突然の乗客需要に対応するためなど、既存のいかなる便(trip)にも関連しない追加の便(trip)です。すべての停留所等(stop)および時刻を含む新しい便(trip)の完全な旅程(journey)は、`StopTimeUpdate`を通じて指定しなければなりません。   <br><br>*静的GTFSに関連しない新しい便(trip)を表すためにADDED列挙値を使用していた既存のproducerおよびconsumerは、NEW列挙値へ移行するために[移行ガイド](../../realtime/examples//migration-duplicated)に従わなければなりません。*<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される可能性があります。  |
| **DELETED** | スケジュール内に存在していたものの、削除され、ユーザーに表示してはいけない便(trip)です。<br><br> DELETEDは、transit providerが対応する便(trip)に関する情報をconsumer applicationから完全に削除したいことを示すために、CANCELEDの代わりに使用するべきです。これにより、たとえば別の便(trip)によって完全に置き換えられる便(trip)について、乗客にキャンセルとして表示されません。この指定は、複数の便(trip)がキャンセルされ、代替サービスに置き換えられる場合に特に重要になります。consumerがキャンセルに関する明示的な情報を表示した場合、より重要なリアルタイム予測から注意をそらすことになります。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

## _message_ ModifiedTripSelector {: #message-modifiedtripselector}


サービスが便の変更(trip modification)の影響を受ける場合、`ModifiedTripSelector` を使用して特定の便(trip)を選択します。詳細は、[Trip Modification](https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/trip-modifications.md#linkage-to-tripupdates) 仕様を参照してください。

**値**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **modifications_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 含まれる `TripModifications` オブジェクトがこの便(trip)に影響を与える `FeedEntity` の `id`。|
| **affected_trip_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | `modifications_id` によって変更される GTFS feed の `trip_id`。|
| **start_time** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | frequency based modified trip に適用される、この便(trip)インスタンスの当初予定されていた開始時刻。[TripDescriptor](#message-tripdescriptor) の **start_time** と同じ定義です。|
| **start_date** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 修正された便(trip)に適用される、YYYYMMDD 形式のこの便(trip)インスタンスの開始日。[TripDescriptor](#message-tripdescriptor) の **start_date** と同じ定義です。|

### _message_ VehicleDescriptor {: #message-vehicledescriptor}


便(trip)を運行する車両の識別情報です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両の内部システム識別子です。車両ごとに**一意**であるべきであり、システム内を進行する車両を追跡するために使用されます。この id はエンドユーザーに表示するべきではありません。その目的には **label** フィールドを使用してください。 |
| **label** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | ユーザーに表示されるラベルです。すなわち、正しい車両を識別するために乗客に表示しなければならないものです。 |
| **license_plate** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両のナンバープレートです。 |
| **wheelchair_accessible** | [WheelchairAccessible](#enum-wheelchairaccessible) | 任意 | 1つ | 指定された場合、静的 GTFS の *wheelchair_accessible* 値を上書きすることができます。 |

### _enum_ WheelchairAccessible {: #enum-wheelchairaccessible}


特定の便(trip)が車椅子で利用可能かどうかを示します。利用可能な場合、この値は静的 GTFS の _wheelchair_accessible_ 値を上書きするべきです。

##### 値 {: #values}


| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **NO_VALUE** | 便(trip)には車椅子対応に関する情報がありません。これは**デフォルト**の動作です。静的 GTFS に _wheelchair_accessible_ 値が含まれている場合、それは上書きされません。 |
| **UNKNOWN** | 便(trip)にはアクセシビリティ値が存在しません。この値は GTFS の値を上書きします。 |
| **WHEELCHAIR_ACCESSIBLE** | 便(trip)は車椅子で利用可能です。この値は GTFS の値を上書きします。 |
| **WHEELCHAIR_INACCESSIBLE** | 便(trip)は車椅子で**利用できません**。この値は GTFS の値を上書きします。 |

### _message_ EntitySelector {: #message-entityselector}


GTFSフィード内のエンティティのセレクターです。フィールド値は、GTFSフィード内の適切なフィールドに対応するべきです。少なくとも1つの指定子を指定しなければなりません。複数指定される場合、それらは論理 `AND` 演算子で結合されているものとして解釈されるべきです。さらに、指定子の組み合わせは、GTFSフィード内の対応する情報と一致しなければなりません。言い換えると、運行情報(alert)がGTFS内のエンティティに適用されるためには、指定されたすべてのEntitySelectorフィールドと一致しなければなりません。例えば、`route_id: "5"` および `route_type: "3"` のフィールドを含むEntitySelectorは、`route_id: "5"` のバスにのみ適用され、`route_type: "3"` の他のルート・路線系統(route)には適用されません。プロデューサーが運行情報(alert)を `route_id: "5"` および `route_type: "3"` に適用したい場合、`route_id: "5"` を参照するものと `route_type: "3"` を参照するものの、2つの別個のEntitySelectorを提供するべきです。

少なくとも1つの指定子を指定しなければなりません。EntitySelector内のすべてのフィールドを空にしてはいけません。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **agency_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | このセレクターが参照するGTFSフィードのagency_idです。
| **route_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | このセレクターが参照するGTFSのroute_idです。direction_idが指定される場合、route_idも指定しなければなりません。
| **route_type** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | このセレクターが参照するGTFSのroute_typeです。
| **direction_id** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | route_idで指定されたルート・路線系統(route)について、一方向のすべての便(trip)を選択するために使用される、GTFSフィードのtrips.txtファイルのdirection_idです。direction_idが指定される場合、route_idも指定しなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。<br>|
| **trip** | [TripDescriptor](#message-tripdescriptor) | 条件付きで必須 | 1つ | このセレクターが参照するGTFS内の便(trip)インスタンスです。このTripDescriptorは、GTFSデータ内の単一の便(trip)インスタンスに解決されなければなりません（例: プロデューサーは、exact_times=0の便(trip)に対してtrip_idのみを提供してはいけません）。このTripDescriptor内のScheduleRelationshipフィールドが設定されている場合、コンシューマーがGTFSの便(trip)を特定しようとする際には無視されます。
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | このセレクターが参照するGTFSフィードのstop_idです。

### _message_ TranslatedString {: #message-translatedstring}


テキストの一部またはURLの言語ごとのバージョンを含む国際化メッセージです。メッセージ内の文字列のうち1つが選択されます。解決は以下のように進行します。UI言語が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。デフォルトのUI言語（例: 英語）が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。いずれかの翻訳に言語コードが指定されていない場合、その翻訳が選択されます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **translation** | [Translation](#message-translation) | 必須 | 複数 | 少なくとも1つの翻訳を提供しなければなりません。 |

### _message_ 翻訳 {: #message-translation}


言語に対応付けられたローカライズ済み文字列です。

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **text** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | メッセージを含む UTF-8 文字列です。 |
| **language** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | BCP-47 言語コードです。言語が不明な場合、またはフィードに対して国際化がまったく行われていない場合は、省略できます。言語タグが指定されていない翻訳は最大で1つのみ許可されます。複数の翻訳がある場合は、言語を指定しなければなりません。 |

### _message_ TranslatedImage {: #message-translatedimage}


画像の言語ごとのバージョンを含む国際化されたメッセージです。メッセージ内の画像のうち1つが選択されます。解決は次のように進みます。UI言語が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。デフォルトのUI言語（例: English）が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。いずれかの翻訳の言語コードが指定されていない場合、その翻訳が選択されます。

**注意:** このメッセージは依然として**experimental**であり、変更される場合があります。将来、正式に採用される可能性があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **localized_image** | [LocalizedImage](#message-localizedimage) | 必須 | 複数 | 少なくとも1つのローカライズされた画像を提供しなければなりません。 |

### _message_ LocalizedImage {: #message-localizedimage}


言語にマッピングされたローカライズされた画像 URL。

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **url** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 画像へのリンクを含む URL 文字列です。リンク先の画像は 2MB 未満でなければなりません。画像が、コンシューマー側で更新を必要とするほど大幅に変更された場合、プロデューサーは URL を新しいものに更新しなければなりません。<br><br> URL は http:// または https:// を含む完全修飾 URL とするべきであり、URL 内の特殊文字は正しくエスケープしなければなりません。完全修飾 URL 値の作成方法については、次の http://www.w3.org/Addressing/URL/4_URI_Recommentations.html を参照してください。  |
| **media_type** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 表示する画像のタイプを指定する IANA メディアタイプです。タイプは "image/" で始まらなければなりません。 |
| **language** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付きで必須 | 1つ | BCP-47 言語コードです。言語が不明な場合、またはフィードに対して国際化がまったく行われていない場合は省略できます。言語タグが未指定である翻訳は最大 1つのみ許可されます。複数の翻訳がある場合、言語を指定しなければなりません。 |

### _message_ Shape {: #message-shape}


アドホックな迂回など、ルート形状(shape)が (CSV) GTFS に含まれていない場合に、車両がたどる物理的な経路を記述します。ルート形状(shape)は便(trip)に属し、より効率的に送信するためのエンコードされたポリラインで構成されます。ルート形状(shape)は停留所等(stop)の位置と正確に交差する必要はありませんが、便(trip)上のすべての停留所等(stop)は、その便(trip)のルート形状(shape)から短い距離の範囲内、すなわちルート形状(shape)の点を結ぶ直線セグメントの近くに存在するべきです。

<br><br>**注意:** このメッセージは依然として**実験的**であり、変更される場合があります。将来的に正式に採用される場合があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **shape_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  ルート形状(shape)の識別子です。(CSV) GTFS で定義されるいかなる `shape_id` とも異なっていなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される場合があります。将来的に正式に採用される場合があります。 |
| **encoded_polyline** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | ルート形状(shape)のエンコードされたポリライン表現です。このポリラインは少なくとも2点を含み、使用される便(trip)の完全なルート形状(shape)を表現しなければなりません。エンコードされたポリラインの詳細については、https://developers.google.com/maps/documentation/utilities/polylinealgorithm を参照してください。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される場合があります。将来的に正式に採用される場合があります。 |

### _message_ 停留所等(stop) {: #message-stop}


フィードに動的に追加される新しい停留所等(stop)を表します。すべてのフィールドは、(CSV) GTFS 仕様で説明されている通りです。新しい停留所等(stop)の location type は `0`（経路探索可能な停留所等(stop)）です。 

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  停留所等(stop)の識別子です。(CSV) GTFS で定義されているすべての `stop_id` と異なっていなければなりません。 |
| **stop_code** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における [stops.stop_code](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **stop_name** | [TranslatedString](#message-translatedstring) | 必須 | 1つ |  (CSV) GTFS における [stops.stop_name](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **tts_stop_name** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における [stops.tts_stop_name](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **stop_desc** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における [stops.stop_desc](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **stop_lat** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  (CSV) GTFS における [stops.stop_lat](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **stop_lon** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  (CSV) GTFS における [stops.stop_lon](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **zone_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における [stops.zone_id](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **stop_url** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における [stops.stop_url](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **parent_station** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における [stops.parent_station](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **stop_timezone** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における [stops.stop_timezone](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **wheelchair_boarding** | [WheelchairBoarding](#enum-wheelchairboarding) | 任意 | 1つ |  (CSV) GTFS における [stops.wheelchair_boarding](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **level_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における [stops.level_id](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |
| **platform_code** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における [stops.platform_code](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt) の定義を参照してください。 |

### _enum_ WheelchairBoarding {: #enum-wheelchairboarding}


**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **UNKNOWN** | 停留所等(stop)のアクセシビリティ情報はありません。 |
| **AVAILABLE** | この停留所等(stop)では、一部の車両に車椅子の乗客が乗車することができます。 |
| **NOT_AVAILABLE** | この停留所等(stop)では、車椅子での乗車はできません。 |

### _message_ TripModifications {: #message-tripmodifications}


`TripModifications` メッセージは、迂回などの特定の変更の影響を受ける、類似した便のリストを識別します。

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

[Trip Modifications の詳細...](../../../documentation/realtime/feed-entities/trip-modifications)

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **selected_trips** | [SelectedTrips](#message-selectedtrips) | 必須 | 複数 | この TripModifications の影響を受ける選択された便のリストです。少なくとも1つの `SelectedTrips` を含めなければなりません。値 `start_times` が設定されている場合、1つの trip_id を持つ `SelectedTrips` を最大1つ列挙できます。  |
| **start_times** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 複数 | trip_ids で定義された trip_id の、リアルタイム trip descriptor における開始時刻のリストです。頻度ベースの便において、1つの trip_id の複数の出発を対象にする場合に便利です。 |
| **service_dates** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 複数 | 変更が発生する日付です。YYYYMMDD 形式で指定します。trip_id は、指定された運行日(service day)に運行する場合にのみ変更されます。便はすべての運行日(service day)に運行する必要はありません。Producers は、次の1週間以内に発生する迂回のみを送信するべきです。提供される日付は user-facing 情報として使用するべきではありません。user-facing の開始日と終了日を提供する必要がある場合は、`service_alert_id` を含むリンクされた service alert で提供できます。 |
| **modifications** | [Modification](#message-modification) | 必須 | 複数 | 影響を受ける便に適用する変更のリストです。 |

### _message_ Modification {: #message-modification}


`Modification` メッセージは、`start_stop_selector` から開始する影響を受ける各便(trip)への変更を記述します。

<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来的に正式に採用される場合があります。

<img src="../../../assets/trip-modification.png">

_特定の便(trip)に対する変更の影響を示す例です。この変更は、他の複数の便(trip)にも適用される場合があります。_

<img src="../../../assets/propagated-delay.png">

_伝播する迂回遅延は、変更の終了後に続くすべての停留所等(stop)に影響します。便(trip)に複数の変更がある場合、遅延は累積されます。_


**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **start_stop_selector** | [StopSelector](#message-stopselector) | 必須 | 1つ | この変更の影響を受ける元の便(trip)の最初の停留所等(stop)の停留所等セレクタです。`end_stop_selector` と組み合わせて使用します。`start_stop_selector` は必須であり、`travel_time_to_stop` で使用される基準停留所等(stop)を定義するために使用されます。詳細については、[`travel_time_to_stop`](#message-replacementstop) を参照してください。 |
| **end_stop_selector** | [StopSelector](#message-stopselector) | 条件付き必須 | 1つ | この変更の影響を受ける元の便(trip)の最後の停留所等(stop)の停留所等セレクタです。選択には終了位置が含まれるため、この変更によって1つの stop_time のみが置き換えられる場合、`start_stop_selector` と `end_stop_selector` は同等でなければなりません。stop_time が置き換えられない場合、`end_stop_selector` を指定してはいけません。それ以外の場合は必須です。  |
| **propagated_modification_delay** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 変更によって挿入された最後の停留所等(stop)の後に続くすべての出発時刻および到着時刻に追加する遅延秒数です。変更がルート形状(shape)のみに影響する場合（すなわち、`end_stop_selector` も `replacement_stops` も指定されない場合）、遅延の伝播は `start_stop_selector` の後に続く停留所等(stop)から開始します。正または負の数にすることができます。同じ便(trip)に複数の変更が適用される場合、便(trip)の進行に伴って遅延は累積されます。<br/><br/>値が指定されない場合、コンシューマは他のデータに基づいて `propagated_modification_delay` を補間または推定することができます。  |
| **replacement_stops** | [ReplacementStop](#message-replacementstop) | 任意 | 複数 | 元の便(trip)の停留所等(stop)を置き換える、代替停留所等(stop)のリストです。新しい停車時刻(stop_time)の数は、置き換えられる停車時刻(stop_time)の数より少なくても、同じでも、多くてもかまいません。 |
| **service_alert_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 利用者向けの通信のために、この Modification を記述する `Alert` を含む `FeedEntity` メッセージの `id` 値です。 |
| **last_modified_time** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | このタイムスタンプは、変更が最後に変更された時点を識別します。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。 |

### _message_ StopSelector {: #message-stopselector}


停留所等(stop)のセレクタです。`stop_id` または `stop_sequence` のいずれかで指定します。2つの値のうち少なくとも1つを指定しなければなりません。 

<br><br>**注意:** このフィールドはまだ**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 対応する GTFS フィードの stop_times.txt と同じでなければなりません。`StopSelector` 内では `stop_sequence` または `stop_id` のいずれかを指定しなければなりません。両方のフィールドを空にすることはできません。予測の対象となる停留所等(stop)を明確に区別するため、同じ stop_id を複数回訪れる便(trip)（例: ループ）では `stop_sequence` が必須です。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 対応する GTFS フィードの stops.txt と同じでなければなりません。`StopSelector` 内では `stop_sequence` または `stop_id` のいずれかを指定しなければなりません。両方のフィールドを空にすることはできません。|

### _message_ SelectedTrips {: #message-selectedtrips}


関連付けられたルート形状(shape)を持つ、選択された便(trip)のリストです。

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip_ids** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 複数 | 含まれる置換の影響を受ける、元の（CSV）GTFSのtrip_idのリストです。少なくとも1つのtrip_idを含める必要があります。`schedule_relationship=REPLACEMENT` を持つ `TripUpdate` が、その便(trip)に対してすでに存在していてはいけません。 |
| **shape_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | このSelectedTrips内の変更された便(trip)の新しいルート形状(shape)のIDです。同じGTFS-RTフィード内の `Shape` メッセージを使用して追加された新しいルート形状(shape)、またはGTFS-Staticフィードのshapes.txtで定義された既存のルート形状(shape)を参照できます。real-timeフィード内の `Shape` エンティティを参照する場合、このフィールドの値はエンティティ内の `shape_id` の値であるべきであり、`FeedEntity` の `id` ではありません。 |

### _message_ ReplacementStop {: #message-replacementstop}


各 `ReplacementStop` メッセージは、便(trip)が新たに訪問する停留所等(stop)を定義し、任意でその停留所等(stop)までの推定移動時間を指定します。 

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。

<img src="../../../assets/first-stop-reference.png">

_変更が便(trip)の最初の停留所等(stop)に影響する場合、その停留所等(stop)は変更の参照停留所等(stop)も兼ねます。_


**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 便(trip)が新たに訪問する代替停留所等(stop) ID。これは、同じ GTFS-RT フィード内の GTFS-RT `Stop` メッセージを使用して追加された新しい停留所等(stop)、または (CSV) GTFS フィードの `stops.txt` で定義されている既存の停留所等(stop)を参照できます。リアルタイムフィード内の `Shape` エンティティを参照する場合、このフィールドの値はエンティティ内の `stop_id` の値であるべきであり、`FeedEntity` の `id` ではありません。停留所等(stop)は `location_type=0`（経路指定可能な停留所等(stop)）でなければなりません。 |
| **travel_time_to_stop** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | この停留所等(stop)への到着時刻と参照停留所等(stop)への到着時刻との差（秒単位）です。参照停留所等(stop)は `start_stop_selector` の前の停留所等(stop)です。変更が便(trip)の最初の停留所等(stop)から始まる場合、便(trip)の最初の停留所等(stop)が参照停留所等(stop)となります。 <br/><br/>この値は単調増加でなければならず、元の便(trip)の最初の停留所等(stop)が参照停留所等(stop)である場合にのみ負の数にすることができます。 <br/><br/>値が指定されない場合、コンシューマは他のデータに基づいて `travel_time_to_stop` を補間または推定することができます。 |
