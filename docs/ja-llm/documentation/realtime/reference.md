# GTFS Realtime リファレンス {: #gtfs-realtime-reference}


GTFS Realtime フィードにより、交通事業者は、サービスの障害（駅の閉鎖、路線の運休、重大な遅延など）、車両の位置、および予想到着時刻に関するリアルタイム情報を利用者に提供することができます。

フィード仕様のバージョン 2.0 は、このサイトで説明および文書化されています。有効なバージョンは "2.0"、"1.0" です。

## 用語の定義 {: #term-definitions}

### 必須 {: #required}


GTFS-realtime v2.0 以降では、*Required* 列は、交通データが有効であり利用アプリケーションにとって意味をなすために、producer が提供しなければならないフィールドを説明します。

*Required* フィールドでは、以下の値が使用されます。

*   **必須**: このフィールドは、GTFS-realtime feed producer が提供しなければなりません。
*   **条件付き必須**: このフィールドは、フィールドの*説明*で示される特定の条件下で必須です。これらの条件以外では、このフィールドは任意です。
*   **条件付き禁止**: このフィールドは、フィールドの*説明*で示される特定の条件下で禁止されています。これらの条件以外では、このフィールドは任意です。
*   **任意**: このフィールドは任意であり、producer が実装する必須はありません。ただし、基盤となる自動車両位置情報システムでデータが利用可能な場合（例: VehiclePosition `timestamp`）、producer は可能な限りこれらの任意フィールドを提供することが推奨されます。

