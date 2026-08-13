# 便の更新(trip update) {: #trip-updates}


便の更新(trip update)は、時刻表の変動を表します。リアルタイム対応の、予定されているすべての便(trip)について、便の更新(trip update)を受信することが期待されます。これらの更新は、ルート・路線系統(route)上の停留所等(stop)について、予測到着時刻または予測出発時刻を提供します。便の更新(trip update)は、便(trip)がキャンセルされたり、時刻表に追加されたり、さらには経路変更されたりする、より複雑なシナリオにも対応できます。

**注意:** [GTFS](../../../schedule/reference)では、便(trip)は特定の時刻に発生する2つ以上の停留所等(stop)の連続です。

予定されている各便(trip)について、便の更新(trip update)は**最大でも**1つであるべきです。予定されている便(trip)に便の更新(trip update)がない場合、その便(trip)にはリアルタイムデータが利用できないと判断されます。データ利用者は、その便(trip)が定刻どおりに運行していると想定しては**いけません**。

車両が同一ブロック内で複数の便(trip)を運行する場合（便(trip)およびブロックの詳細については、[GTFS trips.txt](../../../schedule/reference/#tripstxt)を参照してください）:

* フィードには、車両が現在運行している便(trip)のTripUpdateを含めるべきです。提供者は、将来の便(trip)の予測品質に確信がある場合、この車両のブロックにおいて現在の便(trip)の後に続く1つ以上の便(trip)についてTripUpdateを含めることが推奨されます。同じ車両について複数のTripUpdateを含めることで、車両がある便(trip)から別の便(trip)へ移行する際に乗客にとって予測が「突然表示される」ことを回避でき、また、後続の便(trip)に影響する遅延（例: 既知の遅延が便(trip)間の予定待機時間を超える場合）を乗客に事前通知できます。
* 各TripUpdateエンティティは、ブロック内で予定されている順序と同じ順序でフィードに追加する必要はありません。たとえば、すべて1つのブロックに属する`trip_ids` 1、2、3の便(trip)があり、車両が便(trip) 1、次に便(trip) 2、次に便(trip) 3を運行する場合、`trip_update`エンティティは任意の順序で出現できます。たとえば、便(trip) 2、次に便(trip) 1、次に便(trip) 3を追加することが許可されます。

## StopTimeUpdate {: #stoptimeupdate}


便の更新(trip update)は、車両の停車時刻(stop_time)に対する1つ以上の更新で構成され、これらは[StopTimeUpdates](../../reference/#message-stoptimeupdate)と呼ばれます。これらは過去および将来の停車時刻(stop_time)に対して提供できます。GTFS staticに存在しない新規または置換の便(trip)である場合を除き、過去の停車時刻(stop_time)を削除することは許可されていますが、必須ではありません。指定された便(trip)について、将来の予定到着時刻を持つ停留所等(stop)を参照する過去の`StopTimeUpdate`を、提供者は削除するべきではありません（すなわち、車両が予定より早くその停留所等(stop)を通過した場合）。そうしない場合、この停留所等(stop)には更新がないと判断されます。

たとえば、GTFS-rt feedに次のデータが含まれている場合です。

* 停留所等(stop) 4 – 10:18am到着予測（予定は10:20am – 2分早着）
* 停留所等(stop) 5 – 10:30am到着予測（予定は10:30am – 定時）

...バスが実際に10:18amに停留所等(stop)を通過した場合でも、停留所等(stop) 4の予測は10:21amまでfeedから削除できません。停留所等(stop) 4の`StopTimeUpdate`が10:18amまたは10:19amにfeedから削除され、予定到着時刻が10:20amである場合、consumerはその時点で停留所等(stop) 4にリアルタイム情報が存在しないとみなし、GTFSのスケジュールデータを使用するべきです。

各[StopTimeUpdate](../../reference/#message-stoptimeupdate)は、停留所等(stop)にリンクされます。通常、これはGTFSのstop_sequenceまたはGTFSのstop_idのいずれかを使用して行えます。ただし、GTFS trip_idを持たない便(trip)の更新を提供する場合、stop_sequenceには値がないため、stop_idを指定しなければなりません。stop_idは引き続きGTFS内のstop_idを参照しなければなりません。同じstop_idが1つの便(trip)内で複数回訪問される場合、その便(trip)におけるそのstop_idのすべてのStopTimeUpdatesでstop_sequenceを提供するべきです。

更新では、[StopTimeEvent](../../reference/#message-stoptimeevent)を使用して、[StopTimeUpdates](../../reference/#message-stoptimeupdate)内の停留所等(stop)における**arrival**および/または**departure**の正確な時刻を提供できます。これには、絶対的な**time**または**delay**（すなわち、予定時刻からの秒単位のオフセット）のいずれかを含めるべきです。Delayは、便の更新(trip update)がfrequency-based tripではなく、予定されたGTFS便(trip)を参照する場合にのみ使用できます。この場合、timeは予定時刻 + delayと等しくするべきです。[StopTimeEvent](../../reference/#message-stoptimeevent)とともに予測の**uncertainty**を指定することもできます。これについては、このページの後半にある[Uncertainty](#uncertainty)セクションで詳しく説明します。

各[StopTimeUpdate](../../reference/#message-stoptimeupdate)について、デフォルトのschedule relationshipは**scheduled**です。（これは便(trip)のschedule relationshipとは異なることに注意してください。）停留所等(stop)に停車しない場合はこれを**skipped**に、便(trip)の一部についてのみリアルタイムデータがある場合は**no data**に変更できます。

**更新はstop_sequenceの順序でソートするべきです**（または、便(trip)内で出現する順序のstop_ids）。

便(trip)の途中で1つ以上の停留所等(stop)が欠落している場合、更新の`delay`（または、更新で`time`のみが提供されている場合は、`time`とGTFSの予定時刻を比較して算出されたdelay）が、以降のすべての停留所等(stop)に伝播します。これは、ある停留所等(stop)の停車時刻(stop_time)を更新すると、他の情報がない限り、以降のすべての停留所等(stop)が変更されることを意味します。schedule relationshipが`SKIPPED`の更新はdelayの伝播を停止しませんが、schedule relationshipが`SCHEDULED`（schedule relationshipが提供されない場合のデフォルト値でもあります）または`NO_DATA`の更新は停止することに注意してください。

**例1**

20の停留所等(stop)を持つ便(trip)について、現在の停留所等(stop)のstop_sequenceに対し、arrival delayおよびdeparture delayが0の[StopTimeUpdate](../../reference/#message-stoptimeupdate)（[StopTimeEvents](../../reference/#message-stoptimeevent)）は、その便(trip)が正確に定時であることを意味します。

**例2**

同じ便(trip)インスタンスについて、3つの[StopTimeUpdates](../../reference/#message-stoptimeupdate)が提供されます。

*   stop_sequence 3に対する300秒のdelay
*   stop_sequence 8に対する60秒のdelay
*   stop_sequence 10に対する`NO_DATA`の[ScheduleRelationship](../../reference/#enum-schedulerelationship)

これは次のように解釈されます。

*   stop_sequences 1,2のdelayは不明です。
*   stop_sequences 3,4,5,6,7のdelayは300秒です。
*   stop_sequences 8,9のdelayは60秒です。
*   stop_sequences 10,..,20のdelayは不明です。

## TripDescriptor {: #tripdescriptor}


TripDescriptor によって提供される情報は、更新する便(trip)のスケジュール関係に依存します。設定できる選択肢はいくつかあります。

|_**値**_|_**コメント**_|
|-----------|-------------|
| **Scheduled** | この便(trip)は GTFS スケジュールに従って運行しているか、またはそれに関連付けられる程度に近い状態です。 |
| **Added** | この便(trip)はスケジュールされておらず、追加されました。たとえば、需要に対応するため、または故障した車両を代替するためです。 |
| **Unscheduled** | この便(trip)は運行しており、スケジュールに関連付けられることはありません。たとえば、スケジュールがなく、バスがシャトルサービスとして運行する場合です。 |
| **Canceled** | この便(trip)はスケジュールされていましたが、現在は削除されています。 |
| **Duplicated** | この新しい便(trip)は、運行開始日および時刻を除き、static GTFS 内の既存の便(trip)のコピーです。新しい便(trip)は、TripProperties で指定された運行日(service day)および時刻に運行します。 |

ほとんどの場合、この更新に関連する GTFS 内のスケジュール済み便(trip)の trip_id を提供するべきです。

#### trip_id が繰り返されるシステム {: #systems-with-repeated-trip_ids}


frequency-based trips、すなわち frequencies.txt を使用してモデル化された便(trip)など、trip_id が繰り返されるシステムでは、trip_id には特定の時刻要素がないため、それ自体は単一の旅程(journey)を一意に識別する識別子ではありません。このような便(trip)を TripDescriptor 内で一意に識別するためには、次の3つの識別子を提供しなければなりません。

*    __trip_id__
*    __start_time__
*    __start_date__

start_time は最初に公開するべきであり、以降のフィード更新では、同じ旅程(journey)を参照する際に同じ start_time を使用するべきです。調整を示すには StopTimeUpdates を使用するべきです。start_time は最初の駅からの出発時刻と正確に一致する必要はありませんが、その時刻にかなり近いべきです。

例えば、2015年5月25日10:00に、trip_id=T の便(trip)が start_time=10:10:00 に開始すると決定し、この情報を10:01に realtime feed を通じて提供するとします。10:05までに、その便(trip)が10:10ではなく10:13に開始することが突然判明したとします。新しい realtime feed では、この便(trip)を引き続き (T, 2015-05-25, 10:10:00) として識別できますが、最初の停留所等(stop)からの出発を10:13:00とする StopTimeUpdate を提供します。

#### 代替の便照合 {: #alternative-trip-matching}


frequency に基づかない便(trip)も、以下の組み合わせを含む TripDescriptor によって一意に識別することができます。

*    __route_id__
*    __direction_id__
*    __start_time__
*    __start_date__

ここで、提供された ID の組み合わせが一意の便(trip)に解決される限り、start_time は静的スケジュールで定義されている予定開始時刻です。

## 不確実性 {: #uncertainty}


不確実性は、[StopTimeUpdate](../../reference/#message-stoptimeupdate) の時刻と遅延値の両方に適用されます。不確実性は、実際の遅延における予想誤差を秒単位の整数として大まかに指定します（ただし、正確な統計的意味はまだ定義されていないことに注意してください）。例えば、コンピュータによる時刻制御の下で運行される列車では、不確実性を 0 にすることができます。

例として、次の停留所等(stop)への到着遅延が 15 分と推定され、誤差幅 4 分以内（すなわち +2 / -2 分）である長距離バスの場合、Uncertainty 値は 240 になります。