*意味論的要件は GTFS-realtime version 1.0 では定義されていなかったため、`gtfs_realtime_version` が `1` の feed はこれらの要件を満たさない場合があることに注意してください（詳細については、[意味論的要件に関する提案](https://github.com/google/transit/pull/64)を参照してください）。*

### 多重度(cardinality) {: #cardinality}


*多重度(cardinality)* は、特定のフィールドに指定できる要素数を表し、以下の値があります。

* **1つ** - このフィールドには単一の要素を1つ指定することができます。これは、[Protocol Buffer の *required* および *optional* の多重度](https://developers.google.com/protocol-buffers/docs/proto#simple)に対応します。
* **複数** - このフィールドには複数の要素（0、1、またはそれ以上）を指定することができます。これは、[Protocol Buffer の *repeated* の多重度](https://developers.google.com/protocol-buffers/docs/proto#simple)に対応します。

フィールドが必須、条件付き必須、または任意であるかを確認するには、常に *Required* および *Description* フィールドを参照してください。Protocol Buffer の多重度については、[`gtfs-realtime.proto`](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto)を参照してください。

### Protocol Buffer データ型 {: #protocol-buffer-data-types}


以下の protocol buffer データ型は、フィード要素を記述するために使用されます。

*   **message**: 複合型
*   **enum**: 固定値のリスト

### 実験的フィールド {: #experimental-fields}


**experimental** とラベル付けされたフィールドは変更される可能性があり、まだ仕様に正式に採用されていません。**experimental** フィールドは、将来正式に採用されることがあります。

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
|**entity** | [FeedEntity](#message-feedentity) | 条件付き必須 | 複数 | feed の内容です。交通システムで利用可能なリアルタイム情報がある場合、このフィールドを提供しなければなりません。このフィールドが空の場合、利用者はシステムで利用可能なリアルタイム情報がないとみなすべきです。 |

### _message_ FeedHeader {: #message-feedheader}


フィードに関するメタデータであり、フィードメッセージに含まれます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **gtfs_realtime_version** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | フィード仕様のバージョンです。現在のバージョンは2.0です。 |
| **incrementality** | [Incrementality](#enum-incrementality) | 必須 | 1つ |
| **timestamp** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | このタイムスタンプは、このフィードの内容が作成された時点（サーバー時刻）を示します。POSIX時刻（すなわち、1970年1月1日 00:00:00 UTCからの秒数）です。リアルタイム情報を生成および消費するシステム間の時刻ずれを避けるため、タイムサーバーからtimestampを取得することを強く推奨します。数秒程度の時刻差は許容されるため、Stratum 3またはそれより低い階層のサーバーを使用しても問題ありません。 |
| **feed_version** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | リアルタイムデータの基となるGTFSフィードの`feed_info.feed_version`と一致する文字列です。コンシューマーはこれを使用して、現在有効なGTFSフィード、またはダウンロード可能な新しいGTFSフィードがいつ利用可能になるかを識別できます。 |

### _enum_ Incrementality {: #enum-incrementality}


現在の取得が増分であるかどうかを決定します。

*   **FULL_DATASET**: このフィード更新は、フィードに関する先行するすべてのリアルタイム情報を上書きします。したがって、この更新では、既知のすべてのリアルタイム情報の完全なスナップショットを提供することが期待されます。
*   **DIFFERENTIAL**: 現在、このモードは**サポートされておらず**、このモードを使用するフィードの動作は**未規定**です。DIFFERENTIAL モードの動作を完全に規定することについては、[GTFS Realtime mailing list](http://groups.google.com/group/gtfs-realtime) で議論が行われており、それらの議論が最終決定された際にドキュメントが更新されます。

**値**

| _**値**_ |
|-------------|
| **FULL_DATASET** |
| **DIFFERENTIAL** |

### _message_ FeedEntity {: #message-feedentity}


交通フィード内のエンティティの定義（または更新）です。エンティティが削除されない場合、'trip_update'、'vehicle'、'alert'、'shape'、'stop'、または 'trip_modification' フィールドのうち、ちょうど1つを設定するべきです。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | このエンティティのフィード内で一意の識別子です。idは増分更新のサポートを提供するためにのみ使用されます。フィードによって参照される実際のエンティティは、明示的なセレクタで指定しなければなりません（詳細については、以下のEntitySelectorを参照してください）。 |
| **is_deleted** | [bool](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | このエンティティを削除するかどうかを示します。IncrementalityがDIFFERENTIALのフィードに対してのみ提供するべきです。IncrementalityがFULL_DATASETのフィードに対しては、このフィールドを提供するべきではありません。 |
| **trip_update** | [TripUpdate](#message-tripupdate) | 条件付き必須 | 1つ | 便(trip)のリアルタイムの出発遅延に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらすべてのフィールドを空にすることはできません。 |
| **vehicle** | [VehiclePosition](#message-vehicleposition) | 条件付き必須 | 1つ | 車両のリアルタイム位置に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらすべてのフィールドを空にすることはできません。 |
| **alert** | [Alert](#message-alert) | 条件付き必須 | 1つ | リアルタイムの運行情報(alert)に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらすべてのフィールドを空にすることはできません。 |
| **shape** | [Shape](#message-shape) | 条件付き必須 | 1つ | 迂回などのためにリアルタイムで追加されたルート形状(shape)に関するデータです。trip_update、vehicle、alert、またはshapeフィールドのうち少なくとも1つを提供しなければなりません。これらすべてのフィールドを空にすることはできません。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |
| **stop** | [Stop](#message-stop) | 条件付き必須 | 1つ | フィードに動的に追加される新しい停留所等(stop)です。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |
| **trip_modifications** | [TripModifications](#message-tripmodifications) | 条件付き必須 | 1つ | 迂回など、特定の変更の影響を受ける便(trip)の一覧です。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |

### _message_ TripUpdate {: #message-tripupdate}


便に沿った車両の進行状況に関するリアルタイム更新です。[便の更新(trip update) entities](../../../documentation/realtime/feed-entities/trip-updates)に関する一般的な説明も参照してください。
upd
ScheduleRelationship の値に応じて、TripUpdate は以下を指定できます。

*   スケジュールに従って運行する便。
*   ルートに沿って運行するが、固定スケジュールを持たない便。
*   スケジュールに対して追加または削除された便。
*   static GTFS 内の既存の便を置き換える便。
*   static GTFS 内の既存の便のコピーである新しい便。TripProperties で指定された運行日(service day)および時刻に運行されます。

更新は、将来の予測された到着・出発イベントに対するもの、またはすでに発生した過去のイベントに対するものとすることができます。ほとんどの場合、過去のイベントに関する情報は測定値であるため、その uncertainty 値は 0 にすることが推奨されます。ただし、これが当てはまらない場合もあるため、過去のイベントについて 0 以外の uncertainty 値を持つことが許可されています。更新の uncertainty が 0 でない場合、その更新は、完了していない便に対する概算予測であるか、測定が正確でないか、またはイベント発生後に検証されていない過去に対する予測でした。

車両が同一ブロック内で複数の便を運行する場合（便およびブロックの詳細については、[GTFS trips.txt](../../schedule/reference/#tripstxt)を参照してください）:

* フィードには、車両が現在運行している便の TripUpdate を含めるべきです。提供者は、これらの将来の便についての予測品質に確信がある場合、この車両のブロックにおける現在の便の後の 1つ以上の便についても TripUpdates を含めることが推奨されます。同一車両に対して複数の TripUpdates を含めることで、車両がある便から別の便へ移行する際の乗客にとっての予測の「突然の表示」を回避でき、また、後続の便に影響する遅延（例: 既知の遅延が便間の計画された待機時間を超える場合）を乗客に事前通知できます。
* 各 TripUpdate entities は、ブロック内でスケジュールされている順序と同じ順序でフィードに追加する必要はありません。たとえば、すべて 1つのブロックに属する `trip_ids` 1、2、3 の便があり、車両が便 1、次に便 2、次に便 3 を運行する場合、`trip_update` entities は任意の順序で出現できます。たとえば、便 2、次に便 1、次に便 3 を追加することが許可されています。

更新は、すでに完了した便を記述できることに注意してください。このためには、便の最終停留所等(stop)に対する更新を提供するだけで十分です。最終停留所等(stop)への到着時刻が過去である場合、クライアントは便全体が過去のものであると判断します（重要ではありませんが、先行する停留所等(stop)に対する更新も提供できます）。このオプションは、予定より早く完了したものの、スケジュール上は現在時刻においてまだ運行中である便に最も関連します。この便の更新を削除すると、クライアントが便はまだ運行中であると想定する可能性があります。フィード提供者は過去の更新を削除することが許可されていますが、必須ではないことに注意してください。これは、それが実用的に有用となるケースの 1つです。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip** | [TripDescriptor](#message-tripdescriptor) | 必須 | 1つ | このメッセージが適用される便(trip)です。実際の各便インスタンスに対して、TripUpdate entity は最大 1つ存在できます。存在しない場合、予測情報が利用できないことを意味します。これは、便がスケジュールどおりに進行していることを意味するものでは*ありません*。 |
| **vehicle** | [VehicleDescriptor](#message-vehicledescriptor) | 任意 | 1つ | この便を運行している車両に関する追加情報です。 |
| **stop_time_update** | [StopTimeUpdate](#message-stoptimeupdate) | 条件付き必須 | 複数 | 便の StopTimes に対する更新です（将来のもの、すなわち予測と、場合によっては過去のもの、すなわちすでに発生したものの両方）。更新は stop_sequence によってソートしなければならず、次に指定された stop_time_update までの便の後続するすべての停留所等(stop)に適用されます。<br>trip.schedule_relationship が SCHEDULED または UNSCHEDULED の場合、便には少なくとも 1つの stop_time_update を提供しなければなりません。<br>trip.schedule_relationship が NEW または REPLACEMENT の場合、過去の時刻を持つ停留所等(stop)を含め、新規または置換便のすべての停留所等(stop)に対して stop_time_updates を提供しなければならず、static GTFS の停車時刻(stop_time)は使用されません。<br>便がキャンセルまたは削除された場合、stop_time_updates を提供する必要はありません。キャンセルまたは削除された便に対して stop_time_updates が提供される場合、trip.schedule_relationship は、すべての stop_time_updates およびそれらに関連付けられた schedule_relationship より優先されます。便が複製された場合、新しい便のリアルタイム情報を示すために stop_time_updates を提供することができます。 |
| **timestamp** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 将来の StopTimes を推定するために、車両のリアルタイム進行状況が最後に測定された最新の時点です。過去の StopTimes が提供される場合、到着・出発時刻はこの値より前であることがあります。POSIX 時刻（すなわち、1970 年 1 月 1 日 00:00:00 UTC からの秒数）です。 |
| **delay** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 便の現在のスケジュールからの乖離です。Delay は、予測が GTFS 内の既存スケジュールに対して相対的に与えられる場合にのみ指定するべきです。<br>Delay（秒）は正（車両が遅れていることを意味します）または負（車両がスケジュールより早いことを意味します）にすることができます。Delay が 0 の場合、車両は正確に定時です。<br>StopTimeUpdates 内の Delay 情報は便レベルの Delay 情報より優先されるため、便レベルの Delay は、StopTimeUpdate の Delay 値が指定された便の次の停留所等(stop)までのみ伝播されます。<br>フィード提供者は、データの鮮度を評価するために、Delay 値が最後に更新された時点を示す TripUpdate.timestamp 値を提供することが強く推奨されます。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。|
| **trip_properties** | [TripProperties](#message-tripproperties) | 任意 | 1つ | 便の更新されたプロパティを提供します。<br><br>**注意:** このメッセージは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

### _message_ StopTimeEvent {: #message-stoptimeevent}


単一の予測イベント（到着または出発のいずれか）の時刻情報です。時刻情報は遅延および／または推定時刻、不確実性で構成されます。NEW、REPLACEMENT、または DUPLICATED の便(trip)には、予定時刻を追加することもできます。

*   予測が GTFS 内の既存のスケジュールに対して相対的に示される場合は、delay を使用するべきです。
*   予測スケジュールがあるかどうかにかかわらず time を指定するべきであり、新規または置換便には必ず指定しなければなりません。time と delay の両方が指定されている場合、time が優先されます（ただし通常、スケジュール済みの便に対して time が指定される場合は、GTFS の予定時刻 + delay と等しくするべきです）。
*   便(trip)が新規、置換、または複製便である場合、scheduled_time を指定することができます。

不確実性は time と delay の両方に等しく適用されます。不確実性は、実際の遅延における予想誤差をおおよそ示します（ただし、その正確な統計的意味はまだ定義されていないことに注意してください）。例えば、コンピュータによる時刻制御の下で運行される列車では、不確実性を 0 にすることができます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **delay** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 遅延（秒単位）は正（車両が遅れていることを意味します）または負（車両が予定より早いことを意味します）にすることができます。遅延が 0 の場合、車両は正確に定時です。<br>StopTimeUpdate.schedule_relationship が NO_DATA の場合は**禁止**です。<br>それ以外の場合で time が指定されていない場合は**必須**です。 |
| **time** | [int64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 絶対時刻としての推定または実際のイベントです。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。<br>StopTimeUpdate.schedule_relationship が NO_DATA の場合は**禁止**です。<br>それ以外の場合で delay が指定されていない場合は**必須**です。 |
| **scheduled_time** | [int64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き禁止 | 1つ | 予定時刻です。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。<br>TripUpdate.schedule_relationship が NEW、REPLACEMENT、または DUPLICATED の場合は**任意**であり、それ以外の場合は**禁止**です。 |
| **uncertainty** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | uncertainty が省略された場合、不明として解釈されます。完全に確実な予測を指定するには、その uncertainty を 0 に設定してください。<br>StopTimeUpdate.schedule_relationship が NO_DATA の場合は**禁止**です。 |

### _message_ StopTimeUpdate {: #message-stoptimeupdate}


便(trip)における特定の停留所等(stop)の到着イベントおよび／または出発イベントに対するRealtime更新です。[TripDescriptor](#message-tripdescriptor)および[便の更新(trip update)エンティティ](../../../documentation/realtime/feed-entities/trip-updates)のドキュメントにある停車時刻(stop_time)更新に関する一般的な説明も参照してください。

過去および将来のイベントの両方について更新を提供することができます。`TripUpdate.schedule_relationship` が NEW または REPLACEMENT である場合を除き、producer は必須ではありませんが、過去のイベントを削除することができます。この場合、過去の停留所等(stop)は、車両が運行している便(trip)を定義するため、便(trip)全体が完了するまで削除してはいけません。
更新は stop_sequence または stop_id を通じて特定の停留所等(stop)に関連付けられるため、これらのフィールドのいずれか1つを必ず設定しなければなりません。同じ stop_id が1つの便(trip)内で複数回訪問される場合、その便(trip)におけるその stop_id のすべての StopTimeUpdate で stop_sequence を提供するべきです。

新規または置換の便(trip)では、GTFS Static 内の既存の便(trip)を参照せずに、便(trip)が訪問する停留所等(stop)を指定するために更新が使用されます。このような便(trip)では、`stop_id`、`stop_sequence`、`departure`、および `arrival` をすべて設定しなければなりません。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 対応する GTFS feed の stop_times.txt 内の値と同じでなければなりません。StopTimeUpdate 内では stop_sequence または stop_id のいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。予測の対象となる停留所等(stop)を明確にするため、同じ stop_id を複数回訪問する便(trip)（例: ループ）では stop_sequence が必須です。`StopTimeProperties.assigned_stop_id` が設定されている場合、`stop_sequence` を設定しなければなりません。`TripUpdate.schedule_relationship` が NEW または REPLACEMENT の場合は**必須**であり、その値は便(trip)に沿って増加しなければなりません。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 対応する GTFS feed の stops.txt 内の値と同じでなければなりません。StopTimeUpdate 内では stop_sequence または stop_id のいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。`StopTimeProperties.assigned_stop_id` が設定されている場合、stop_id を省略し、stop_sequence のみを使用することが推奨されます。`StopTimeProperties.assigned_stop_id` と `stop_id` が設定されている場合、`stop_id` は `assigned_stop_id` と一致しなければなりません。`TripUpdate.schedule_relationship` が NEW または REPLACEMENT の場合は**必須**です。 |
| **arrival** | [StopTimeEvent](#message-stoptimeevent) | 条件付き必須 | 1つ | schedule_relationship が空または SCHEDULED の場合、StopTimeUpdate 内では arrival または departure のいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。schedule_relationship が SKIPPED の場合、arrival と departure は両方とも空にすることができます。`TripUpdate.schedule_relationship` が NEW または REPLACEMENT の場合は**必須**です。 |
| **departure** | [StopTimeEvent](#message-stoptimeevent) | 条件付き必須 | 1つ | schedule_relationship が空または SCHEDULED の場合、StopTimeUpdate 内では arrival または departure のいずれかを提供しなければなりません。両方のフィールドを空にすることはできません。schedule_relationship が SKIPPED の場合、arrival と departure は両方とも空にすることができます。`TripUpdate.schedule_relationship` が NEW または REPLACEMENT の場合は**必須**です。 |
| **departure_occupancy_status** | [OccupancyStatus](#enum-occupancystatus) | 任意 | 1つ | 指定された停留所等(stop)を出発した直後の車両における、予測される乗客混雑状態です。提供する場合、stop_sequence を提供しなければなりません。リアルタイムの到着または出発予測を提供せずに departure_occupancy_status を提供するには、このフィールドを設定し、StopTimeUpdate.schedule_relationship = NO_DATA を設定してください。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |
| **schedule_relationship** | [ScheduleRelationship](#enum-schedulerelationship) | 任意 | 1つ | デフォルトの関係は SCHEDULED です。 |
| **stop_time_properties** | [StopTimeProperties](#message-stoptimeproperties) | 任意 | 1つ | GTFS stop_times.txt 内で定義される特定のプロパティに対するRealtime更新です。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |

### _enum_ ScheduleRelationship {: #enum-schedulerelationship}


このStopTimeと静的スケジュールとの関係です。

**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **SCHEDULED** | 車両は、必ずしもスケジュール上の時刻どおりではないものの、静的な停留所等(stop)スケジュールに従って運行しています。これは**デフォルト**の動作です。arrivalとdepartureの少なくとも一方を指定しなければなりません。頻度ベースの便(trip)（exact_times = 0 の GTFS frequencies.txt）は、SCHEDULED値を持つべきではなく、代わりにUNSCHEDULEDを使用するべきです。 |
| **SKIPPED** | 停留所等(stop)は通過されます。すなわち、車両はこの停留所等(stop)に停車しません。arrivalおよびdepartureは任意です。`SKIPPED`が設定された場合、同じ便(trip)内の後続の停留所等(stop)には伝播しません（すなわち、それらの停留所等(stop)にも`schedule_relationship: SKIPPED`を持つ`stop_time_update`がない限り、車両は便(trip)内の後続の停留所等(stop)に停車します）。便(trip)内の前の停留所等(stop)からの遅延は、`SKIPPED`の停留所等(stop)を越えて伝播します。言い換えると、`SKIPPED`の停留所等(stop)の後の停留所等(stop)に対してarrivalまたはdepartureの予測を持つ`stop_time_update`が設定されていない場合、`SKIPPED`の停留所等(stop)より前の予測が、`SKIPPED`の停留所等(stop)の後の停留所等(stop)および、後続の停留所等(stop)に対する`stop_time_update`が提供されるまで便(trip)内の後続の停留所等(stop)へ伝播します。  |
| **NO_DATA** | この停留所等(stop)にはリアルタイムデータが提供されていません。これは、利用可能なリアルタイムの時刻情報がないことを示します。NO_DATAが設定された場合、後続の停留所等(stop)を通じて伝播するため、リアルタイムの時刻情報を持たない停留所等(stop)を指定する推奨の方法です。NO_DATAが設定されている場合、`TripDescriptor.schedule_relationship`が`NEW`または`REPLACEMENT`である場合を除き、arrivalまたはdepartureを指定してはいけません。その場合は、予測ではなくスケジュール時刻のみを指定しなければなりません。`TripDescriptor.schedule_relationship`が`NEW`または`REPLACEMENT`である場合、StopTimeUpdateが便(trip)の停留所等(stop)リストを定義するため、arrivalおよびdepartureには引き続きスケジュール時刻を指定しなければなりません。この場合、スケジュールが静的GTFSとは無関係であるものの、リアルタイム予測がまだ利用できないことを示します。 |
| **UNSCHEDULED** | 車両は頻度ベースの便(trip)（exact_times = 0 の GTFS frequencies.txt）を運行しています。この値は、GTFS frequencies.txtで定義されていない便(trip)、またはexact_times = 1 の GTFS frequencies.txt内の便(trip)には使用するべきではありません。`schedule_relationship: UNSCHEDULED`を持つ`stop_time_updates`を含む便(trip)は、TripDescriptorにも`schedule_relationship: UNSCHEDULED`を設定しなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用されることがあります。

### _message_ StopTimeProperties {: #message-stoptimeproperties}


GTFS stop_times.txt 内で定義される特定のプロパティに対するリアルタイム更新です。

**注意:** このメッセージは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。<br> 

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **assigned_stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | リアルタイムの停留所等(stop)割り当てをサポートします。GTFS `stops.txt` で定義された `stop_id` を参照します。<br> 新しい `assigned_stop_id` は、GTFS `stop_times.txt` で定義された `stop_id` と比較して、エンドユーザーにとって著しく異なる便(trip)体験をもたらすべきではありません。言い換えると、追加のコンテキストなしにアプリ内で新しい停留所等(stop)が提示された場合、エンドユーザーはこの新しい `stop_id` を「通常ではない変更」と見なすべきではありません。たとえば、このフィールドは、GTFS `stop_times.txt` で当初定義された停留所等(stop)と同じ駅に属する `stop_id` を使用して、プラットフォームの割り当てに使用することを意図しています。<br> リアルタイムの到着予測または出発予測を提供せずに停留所等(stop)を割り当てるには、このフィールドを設定し、`StopTimeUpdate.schedule_relationship = NO_DATA` を設定してください。<br> このフィールドが設定されている場合、`StopTimeUpdate.stop_sequence` を設定しなければならず、`StopTimeUpdate.stop_id` は設定するべきではありません。停留所等(stop)の割り当ては、他の GTFS-realtime フィールド（例: `VehiclePosition.stop_id`）にも反映するべきです。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。 |
| **stop_headsign** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 停留所等(stop)における車両の更新された行先表示(headsign)です。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。 |
| **drop_off_type** | [DropOffPickupType](#enum-dropoffpickuptype) | 任意 | 1つ | 停留所等(stop)における車両の更新された降車です。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。 |
| **pickup_type** | [DropOffPickupType](#enum-dropoffpickuptype) | 任意 | 1つ | 停留所等(stop)における車両の更新された乗車です。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。 |

## _enum_ DropOffPickupType {: #enum-dropoffpickuptype}


**値**

| _**値**_                   | _**コメント**_                                         |
|----------------------------|--------------------------------------------------------|
| **REGULAR**                | 定期的に予定された乗車／降車です。                     |
| **NONE**                   | 乗車／降車は利用できません。                           |
| **PHONE_AGENCY**           | 乗車／降車を手配するために事業者へ電話しなければなりません。 |
| **COORDINATE_WITH_DRIVER** | 乗車／降車を手配するために運転手と調整しなければなりません。 |

### _message_ TripProperties {: #message-tripproperties}


便の更新されたプロパティを定義します

**注意:** このメッセージは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。<br>.

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | (CSV) GTFS trips.txt で定義されている既存の便(trip)の複製であるものの、異なる運行日(service day)および/または時刻（`TripProperties.start_date` および `TripProperties.start_time` を使用して定義）に開始する新しい便(trip)の識別子を定義します。(CSV) GTFS における `trips.trip_id` の定義を参照してください。その値は、(CSV) GTFS で使用されているものとは異ならなければなりません。このフィールドは、`schedule_relationship` が `DUPLICATED` の場合は必須です。それ以外の場合、このフィールドには値を設定してはいけません。設定された場合、コンシューマーによって無視されます。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **start_date** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 複製された便(trip)が運行される運行日(service day)です。YYYYMMDD 形式で指定しなければなりません。このフィールドは、`schedule_relationship` が `DUPLICATED` の場合は必須です。それ以外の場合、このフィールドには値を設定してはいけません。設定された場合、コンシューマーによって無視されます。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **start_time** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 便(trip)が複製される際の出発開始時刻を定義します。(CSV) GTFS における `stop_times.departure_time` の定義を参照してください。複製された便(trip)の予定到着時刻および出発時刻は、元の便(trip)の `departure_time` とこのフィールドとのオフセットに基づいて計算されます。たとえば、GTFS の便(trip)に `departure_time` が `10:00:00` の停留所等(stop) A と、`departure_time` が `10:01:00` の停留所等(stop) B があり、このフィールドに `10:30:00` の値が設定されている場合、複製された便(trip)の停留所等(stop) B の予定 `departure_time` は `10:31:00` になります。予測時刻を決定するために、リアルタイム予測の `delay` 値がこの計算された予定時刻に適用されます。たとえば、停留所等(stop) B に対して出発 `delay` が `30` として提供される場合、予測出発時刻は `10:31:30` になります。リアルタイム予測の `time` 値にはオフセットは適用されず、提供された予測時刻を示します。たとえば、10:31:30 を表す出発 `time` が停留所等(stop) B に対して提供される場合、予測出発時刻は `10:31:30` になります。このフィールドは、`schedule_relationship` が `DUPLICATED` の場合は必須です。それ以外の場合、このフィールドには値を設定してはいけません。設定された場合、コンシューマーによって無視されます。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **trip_headsign** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | この便(trip)の行先表示(headsign)が元のものと異なる場合に、その行先表示(headsign)を指定します。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **trip_short_name** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | この便(trip)の名称が元のものと異なる場合に、その名称を指定します。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **shape_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 便(trip)のルート形状(shape)が (CSV) GTFS で指定されたルート形状(shape)と異なる場合、または乗客の需要に応じて異なる経路を通る車両など、(CSV) GTFS で提供されていない場合にリアルタイムで指定するため、車両走行経路のルート形状(shape)の識別子を指定します。(CSV) GTFS における `trips.shape_id` の定義を参照してください。<br>ルート形状(shape)が (CSV) GTFS にもリアルタイムにも定義されていない場合、ルート形状(shape)は不明と見なされます。このフィールドは、(CSV) GTFS の shapes.txt で定義されたルート形状(shape)、または同じ (protobuf) リアルタイムフィード内の `Shape` を参照できます。この便(trip)の停留所等(stop)の順序（stop sequences）は、(CSV) GTFS と同じままでなければなりません。同じリアルタイムフィード内の `Shape` エンティティを参照する場合、このフィールドの値はエンティティ内の `shape_id` の値であるべきであり、`FeedEntity` の `id` ではありません。<br>迂回が発生した場合など、元の便(trip)の一部であるものの今後は停車しない停留所等(stop)は、schedule_relationship=SKIPPED としてマークするべきです。または、`TripModifications` メッセージを通じて詳細を提供することができます。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |

### _message_ VehiclePosition {: #message-vehicleposition}


特定の車両に関するRealtimeの位置情報です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip** | [TripDescriptor](#message-tripdescriptor) | 任意 | 1つ | この車両が運行している便(trip)です。車両を特定の便のインスタンスとして識別できない場合は、空または部分的なものにすることができます。 |
| **vehicle** | [VehicleDescriptor](#message-vehicledescriptor) | 任意 | 1つ | この便を運行している車両に関する追加情報です。各エントリは**一意の**車両IDを持つべきです。 |
| **position** | [Position](#message-position) | 任意 | 1つ | この車両の現在位置です。 |
| **current_stop_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 現在の停留所等(stop)の停留所順序インデックスです。current_stop_sequenceの意味（すなわち、それが参照する停留所等(stop)）はcurrent_statusによって決まります。current_statusがない場合は、IN_TRANSIT_TOが想定されます。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 現在の停留所等(stop)を識別します。値は、対応するGTFSフィードのstops.txt内の値と同じでなければなりません。`StopTimeProperties.assigned_stop_id`を使用して`stop_id`を割り当てる場合、このフィールドにも`stop_id`の変更を反映するべきです。 |
| **current_status** | [VehicleStopStatus](#enum-vehiclestopstatus) | 任意 | 1つ | 現在の停留所等(stop)に対する車両の正確な状態です。current_stop_sequenceがない場合は無視されます。 |
| **timestamp** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両位置情報(vehicle position)が測定された時点です。POSIX時刻（すなわち、1970年1月1日00:00:00 UTCからの秒数）です。 |
| **congestion_level** | [CongestionLevel](#enum-congestionlevel) | 任意 | 1つ |
| **occupancy_status** | [OccupancyStatus](#enum-occupancystatus) | _任意_ | 1つ | 車両または車両編成の乗客混雑状態です。multi_carriage_detailsに車両ごとのOccupancyStatusが設定されている場合、このフィールドは乗客を受け入れるすべての車両編成を考慮した車両全体を説明するべきです。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。|
| **occupancy_percentage** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両内の乗客混雑度を示す百分率値です。値100は、座席および立席の両方の定員と、現在の運行規則で許容される定員を含む、車両が設計された最大総定員を表すべきです。乗客数が設計上の最大定員を超える場合、値は100を超えることがあります。occupancy_percentageの精度は、個々の乗客が車両に乗車または降車することを追跡できない程度に低くするべきです。multi_carriage_detailsに車両ごとのoccupancy_percentageが設定されている場合、このフィールドは乗客を受け入れるすべての車両編成を考慮した車両全体を説明するべきです。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |
| **multi_carriage_details** | [CarriageDetails](#message-carriagedetails) | 任意 | 複数 | この特定の車両の複数の車両編成に関する詳細です。最初の出現は、**現在の進行方向を基準として**車両の先頭車両を表します。multi_carriage_detailsフィールドの出現数は、車両の車両編成数を表します。また、エンジン、保守用車両などの乗車できない車両編成も含まれます。これらは、プラットフォーム上でどこに立つべきかについて乗客に有益な情報を提供するためです。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。 |

### _enum_ VehicleStopStatus {: #enum-vehiclestopstatus}


**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **INCOMING_AT** | 車両は停留所等(stop)にまもなく到着します（停留所等(stop)の表示では、通常、車両記号が点滅します）。 |
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


車両または車両編成の乗客混雑状態です。

個々のプロデューサーは、すべての OccupancyStatus 値を公開しない場合があります。したがって、コンシューマーは OccupancyStatus 値が線形スケールに従うと仮定してはいけません。コンシューマーは、OccupancyStatus 値をプロデューサーが示し意図した状態として表現するべきです。同様に、プロデューサーは実際の車両混雑状態に対応する OccupancyStatus 値を使用しなければなりません。

線形スケールで乗客混雑レベルを記述するには、`occupancy_percentage` を参照してください。

**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される場合があります。

***値***

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| _**EMPTY**_ | _車両はほとんどの基準で空車と見なされ、乗車中の乗客はほとんどまたはいませんが、引き続き乗客を受け入れています。_ |
| _**MANY_SEATS_AVAILABLE**_ | _車両または車両編成には多数の空席があります。このカテゴリに該当するほど十分に多いと見なされる、総座席数に対する空席数は、プロデューサーの裁量で決定されます。_ |
| _**FEW_SEATS_AVAILABLE**_ | _車両または車両編成には少数の空席があります。このカテゴリに該当するほど十分に少ないと見なされる、総座席数に対する空席数は、プロデューサーの裁量で決定されます。_ |
| _**STANDING_ROOM_ONLY**_ | _車両または車両編成は現在、立っている乗客のみを収容できます。_ |
| _**CRUSHED_STANDING_ROOM_ONLY**_ | _車両または車両編成は現在、立っている乗客のみを収容でき、そのためのスペースも限られています。_ |
| _**FULL**_ | _車両はほとんどの基準で満員と見なされますが、依然として乗客の乗車を許可している場合があります。_ |
| _**NOT_ACCEPTING_PASSENGERS**_ | _車両または車両編成は乗客を受け入れていません。この車両または車両編成は通常、乗客の乗車を受け入れます。_ |
| _**NO_DATA_AVAILABLE**_ | _車両または車両編成には、その時点で利用可能な混雑データがありません。_ |
| _**NOT_BOARDABLE**_ | _車両または車両編成は乗車できず、乗客を受け入れることはありません。特殊車両または車両編成（機関車、保守用車両など）に有用です。_ |

### _message_ CarriageDetails {: #message-carriagedetails}


複数の車両で構成される車両に使用される、車両ごとの詳細です。

**注意:** このメッセージは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両の識別子です。車両ごとに一意であるべきです。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **label** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 乗客が車両を識別するのに役立つよう表示される場合がある、ユーザーに表示されるラベルです。例: "7712"、"Car ABC-32" など。<br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **occupancy_status** | [OccupancyStatus](#enum-occupancystatus) | 任意 | 1つ | この車両内の当該車両の混雑状況です。デフォルトは `NO_DATA_AVAILABLE` に設定されます。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。|
| **occupancy_percentage** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | この車両内の当該車両の混雑率です。"VehiclePosition.occupancy_percentage" と同じ規則に従います。当該車両のデータが利用できない場合は -1 を使用してください。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |
| **carriage_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 車両の CarriageStatus リスト内の他の車両に対する、この車両の順序を識別します。進行方向の先頭車両は値 1 でなければなりません。2番目の値は進行方向の2番目の車両に対応し、値 2 でなければなりません。以降も同様です。例えば、進行方向の先頭車両の値は 1 です。進行方向の2番目の車両の値が 3 の場合、コンシューマーはすべての車両のデータ（すなわち、multi_carriage_details フィールド）を破棄します。データのない車両は、有効な carriage_sequence 番号で表現しなければならず、データのないフィールドは省略するべきです（代替として、それらのフィールドを含めて「データなし」の値に設定することもできます）。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。 |

### _message_ Alert {: #message-alert}


公共交通ネットワークにおける何らかの事象を示す運行情報(alert)です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **active_period** | [TimeRange](#message-timerange) | 任意 | 複数 | 運行情報(alert)をユーザーに表示するべき時間です。存在しない場合、運行情報(alert)はフィードに存在する限り表示されます。複数の範囲が指定されている場合、運行情報(alert)はそれらすべての期間中に表示されます。 |
| **communication_period** | [TimeRange](#message-timerange) | 任意 | 複数 | 情報提供のみを目的として運行情報(alert)をユーザーに表示するべき時間です。存在しない場合、利用側アプリケーションは表示するのが適切な時期を決定することができます。 
| **impact_period** | [TimeRange](#message-timerange) | 任意 | 複数 | サービスが運行情報(alert)の影響を受ける時間です。communication_periodが指定されている場合、impact_period内のすべての時間間隔は、communication_periodの少なくとも1つの時間間隔内に完全に含まれていなければなりません。|
| **informed_entity** | [EntitySelector](#message-entityselector) | 必須 | 複数 | この運行情報(alert)を通知するべきユーザーを持つエンティティです。少なくとも1つのinformed_entityを提供しなければなりません。 |
| **cause** | [Cause](#enum-cause) | 条件付き必須 | 1つ | cause_detailが含まれる場合、Causeも含まれなければなりません。
| **cause_detail** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 事業者固有の表現を可能にする、Causeよりも具体的な運行情報(alert)の原因の説明です。cause_detailが含まれる場合、Causeも含まれなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。
| **effect** | [Effect](#enum-effect) | 条件付き必須 | 1つ | effect_detailが含まれる場合、Effectも含まれなければなりません。
| **effect_detail** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 事業者固有の表現を可能にする、Effectよりも具体的な運行情報(alert)の影響の説明です。effect_detailが含まれる場合、Effectも含まれなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。
| **url** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 運行情報(alert)に関する追加情報を提供するURLです。 |
| **header_text** | [TranslatedString](#message-translatedstring) | 必須 | 1つ | 運行情報(alert)のヘッダーです。このプレーンテキスト文字列は、たとえば太字で強調表示されます。 |
| **description_text** | [TranslatedString](#message-translatedstring) | 必須 | 1つ | 運行情報(alert)の説明です。このプレーンテキスト文字列は、運行情報(alert)の本文として整形されます（またはユーザーによる明示的な「展開」要求時に表示されます）。説明内の情報は、ヘッダーの情報を補完するべきです。 |
| **tts_header_text** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 読み上げ用実装で使用する運行情報(alert)のヘッダーを含むテキストです。このフィールドはheader_textの読み上げ用フィールド(text-to-speech field)版です。header_textと同じ情報を含むべきですが、読み上げ可能な形式に整形するべきです（たとえば、略語を削除する、数字を読み上げ表記にするなど）。 |
| **tts_description_text** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | 読み上げ用実装で使用する運行情報(alert)の説明を含むテキストです。このフィールドはdescription_textの読み上げ用フィールド(text-to-speech field)版です。description_textと同じ情報を含むべきですが、読み上げ可能な形式に整形するべきです（たとえば、略語を削除する、数字を読み上げ表記にするなど）。 |
| **severity_level** | [SeverityLevel](#enum-severitylevel) | 任意 | 1つ | 運行情報(alert)の重大度です。 |
| **image** | [TranslatedImage](#message-translatedimage) | 任意 | 1つ | 運行情報(alert)のテキストとともに表示するTranslatedImageです。迂回、駅閉鎖などの運行情報(alert)の影響を視覚的に説明するために使用されます。画像は運行情報(alert)の理解を促進するべきであり、重要な情報が存在する唯一の場所であってはいけません。次の種類の画像は推奨されません: 主にテキストを含む画像、追加情報を提供しないマーケティング画像またはブランド画像。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **image_alternative_text** | [TranslatedString](#message-translatedstring) | 任意 | 1つ | `image`フィールド内のリンクされた画像の外観を説明するテキストです（たとえば、画像を表示できない場合や、アクセシビリティ上の理由でユーザーが画像を見られない場合）。HTMLの[代替画像テキストの仕様](https://html.spec.whatwg.org/#alt)を参照してください。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

### _enum_ Cause {: #enum-cause}


この運行情報(alert)の原因です。

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

**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

**値**

| _**値**_ |
|-------------|
| **UNKNOWN_SEVERITY** |
| **INFO** |
| **WARNING** |
| **SEVERE** |

### _message_ TimeRange {: #message-timerange}


時間間隔です。時刻 `t` が開始時刻以上かつ終了時刻未満である場合、その間隔は時刻 `t` において有効と見なされます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **start** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | POSIX time（すなわち、1970年1月1日 00:00:00 UTCからの秒数）による開始時刻です。存在しない場合、間隔は負の無限大から開始します。TimeRange が提供される場合、start または end のいずれかを提供しなければなりません。両方のフィールドを空にしてはいけません。 |
| **end** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | POSIX time（すなわち、1970年1月1日 00:00:00 UTCからの秒数）による終了時刻です。存在しない場合、間隔は正の無限大で終了します。TimeRange が提供される場合、start または end のいずれかを提供しなければなりません。両方のフィールドを空にしてはいけません。 |

### _message_ Position {: #message-position}


車両の地理的位置です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **latitude** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | WGS-84座標系における北緯（度）です。 |
| **longitude** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | WGS-84座標系における東経（度）です。 |
| **bearing** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 真北から時計回りの度数で表した方位です。すなわち、0は北、90は東です。これはコンパス方位、または次の停留所等(stop)もしくは中間地点への方向とすることができます。クライアントは以前のデータから計算できるため、これは以前の位置情報の連続から推定するべきではありません。 |
| **odometer** | [double](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | メートル単位の走行距離計の値です。 |
| **speed** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両によって測定された瞬間速度です。単位はメートル毎秒です。 |

### _message_ TripDescriptor {: #message-tripdescriptor}


`schedule_relationship` が `NEW` である場合を除き、GTFS の便(trip)の単一のインスタンスを識別する記述子です。`NEW` の場合は、追加する新しい便(trip)のインスタンスを指定します。

単一の便(trip)インスタンスを指定するには、多くの場合、`trip_id` のみで十分です。ただし、単一の便(trip)インスタンスに解決するために、以下のケースでは追加情報が必要です。

* frequencies.txt で定義された便(trip)では、`trip_id` に加えて `start_date` および `start_time` が必須です
* 便(trip)が24時間を超えて継続する場合、または翌日のスケジュールされた便(trip)と重複するほど遅延している場合、`trip_id` に加えて `start_date` が必須です
* `trip_id` フィールドを提供できない場合、`route_id`、`direction_id`、`start_date`、および `start_time` をすべて提供しなければなりません

すべての場合において、`trip_id` に加えて `route_id` が提供される場合、`route_id` は GTFS trips.txt で指定された便(trip)に割り当てられている `route_id` と同じでなければなりません。

`trip_id` フィールドは、単独でも他の TripDescriptor フィールドとの組み合わせでも、複数の便(trip)インスタンスを識別するために使用してはいけません。たとえば、GTFS frequencies.txt の exact_times=0 の便(trip)について、TripDescriptor が `trip_id` のみを指定するべきではありません。これは、特定の時刻に開始する単一の便(trip)インスタンスに解決するためには `start_time` も必須であるためです。TripDescriptor が単一の便(trip)インスタンスに解決されない場合（つまり、0件または複数の便(trip)インスタンスに解決される場合）、エラーと見なされ、エラーのある TripDescriptor を含むエンティティはコンシューマーによって破棄されることがあります。

trip_id が不明な場合、TripUpdate 内の駅順序 ID では不十分であり、stop_ids も提供しなければならないことに注意してください。さらに、絶対的な到着時刻／出発時刻を提供しなければなりません。

TripDescriptor.route_id は、ルート・路線系統(route)のすべての便(trip)に影響するルート・路線系統(route)全体の運行情報(alert)を指定するために Alert EntitySelector 内で使用することはできません。代わりに EntitySelector.route_id を使用してください。

`schedule_relationship` が `NEW` の場合、`trip_id` は GTFS feed に記載されていない値に設定しなければならず、便(trip)をルート・路線系統(route)に関連付けるため、`route_id` は GTFS static の `routes.txt` に記載されている値に設定しなければなりません。新しい便(trip)には `start_date` を設定するべきであり、`direction_id` を設定することができます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクターが参照する GTFS feed の trip_id です。非頻度ベースの便(trip)（GTFS frequencies.txt で定義されていない便(trip)）では、このフィールドだけで便(trip)を一意に識別できます。GTFS frequencies.txt で定義された頻度ベースの便(trip)では、trip_id、start_time、および start_date がすべて必須です。スケジュールベースの便(trip)（GTFS frequencies.txt で定義されていない便(trip)）では、route_id、direction_id、start_time、および start_date の組み合わせによって便(trip)を一意に識別でき、かつそれらすべてのフィールドが提供される場合にのみ、trip_id を省略できます。schedule_relationship が NEW の場合、GTFS static で定義されていない一意の値で指定しなければなりません。schedule_relationship が REPLACEMENT の場合、trip_id は置き換えられる static GTFS の便(trip)を識別します。TripUpdate 内で schedule_relationship が DUPLICATED の場合、trip_id は複製される static GTFS の便(trip)を識別します。VehiclePosition 内で schedule_relationship が DUPLICATED の場合、trip_id は新しい複製便(trip)を識別し、対応する TripUpdate.TripProperties.trip_id の値を含まなければなりません。 |
| **route_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクターが参照する GTFS の route_id です。trip_id が省略される場合、便(trip)インスタンスを識別するために route_id、direction_id、start_time、および schedule_relationship=SCHEDULED をすべて設定しなければなりません。TripDescriptor.route_id は、ルート・路線系統(route)のすべての便(trip)に影響するルート・路線系統(route)全体の運行情報(alert)を指定するために Alert EntitySelector 内で使用するべきではありません。代わりに EntitySelector.route_id を使用してください。schedule_relationship が NEW の場合、新しい便(trip)が属するルート・路線系統(route)の route_id を指定しなければなりません。 |
| **direction_id** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクターが参照する便(trip)の進行方向を示す、GTFS feed の trips.txt ファイルの direction_id です。trip_id が省略される場合、direction_id を提供しなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用されることがあります。<br> |
| **start_time** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | この便(trip)インスタンスの当初スケジュールされた開始時刻です。trip_id が非頻度ベースの便(trip)に対応する場合、このフィールドは省略するか、GTFS feed 内の値と等しくするべきです。trip_id が GTFS frequencies.txt で定義された頻度ベースの便(trip)に対応する場合、start_time は必須であり、便の更新(trip update)および車両位置情報(vehicle position)に指定しなければなりません。便(trip)が exact_times=1 の GTFS レコードに対応する場合、start_time は対応する時間帯の frequencies.txt の start_time より headway_secs の整数倍（ゼロを含む）だけ後の時刻でなければなりません。便(trip)が exact_times=0 に対応する場合、その start_time は任意であり、当初は便(trip)の最初の出発時刻であることが想定されます。一度確立された後、この頻度ベースの exact_times=0 の便(trip)の start_time は、最初の出発時刻が変更された場合でも不変と見なすべきです。その時刻変更は、代わりに StopTimeUpdate に反映することができます。trip_id が省略される場合、start_time を提供しなければなりません。フィールドの形式および意味は GTFS/frequencies.txt/start_time と同じです。例: 11:15:35 または 25:15:35。 |
| **start_date** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | YYYYMMDD 形式の、この便(trip)インスタンスの開始日です。スケジュールされた便(trip)（GTFS frequencies.txt で定義されていない便(trip)）では、翌日のスケジュールされた便(trip)と重複するほど遅延した便(trip)を区別するために、このフィールドを提供しなければなりません。たとえば、毎日 8:00 と 20:00 に出発する列車が12時間遅延した場合、同じ時刻に2つの異なる便(trip)が存在することになります。このような重複が不可能なスケジュールでは、このフィールドを提供できますが必須ではありません。たとえば、1時間遅れた車両がもはやスケジュールに関連しているとは見なされない、毎時運行のサービスなどです。このフィールドは、GTFS frequencies.txt で定義された頻度ベースの便(trip)では必須です。trip_id が省略される場合、start_date を提供しなければなりません。 |
| **schedule_relationship** | [ScheduleRelationship](#enum-schedulerelationship_1) | 任意 | 1つ | この便(trip)と static schedule の関係です。TripDescriptor が運行情報(alert)の `EntitySelector` 内で提供される場合、`schedule_relationship` フィールドは、一致する便(trip)インスタンスを識別する際にコンシューマーによって無視されます。 |
| **modified_trip** | [ModifiedTripSelector](#message-modifiedtripselector) | 任意 | 1つ | この便(trip)に対して行われた変更（ルート形状(shape)の変更、停留所等(stop)の削除または追加）へのリンクです。このフィールドが提供される場合、`ModifiedTripSelector` 値を参照しないコンシューマーの混乱を避けるため、`TripDescriptor` の `trip_id`、`route_id`、`direction_id`、`start_time`、`start_date` フィールドは空のままにしておかなければなりません。 |

### _enum_ ScheduleRelationship {: #enum-schedulerelationship}


この便(trip)と静的スケジュールとの関係です。GTFSに反映されていない一時的なスケジュールに従って新しい便(trip)が運行される場合、SCHEDULEDとしてマークするべきではなく、NEWとしてマークするべきです。GTFSに反映されていない変更後のスケジュールに従って便(trip)が運行される場合、SCHEDULEDとしてマークするべきではなく、REPLACEMENTとしてマークするべきです。

**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **SCHEDULED** | GTFSスケジュールに従って運行している便(trip)、または予定された便(trip)に関連付けるのに十分近い便(trip)です。 |
| **ADDED** | *注: 動作が未規定であったため、この値は非推奨となりました。開始日または時刻を除いて予定された便(trip)と同一の追加便(trip)には**DUPLICATED**を使用し、既存の便(trip)と無関係な追加便(trip)には**NEW**を使用してください。* |
| **UNSCHEDULED** | 関連付けられたスケジュールなしで運行している便(trip)です。この値は、exact_times = 0 を持つGTFS frequencies.txtで定義された便(trip)を識別するために使用されます。GTFS frequencies.txtで定義されていない便(trip)、または exact_times = 1 を持つGTFS frequencies.txt内の便(trip)を記述するために使用するべきではありません。`schedule_relationship: UNSCHEDULED` を持つ便(trip)は、すべてのStopTimeUpdatesにも `schedule_relationship: UNSCHEDULED` を設定しなければなりません。 |
| **CANCELED** | スケジュールには存在していたものの、削除された便(trip)です。 |
| **REPLACEMENT** | 変更されたスケジュールまたは迂回経路などにより、既存の予定された便(trip)を置き換える便(trip)です。置換便(trip)の完全な旅程(journey)は `StopTimeUpdate`s を通じて指定しなければならず、置き換えられるインスタンスには静的GTFSの元のスケジュールは使用されません。<br>`REPLACEMENT` は、便(trip)が改訂されたスケジュールで運行される場合に使用できますが、車両が静的GTFSの `stop_times.txt` に記載されたスケジュールに従うことを意図している場合、リアルタイムのスケジュール逸脱（予測）を伝達するために使用してはいけません。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **DUPLICATED** | 運行開始日および時刻を除いて、既存の予定された便(trip)と同一の新しい便(trip)です。`TripUpdate.TripProperties.trip_id`、`TripUpdate.TripProperties.start_date`、および `TripUpdate.TripProperties.start_time` とともに使用し、静的GTFSから既存の便(trip)をコピーして、異なる運行日(service day)および/または時刻に開始します。元の便(trip)に関連する（CSV）GTFS内のサービス（`calendar.txt` または `calendar_dates.txt` 内）が今後30日以内に運行される場合、便(trip)の複製が許可されます。複製する便(trip)は `TripUpdate.TripDescriptor.trip_id` を通じて識別されます。 <br><br> この列挙型は `TripUpdate.TripDescriptor.trip_id` によって参照される既存の便(trip)を変更しません。プロデューサーが元の便(trip)をキャンセルしたい場合、CANCELEDの値を持つ別個の `TripUpdate` を公開しなければなりません。`exact_times` が空または `0` に等しいGTFS `frequencies.txt` で定義された便(trip)は複製できません。新しい便(trip)の `VehiclePosition.TripDescriptor.trip_id` には `TripUpdate.TripProperties.trip_id` の対応する値を含めなければならず、`VehiclePosition.TripDescriptor.ScheduleRelationship` も `DUPLICATED` に設定しなければなりません。  <br><br>*複製された便(trip)を表すためにADDED列挙型を使用していた既存のプロデューサーおよびコンシューマーは、DUPLICATED列挙型へ移行するために[migration guide](../../realtime/examples//migration-duplicated)に従わなければなりません。* |
| **NEW** | 突発的な乗客需要に対応する場合など、既存のいかなる便(trip)とも無関係な追加便(trip)です。すべての停留所等(stop)および時刻を含む新しい便(trip)の完全な旅程(journey)は、`StopTimeUpdate`s を通じて指定しなければなりません。   <br><br>*静的GTFSと無関係な新しい便(trip)を表すためにADDED列挙型を使用していた既存のプロデューサーおよびコンシューマーは、NEW列挙型へ移行するために[migration guide](../../realtime/examples//migration-duplicated)に従わなければなりません。*<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される可能性があります。  |
| **DELETED** | スケジュールには存在していたものの、削除され、ユーザーに表示してはいけない便(trip)です。 <br><br> DELETEDは、交通事業者が対応する便(trip)に関する情報をコンシューマーアプリケーションから完全に削除したい場合に、CANCELEDの代わりに使用するべきです。これにより、例えば別の便(trip)によって完全に置き換えられる便(trip)について、乗客にキャンセルとして表示されません。複数の便(trip)がキャンセルされ、代替サービスに置き換えられる場合、この指定は特に重要になります。コンシューマーがキャンセルに関する明示的な情報を表示すると、より重要なリアルタイム予測から注意をそらすことになります。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

## _message_ ModifiedTripSelector {: #message-modifiedtripselector}


運行サービスが便の変更(trip modification)の影響を受ける場合、`ModifiedTripSelector` を使用して便(trip)を選択します。詳細は、[Trip Modification](https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/trip-modifications.md#linkage-to-tripupdates)仕様を参照してください。

**値**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **modifications_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 含まれる `TripModifications` オブジェクトがこの便(trip)に影響を与える `FeedEntity` の `id` です。|
| **affected_trip_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | `modifications_id` によって変更される GTFS feed の `trip_id` です。|
| **start_time** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | frequency based の変更された便(trip)に適用される、この便(trip)インスタンスの当初予定されていた開始時刻です。[TripDescriptor](#message-tripdescriptor) の **start_time** と同じ定義です。|
| **start_date** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 変更された便(trip)に適用される、この便(trip)インスタンスの開始日です。YYYYMMDD 形式です。[TripDescriptor](#message-tripdescriptor) の **start_date** と同じ定義です。|

### _message_ VehicleDescriptor {: #message-vehicledescriptor}


便(trip)を運行する車両の識別情報です。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両の内部システム識別子です。車両ごとに**一意**であるべきであり、システム内を移動する車両を追跡するために使用されます。このidをエンドユーザーに表示してはいけません。その目的には**label**フィールドを使用してください。 |
| **label** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | ユーザーに表示されるラベルです。すなわち、正しい車両を識別するために乗客に表示しなければならないものです。 |
| **license_plate** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 車両のナンバープレートです。 |
| **wheelchair_accessible** | [WheelchairAccessible](#enum-wheelchairaccessible) | 任意 | 1つ | 提供される場合、静的GTFSの*wheelchair_accessible*値を上書きすることができます。 |

### _enum_ WheelchairAccessible {: #enum-wheelchairaccessible}


特定の便(trip)が車椅子で利用可能かどうかを示します。利用可能な場合、この値は静的 GTFS の _wheelchair_accessible_ 値を上書きするべきです。

##### 値 {: #values}


| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **NO_VALUE** | 便(trip)には車椅子対応に関する情報がありません。これは**デフォルト**の動作です。静的GTFSに_wheelchair_accessible_値が含まれている場合、それは上書きされません。 |
| **UNKNOWN** | 便(trip)にはアクセシビリティ値が存在しません。この値はGTFSの値を上書きします。  |
| **WHEELCHAIR_ACCESSIBLE** | 便(trip)は車椅子で利用可能です。この値はGTFSの値を上書きします。 |
| **WHEELCHAIR_INACCESSIBLE** | 便(trip)は車椅子で**利用できません**。この値はGTFSの値を上書きします。 |

### _message_ EntitySelector {: #message-entityselector}


GTFS feed内のエンティティのセレクタです。フィールドの値は、GTFS feed内の該当するフィールドに対応するべきです。少なくとも1つの指定子を指定しなければなりません。複数指定する場合、それらは論理 `AND` 演算子で結合されているものとして解釈するべきです。さらに、指定子の組み合わせは、GTFS feed内の対応する情報と一致しなければなりません。言い換えると、運行情報(alert)がGTFS内のエンティティに適用されるためには、提供されたすべてのEntitySelectorフィールドと一致しなければなりません。例えば、`route_id: "5"` および `route_type: "3"` のフィールドを含むEntitySelectorは、`route_id: "5"` のバスにのみ適用されます。`route_type: "3"` の他のルート・路線系統(route)には適用されません。作成者が `route_id: "5"` と `route_type: "3"` の両方に運行情報(alert)を適用したい場合、`route_id: "5"` を参照するものと `route_type: "3"` を参照するものの、2つの別個のEntitySelectorを提供するべきです。

少なくとも1つの指定子を指定しなければなりません。EntitySelector内のすべてのフィールドを空にすることはできません。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **agency_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクタが参照するGTFS feedのagency_idです。 |
| **route_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクタが参照するGTFSのroute_idです。direction_idが指定される場合、route_idも指定しなければなりません。 |
| **route_type** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクタが参照するGTFSのroute_typeです。 |
| **direction_id** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | route_idで指定されたルート・路線系統(route)について、一方向のすべての便(trip)を選択するために使用される、GTFS feedのtrips.txtファイル内のdirection_idです。direction_idが指定される場合、route_idも指定しなければなりません。<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来、正式に採用されることがあります。<br> |
| **trip** | [TripDescriptor](#message-tripdescriptor) | 条件付き必須 | 1つ | このセレクタが参照するGTFS内の便(trip)インスタンスです。このTripDescriptorは、GTFSデータ内の単一の便(trip)インスタンスに解決されなければなりません（例: 作成者はexact_times=0の便(trip)に対してtrip_idのみを提供することはできません）。このTripDescriptor内のScheduleRelationshipフィールドが設定されている場合、GTFSの便(trip)を識別しようとする際にコンシューマーによって無視されます。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | このセレクタが参照するGTFS feedのstop_idです。 |

### _message_ TranslatedString {: #message-translatedstring}


テキストの断片または URL の言語ごとのバージョンを含む国際化メッセージです。メッセージ内の文字列のいずれか1つが選択されます。解決は次のように進みます。UI 言語が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。デフォルトの UI 言語（例: English）が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。いずれかの翻訳に言語コードが指定されていない場合、その翻訳が選択されます。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **translation** | [Translation](#message-translation) | 必須 | 複数 | 少なくとも1つの翻訳を提供しなければなりません。 |

### _message_ Translation {: #message-translation}


言語にマッピングされたローカライズ済み文字列です。

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **text** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | メッセージを含む UTF-8 文字列です。 |
| **language** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | BCP-47 言語コードです。言語が不明な場合、またはフィードに対して国際化がまったく行われていない場合は、省略することができます。言語タグが指定されていない翻訳は最大で1つのみ許可されます。複数の翻訳がある場合は、言語を指定しなければなりません。 |

### _message_ TranslatedImage {: #message-translatedimage}


画像の言語ごとのバージョンを含む国際化されたメッセージです。メッセージ内の画像のうち1つが選択されます。解決は次のように進みます。UI言語が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。デフォルトのUI言語（例: English）が翻訳の言語コードと一致する場合、最初に一致した翻訳が選択されます。いずれかの翻訳に言語コードが指定されていない場合、その翻訳が選択されます。

**注意:** このメッセージは依然として**実験的**であり、変更される可能性があります。将来、正式に採用されることがあります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **localized_image** | [LocalizedImage](#message-localizedimage) | 必須 | 複数 | 少なくとも1つのローカライズされた画像を提供しなければなりません。 |

### _message_ LocalizedImage {: #message-localizedimage}


言語にマッピングされたローカライズ済み画像 URL です。

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **url** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 画像にリンクする URL を含む文字列です。リンク先の画像は 2MB 未満でなければなりません。消費者側で更新が必要となるほど画像が大幅に変更された場合、生成者は URL を新しいものに更新しなければなりません。<br><br> URL は http:// または https:// を含む完全修飾 URL であるべきであり、URL 内の特殊文字は正しくエスケープしなければなりません。完全修飾 URL 値の作成方法については、次の http://www.w3.org/Addressing/URL/4_URI_Recommentations.html を参照してください。  |
| **media_type** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 表示する画像の種類を指定する IANA メディアタイプです。この種類は "image/" で始まらなければなりません。 |
| **language** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | BCP-47 言語コードです。言語が不明な場合、またはフィードに対して国際化がまったく行われていない場合は省略できます。言語タグが未指定の翻訳は最大 1つまで許可されます。複数の翻訳がある場合は、言語を指定しなければなりません。 |

### _message_ Shape {: #message-shape}


アドホックな迂回など、shape が（CSV）GTFS の一部ではない場合に、車両がたどる物理的な経路を記述します。shape は便(trip)に属し、より効率的な送信のためのエンコードされたポリラインで構成されます。shape は停留所等(stop)の位置を正確に通過する必要はありませんが、便(trip)上のすべての停留所等(stop)は、その便(trip)の shape から短い距離内、すなわち shape の点を結ぶ直線セグメントの近くに存在するべきです。

<br><br>**注意:** このメッセージは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **shape_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  shape の識別子。（CSV）GTFS で定義されているいかなる `shape_id` とも異ならなければなりません。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |
| **encoded_polyline** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | shape のエンコードされたポリライン表現です。このポリラインは少なくとも2つの点を含み、使用される便(trip)の完全な shape を表現しなければなりません。エンコードされたポリラインの詳細については、https://developers.google.com/maps/documentation/utilities/polylinealgorithm を参照してください。<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来、正式に採用される可能性があります。 |

### _message_ Stop {: #message-stop}


フィードに動的に追加される新しい停留所等(stop)を表します。すべてのフィールドは、(CSV) GTFS 仕様で説明されているとおりです。新しい停留所等(stop)のlocation typeは`0`（経路探索可能な停留所等(stop)）です。 

<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来的に正式に採用されることがあります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  停留所等(stop)の識別子です。(CSV) GTFS で定義されているいかなる`stop_id`とも異ならなければなりません。 |
| **stop_code** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における[stops.stop_code](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **stop_name** | [TranslatedString](#message-translatedstring) | 必須 | 1つ |  (CSV) GTFS における[stops.stop_name](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **tts_stop_name** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における[stops.tts_stop_name](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **stop_desc** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における[stops.stop_desc](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **stop_lat** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  (CSV) GTFS における[stops.stop_lat](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **stop_lon** | [float](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ |  (CSV) GTFS における[stops.stop_lon](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **zone_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における[stops.zone_id](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **stop_url** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における[stops.stop_url](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **parent_station** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における[stops.parent_station](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **stop_timezone** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における[stops.stop_timezone](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **wheelchair_boarding** | [WheelchairBoarding](#enum-wheelchairboarding) | 任意 | 1つ |  (CSV) GTFS における[stops.wheelchair_boarding](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **level_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ |  (CSV) GTFS における[stops.level_id](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |
| **platform_code** | [TranslatedString](#message-translatedstring) | 任意 | 1つ |  (CSV) GTFS における[stops.platform_code](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md#stopstxt)の定義を参照してください。 |

### _enum_ WheelchairBoarding {: #enum-wheelchairboarding}


**値**

| _**値**_ | _**コメント**_ |
|-------------|---------------|
| **UNKNOWN** | 停留所等(stop)に関するアクセシビリティ情報はありません。 |
| **AVAILABLE** | この停留所等(stop)では、一部の車両に車椅子の乗客が乗車することができます。 |
| **NOT_AVAILABLE** | この停留所等(stop)では、車椅子での乗車はできません。 |

### _message_ TripModifications {: #message-tripmodifications}


`TripModifications` メッセージは、迂回などの特定の変更の影響を受ける、類似した便(trip)のリストを識別します。

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

[Trip Modifications の詳細...](../../../documentation/realtime/feed-entities/trip-modifications)

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **selected_trips** | [SelectedTrips](#message-selectedtrips) | 必須 | 複数 | この TripModifications の影響を受ける選択された便(trip)のリストです。少なくとも1つの `SelectedTrips` を含めなければなりません。値 `start_times` が設定されている場合、1つの trip_id を持つ `SelectedTrips` は最大1つのみリストできます。  |
| **start_times** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 複数 | trip_ids で定義された trip_id に対する、リアルタイム便記述子内の開始時刻のリストです。頻度ベースの便(trip)において、1つの trip_id の複数の出発を対象とする場合に有用です。 |
| **service_dates** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 複数 | 変更が発生する日付です。YYYYMMDD 形式で指定します。trip_id は、指定された運行日(service day)に運行される場合にのみ変更されます。便(trip)はすべての運行日(service day)に運行される必要はありません。プロデューサーは、次の1週間以内に発生する迂回のみを送信するべきです。提供される日付はユーザー向け情報として使用するべきではありません。ユーザー向けの開始日と終了日を提供する必要がある場合は、`service_alert_id` を持つリンクされた運行情報(alert)で提供できます。 |
| **modifications** | [Modification](#message-modification) | 必須 | 複数 | 影響を受ける便(trip)に適用する変更のリストです。 |

### _message_ Modification {: #message-modification}


`Modification` メッセージは、`start_stop_selector` から開始する、影響を受ける各便(trip)への変更を記述します。

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

<img src="../../../assets/trip-modification.png">

_特定の便(trip)に対する変更の影響を示す例です。この変更は、他の複数の便(trip)にも適用される場合があります。_

<img src="../../../assets/propagated-delay.png">

_伝播する迂回遅延は、変更の終了後に続くすべての停留所等(stop)に影響します。便(trip)に複数の変更がある場合、遅延は累積されます。_


**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **start_stop_selector** | [StopSelector](#message-stopselector) | 必須 | 1つ | この変更の影響を受ける元の便(trip)の最初の停留所等(stop)の停留所等(stop)セレクタです。`end_stop_selector` と組み合わせて使用されます。`start_stop_selector` は必須であり、`travel_time_to_stop` とともに使用される基準停留所等(stop)を定義するために使用されます。詳細は [`travel_time_to_stop`](#message-replacementstop) を参照してください。 |
| **end_stop_selector** | [StopSelector](#message-stopselector) | 条件付き必須 | 1つ | この変更の影響を受ける元の便(trip)の最後の停留所等(stop)の停留所等(stop)セレクタです。選択範囲は両端を含むため、この変更によって置き換えられる停車時刻(stop_time)が1つだけの場合、`start_stop_selector` と `end_stop_selector` は同等でなければなりません。停車時刻(stop_time)が置き換えられない場合、`end_stop_selector` を指定してはいけません。それ以外の場合は必須です。  |
| **propagated_modification_delay** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 変更によって挿入された最後の停留所等(stop)より後のすべての出発時刻および到着時刻に加算する遅延秒数です。変更がルート形状(shape)のみに影響する場合（すなわち、`end_stop_selector` も `replacement_stops` も指定されない場合）、遅延の伝播は `start_stop_selector` の次の停留所等(stop)から開始します。正または負の数にすることができます。同じ便(trip)に複数の変更が適用される場合、便(trip)の進行に伴って遅延は累積されます。<br/><br/>値が指定されない場合、コンシューマーは他のデータに基づいて `propagated_modification_delay` を補間または推定することができます。  |
| **replacement_stops** | [ReplacementStop](#message-replacementstop) | 任意 | 複数 | 元の便(trip)の停留所等(stop)を置き換える、置換停留所等(stop)のリストです。新しい停車時刻(stop_time)の数は、置き換えられる停車時刻(stop_time)の数より少なく、同じ、または多くすることができます。 |
| **service_alert_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | 利用者向けの通信のために、この Modification を記述する `Alert` を含む `FeedEntity` メッセージの `id` 値です。 |
| **last_modified_time** | [uint64](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | このタイムスタンプは、変更が最後に変更された時点を識別します。POSIX 時刻（すなわち、1970年1月1日 00:00:00 UTC からの秒数）です。 |

### _message_ StopSelector {: #message-stopselector}


停留所等(stop)のセレクタです。`stop_id` または `stop_sequence` のいずれかで指定します。2つの値のうち少なくとも1つを指定しなければなりません。 

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用されることがあります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_sequence** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 対応する GTFS feed の stop_times.txt 内の値と同じでなければなりません。`StopSelector` 内では `stop_sequence` または `stop_id` のいずれかを指定しなければなりません。両方のフィールドを空にしてはいけません。予測の対象となる停留所等(stop)を明確にするため、同じ stop_id を複数回訪問する便(trip)（例: ループ）では `stop_sequence` が必須です。 |
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 条件付き必須 | 1つ | 対応する GTFS feed の stops.txt 内の値と同じでなければなりません。`StopSelector` 内では `stop_sequence` または `stop_id` のいずれかを指定しなければなりません。両方のフィールドを空にしてはいけません。 |

### _message_ SelectedTrips {: #message-selectedtrips}


関連するshapeを持つ、選択された便(trip)のリストです。

<br><br>**注意:** このフィールドは依然として**experimental**であり、変更される可能性があります。将来的に正式採用される場合があります。

**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **trip_ids** | [uint32](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 複数 | 含まれるreplacementの影響を受ける、元の（CSV）GTFSのtrip_idのリストです。少なくとも1つのtrip_idを含めなければなりません。その便(trip)に対して、`schedule_relationship=REPLACEMENT`を持つ`TripUpdate`がすでに存在していてはいけません。 |
| **shape_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | このSelectedTrips内で変更される便(trip)の新しいshapeのIDです。同じGTFS-RT feed内の`Shape`メッセージを使用して追加された新しいshape、またはGTFS-Static feedのshapes.txtで定義された既存のshapeを参照することができます。real-time feed内の`Shape` entityを参照する場合、このフィールドの値はentity内の`shape_id`の値であるべきであり、`FeedEntity`の`id`ではありません。 |

### _message_ ReplacementStop {: #message-replacementstop}


各 `ReplacementStop` メッセージは、便(trip)が新たに訪問する停留所等(stop)を定義し、任意でその停留所等(stop)までの推定所要時間を指定します。 

<br><br>**注意:** このフィールドは依然として**実験的**であり、変更される可能性があります。将来的に正式に採用される場合があります。

<img src="../../../assets/first-stop-reference.png">

_変更が便(trip)の最初の停留所等(stop)に影響する場合、その停留所等(stop)は変更の基準停留所等(stop)も兼ねます。_


**フィールド**

| _**フィールド名**_ | _**型**_ | _**必須**_ | _**多重度(cardinality)**_ | _**説明**_ |
|------------------|------------|----------------|-------------------|-------------------|
| **stop_id** | [string](https://protobuf.dev/programming-guides/proto2/#scalar) | 必須 | 1つ | 便(trip)が新たに訪問する代替停留所等(stop) IDです。同じ GTFS-RT feed 内の GTFS-RT `Stop` メッセージを使用して追加された新しい停留所等(stop)、または (CSV) GTFS feed の `stops.txt` で定義された既存の停留所等(stop)を参照できます。real-time feed 内の `Shape` entity を参照する場合、このフィールドの値は entity 内の `stop_id` の値であるべきであり、`FeedEntity` の `id` ではありません。停留所等(stop)は `location_type=0`（経路設定可能な停留所等(stop)）でなければなりません。 |
| **travel_time_to_stop** | [int32](https://protobuf.dev/programming-guides/proto2/#scalar) | 任意 | 1つ | この停留所等(stop)への到着時刻と基準停留所等(stop)への到着時刻との差（秒）です。基準停留所等(stop)は `start_stop_selector` の前の停留所等(stop)です。変更が便(trip)の最初の停留所等(stop)から始まる場合、便(trip)の最初の停留所等(stop)が基準停留所等(stop)となります。<br/><br/>この値は単調増加でなければならず、元の便(trip)の最初の停留所等(stop)が基準停留所等(stop)である場合にのみ負の数にできます。<br/><br/>値が提供されない場合、consumer は他のデータに基づいて `travel_time_to_stop` を補間または推定できます。 |
